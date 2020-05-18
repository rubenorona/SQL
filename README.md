# Apuntamentos de SQL

### Axuda para coavaliadores

![help](/img/help.png)

Espero que con esta imaxe, todos teñades clara a relación entre os links e as diferentes tarefas. Podedes atopar o índice real abaixo.

## Índice

- [Primeiros pasos](#primeiros-pasos)
  - [Sublinguaxes de SQL](#sublinguaxes-de-SQL-e-principais-sentencias)
- [Data Query Language](DQL.md)
- [Data Definition Language](DDL.md)
- [Data Manipulation Language](DML.md)
- [Instalación de MariaDB en Linux](instalacionMariaDB.md)
  - [Exemplo1 implementación: Proxectos de Investigación](exemplo1_MariaDB.md)
  - [Exemplo2 implementación: Naves Espaciais](exemplo2_MariaDB.md)
  - [Metadata en MariaDB](metadata_MariaDB.md)
- [Ferramentas empregadas](#ferramentas-empregadas)

## Primeiros pasos

**SQL** é unha linguaxe standard empregada na xestión de bases de datos relacionais, que permite crear e modificar esquemas, ademais de engadir, borrar ou consultar información dunha BD. Podemos dicir que é unha **liguaxe declarativa**, pois debemos proclamar propiedades e condicións en vez de instrucións; isto é, a importancia do **QUE?** sobre o **COMO?**.

### Sublinguaxes de SQL e principais sentencias

- [**DQL**](DQL.md) *(Data Query Language)* &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```SELECT```
- [**DDL**](DDL.md) *(Data Definition Language)* &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```CREATE``` &nbsp; ```ALTER``` &nbsp;&nbsp;&nbsp; ```DROP```
- [**DML**](DML.md) *(Data Manipulation Language)* &nbsp;&nbsp;&nbsp;&nbsp; ```INSERT``` &nbsp; ```UPDATE``` &nbsp; ```DELETE```
- **TCL** *(Transaction Control Language)* &nbsp;&nbsp;&nbsp;&nbsp; ```COMMIT``` &nbsp; ```ROLLBACK```
- **DCL** *(Data Control Language)* &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```GRANT``` &nbsp;&nbsp;&nbsp; ```REVOKE```
- **SCL** *(Session Control Language)* &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```ALTER```

### Ferramentas empregadas

Sistema operativo empregado: LUBUNTU 19.10, instalado como máquina virtual con Oracle VirtualBox (versión 6.1.4).

Editor de texto: MarkdownPad (versión 2.5.0.2), que conta con highlight de sintaxe e preview a tempo real.

Os exemplos de consulta foron testados mediante as bases de datos presentes na páxina web www.SQLzoo.net

O exemplo proposto nas sublinguaxes DDL e DML foi implementado e testado previamente en ElephantSQL, co xestor PostgreSQL.


© 2020 SeicaNonVelaVara, Inc. [Terms of Service](https://youtu.be/dQw4w9WgXcQ)
