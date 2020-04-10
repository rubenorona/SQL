# Metadata en MariaDB

Xa vimos o proceso para [instalar MariaDB en Linux](instalacionMariaDB.md), ademais de [varios exemplos](exemplo2_MariaDB.md) de como implementar bases de datos no SXBD, todo mediante a súa liña de comandos. Agora imos ver outra posibilidade que nos ofrece MariaDB, que consiste na visualización da estrutura dunha base de datos mediante táboas, simulando unha interface gráfica na mesma ventá de comandos que vimos empregando ata o de agora. Iremos apoiando as explicacións de cada función con capturas do seu uso aplicado ó [exemplo1 de implementación física](exemplo1_MariaDB.md).

### ```SHOW COLUMNS```, o método máis sinxelo para obter información

Con este simple comando, podemos consultar a estrutura básica dunha táboa.
```sql
SHOW COLUMNS
FROM nome_da_base_de_datos.NOME_DA_TABOA
;
```
Se ben é moi fácil de empregar, tamén amosa bastantes limitacións. Só nos permite ver unha táboa de cada vez, e non podemos escoller que información queremos ou non visualizar. As columnas predeterminadas son as seguintes:

- *Field*: nome_do_atributo.
- *Type*: tipo de dato de cada columna (coa súa lonxitude máxima).
- *Null*: especifica a nulidade dun atributo, sendo ```YES``` a representación de que unha columna admite o rexistro valores nulos.
- *Key*: pode tomar un dos seguintes tres valores, por orde de prioridade
  - ```PRI```: o atributo é unha clave primaria ou forma parte dunha.
  - ```UNI```: resitrición de unicidade, isto é, a columna só admite valores únicos.
  - ```MUL```: o atributo é a primeira columna dun conxunto, que pode tomar valores múltiples. Na práctica, aparecen como ```MUL``` as claves alleas (ou o primeiro atributo dunha clave allea composta) que non funcionen á súa vez como claves primarias (pois neste caso aparecerían como ```PRI```). 
- *Default*: valor predeterminado dunha columna. Se aparece un valor baleiro, significa que non foi especificado.
- *Extra*: información adicional, como o posible autoincremento dun atributo.

## INFORMATION_SCHEMA, como empregar o dicionario de datos

O método que vimos con anterioridade é demasiado básico, polo que non recomendamos o seu uso. Se queremos información sobre unha base de datos, o mellor que podemos facer é acudir á propia fonte, isto é, o dicionario de datos. Así pois, **INFORMATION_SCHEMA** é unha repositorio predeterminado que almacena todos os metadatos das nosas bases de datos. Está formado por máis de trinta táboas diferentes, cada unha con columnas que aportan distinta información, e sobre as que podemos realizar consultas personalizadas. Despois de probalas e consultar a documentación oficial reducimos, dende un punto de vista didáctico, as tres ducias de táboas que forman o dicionario de datos a só unha terna: ```INFORMATION_SCHEMA.COLUMNS```, ```INFORMATION_SCHEMA.KEY_COLUMN_USAGE``` e ```INFORMATION_SCHEMA.CHECK_CONSTRAINTS```. Estas tres táboas permitirannos consultar toda a información que xa estudamos previamente sobre a estrutura dunha base de datos.
![meta1cap1](/img/meta1cap1.PNG)

### Same. But better. INFORMATION_SCHEMA.COLUMNS

Ofrece unha información similar a ```SHOW COLUMNS```, pero coas vantaxes que aporta unha consulta personalizada: escoller as columnas a amosar, ensinar todas as táboas dunha base de datos nunha mesma busca ou engadir varias databases diferentes, aparte de todos os coñecementos de [SQL DQL](DQL.md) que podemos aplicar aquí. 

Así pois, podemos recrear a consulta que fixemos nun primeiro lugar, servindo de excusa para repasar coñecementos e tamén para explorar as outras posibilidades que brinda a táboa ```INFORMATION_SCHEMA.COLUMNS```. Para iso, seguimos os mesmos principios que nunha consulta ordinaria. No ```SELECT``` separamos entre comas as columnas a amosar; mediante```FROM``` referenciamos á táboa sobre a que facemos a busca; por último, ```WHERE``` permítenos delcarar predicados. 
```sql
SELECT table_name,                             /* SHOW COLUMNS só permite unha táboa de cada vez */
       column_key               as 'key',      /* = Key */
       column_name,                            /* = Field */
       data_type,
       character_maximum_length as 'length',
       is_nullable                             /* = Null */
FROM                  INFORMATION_SCHEMA.COLUMNS
WHERE table_schema = 'proxectos_de_investigacion'
;
```
Neste caso, prescindimos das columnas que nos permiten ver o valor predeterminado dun atributo [```column_default```] e a información adicional [```extra```], pois non son relevantes na nosa base de datos. Ademais, desglosamos o tipo de dato en dúas columnas, co obxectivo de que os atributos de lonxitude limitada destacasen máis. Sen embargo, ```data_type``` e ```character_maximum_length``` ben poderían ter sido substituídos unicamente por ```column_type```. Adicionalmente, **INFORMATION_SCHEMA.COLUMNS** contén outro tipo de información a maiores, como ```table_schema``` (útil si consultamos máis dunha base de datos á vez), ```ordinal_position``` (orde dos atributos dentro dunha táboa), ```character_octet_lenght``` (tamaño máximo dun string en bits), ```numeric_precision``` ou ``` datetime_precision```.
![meta1cap2](/img/meta1cap2.PNG)

### Claves alleas. INFORMATION_SCHEMA.KEY_COLUMN_USAGE]



![meta1cap3](/img/meta1cap3.PNG)

### Restricións no rexistro de datos. INFORMATION_SCHEMA.CHECK_CONSTRAINTS



![meta1cap4](/img/meta1cap4.PNG)