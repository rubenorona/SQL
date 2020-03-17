Unha vez que xa dominamos o SQL DQL (**link**), aprendendo a realizar consultas mediante páxinas web interactivas, agora imos a ver como facer bases de datos dende cero. Nisto consiste a sublinguaxe **SQL DDL**, que nos permite crear bases de datos, táboas, usuarios ou dominios, engadir atributos cun tipo de dato definido, establecer restricións, impoñer un criterio nas táboas interrelacionadas ante o borrado ou modificación de datos, así como a posibilidade de alterar calquera destes parámetros a posteriori.

## ```CREATE```, xénese de toda base de datos

Antes de nada, imos ver a estrutura básica da creación dunha base de datos. Recordamos que o contido definido entre [corchetes] é voluntario, mentres que entre (parénteses) só pode aparecer a totalidade do contido dunha función ```CREATE```, ou ben a declaración de atributo/s. Ademais, recoméndase evitar o uso de acentos e espazos nas expresións, optando por barras baixas ou alternar maiúsculas para distinguir entre palabras. Por último, despois de cada ```CREATE``` debemos rematar **sempre** con punto e coma;

```sql
CREATE DATABASE | SCHEMA | DOMAIN | TABLE | USER 
       [IF NOT EXISTS] <nomeDaCreacion> (
       ...
);
```

### ```CREATE DATABASE``` e ```CREATE SCHEMA```

Serven para declarar a BD onde engadiremos despois as táboas, datos e restricións. Dependendo do xestor de bases de datos que empreguemos, o significado de ambos variará ou non. Por exemplo, en **MySQL** unha ```DATABASE``` é equivalente a un ```SCHEMA```, e poden ser empregados indistintamente. Por outra banda, **PostreSQL** si que establece certas diferenciacións, funcionando ```SCHEMA``` como unha especie de capa intermedia entre ```DATABASE``` e as táboas que a compoñen. Na práctica, recoméndase empregar sempre unha ou otra, tendo ```DATABASE``` un maior nivel de restricións. Polo tanto, dende un punto de vista didáctico, imos optar a partir de agora por usar únicamente a función ```CREATE SCHEMA```. Aparte do nome que establecemos á base de datos, tamén podemos configurarlle un CharSet, como por exemplo UTF-8.

```sql
CREATE SCHEMA [IF NOT EXISTS] <nomeDaBaseDeDatos> 
              [CHARACTER SET  <nomeDoCharset>]
;
```

### ```CREATE DOMAIN```

Se imos a repetir nunha BD o mesmo tipo de datos en diferentes atributos, cómpre crer un dominio común. Desta maneira, realizar unha posterior modificación do mesmo resultará moito máis sinxelo. Ademais, o resto de colaboradores na xestión da base de datos agradecerán esta organización e orde. 

```sql
CREATE DOMAIN [nomeDoSchema.]<nomeDoDominio> <tipoDeDato>
              [DEFAULT  <'expresion'>]
              [NULL | NOT NULL]
              [CHECK (restrición)]
;
```
Os únicos parámetros obrigatorios son o nome do dominio e a declaración do tipo de dato (que veremos a continuación). Por defecto, os dominios son creados no esquema que esteamos a empregar nese momento, pero podemos establecelo noutro no mesmo intre que nomeamos o dominio. Tamén de forma predetirminada, os dominios admiten valores nulos, polo que cómpre usar ```NOT NULL``` para evitalo. Ademais, ```CREATE DOMAIN``` permítenos engadir restricións ós tipos de datos con ```CHECK``` (verémolo con profundidade nos ```CONSTRAINTS```(**link**)). Por último, se queremos definir un valor por defecto, esta expresión debe cumprir as restricións establecidas e ser coherente co tipo de dato escollido.

### Tipos de datos

Debido á diversidade de información que se pode engadir nunha base de datos, existe unha gran variedade de tipos de datos, coas súas vantaxes e inconvintes. Como hai diferenzas dependendo do xestor de BD, facemos unha táboa resumo cos máis xerales e interesantes dende o noso punto de vista como estudantes, pero recordando que á hora de usar un xestor específico, deberemos antes consultar o manual. 

