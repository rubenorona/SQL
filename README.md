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

#### Normas básicas: 

- As consultas deben rematar <u>**sempre**</u> con **punto e coma** ```;```
- Por convenio, as sentencias e funcións adoitan escribirse en **maiúsculas**, pero é importante coñecer que SQL non as distingue das minúsculas. 
- As cadeas de texto ou strings van sempre entre **comiñas** simples [```cidade = 'A Coruña'```]. Pola contra, os valores numéricos están exentos, sempre que os díxitos non formen un string [```ano = 1902``` vs ```bus.liña = '12'```].
- Podemos diferenciar entre expresións regulares e strings dependendo de si empregamos ```LIKE``` ou ```=``` nos predicados: ```nome LIKE 'S%'``` [*Samuel, Sarif, Selena...*] vs ```nome = 'S%'``` [*S%*].
- Igual que noutras linguaxes, os comentarios realízanse desta maneira ```/* texto que non interfire na consulta, salvo que o comentario inclúa punto e coma */```
- Orde na que se escriben as diferentes funcións:
```sql
1. SELECT columna1, columna2, columnaN
2. FROM táboa
3. [WHERE predicado*]
4. [GROUP BY columnaN*]
5. [HAVING predicado*]
6. [ORDER BY columnaN ASC|DESC*] **;**
```

A continuación imos estudar as diferentes sentencias e funcións que soportan os xestores SQL de consulta, vendo as súas estruturas e acompañadas de exemplos.

## ```SELECT``` e ```FROM```, nai e pai da consulta básica

Son os dous elementos de presenza obrigatoria en calquera consulta SQL. Por un lado, o ```SELECT``` é unha declaración que serve para retornar unha ou máis columnas de unha ou más táboas. Por outra banda, ```FROM``` indica sobre que táboa/s estamos a reclamar a consulta. Máis adiante amosaremos como realizar un ```FROM``` composto mediante consultas aniñadas (**link**) ou empregando ```JOIN``` (**link**). 

```sql
SELECT taboa1.columna1, [taboaN.columnaN]
FROM taboa1 [JOIN taboaN];
```

Cunha consulta deste estilo, o resultado contén todas as tuplas existentes reclamadas no ```FROM```, cos atributos declarados no ```SELECT```.


## ```WHERE```, exemplificación do paradigma declarativo

A pesar de que non é obrigatorio, o certo é que unha consulta que non filtre resultados parece pouco útil. Aquí aparece a figura do ```WHERE```, principal ferramenta para establecer predicados. Como xa dixemos, SQL era unha linguaxe declarativa, e isto vese claramente reflexado no funcionamento do ```WHERE```. Se queremos que as tuplas a amosar cumpran unhas certas condicións, basta con declarar estos predicados, trasladando o *pseudocódigo* de maneira case literal.

#### Exemplo

*Países e número de habitantes de todos os países asiáticos con máis de 40 millóns de habitantes*

Pseudocódigo:
```sh
> Amosar países e poboación
> Da nosa base de datos xeográfica mundial
> Onde o continente é Asia e a poboación é maior de 40 millóns
```

```sql
SELECT name, population
FROM world
WHERE continent = 'Asia'
  AND population > 40000000;
```

## Isto está moi ben, pero preciso de máis ferramentas

Existe un gran variedade de funcións que podemos combinar con ```SELECT``` ou ```WHERE```, e que aumentan o número de posibilidades nas nosas consultas. 

