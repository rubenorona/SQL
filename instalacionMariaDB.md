# Instalación de MariaDB en Linux

Nos exemplos prácticos vistos durante os apuntamentos SQL [DDL](DDL.md#agora-toca-aplicar-os-coñecementos) e [DML](DML.md#agora-toca-aplicar-os-coñecementos) usamos ElephantSQL, aplicación online que emprega PostgreSQL. Agora imos ir un paso máis adiante, realizando unha instalación local dun sistema de xestión de bases de datos. O SXBD escollido neste caso será **MariaDB**, xestor de código aberto que nace no ano 2009 tras un fork de MySQL, despois de que este fose comprado por parte de Oracle.

O sistema operativo que imos empregar será Lubuntu 19.10 *Eoan Ermine*, unha distribución lixeira de Linux que se basea en Ubuntu. Así pois, o primeiro paso é visitar a [páxina oficial de MariaDB](https://downloads.mariadb.org/mariadb/repositories/#distro=Ubuntu&distro_release=eoan--ubuntu_eoan&mirror=yamagata-university&version=10.4) para realizar a instalación en función do noso OS.
![repositoryMariaDB](/img/repositoryMariaDB.png)

Polo tanto, no noso caso debemos introducir os seguintes comandos, sempre mediante un usuario con privilexios sudoers:
```sh
sudo apt-get install software-properties-common
sudo apt-key adv --fetch-keys 'https://mariadb.org/mariadb_release_signing_key.asc'
sudo add-apt-repository 'deb [arch=amd64] http://ftp.yz.yamagata-u.ac.jp/pub/dbms/mariadb/repo/10.4/ubuntu eoan main'
```
![install1](/img/install1.png)

Unha vez cargamos o repositorio de MariaDB no noso equipo, procedemos a instalalo:
```sh
sudo apt update
sudo apt install mariadb-server
```
![install2](/img/install2.png)

Unha maneira de verificar a instalación é comprobar a súa versión:
```sh
mysql -V
```

Antes de empregar o xestor, é recomendable que antes executemos o seguinte script, no que configuraremos o contrasinal root, ademais de eliminar os usuarios anónimos, deshabilitar o root remoto e suprimir unha base de datos de proba:
```sh
sudo mysql_secure_installation
```
![install3](/img/install3.png)
![install4](/img/install4.png)

Xa temos o SXBD operativo. Na seguinte captura podemos ver o aspecto da liña de comandos, ademais de amosar as tres bases de datos que veñen incluídas por defecto. Ó contrario que pasou coa test database que borramos antes, esta terna de schemas non se debe eliminar, pois ten información fundamental como o dicionario de datos.
![defaultDB](/img/defaultDB.png)
