# Data Manipulation Language

## Índice

- [```INSERT INTO```, pois unha base de datos baleira é inútil](#insert-into-pois-unha-base-de-datos-baleira-é-inútil)
- [```UPDATE``` para completar ou modificar unha tupla](#update-para-completar-ou-modificar-unha-tupla)
- [```DELETE FROM```, [ten coidado] para eliminar tuplas](#delete-from-ten-coidado-para-eliminar-tuplas)
- [Agora toca aplicar os coñecementos](#agora-toca-aplicar-os-coñecementos)

Xa aprendemos a facer consultas mediante o SQL DQL(**link**), e despois vimos como crear bases de datos con SQL DDL(**link**). Polo tanto, podemos dar paso a **SQL DML**, terceira sublinguaxe que imos estudar e que se basea no manexo de datos, permitindo engadir, modificar e eliminar tuplas nas bases de datos existentes.

## ```INSERT INTO```, pois unha base de datos baleira é inútil

Cando rematamos de crear a estrutura dunha base de datos, esta non contén dato algún. Para poñer remedio a isto empregamos ```INSERT INTO```, que nos permite engadir tuplas a unha táboa.

```sql
INSERT INTO <nomeDaTaboa>
	    [(<atributoA>, <atributoB>, <atributoX>)]
	    VALUES
		(<1valorA>, <1valorB>, <1valorX>),
		(<2valorA>, <2valorB>, <2valorX>),
		(<NvalorA>, <NvalorB>, <NvalorX>)
;

/*tamén podemos engadir múltiples tuplas mediante consultas DQL*/

INSERT INTO <nomeDaTaboa>
	    [(<atributoA>, <atributoB>, <atributoN>)]
		SELECT (<atributoX>, <atributoY>, <atributoZ>) /*dominios equivalentes*/
		FROM    <calqueraTaboaDoSchema>
	       [WHERE <predicado>]
;
```
Aínda que listar os atributos que imos a insertar non é obrigatorio, si resulta moi recomendable. Desta maneira, o rexistro de datos pode ser en calquera orde. Debemos ter en conta que, ó crear unha tupla, temos que insertar todos os datos que non admitan valores nulos. Naturalmente, non é posible rexistrar un valor nunha columna propagada si aínda non existe na táboa orixinal. Si nos atopamos con táboas que dependan sucesivamente entre elas, quizais resulte conveniente evitar o uso de ```NOT NULL``` de maneira provisional nalgunhas claves alleas, pois senón pode resultar imposible o rexistro de datos.

## ```UPDATE``` para completar ou modificar unha tupla

Serve para actualizar os valores existentes dunha táboa ou introducir datos que non fosen de rexistro obrigatorio. 

```sql
UPDATE <nomeDaTaboa>
       SET <atributo1> = <valor1>,
	   <atributo2> = <valor2>,
	   <atributoN> = <valorN>
      [WHERE <predicado>]
;
```
Declarar un predicado serve para filtrar as tuplas nas que imos rexistrar os novos datos. Polo tanto, ó prescindir dun predicado, estableceranse estes valores ó longo de toda a táboa.

## ```DELETE FROM```, [ten coidado] para eliminar tuplas

Self-explanatory. Nunha táboa específica, é posible borrar todas as tuplas [que cumpran unha condición].

```sql
DELETE FROM <nomeDaTaboa>
	    [WHERE <predicado>]
;
```
**BEWARE**: Se non se establece un predicado, o resultado será baleirar toda a táboa. *Don't hate the player, hate the game*

## Agora toca aplicar os coñecementos

Tomando como punto de partida o [suposto presentado no estudo do SQL DDL](DDL.md#agora-toca-aplicar-os-coñecementos), agora imos a rexistrar os seguintes datos nesa base de datos xa creada pero aínda baleira de información.

| **nif**   | nrp             | nome            | data_ingreso | *xefe*    |
|--------------------------------------------------------------------------|
| 87425812Y | 8742581257A0591 | Xiana Raboduro  | 2004-06-01   |           |
| 63845991S | 6384599168A0591 | Armando Paredes | 2016-07-15   | 87425812Y |
| 41357625Z | 4135762513I0593 | Álvaro Siza     | 2020-03-19   | 87425812Y |


| **codigo** | denominacion  | horas_semanais | comentario                                                                                        |
|-------------------------------------------------------------------------------------------------------------------------------------------------|
| 200602     | Debuxo II     | 6              | Representación gráfica enfocada á arquitectura. Saír a debuxar ó exterior cando sexa posible      |
| 200204     | Estruturas IV | 12             | Cálculo de estruturas metálicas de grande escala. Debe presentarse un proxecto final por parellas |
| 200501     | Urbanismo I   | 9              | Introdución ó estudo do entramado territorial. A cidade obxecto de estudo será A Coruña           |


