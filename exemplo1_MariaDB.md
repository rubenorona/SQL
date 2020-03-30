# Implementación en MariaDB, exemplo1: Proxectos de Investigación

A partir do [primeiro exercicio de implementación DDL](https://github.com/davidgchaves/first-steps-with-git-and-github-wirtz-asir1-and-dam1/tree/master/exercicios-ddl/1-proxectos-de-investigacion) que fixemos na clase, imos agora crear a base de datos en MariaDB, o sistema xestor de bases de datos que [acabamos de instalar localmente](instalacionMariaDB.md).

# Criterios seguidos: 

- Evitar acentos e espazos en branco na nomenclatura de obxectos.
- Comezar por crear todas as táboas, declarando o tipo de dato, as claves principais e os atributos únicos. 
- ```UNIQUE``` e ```PRIMARY KEY``` sempre de maneira simplificada, salvo cando son atributos compostos.
- Despois disto, ```ALTER TABLE``` para engadir todas as claves alleas (nomeando os ```CONSTRAINT```).
- Finalmente, engadir as restricións de ```CHECK``` (tamen dándolles nome).