| TIPO DE DATO  | PARA QUE SERVE?                                                                    |
|---------------|------------------------------------------------------------------------------------|
| **numérico**  |                                                                                    |
| INTEGER       | número enteiro; é o tipo numérico máis empregado                                   |
| DECIMAL(n,m)  | número preciso (díxitos a introducir, díxitos decimais)                            |
| REAL          | número aproxiamdo; ocupa só 4 bytes                                                |
| **texto**     |                                                                                    |
| CHAR(n)       | lonxitude fixa (só pode ter n caracteres)                                          |
| VARCHAR(n)    | lonxitude variable (pode ter un máximo de n caracteres)                            |
| TEXT          | lonixutde variable e ilimitada; emprégase para descripcións                        |
| **data**      |                                                                                    |
| DATE          | 'aaaa-mm-dd'; precisión de 1 día                                                   |
| TIME          | 'hh:mm:ss[.sss]'; precisión de ata 1 microsegundo                                  |
| TIMESTAMP     | 'aaaa-mm-dd hh:mm:ss'; combinación de DATE e TIME                                  |
| **outro**     |                                                                                    |
| BOOLEAN       | TRUE[1] ou FALSE[0]; cómpre usar NOT NULL, xa que por defecto admite valores nulos |
| MONEY         | precisión limitada, ata catro cifras decimais; optimizado para ```SUM```           |
| UUID          | identificador universal único; 128 bits = 32 díxitos hexadecimais; 8-4-4-4-12      |
| JSON          | extensión de arquivos .json [JavaScript Object Notation]                           |
| XML           | extensión de arquivos .xml [eXtensible Markup Language]                            |
| CIDR          | direcións IPv4 e IPv6; non permite bits distintos a 0 á dereita da máscara         |
| INET          | direcións IPv4 e IPv6                                                              |

### ```CREATE TABLE```

Debemos crear as táboas unha a unha, rematando sempre con punto e coma. Todo o contido da mesma vai entre parénteses, e separando os disntintos elementos mediante comas. Igual que ocorría cos dominios, unha táboa será xerada na basa de datos que se esté empregando nese momento, salvo que especifiquemos o contrario á hora de establecer o nome. Os elementos básicos que compoñen as táboas son os atributos [columnas], que declaramos mediante un nome e un tipo de dato (de maneira explícita ou mediante un dominio previamente creado). Voluntariamente, podemos establecer [de maneira implícita] parámetros de clave primaria, unicidade, imposibilidade de valores nulos ou un valor por defecto ó final de cada atributo. Outro método para establecer estas restricións é a función ```CONSTRAINT```, que podemos realizar tras declarar os atributos.

```sql
CREATE TABLE [IF NOT EXISTS]   [nomeDoSchema.]<nomeDaTaboa> (
	     <nomeDoAtributo1> <tipoDeDato> [PRIMARY KEY],
	     <nomeDoAtributoN> <dominioN>   [DEFAULT <'expresion'>] [NOT NULL] [UNIQUE],
	     [restriciónsAKAconstraints]
);
```

## ```CONSTRAINT```, establecendo restricións sobre as táboas

Método **explícito** para establecer claves primarias, claves alleas, restricións de unicidade ou límites ós valores que se poden introducir nunha columna. Estes pódense engadir ó final dun ```CREATE TABLE``` tras os atributos, ou mediante un ```ALTER``` (que veremos máis adiante (**link**)).

### ```PRIMARY KEY```

Segundo a regra de integridade das entidades, cada táboa debe ter unha e só unha clave principal, cuxo/s atributo/s debe/n permitir identificar unha tupla sobre as outras. Polo tanto, este [conxuto de] atributo/s só pode tomar valores únicos e non nulos. 

```sql
CREATE TABLE <exemplo1> (
	     <atributo1> <dominio1> UNIQUE NOT NULL,
	     <atributoN> <dominioN>
);

CREATE TABLE <exemplo2> (
	     <atributo1> <dominio1> PRIMARY KEY,
	     <atributoN> <dominioN>
);

CREATE TABLE <exemplo3> (
	     <atributo1> <dominio1>,
	     <atributoN> <dominioN>,
	     [CONSTRAINT <nomeDoConstraint>]
		 PRIMARY KEY (atributo1[, atributoN])
);
```
Como podemos ver, temos tres maneiras de establecer unha clave principal. No *exemplo1* facemos cumprir os dous parámetros básicos da primary key. Sen embargo, non recomendamos este método, pois é o empregado para declarar o resto de claves candidatas que tornan en alternativas, das que sí pode haber máis dunha na táboa e xerar conflicto coa principal. O *exemplo2* resulta o máis sinxelo, xa que engadindo ```PRIMARY KEY``` tras a declararción do atributo, establecemos de maneira implícita as propiedades que esta columna debe cumprir. Por último, no *exemplo3* vemos unha maneira de facelo por separado, que ademáis de dar a posibilidade de nomear a restrición, é a única que permite xerar unha clave principal composta por varios atributos. 

### ```UNIQUE``` e ```NOT NULL```

