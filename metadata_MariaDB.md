# Metadata en MariaDB

Xa vimos o proceso para [instalar MariaDB en Linux](instalacionMariaDB.md), ademais de [varios](exemplo1_MariaDB.md) [exemplos](exemplo2_MariaDB.md) de como implementar bases de datos no SXBD, todo mediante a súa liña de comandos. Agora imos ver outra posibilidade que nos ofrece MariaDB, que consiste na visualización da estrutura dunha base de datos mediante táboas, simulando unha interface gráfica na mesma ventá de comandos que vimos empregando ata o de agora.

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

![meta1cap1](/img/meta1cap1.PNG)

## INFORMATION_SCHEMA, como empregar o dicionario de datos




### Táboas, atributos, tipo de datos e restricións de clave primaria e unicidade



### Claves alleas



### Restricións no rexistro de datos: ```CHECK```
