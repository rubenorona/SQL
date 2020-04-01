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
![ex1cap2](/img/ex1cap2.PNG)
![ex1cap3](/img/ex1cap3.PNG)

## Restrición da clave allea


