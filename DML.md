# Data Manipulation Language

## Índice


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
Aínda que listar os atributos que imos a insertar non é obrigatorio, si resulta moi recomendable. Desta maneira, o rexistro de datos pode ser en calquera orde. Debemos ter en conta que, ó crear unha tupla, temos que insertar todos os datos que non admitan valores nulos. Naturalmente, non é posible rexistrar un valor nunha columna propagada si aínda non existe na táboa orixinal. Por esta razón, durante a creación da base de datos resulta conveniente evitar o uso de ```NOT NULL``` para as claves alleas, pois a sucesiva dependencia entre táboas pode facer imposible o rexistro de datos.

## ```UPDATE``` para completar ou modificar unha tupla

Serve para actualizar os valores existentes dunha táboa ou introducir datos que non fosen de rexistro obrigatorio. Como comentamos anteriormente, resulta útil para engadir os atributos das claves alleas que aínda non existisen na táboa referenciada.

```sql
UPDATE <nomeDaTaboa>
       SET <atributo1> = <valor1>,
	   <atributo2> = <valor2>,
	   <atributoN> = <valorN>
      [WHERE <predicado>]
;
```
Declarar un predicado serve para filtrar as tuplas nas que imos rexistrar os novos datos. Polo tanto, ó prescindir dun predicado, estableceranse estes valores ó longo de toda a táboa.
