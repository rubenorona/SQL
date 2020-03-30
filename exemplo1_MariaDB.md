# Implementación en MariaDB, exemplo1: Proxectos de Investigación

A partir do [primeiro exercicio de implementación DDL](https://github.com/davidgchaves/first-steps-with-git-and-github-wirtz-asir1-and-dam1/tree/master/exercicios-ddl/1-proxectos-de-investigacion) que fixemos na clase, imos agora crear a base de datos en MariaDB, o sistema xestor de bases de datos que [acabamos de instalar localmente](instalacionMariaDB.md).

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

