## Primeiros pasos

**```SQL```** é unha linguaxe standard empregada na xestión de bases de datos relacionais, que permite crear/ modificar esquemas, ademais de engadir/ borrar/ consultar información dunha BD. Podemos dicir que é unha **liguaxe declarativa**, pois debemos declarar propiedades e condicións en vez de instrucións; isto é, a importancia do *```QUE?```* sobre o *```COMO?```*.

### Sublinguaxes de SQL e principais sentencias

- **DDL** *Data Definition Language* &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```CREATE``` &nbsp; ```ALTER``` &nbsp; ```DROP```
- **DML** *Data Manipulation Language* &nbsp;&nbsp;&nbsp;&nbsp; ```INSERT``` &nbsp; ```UPDATE``` &nbsp; ```DELETE```
- **DQL** *Data Query Language* &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```SELECT```
- **TCL** *Transaction Control Language* &nbsp;&nbsp;&nbsp;&nbsp; ```COMMIT``` &nbsp; ```ROLLBACK```
- **DCL** *Data Control Language* &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```GRANT``` &nbsp;&nbsp;&nbsp; ```REVOKE```
- **SCL** *Session Control Language* &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```ALTER```

No noso caso, imos comezar por estudar o **SQL DQL**, xa que a pesar de que é a sublinguaxe máis complexa, é posible familiarizarse con ela dunha maneira más dinámica, realizando consultas en diferentes páxinas web interactivas. Desta maneira, á hora de xerar bases de datos dende cero, xa teremos unha base de aprendizaxe tralas nosas costas e limitarémonos a estudar as novas estruturas.

##### Normas básicas: 

- As consultas deben rematar <u>**sempre**</u> con **punto e coma** ```;```
- Por convenio, as sentencias e funcións adoitan escribirse en **maiúsculas**, pero é importante coñecer que SQL non as distingue das minúsculas. 
- As cadeas de texto ou strings van sempre entre **comiñas** simples [```cidade = 'A Coruña'```]. Pola contra, os valores numéricos están exentos, sempre que os díxitos non formen un string [```ano = 1902``` vs ```bus.liña = '12'```].
- Podemos diferenciar entre expresións regulares e strings dependendo de si empregamos ```LIKE``` ou ```=``` nos predicados: ```nome LIKE 'S%'``` [*Samuel, Sarif, Selena...*] vs ```nome = 'S%'``` [*S%*].
- Igual que noutras linguaxes, os comentarios realízanse desta maneira ```/* texto que non interfire na consulta, salvo que o comentario inclúa punto e coma */```
- Orde na que se escriben as diferentes funcións:
	> **```SELECT```** columna1, columna2, columnaN
	> **```FROM```** táboa
	> [*```WHERE```  predicado*]
	> [*```GROUP BY``` columnaN*]
	> [*```HAVING```  predicado*]
	> [*```ORDER BY``` columnaN ASC|DESC*] **;**

A continuación imos estudar as diferentes sentencias e funcións que soportan os xestores SQL de consulta, vendo as súas estruturas e acompañadas de exemplos.

## ```SELECT``` e ```FROM```, nai e pai da consulta básica

Son os dous elementos de presenza obrigatoria en calquera consulta SQL. Por un lado, o ```SELECT``` é unha declaración que serve para retornar unha ou máis columnas de unha ou más táboas. Por outra banda, ```FROM``` indica sobre que táboa/s estamos a reclamar a consulta.
.

.

.

.

.

.

.

.

.

.


Sistema operativo empregado: UBUNTU 18.04, instalado como máquina virtual con Oracle VirtualBox (versión 6.1.2).

Editor de texto: MarkdownPad (versión 2.5.0.2), que conta con highlight de sintaxe e preview a tempo real.
