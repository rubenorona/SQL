# Implementación en MariaDB, exemplo1: Proxectos de Investigación

A partir do [primeiro exercicio de implementación DDL](https://github.com/davidgchaves/first-steps-with-git-and-github-wirtz-asir1-and-dam1/tree/master/exercicios-ddl/1-proxectos-de-investigacion) que fixemos na clase, imos agora crear a base de datos en MariaDB, o sistema xestor de bases de datos que [acabamos de instalar localmente](instalacionMariaDB.md).

**Nota**: Danse por explicados os conceptos básicos do [SQL DDL](DDL.md). Volver a explicar cada unha das funcións sería caer nunha redundancia de información. Polo tanto, neste exemplo basearémonos en como implementar unha base de datos dende a liña de comandos dun SXBD instalado no propio sistema, facendo especial fincapé nas diferenzas de MariaDB con PostgreSQL, xestor empregado nos supostos prácticos realizados nos apuntamentos. 

### Resumo dos criterios seguidos: 

- Evitar acentos e espazos en branco na nomenclatura de obxectos.
- Comezar por crear todas as táboas, declarando o tipo de dato, as claves primaria e os atributos únicos.
- ```NOT NULL``` en todos os atributos, salvo nas claves primarias e nos datos de rexistro non obrigatorio. 
- ```UNIQUE``` e ```PRIMARY KEY``` sempre de maneira simplificada, salvo cando son atributos compostos.
- Despois disto, ```ALTER TABLE``` para engadir todas as claves alleas (nomeando os ```CONSTRAINT```).
- Finalmente, engadir as restricións de ```CHECK``` (tamen de forma moi declarativa e dándolles nome).

### Crear e empregar a base de datos

```sql
sudo mysql
CREATE SCHEMA proxectos_de_investigacion;
USE           proxectos_de_investigacion;
```
En MariaDB debemos ter especificada a base de datos sobre a que estamos a definir datos, aparecendo o nome desta entre corchetes na liña de comandos. 
![ex1cap1](/img/ex1cap1.PNG)

### Creación das táboas

```sql
CREATE TABLE SEDE (
  nome_sede            VARCHAR(30)   PRIMARY KEY,
  campus               VARCHAR(30)   NOT NULL
);

CREATE TABLE UBICACION (
  nome_sede            VARCHAR(30),
  nome_departamento    VARCHAR(30),
  PRIMARY KEY (nome_sede, nome_departamento)
);

CREATE TABLE DEPARTAMENTO (
  nome_departamento    VARCHAR(30)   PRIMARY KEY,
  telefono             CHAR(9)       NOT NULL,
  director             CHAR(9)
);

CREATE TABLE GRUPO (
  nome_grupo           VARCHAR(30),
  nome_departamento    VARCHAR(30),
  area                 VARCHAR(30)   NOT NULL,
  lider                CHAR(9),
  PRIMARY KEY (nome_grupo, nome_departamento)
);

CREATE TABLE PROFESOR (
  dni                  CHAR(9)       PRIMARY KEY,
  nome_profesor        VARCHAR(30)   NOT NULL, 
  titulacion           VARCHAR(20)   NOT NULL,
  experiencia          INTEGER,
  grupo                VARCHAR(30),
  departamento         VARCHAR(30)
);

CREATE TABLE PARTICIPA (
  dni                  CHAR(9),
  codigo_proxecto      CHAR(5),
  data_inicio          DATE          NOT NULL,
  data_cese            DATE,
  dedicacion           INTEGER       NOT NULL,
  PRIMARY KEY (dni, codigo_proxecto)
);

CREATE TABLE PROXECTO (
  codigo_proxecto      CHAR(5)       PRIMARY KEY,
  nome_proxecto        VARCHAR(30)   NOT NULL  UNIQUE,
  orzamento            DECIMAL(15,2) NOT NULL,
  data_inicio          DATE          NOT NULL,
  data_fin             DATE,
  grupo                VARCHAR(30),
  departamento         VARCHAR(30)
);

CREATE TABLE PROGRAMA (
  nome_programa        VARCHAR(30)   PRIMARY KEY
);

CREATE TABLE FINANCIA (
  nome_programa        VARCHAR(30),
  codigo_proxecto      CHAR(5),
  numero_programa      CHAR(5)       NOT NULL,
  cantidade_financiada DECIMAL(15,2) NOT NULL,
  PRIMARY KEY (nome_programa, codigo_proxecto)
);
```
Mediante ```CREATE TABLE``` xeramos a estrutura da base de datos. Unha limitación que amosan MariaDB e MySQL é o feito de non poder crear dominios, algo moi conveniente con tipos de datos repetitivos de lonxitude limitada.
![ex1cap2](/img/ex1cap2.PNG)
![ex1cap3](/img/ex1cap3.PNG)

### Restrición da clave allea

```sql
ALTER TABLE UBICACION
  ADD CONSTRAINT FK_SEDE_UBICACION
    FOREIGN KEY             (nome_sede)
    REFERENCES SEDE         (nome_sede)
    ON DELETE  CASCADE
    ON UPDATE  CASCADE,
  ADD CONSTRAINT FK_DEPARTAMENTO_UBICACION
    FOREIGN KEY             (nome_departamento)
    REFERENCES DEPARTAMENTO (nome_departamento)
    ON DELETE  CASCADE
    ON UPDATE  CASCADE
;

ALTER TABLE DEPARTAMENTO
  ADD CONSTRAINT FK_PROFESOR_DEPARTAMENTO
    FOREIGN KEY             (director)
    REFERENCES PROFESOR     (dni)
    ON DELETE  SET NULL
    ON UPDATE  CASCADE
;

ALTER TABLE GRUPO
  ADD CONSTRAINT FK_DEPARTAMENTO_GRUPO
    FOREIGN KEY             (nome_departamento)
    REFERENCES DEPARTAMENTO (nome_departamento)
    ON DELETE  CASCADE 
    ON UPDATE  CASCADE,
  ADD CONSTRAINT FK_PROFESOR_GRUPO
    FOREIGN KEY             (lider)
    REFERENCES PROFESOR     (dni)
    ON DELETE  SET NULL
    ON UPDATE  CASCADE
;

ALTER TABLE PROFESOR
  ADD CONSTRAINT FK_GRUPO_PROFESOR
    FOREIGN KEY             (grupo,      departamento)
    REFERENCES GRUPO        (nome_grupo, nome_departamento)
    ON DELETE  SET NULL
    ON UPDATE  CASCADE
;

ALTER TABLE PARTICIPA
  ADD CONSTRAINT FK_PROFESOR_PARTICIPA
    FOREIGN KEY             (dni)
    REFERENCES PROFESOR     (dni)
    ON DELETE  NO ACTION
    ON UPDATE  CASCADE,
  ADD CONSTRAINT FK_PROXECTO_PARTICIPA
    FOREIGN KEY             (codigo_proxecto)
    REFERENCES PROXECTO     (codigo_proxecto)
    ON DELETE  NO ACTION
    ON UPDATE  CASCADE
;

ALTER TABLE PROXECTO
  ADD CONSTRAINT FK_GRUPO_PROXECTO
    FOREIGN KEY             (grupo,      departamento)
    REFERENCES GRUPO        (nome_grupo, nome_departamento)
    ON DELETE  SET NULL
    ON UPDATE  CASCADE
;

ALTER TABLE FINANCIA 
  ADD CONSTRAINT FK_PROXECTO_FINANCIA
    FOREIGN KEY             (codigo_proxecto)
    REFERENCES PROXECTO     (codigo_proxecto)
    ON DELETE  CASCADE
    ON UPDATE  CASCADE
;
```
Unha vez están todas as columnas da base de datos declaradas, resulta moi sinxelo facer a interrelación entre táboas. Empregamos o método máis declarativo posible.
![ex1cap4](/img/ex1cap4.PNG)
![ex1cap5](/img/ex1cap5.PNG)

### Establecer límites no rexistro de datos

```sql
ALTER TABLE PARTICIPA
  ADD CONSTRAINT data_inicio_antes_cese
    CHECK (data_inicio < data_cese)
;

ALTER TABLE PROXECTO
  ADD CONSTRAINT data_inicio_antes_fin
    CHECK (data_inicio < data_fin)
;
```
Os ```CHECK``` son o último tipo de restrición que facemos. Neste caso, temos que asegurarnos de que as datas de inicio sexan necesariamente anteriores ás de cese.

### Principais diferenzas DDL detectadas entre MariaDB e PostgreSQL

- Non se poden crear dominios, polo que a declaración de tipos de datos non pode ser tan ordeada e simplificada.
- Non existe ```MONEY``` como tipo de dato. No seu lugar empregamos un tipo numérico similar: ```DECIMAL(15,2)```.
- MariaDB ignora o nome establecido á restrición da ```PRIMARY KEY```, creando un warning. Polo tanto, evitámolo.