### ```AS```
Emprégase para nomear un atributo ou unha táboa por unha cadea regular desexada.
```sql
SELECT W.name, 
       W.population AS 'xente'
FROM world AS W;
```
### ```*```
Comodín que permite seleccionar todas as columnas dispoñibles nunha consulta.
```sql
SELECT *
FROM táboa;
```
### ```REPLACE```
Permite sustituir datos dun atributo ou string por outro string personalizado.
```sql
SELECT REPLACE (expresión, 'patrón', 'reemprazo')
FROM táboa;
```
### ```DISTINCT```
Elimina os duplicados dunha columna, ofrecendo só os valores únicos dese atributo.
```sql
SELECT DISTINCT (columnaN)
FROM táboa;
```
### ```ROUND```
Permite redondear un atributo numerado ou unha operación ás deceas, centeas..., ou amosar un número concreto de decimais.
```sql
SELECT ROUND (atributo, N)
FROM táboa;
```
O díxito tras a coma representa o número de cifras decimais a amosar. Se a cifra se representa en negativo, isto supón a cantidade de díxitos que se redondean (*-1 en deceas, -2 en centeas, -3 en millares...*).
### ```[NOT] IN```
Serve para especificar, nun único paso, un predicado con múltiples valores. 
```sql
SELECT columnaN
FROM táboa
WHERE columnaN IN ('valor1', 'valor2', 'valorN');
```
Esta mesma consulta podería realizarse, de maneira menos eficiente, con múltiples ```OR```.
```sql
SELECT columnaN
FROM táboa
WHERE columnaN = 'valor1'
   OR columnaN = 'valor2'
   OR columnaN = 'valorN';
```
### ```BETWEEN```
Posibilita filtrar un predicado entre un rango establecido. Cabe reseñar que ```BETWEEN``` **inclúe os valores extremos**.
```sql
SELECT columnaN
FROM táboa
WHERE columnaN BETWEEN 'valor1' AND 'valor2';
```
De igual maneira que no caso anterior, tamén hai maneira alternativa de representar esta consulta, cos operadores maior/ menor que.
```sql
SELECT columnaN
FROM táboa
WHERE columnaN >= 'valor1' 
  AND columnaN <= 'valor2';
```
### ```<>```
Expresión empregada para representar que un atributo non pode ser igual a un valor específico.
```sql
SELECT columnaN
FROM táboa
WHERE columnaN <> 'valorN';
```
### ```[NOT] LIKE```
Como xa explicamos anteriormente nas normas básicas (**link**), serve para filtrar unha columna por un valor expresado como cadea regular. 
```sql
SELECT columnaN
FROM táboa
WHERE columnaN LIKE 'valor1';
```
Ademais, soporta o uso de comodíns como **_** para representar un só caracter, ou **%** para sustituir entre cero e múltiples caracteres. A diferenza co ```=``` radica en que este último busca strings, e si empregamos comodíns, tomaraos como caracteres co seu significado literal.
### ```XOR```
Ten o funcionamento dun OR exclusivo; isto é, de dúas condicións, debe cumprir unha e só unha delas. 
```sql
SELECT columnaX, columnaY
FROM táboa
WHERE columnaX = condiciónX XOR columnaY = condiciónY;
```
### ```LENGTH```
Mide a cantidade de caracteres dos strings pertencentes a un atributo referenciado. 
```sql
SELECT LENGTH (columnaN)
FROM táboa
[WHERE LENGTH (columnaN) = X];
```
### ```LEFT/ RIGHT```
Extrae un número específico de caracteres (comezando pola esquerda ou a dereita) das cadeas de texto que forman os valores dunha columna. 
```sql
SELECT LEFT (columnaN, n)
FROM táboa;
```
### ```CONCAT```
Función que combina dous ou máis strings personalizados ou pertencentes ós valores dun atributo.
```sql
SELECT CONCAT ('string1', 'string2', 'stringN')
FROM táboa;
```
### ```IS [NOT] NULL```
Permite delcarar que un atributo sexa [ou non] nulo; isto é, que non ten un valor definido. Cabe recordar que, segundo a regra de identidade das entidades, o atributo que serve como clave principal nunca pode ser nulo. 
```sql
SELECT columnaN
FROM táboa
WHERE columnaN IS NOT NULL;
```
### ```COALESCE```
Serve de gran utilidade si queremos substituir os valores nulos dun atributo por un string concreto.
```sql
SELECT COALESCE (columnaN, 'string')
FROM táboa;
```

#### Exemplos prácticos

*Ganadores dun premio nobel que se chamen John e cuxa disciplina non sexa Física nen Química. Reempraza o nome John por Manolo.* 
```sql
SELECT subject, 
       REPLACE (winner, 'John', 'Manolo') AS 'manobel winners'
FROM nobel
WHERE winner  LIKE 'John%' 
  AND subject NOT IN ('Physics', 'Chemistry');
```

*Calcula a renta per capita (con dous decimais) daqueles países que, ou ben teñen polo menos 250 millóns de habitantes, ou contan cunha área de máis de 3 millóns de km cadrados. Amosa a tres primeiras letras de cada país a modo de abreviatura.*
```sql
SELECT name, 
       LEFT (name, 3) AS 'abreviatura', 
       ROUND (gdp/population, 2) AS 'per capita'
FROM world
WHERE population > 250000000 
  XOR area       > 3000000;
```



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

Os exemplos de consulta e as bases de datos asociadas foron elaborados a través da páxina web www.SQLzoo.net