No apartado anterior xa introducimos este termo, porque explicamos que segundo a regra de integridade das entidades, as claves candidatas deben tomar valores únicos e non nulos. Como unha táboa só pode ter unha clave principal, a sentenza ```PRIMARY KEY``` unicamente se pode empregar nunha ocasión. Pero coa suma de ```UNIQUE``` + ```NOT NULL``` podemos declarar todas as claves alternativas que existan.

```sql
CREATE TABLE <exemplo1> (
	     <atributo1> <tipoDeDato>,
	     <atributo2> <tipoDeDato> UNIQUE,
	     <atributoN> <tipoDeDato>,
	     [CONSTRAINT <nomeDoConstraintX>]
		 PRIMARY KEY (atributo1[, atributoN]),
	     [CONSTRAINT <nomeDoConstraintY>]
		 CHECK (atributo2 IS NOT NULL)
);

CREATE TABLE <exemplo2> (
	     <atributo1> <tipoDeDato>  PRIMARY KEY,
	     <atributo2> <tipoDeDato>  NOT NULL,
	     <atributoN> <tipoDeDato> [NOT NULL],
	     [CONSTRAINT <nomeDoConstraint>]
		 UNIQUE (atributo2[, atributoN])
);
```
En ambos casos, o atributo1 funciona como clave principal, mentres que o atributo2 é clave alternativa. Xa que previamente vimos como declarar ```UNIQUE``` e ```NOT NULL``` ó final do propio atributo, agora facemos dous exemplos mesturando este método coa xeración ex professo dun ```CONSTRAINT```. Sen embargo, non recomendamos crear un ```CHECK``` para establecer valores non nulos, resultando máis eficiente engadilo como parámetro xunto coa delcaración da columna.

### ```CHECK```

Permítenos limitar os valores que pode tomar un atributo. As que non teñen límite son as posibilidades que ofrece ```CHECK```, pois podemos realizar calquera tipo de predicado aplicando os coñecementos aprendidos en SQL DQL. O que sí debemos ter en conta é que os valores referenciados nese predicado deben pertencer á mesma táboa na que se crea o ```CHECK```.

```sql
CREATE TABLE <exemplo1> (
	     <atributo1> <tipoDeDato> PRIMARY KEY,
	     <atributo2> <tipoDeDato> CHECK (<predicado>),
	     <atributoN> <tipoDeDato>
);

CREATE TABLE <exemplo2> (
	     <atributo1> <tipoDeDato> PRIMARY KEY,
	     <atributo2> <tipoDeDato>,
	     <atributoN> <tipoDeDato>,
	     [CONSTRAINT <nomeDoConstraint>]
		 CHECK (<predicado>)
	         [NOT DEFERRABLE | DEFERRABLE]
	         [INITIALLY IMMEDIATE | INITIALLY DEFERRED]
);
```
Por defecto, a comprobación a este tipo de restricións prodúcese inmediatamente [```NOT DEFERRABLE``` e ```INITIALLY IMMEDIATE```], pero podemos aplazala ó final.

### ```FOREIGN KEY```

Tipo de ```CONSTRAINT``` máis complexo, pois serve para establecer unha interrelación entre táboas. Cando un atributo fai referencia á clave principal doutra táboa, herda as súas propiedades, polo que non é preciso especificar que os valores a tomar deben ser non nulos e únicos. Algo moi a ter en conta é que as columnas relacionadas deben ter dominios ou tipos de datos coincidentes entre si.

```sql
CREATE TABLE <taboaX> (
	     <xAtributo1> <dominio1> PRIMARY KEY,
	     <xAtributo2> <dominio2>,
	     <xAtributoN> <dominioN>
);

CREATE TABLE <taboaY> (
	     <yAtributo1> <dominio1> PRIMARY KEY,
	     <yAtributo2> <dominio2> REFERENCES taboaX (xAtributo1),
	     <yAtributoN> <dominioN>,
);
```
Este é o exemplo de clave allea máis sinxelo e limitado, no que a clave principal dunha táboa se converte en clave allea doutra. Se a declaramos xunto co atributo, a foreign key non pode ser composta e ten unha modificación a posteriori moito máis complicada, polo que non é nada recomendable.

