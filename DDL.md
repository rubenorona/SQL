Unha vez que xa dominamos o SQL DQL (**link**), aprendendo a realizar consultas mediante páxinas web interactivas, agora imos a ver como facer bases de datos dende cero. Nisto consiste a sublinguaxe **SQL DDL**, que nos permite crear bases de datos, táboas, usuarios ou dominios, engadir atributos cun tipo de dato definido, establecer restricións, impoñer un criterio nas táboas interrelacionadas ante o borrado ou modificación de datos, así como a posibilidade de alterar calquera destes parámetros a posteriori.

## ```CREATE```, xénese de toda base de datos

Antes de nada, imos ver a estrutura básica da creación dunha base de datos. Recordamos que o contido definido entre [corchetes] é voluntario, mentres que entre (parénteses) só pode aparecer a totalidade do contido dunha función ```CREATE```, ou ben a declaración de atributo/s. E **sempre** rematamos con punto e coma;

```sql
CREATE DATABASE | SCHEMA | DOMAIN | TABLE | USER 
       [IF NOT EXISTS] <nomeDaCreación> (
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
              [DEFAULT  <'expresión'>]
              [NULL | NOT NULL]
              [CHECK (restrición)]
;
```
Os únicos parámetros obrigatorios son o nome do dominio e a declaración do tipo de dato (que veremos a continuación). Por defecto, os dominios son creados no esquema que esteamos a empregar nese momento, pero podemos establecelo noutro no mesmo intre que nomeamos o dominio. Tamén de forma predetirminada, os dominios admiten valores nulos, polo que cómpre usar ```NOT NULL``` para evitalo. Ademais, ```CREATE DOMAIN``` permítenos engadir restricións ós tipos de datos con ```CHECK``` (verémolo con profundidade nos ```CONSTRAINTS```**link**). Por último, se queremos definir un valor por defecto, esta expresión debe cumprir as restricións establecidas e ser coherente co tipo de dato escollido.
