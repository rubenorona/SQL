# Implementación en MariaDB, exemplo2: Naves Espaciais

Continuamos a realizar exemplos de deseño interno en MariaDB con este [segundo suposto práctico](https://github.com/davidgchaves/first-steps-with-git-and-github-wirtz-asir1-and-dam1/tree/master/exercicios-ddl/2-naves-espaciais) realizado en clase. Este exercicio vai ter paralelismos e amosar unha estrutura moi similar ó [exemplo que fixemos antes](exemplo1_MariaDB.md). Sen embargo, cómpre repetir que o funcionamento das setencias DDL xa se da por explicado nos [apuntamentos](DDL.md). Neste caso, pretendemos amosar a maneira de implementar unha base de datos nun SXBD instalado localmente, facendo fincapé nas diferenzas entre MariaDB e PostgreSQL.

## Índice

- [Resumo dos criterios seguidos](#resumo-dos-criterios-seguidos)
- [Crear e empregar a base de datos](#crear-e-empregar-a-base-de-datos)
- [Creación das táboas](#creación-das-táboas)
- [Restrición da clave allea](#restrición-da-clave-allea)
- [Establecer límites no rexistro de datos](#establecer-limites-no-rexistro-de-datos)
- [Principais diferenzas DDL detectadas entre MariaDB e PostgreSQL](#principais-diferenzas-ddl-detectadas-entre-mariadb-e-postgresql)

### Resumo dos criterios seguidos

Son os mesmos que no suposto práctico anterior:

- Evitar acentos e espazos en branco na nomenclatura de obxectos, empregando barras baixas como obxecto separador.
- Táboas en maiúsculas, atributos en minúsculas. Razón explicada nas [diferenzas entre MariaDB e PostgreSQL](#principais-diferenzas-ddl-detectadas-entre-mariadb-e-postgresql).
- Comezar por crear todas as táboas, declarando o tipo de dato, as claves primaria e as columnas de valor único.
- ```NOT NULL``` en todos os atributos, salvo nas claves primarias e nos datos de rexistro non obrigatorio. 
- ```UNIQUE``` e ```PRIMARY KEY``` sempre de maneira simplificada, salvo cando están formadas por atributos compostos.
- Despois disto, ```ALTER TABLE``` para engadir todas as claves alleas (nomeando os ```CONSTRAINT```).
- Finalmente, engadir as restricións de ```CHECK``` (tamen de forma moi declarativa e dándolles nome).

## Crear e empregar a base de datos

```sql
sudo mysql
CREATE SCHEMA naves_espaciais;
USE           naves_espaciais;
```
Temos que comezar por crear a base de datos, e despois especificar que queremos empregala ata nova orde (a BD activa aparece entre corchetes).
![ex2cap1](/img/ex2cap1.PNG)

## Creación das táboas

```sql
CREATE TABLE SERVIZO (
  clave_servizo      CHAR(5),
  nome_servizo       VARCHAR(30),
  PRIMARY KEY (Clave_Servizo, Nome_Servizo)
);

CREATE TABLE DEPENDENCIA (
  codigo_dependencia CHAR(5)     PRIMARY KEY,
  nome_dependencia   VARCHAR(30) NOT NULL UNIQUE,
  clave_servizo      CHAR(5)     NOT NULL,
  nome_servizo       VARCHAR(30) NOT NULL,
  funcion            VARCHAR(30),
  localizacion       VARCHAR(30)
);

CREATE TABLE CAMARA (
  codigo_dependencia CHAR(5)     PRIMARY KEY,
  categoria          VARCHAR(30) NOT NULL,
  capacidade         INTEGER     NOT NULL
);

CREATE TABLE TRIPULACION (
  codigo_tripulacion CHAR(5)     PRIMARY KEY,
  nome_tripulacion   VARCHAR(30) NOT NULL,
  codigo_camara      CHAR(5)     NOT NULL,
  codigo_dependencia CHAR(5)     NOT NULL,
  categoria          VARCHAR(30) NOT NULL,
  antigüidade        INTEGER     NOT NULL DEFAULT 0,
  procedencia        VARCHAR(30) NOT NULL,
  situacion_admin    VARCHAR(30) NOT NULL
);

CREATE TABLE VISITA (
  codigo_tripulacion CHAR(5),
  codigo_planeta     CHAR(5),
  data_visita        DATE,
  tempo              INTEGER     NOT NULL,
  PRIMARY KEY (codigo_tripulacion, codigo_planeta, data_visita)
);

CREATE TABLE PLANETA (
  codigo_planeta     CHAR(5)     PRIMARY KEY,
  nome_planeta       VARCHAR(30) NOT NULL UNIQUE,
  galaxia            VARCHAR(30) NOT NULL,
  coordenadas        CHAR(15)    NOT NULL UNIQUE
);

CREATE TABLE HABITA (
  codigo_planeta     CHAR(5),
  nome_raza          VARCHAR(30),
  poboacion_parcial  INTEGER     NOT NULL,
  PRIMARY KEY (codigo_planeta, nome_raza)
);

CREATE TABLE RAZA (
  nome_raza          VARCHAR(30) PRIMARY KEY,
  altura             INTEGER     NOT NULL,
  anchura            INTEGER     NOT NULL,
  peso               INTEGER     NOT NULL,
  poboacion_total    INTEGER     NOT NULL
);
```
Seguindo os criterios expostos, primeiro xeramos a estrutura da base de datos con ```CREATE TABLE```. Unha grande limitación de MariaDB consiste en que non se poden crear dominios para definir os tipos de dato, o cal sería realmente conveniente nos atributos de lonxitude limitada.
![ex2cap2](/img/ex2cap2.PNG)
![ex2cap3](/img/ex2cap3.PNG)

## Restrición da clave allea

```sql
ALTER TABLE DEPENDENCIA
  ADD CONSTRAINT FK_SERVIZO_DEPENDENCIA
    FOREIGN KEY            (clave_servizo, nome_servizo)
    REFERENCES SERVIZO     (clave_servizo, nome_servizo)
    ON DELETE  CASCADE
    ON UPDATE  CASCADE
;

ALTER TABLE CAMARA
  ADD CONSTRAINT FK_DEPENDENCIA_CAMARA
    FOREIGN KEY            (codigo_dependencia)
    REFERENCES DEPENDENCIA (codigo_dependencia)
    ON DELETE  CASCADE
    ON UPDATE  CASCADE
;

ALTER TABLE TRIPULACION
  ADD CONSTRAINT FK_CAMARA_TRIPULACION
    FOREIGN KEY            (codigo_camara)
    REFERENCES CAMARA      (codigo_dependencia)
    ON DELETE  CASCADE
    ON UPDATE  CASCADE,
  ADD CONSTRAINT FK_DEPENDENCIA_TRIPULACION
    FOREIGN KEY            (codigo_dependencia)
    REFERENCES DEPENDENCIA (codigo_dependencia)
    ON DELETE  CASCADE
    ON UPDATE  CASCADE
;

ALTER TABLE VISITA
  ADD CONSTRAINT FK_TRIPULACION_VISITA
    FOREIGN KEY            (codigo_tripulacion)
    REFERENCES TRIPULACION (codigo_tripulacion)
    ON DELETE  CASCADE
    ON UPDATE  CASCADE,
  ADD CONSTRAINT FK_PLANETA_VISITA
    FOREIGN KEY            (codigo_planeta)
    REFERENCES PLANETA     (codigo_planeta)
    ON DELETE  CASCADE
    ON UPDATE  CASCADE
;

ALTER TABLE HABITA
  ADD CONSTRAINT FK_PLANETA_HABITA
    FOREIGN KEY            (codigo_planeta)
    REFERENCES PLANETA     (codigo_planeta)
    ON DELETE  CASCADE
    ON UPDATE  CASCADE,
  ADD CONSTRAINT FK_RAZA_HABITA
    FOREIGN KEY            (nome_raza)
    REFERENCES RAZA        (nome_raza)
    ON DELETE  CASCADE
    ON UPDATE  CASCADE
;
```
A vantaxe de ter definida toda a estrutura da base de datos radica en que agora é moi fácil facer a interrelación entre táboas, pois nunca imos ter o problema de referenciar algo que non exista. Así pois, establecemos as claves alleas mediante un ```ALTER TABLE```. empregando o método máis declarativo posible.
![ex2cap4](/img/ex2cap4.PNG)
![ex2cap5](/img/ex2cap5.PNG)

## Establecer límites no rexistro de datos

```sql
ALTER TABLE CAMARA
  ADD CONSTRAINT capacidade_maior_a_cero
    CHECK (capacidade > 0)
;

ALTER TABLE HABITA
  ADD CONSTRAINT poboacion_maior_a_cero
    CHECK (poboacion_parcial > 0)
;

ALTER TABLE RAZA
  ADD CONSTRAINT altura_maior_a_cero
    CHECK (altura > 0),
  ADD CONSTRAINT anchura_maior_a_cero
    CHECK (anchura > 0),
  ADD CONSTRAINT peso_maior_a_cero
    CHECK (peso > 0),
  ADD CONSTRAINT poboacion_maior_a_cero
    CHECK (poboacion_total > 0)
;
```
Igual que no exemplo anterior, deixamos os ```CHECK``` para o final. Neste caso, o universo do discurso non permite que a capacidade dunha cámara e o número de habitantes sexan iguais ou menores a cero. Ademias, non contemplamos a existencia de razas etéreas, polo que as dimensións dos seres tamén deben ser superiores a cero. 