```sql
CREATE TABLE <taboaX> (
	     <xAtributo1> <dominio1>,
	     <xAtributo2> <dominio2>,
	     <xAtributoN> <dominioN>,
	     [CONSTRAINT <PK_taboaX>]
		 PRIMARY KEY (xAtributo1[, xAtributoN])
);

CREATE TABLE <taboaY> (
	     <yAtributo1> <dominio1>,
	     <yAtributo2> <dominio2>,
	     <yAtributoN> <dominioN>,
	     [CONSTRAINT <PK_taboaY>]
		 PRIMARY KEY (yAtributo1[, yAtributoN]),
	     [CONSTRAINT <FK_taboaX_taboaY>]
		 FOREIGN KEY       (yAtributo1[, yAtributoN])
	         REFERENCES taboaX (xAtributo1[, xAtributoN])
	         [ON DELETE NO ACTION | CASCADE | SET NULL | SET DEFAULT <'expresion'>]
	         [ON UPDATE NO ACTION | CASCADE | SET NULL | SET DEFAULT <'expresion'>]
);
```
Crear un ```CONSTRAINT``` ex professo para a clave allea resulta moito máis conveniente, pois permite nomear a acción, facer claves compostas e escoller o criterio a seguir ante o **borrado e modificación de datos** presentes en múltiples táboas. 

| CRITERIO    | PARA QUE SERVE?                                                                                  |
|-------------|--------------------------------------------------------------------------------------------------|
| NO ACTION   | [**R**] restrictiva, non altera os datos durante o borrado/ modificación; opción por defecto     |
| CASCADE     | [**C**] actúa en cascada en todas as táboas nas que o dato esté presente                         |
| SET NULL    | [**N**] normalmente só se emprega ante o borrado, onde se establecen valores nulos               |
| SET DEFAULT | [**D**] opción menos recomendada, pois engade datos que poden comprometer a integridade da táboa |

Naturalmente, para crear a restrición da clave allea, deben previamente existir as táboas e os atributos implicados. Polo tanto, a maneira máis recomendada e ordenada de establecer as interrelación entre táboas é mediante un ```ALTER```, que nos permtite engadir un ```CONSTRAINT``` sobre unha base de datos xa declarada. Desta forma, evitamos a problemática que xurde cando as táboas dependen sucesivamente entre elas.

## ```ALTER TABLE```, modificando táboas xa creadas

Existe unha gran variedade de parámetros que podemos alterar nunha base de datos, pero ímonos a centrar en ```ALTER TABLE```, que nos permite engadir ou eliminar tanto columnas como restricións. Ademais, podemos renomear elementos e vincular unha táboa a outra base de datos diferente.

```sql
ALTER TABLE [IF NOT EXISTS] <nomeDaTaboa>
	    [RENAME TO <novoNomeDaTaboa>],
	    [RENAME [COLUMN | CONSTRAINT] <nomeOrixinal> TO <novoNome>],
	    [SET SCHEMA <novoSchema>],
	    [ADD | DROP [COLUMN | CONSTRAINT] <nome>]
	       [...]
;
```
Aínda que ```ALTER TABLE``` permite executar no seu anterior outro ```ALTER```, co que variar un parámetro concreto dunha columna ou unha restrición, non recomendamos este método. Para evitar fallos na estrutura da táboa, resulta moito máis eficiente facer un ```DROP``` do elemento que queiramos modificar, para despois declaralo todo de novo mediante un ```ADD```. Por esta razón, cómpre nomear sempre os restricións. Finalmente, como dixemos con anterioridade, o criterio máis ordeado para realizar interrelacións é facelo mediante un ```ALTER TABLE```, onde engadimos a restrición da clave allea sobre unhas táboas xa creadas previamente.

```sql
CREATE TABLE <taboaX> (
	     <xAtributo1> <dominio1>,
	     <xAtributo2> <dominio2>,
	     <xAtributoN> <dominioN>,
	     [CONSTRAINT <PK_taboaX>]
		 PRIMARY KEY (xAtributo1[, xAtributoN])
);

CREATE TABLE <taboaY> (
	     <yAtributo1> <dominio1>,
	     <yAtributo2> <dominio2>,
	     <yAtributoN> <dominioN>,
	     [CONSTRAINT <PK_taboaY>]
		 PRIMARY KEY (yAtributo1[, yAtributoN]),
);

[...]

ALTER TABLE <taboaY>
	    ADD [CONSTRAINT <FK_taboaX_taboaY>]
		 FOREIGN KEY       (yAtributo1[, yAtributoN])
	         REFERENCES taboaX (xAtributo1[, xAtributoN])
	        [ON DELETE NO ACTION | CASCADE | SET NULL | SET DEFAULT <'expresion'>]
	        [ON UPDATE NO ACTION | CASCADE | SET NULL | SET DEFAULT <'expresion'>]
;
```
Desta maneira, cando manexamos unha base de datos moi grande, non temos que estar levando conta de si está creada ou non unha táboa sobre a que precisamos facer referencia. Así pois, declaramos todas as táboas e os seus atributos, con claves principais e alternativas. Só no final é cando engadimos a totalidade de claves alleas.
