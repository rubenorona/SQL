# Data Query Language

## Índice

- [Normas básicas](#normas-básicas)
- [SELECT e FROM, nai e pai da consulta básica](#SELECT-e-FROM-nai-e-pai-da-consulta-básica)
- [WHERE, exemplificación do paradigma declarativo](#WHERE-exemplificación-do-paradigma-declarativo)
- [Isto está moi ben, pero preciso de máis ferramentas](#isto-está-moi-ben-pero-preciso-de-máis-ferramentas)
- [Funcións de agregado, aumentando as posibilidades alxebraicas](#funcións-de-agregado-aumentando-as-posibilidades-alxebraicas)
  - [MAX, MIN e AVG](#MAX-MIN-e-AVG)
  - [SUM e COUNT](#SUM-e-COUNT)
- [Xa, pero quero ver máis dunha tupla](#xa-pero-quero-ver-máis-dunha-tupla-GROUP-BY-e-HAVING)
  - [GROUP BY](#GROUP-BY)
  - [HAVING](#HAVING)
- [E como combino varias táboas?](#e-como-combino-varias-táboas-SELECT-aniñados-e-JOIN)
  - [SELECT aniñados](#SELECT-aniñados)
  - [JOIN](#JOIN)
  - [LEFT | RIGHT JOIN](#LEFT--RIGHT-JOIN)
- [Agora toca aplicar os coñecementos](#agora-toca-aplicar-os-coñecementos)

Imos comezar por estudar **SQL DQL**, xa que a pesar de que posiblemente é a sublinguaxe máis complexa, é posible familiarizarse con ela dunha maneira dinámica, realizando consultas en diferentes páxinas web interactivas. Desta maneira, á hora de xerar bases de datos dende cero, xa teremos unha base de aprendizaxe tralas nosas costas e limitarémonos a estudar as novas estruturas do resto de sublinguaxes.

### Normas básicas: 

- As consultas deben rematar ***sempre*** con **punto e coma** ```;```
- Por convenio, as sentencias e funcións adoitan escribirse en **maiúsculas**, pero é importante coñecer que SQL non as distingue das minúsculas. 
- As cadeas de texto ou strings van sempre entre **comiñas** simples [```cidade = 'A Coruña'```]. Pola contra, os valores numéricos están exentos, sempre que os díxitos non formen un string [```ano = 1902``` vs ```bus.liña = '12'```].
- Podemos diferenciar entre expresións regulares e strings dependendo de si empregamos ```LIKE``` ou ```=``` nos predicados: ```nome LIKE 'S%'``` [*Samuel, Sarif, Selena...*] vs ```nome = 'S%'``` [*S%*].
- Igual que noutras linguaxes, os comentarios realízanse desta maneira: ```/* texto que non interfire na consulta, salvo que o comentario inclúa punto e coma */```
- Orde na que se escriben as diferentes funcións:
```sql
1. SELECT columna1, columna2, columnaN
2. FROM táboa
3. [WHERE predicado]
4. [GROUP BY columnaN]
5. [HAVING predicado]
6. [ORDER BY columnaN ASC|DESC] ;
```

A continuación imos estudar as diferentes sentencias e funcións que soportan os xestores SQL de consulta, vendo as súas estruturas e acompañadas de exemplos.

## ```SELECT``` e ```FROM```, nai e pai da consulta básica

Son os dous elementos de presenza obrigatoria en calquera consulta SQL. Por un lado, o ```SELECT``` é unha declaración que serve para retornar unha ou máis columnas de unha ou más táboas. Por outra banda, ```FROM``` indica sobre que táboa/s estamos a reclamar a consulta. Máis adiante amosaremos como realizar un ```FROM``` composto mediante [consultas aniñadas](#SELECT-aniñados) ou [empregando ```JOIN```](#JOIN). 

```sql
SELECT taboa1.columna1, [taboaN.columnaN]
FROM taboa1 [JOIN taboaN];
```

Cunha consulta deste estilo, o resultado contén todas as tuplas existentes reclamadas no ```FROM```, cos atributos declarados no ```SELECT```.


## ```WHERE```, exemplificación do paradigma declarativo

A pesar de que non é obrigatorio, o certo é que unha consulta que non **filtre** resultados parece pouco útil. Aquí aparece a figura do ```WHERE```, principal ferramenta para establecer **predicados**. Como xa dixemos, SQL é unha linguaxe declarativa, e isto vese claramente reflexado no funcionamento do ```WHERE```. Se queremos que as tuplas a amosar cumpran unhas certas condicións, basta con declarar estos predicados, trasladando o *pseudocódigo* de maneira case literal.

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
Emprégase para **renomear** un atributo ou unha táboa por unha cadea regular desexada.
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
Permite **substituir** datos dun atributo ou string por outro string personalizado.
```sql
SELECT REPLACE (expresión, 'patrón', 'reemprazo')
FROM táboa;
```
### ```DISTINCT```
Elimina os duplicados dunha columna, ofrecendo só os **valores únicos** dese atributo.
```sql
SELECT DISTINCT (columnaN)
FROM táboa;
```
### ```ROUND```
Permite **redondear** un atributo numerado ou unha operación ás deceas, centeas..., ou amosar un número concreto de decimais.
```sql
SELECT ROUND (atributo, N)
FROM táboa;
```
O díxito trala coma representa o número de cifras decimais a amosar. Se a cifra se representa en negativo, isto supón a cantidade de díxitos que se redondean (*-1 en deceas, -2 en centeas, -3 en millares...*).
### ```[NOT] IN```
Serve para especificar, nun único paso, un predicado con **múltiples valores**. 
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
Posibilita filtrar un predicado entre un **rango** establecido. Cabe reseñar que ```BETWEEN``` **inclúe os valores extremos**.
```sql
SELECT columnaN
FROM táboa
WHERE columnaN BETWEEN 'valor1' AND 'valor2';
```
De igual maneira que no caso anterior, tamén hai maneira alternativa de representar esta consulta, mediante os operadores ≥ | ≤.
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
Como xa explicamos anteriormente nas [normas básicas](#normas-básicas), serve para filtrar unha columna por un valor expresado como cadea regular. 
```sql
SELECT columnaN
FROM táboa
WHERE columnaN LIKE 'valor1';
```
Ademais, soporta o uso de **comodíns** como '**_**' para representar un só caracter, ou '**%**' para sustituir entre cero e múltiples caracteres. A diferenza co ```=``` radica en que este último busca strings, e si empregamos comodíns, tomaraos como caracteres co seu significado literal.
### ```XOR```
Ten o funcionamento dun **```OR``` exclusivo**; isto é, de dúas condicións, debe cumprir unha e só unha delas. 
```sql
SELECT columnaX, columnaY
FROM táboa
WHERE columnaX = condiciónX
  XOR columnaY = condiciónY;
```
### ```LENGTH```
Mide a cantidade de caracteres dos strings pertencentes a un atributo referenciado. 
```sql
SELECT LENGTH (columnaN)
FROM táboa
[WHERE LENGTH (columnaN) = X];
```
### ```LEFT | RIGHT```
Extrae un número específico de caracteres (comezando pola esquerda ou a dereita) das cadeas de texto que forman os valores dunha columna. 
```sql
SELECT LEFT (columnaN, n)
FROM táboa;
```
### ```CONCAT```
Función que **combina** dous ou máis strings personalizados ou pertencentes ós valores dun atributo.
```sql
SELECT CONCAT ('string1', 'string2', 'stringN')
FROM táboa;
```
### ```IS [NOT] NULL```
Permite delcarar que un atributo sexa [ou non] nulo; isto é, que non ten un **valor definido**. Cabe recordar que, segundo a regra de identidade das entidades, o atributo que serve como clave principal nunca pode ser nulo. 
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
### ```ORDER BY```
Sempre ó **final** da consulta, permite ordenar os strings (alfabeticamente) ou os valores numéricos dunha ou varias columnas. Por defecto, a orde é ascendente.
```sql
SELECT columnaN
FROM táboa
ORDER BY columnaN ASC|DESC;
```

### Un par de exemplos prácticos

*Gañadores dun premio nobel que se chamen John e cuxa disciplina non sexa Física nen Química. Reempraza o nome John por Manolo, e amosa as disciplinas en orde descendente.* 
```sql
SELECT subject, 
       REPLACE (winner, 'John', 'Manolo') AS 'manobel winners'
FROM nobel
WHERE winner  LIKE 'John%' 
  AND subject NOT IN ('Physics', 'Chemistry')
ORDER BY subject DESC;
```

*Calcula a renta per capita (con dous decimais) daqueles países que, ou ben teñen polo menos 250 millóns de habitantes, ou contan cunha área de máis de 3 millóns de km cadrados. Amosa a tres primeiras letras de cada país a modo de abreviatura.*
```sql
SELECT name, 
       LEFT (name, 3) AS 'abreviatura', 
       ROUND (gdp/population, 2) AS 'per capita'
FROM world
WHERE population >= 250000000 
  XOR area       > 3000000;
```

## Funcións de agregado, aumentando as posibilidades alxebraicas

Serven para **reducir** unha serie de valores a un só resultado. Estas funcións de agregado operan sobre strings numéricos, ou permiten contar todas as filas pertencentes a unha columna. En todo caso, o resultado final queda reducido a unha tupla. 

#### Diferentes funcións de agregado:

### ```MAX```, ```MIN``` e ```AVG```
Dunha columna con **atributos numéricos**, estas función retornan unha única tupla co maior valor, o menor, ou o valor promedio de entre todas as tuplas. Cabe destacar que, empregando estas funcións, os valores nulos son ignorados.

Imaxinemos unha táboa onde a columnaN ten 15 tuplas, todas con valores únicos que aumentan de catro en catro, dende o 4 ata o 60 *[4, 8, 12, 16, 20, 24, 28, 32, 36, 40, 44, 48, 52, 56, 60]*
```sql
SELECT MAX (columnaN),
       MIN (columnaN),
       AVG (columnaN)
FROM táboa;
```
Neste caso, ```MAX (columnaN)```= 60, ```MIN (columnaN)```= 4 e ```AVG (columnaN)```= 32

### ```SUM``` e ```COUNT```
Aínda que poidan resultar similares, en realidade non o son. Como o seu nome suxire, ```SUM``` fai unha **suma de todos os valores numéricos** pertencentes a unha columna, derivando nunha tupla co resultado final da suma. Igual que ocorría nas funcións anteriores, os valores nulos tamén se ignoran. 

Por outra banda, ```COUNT``` **contabiliza o total de tuplas** presentes na consulta. Neste caso, os valores nulos si se teñen en conta. Ademais, a función non diferencia entre valores idénticos. Por esta mesma razón, é moi interesante empregalo xunto con ```DISTINCT```, pois desta maneira é posible contabilizar o número total de valores únicos presentes nunha columna: ```COUNT (DISTINCT columnaN)```
```sql
SELECT SUM (columnaN),
     COUNT (columnaN)
FROM táboa;
```
Se tomamos de exemplo a táboa anterior, a suma total dos valores numéricos sería ```SUM (columnaN)```= 480. Pola contra, ```COUNT (columnaN)```= 15, xa que se limita a contabilizar a cantidade total de tuplas.

## Xa, pero quero ver máis dunha tupla: ```GROUP BY``` e ```HAVING```

Nos exemplos previos, as consultas finais eran dunha tupla, con tantas columnas como funcións de agregado fosen declaradas no ```SELECT```. Ademais, poderiamos ter empregado ```WHERE```, cuxa función sería a de **filtrar** o número de tuplas que se verían posteriormente reducidas. Pero eran exemplos teóricos cun pouco de trampa, xa que non empregamos no ```SELECT``` nada que non fose unha función de agregado, pois a consulta sería errónea sen o uso dun ```GROUP BY```.

### ```GROUP BY```
Permite agrupar as tuplas en función dun criterio establecido. Como dixemos antes, todo atributo declarado nun ```SELECT``` que non sexa unha función de agregado **debe ir obrigatoriamente incluído** nun ```GROUP BY```. A razón é que este último agrupa os valores idénticos e xenera unha tupla por cada tipo. Por outra banda, un atributo só no ```SELECT``` vai devolver todas as filas que cumpran as condicións da consulta. Loxicamente, unha consulta non pode amosar dúas cantidades diferentes de tuplas.

Imos poñer un exemplo: *Sabemos que no mundo hai quince países que teñan entre 50 e 100 millóns de habitantes. Estas nacións están repartidas entre tres continentes: África, Asia e Europa. Para saber cantos países pertencen a cada continente, a consulta correcta sería:*
```sql
SELECT continent, 
 COUNT (name) AS 'total'
FROM world
WHERE population BETWEEN 50000000 AND 100000000
GROUP BY continent;
```
As tuplas resultantes serían tres, tantas como continentes diferentes cumpren o predicado. A partir disto, cóntanse os países en función desa agrupación.

| continent | total |
|-----------|-------|
| Africa    |     3 |
| Asia      |     8 |
| Europe    |     4 |

Si da consulta anterior prescindimos do ```GROUP BY```, estamoslle a pedir ó xestor da base de datos que, por un lado, amose os continentes que cumpren o predicado do ```WHERE``` (quince tuplas, cuxo valor sería algún dos tres continentes). Por outra banda, un ```COUNT``` non asociado a unha agrupación vai tratar de devolver sempre unha soa tupla. Polo tanto, a consulta sería errónea. 

### ```HAVING```
Única ferramenta que temos para realizar predicados empregando calquera das cinco funcións de agregado explicadas con anterioridade. Naturalmente, o ```HAVING``` aplica ese filtro en función dunha agrupación, polo que o seu uso combinado con ```GROUP BY``` é **obrigatorio**.

Recuperamos o exemplo anterior: *Agora queremos que a consulta só amose os continentes que teñan máis de catro países cunha poboación entre 50 e 100 millóns de habitantes*.
```sql
SELECT continent, 
 COUNT (name) AS 'total'
FROM world
WHERE population BETWEEN 50000000 AND 100000000
GROUP BY continent
HAVING COUNT (name) > '4';
```
| continent | total |
|-----------|-------|
| Asia      |     8 |

Cal é a orde de execución nesta consulta?
```sql
> 1. FROM:     Atopar a base de datos (táboa world, 195 tuplas)
> 2. WHERE:    Filtrar os países cunha poboación entre 50 e 100 millóns de habitantes (15 tuplas)
> 3. GROUP BY: Agrupar en función dos diferentes continentes (3 tuplas)
> 4. HAVING:   Filtrar que, respeto da agrupación e o predicado establecidos, o número total de países sexa maior a catro (1 tupla)
> 5. SELECT:   Amosar certas columnas desexadas
```

## E como combino varias táboas?: ```SELECT``` aniñados e ```JOIN```

Xa temos todas as ferramentas para sacar o máximo partido ás consultas que impliquen unha táboa. Pero o certo é que as bases de datos son máis complexas que isto, polo que debemos aprender a referenciar elementos externos. 

### ```SELECT``` aniñados
Realmente a súa maior utilidade non radica en combinar dúas táboas, senón en facer referencia de maneira transitiva a datos que non é posible **declarar de maneira directa**. O segundo ```SELECT``` adoita aparecer nunha comparación co principal ```WHERE```, polo que os atributos que declaran ambos deben ser os **mesmos**.

Exemplo: *Amosar todos os países que teñan unha poboación maior á do Brasil*. Non sabemos o número de habitantes do Brasil, polo que debemos aniñar unha subconsulta que compare todas as tuplas de poboación da táboa cos habitantes totais brasileiros. Para resolver este tipo de cuestións, é aconsellable comezar pola subconsulta, pois atopar os habitantes dun país específico é moi sinxelo. A partir disto, completamos o resto da consulta. 
```sql
SELECT name
FROM world
WHERE population > (SELECT population
	            FROM world
	            WHERE name = 'Brazil');
```

A veces cómpre realizar un **bucle aniñado**, onde resulta necesario renomear ambas táboas, pois o obxectivo é facer unha comparativa entre dous atributos da mesma táboa.

Exemplo: *Amosa o país con maior superficie de cada continente*. Neste caso facemos un loop que, sempre que os continentes de ambas consultas coincidan, compare as súas aŕeas. Como só pode haber un país cuxa superficie sexa ≥ a súa propia, o resultado da consulta conterá unha tupla por continente. 
```sql
SELECT O.continent, O.name, O.area
FROM world AS O
WHERE  O.area >= ALL (SELECT I.area
	              FROM world AS I
	              WHERE  I.continent = O.continent
	                AND  I.area IS NOT NULL);
```

### ```JOIN```

Tamén expresado como ```INNER JOIN```, permite combinar dúas ou máis táboas nunha consulta. Mediante ```ON``` debemos expresar un predicado que relacione ambas partes, normalmente a **clave principal** dunha táboa coa *clave allea* doutra (este predicado tamén se pode declarar con ```WHERE```). Isto é fundamental, pois doutra maneira o xestor de BD relacionaría ambas mediante un **produto cartesiano**. Polo tanto, si se relacionasen dúas táboas con 100 tuplas cada unha, a consulta final contaría con 100000 tuplas. Cabe destacar que ó facer un ```JOIN```, suprímense as tuplas que conteñan valores nulos. 
```sql
SELECT columnaX, columnaY
FROM táboaX JOIN táboa Y
	      ON principalX = alleaY;
```

Exemplo de base de datos con varias táboas:
- movie (**id**, title, yr, director, budget, gross)
- actor (**id**, name)
- casting (***movieid***, *actorid*, ord)

*Amosa todas as películas nas que participou Joe Pesci e non foi un dos dous principais actores* Neste caso, mediante un dobre ```JOIN```, conseguimos *desbloquear* a base de datos, e permítenos facer referencia a calquera atributo de calquera táboa.
```sql
SELECT M.title
FROM actor AS A JOIN casting AS C
	          ON A.id = C.actorid
	        JOIN movie AS M
	          ON C.movieid = M.id
WHERE A.name = 'Joe Pesci'
  AND C.ord > 2;
```
Unha alternativa máis complicada para resolver esta consulta sería añidar dous ```SELECT```:
```sql
SELECT M.title
FROM movie AS M
WHERE M.id IN (SELECT C.movieid
	       FROM casting AS C
	       WHERE C.actorid IN (SELECT A.id
	                           FROM actor AS A
	                           WHERE A.name = 'Joe Pesci')
	         AND C.ord > 2);
```

### ```LEFT | RIGHT JOIN```

Como xa dixemos anteriormente, ó realizar un ```JOIN``` son eliminadas as tuplas que conteñan valores nulos. Para evitar isto, podemos enlazar unha táboa respeto da outra. Desta maneira, no lado escollido aparecerán todos os valores, aínda que no outro haxa valores nulos. 

Imaxinemos esta base de datos, onde varios profesores non teñen departamento asignado ou teléfono móbil rexistrado. Ademais, un departamento non contén profesores:
- dept (**id**, name)
- teacher (**id**, *dept*, name, phone, mobile)

*Fai un ```JOIN``` que amose todos os profesores, aínda que non teñan departamento asignado*
```sql
SELECT teacher.name, dept.name
FROM teacher LEFT JOIN dept
  ON teacher.dept = dept.id;
```
O feito de facer un ```LEFT JOIN```, permítenos que a táboa da esquerda (*teacher*) amose todos os valores, aínda que na outra táboa existan tuplas con ```NULL```. Naturalmente, podemos acadar esta mesma consulta con un ```RIGHT JOIN``` se damos a volta ás táboas. Ademais, imos aproveitar este exemplo para substituir os valores nulos cun ```COALESCE```.
```sql
SELECT teacher.name, 
       COALESCE (dept.name, 'None') AS 'dept'
FROM dept RIGHT JOIN teacher
  ON dept.id = teacher.dept
```
| name       | dept      |
|------------|-----------|
| Shrivell   | Computing |
| Throd      | Computing |
| Splint     | Computing |
| Spiregrain | None      |
| Cutflower  | Design    |
| Deadyawn   | None      |

## Agora toca aplicar os coñecementos

*Ordena alfabeticamente todas as persoas que traballaron con Katharine Hepburn*

Base de datos:
- movie (**id**, title, yr, director, budget, gross)
- actor (**id**, name)
- casting (***movieid***, *actorid*, ord)

O primeiro que recoñecemos é que a táboa movie non é necesaria nesta consulta. Polo tanto, comezamos por facer un ```JOIN``` ordinario entre actor e casting. Ademais, imos precisar dun bucle que comprobe si os actores da lista traballaron ou non con Katharine Hepburn. Para isto, facemos un ```JOIN``` de casting consigo mesmo, ademais dun derradeiro ```JOIN``` con outra táboa actor, mediante a que declaramos o nome da actriz obxecto da consulta. Coa estrutura xa montada, precisamos dun predicado que elimine a tupla de Katharine Hepburn, pois naturalmente traballou consigo mesma pero non nos interesa. Agora só falta un ```DISTINCT``` que descarte os actores duplicados, e un ```ORDER BY``` final.

```sql
SELECT DISTINCT A1.name
FROM actor AS A1 JOIN casting AS C1 ON A1.id      = C1.actorid
	         JOIN casting AS C2 ON C1.movieid = C2.movieid
	         JOIN actor   AS A2 ON A2.id      = C2.actorid
WHERE A2.name = 'Katharine Hepburn'
  AND A1.id <> A2.id
ORDER BY A1.name ASC;
```
