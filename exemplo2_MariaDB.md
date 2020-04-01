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


