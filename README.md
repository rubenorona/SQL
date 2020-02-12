## Primeiros pasos

**```SQL```** é unha linguaxe standard empregada na xestión de bases de datos relacionais, que permite crear/ modificar esquemas, ademais de engadir/ borrar/ consultar información dunha BD. Podemos dicir que é unha **liguaxe declarativa**, pois debemos declarar propiedades e condicións en vez de instrucións; isto é, a importancia do *```QUE?```* sobre o *```COMO?```*.

### Sublinguaxes de SQL e principais sentencias

- **DDL** *Data Definition Language* &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```CREATE``` &nbsp; ```ALTER``` &nbsp; ```DROP```
- **DML** *Data Manipulation Language* &nbsp;&nbsp;&nbsp;&nbsp; ```INSERT``` &nbsp; ```UPDATE``` &nbsp; ```DELETE```
- **DQL** *Data Query Language* &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```SELECT```
- **TCL** *Transaction Control Language* &nbsp;&nbsp;&nbsp;&nbsp; ```COMMIT``` &nbsp; ```ROLLBACK```
- **DCL** *Data Control Language* &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```GRANT``` &nbsp;&nbsp;&nbsp; ```REVOKE```
- **SCL** *Session Control Language* &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```ALTER```

No noso caso, imos comezar por estudar o **SQL DQL**, co fin de familiarizarnos coa linguaxe dunha maneira máis dinámica realizando diferentes consultas. Desta maneira, á hora de xerar bases de datos dende cero, xa teremos unha base de aprendizaxe tralas nosas costas.

##### Normas básicas: 

- As consultas deben rematar <u>**sempre**</u> con **punto e coma** ```;```
- Por convenio, as sentencias e funcións adoitan escribirse en **maiúsculas**, pero é importante coñecer que SQL non as distingue das minúsculas. 
- As cadeas de texto ou strings van sempre entre **comiñas** simples [```cidade = 'A Coruña'```]. Pola contra, os valores numéricos están exentos, sempre que os díxitos non formen un string [```ano = 1902``` vs ```bus.liña = '12'```].
- Podemos diferenciar entre expresións regulares e strings dependendo de si empregamos ```LIKE``` ou ```=``` nos predicados: ```nome LIKE 'S%'``` [*Samuel, Sarif, Selena...*] vs ```nome = 'S%'``` [*S%*].
- Igual que noutras linguaxes, os comentarios realízanse desta maneira ```/* texto que non interfire na consulta, salvo que o comentario inclúa punto e coma */```
- Orde na que se escriben as diferentes funcións:
	>1. **```SELECT```** columna1, columna2, columnaN
	>2. **```FROM```** táboa
	>3. *```WHERE```  predicado*
	>4. *```GROUP BY``` columnaN*
	>5. *```HAVING```  predicado*
	>6. *```ORDER BY``` columnaN ASC|DESC* **;**


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

.


Sistema operativo empregado: UBUNTU 18.04, instalado como máquina virtual con Oracle VirtualBox (versión 6.1.2).

Editor de texto: MarkdownPad (versión 2.5.0.2), que conta con highlight de sintaxe e preview a tempo real.
