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
	     <nomeDoAtributo1> <dominio1> PRIMARY KEY,
	     <nomeDoAtributoN> <dominioN>
);

CREATE TABLE <exemplo2> (
	     <nomeDoAtributo1> <dominio1> UNIQUE NOT NULL,
	     <nomeDoAtributoN> <dominioN>
);

CREATE TABLE <exemplo3> (
	     <nomeDoAtributo1> <dominio1>,
	     <nomeDoAtributoN> <dominioN>,
	     [CONSTRAINT <nomeDoConstraint>]
		 PRIMARY KEY (atributo1[, atributoN])
);
```
Como podemos ver, temos tres maneiras de establecer unha clave principal. No exemplo1 realízase de maneira sinxela tras declarar o atributo, sendo a maneira máis sinxela de facelo se a clave está formada por unha única columna. O exemplo2 é igual de válido, xa que facemos cumprir os dous parámetros básicos da primary key. Sen embargo, non recomendamos este método, pois é o empregado para declarar o resto de claves candidatas que tornan en alternativas, das que sí pode haber máis dunha na táboa e xerar conflicto coa principal. Por último, no exemplo3 vemos o método explícito, que ademáis é o único que permite xerar unha clave principal composta de varios atributos. 

