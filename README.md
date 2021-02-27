# sql_mysql
Ejercicios y Clases de Sql


## Clase 1

### Diferencia entre SQL y MySQL

- SQL es un lenguaje de progrmacion orientado a consultas de base de datos (Structure Query Language).
-  MySQL es un sistema de administracion de base de datos (Database Management System, DBMS) o tambien llamado Base de Datos.

EXisten muchos tipos de base de datos, desde un simple archivos hasta sistemas relacionales orientados a objetos. MySQL como base de datos relacional, utiliza multples tablas para almacenar y organizar la informacion.

Fue escrito en C y C++y se integra perfectamente con los lenguajes de programacion mas usados en todo el mundo.


![Alt text](https://kkslinuxinfo.files.wordpress.com/2016/02/database.png "DBMS")

### Base de Datos basadas en SQL

Las bases de datos más comunes basadas en SQL son:
#### _MySQL: MySQL es una base de datos SQL de código abierto, desarrollada por una empresa sueca MySQL AB_

#### _Oracle: Oracle es un sistema de gestión de bases de datos relacional desarrollado por Oracle Corporation._

#### _Access: Microsoft Access es un software de gestión de base de datos de nivel de entrada._


## Clase 2

La forma de conectarnos a nuestro servidor MySQL a través de nuestra terminal podría ser:

```
$ mysql -h <dirección_de_nuestro_servidor> -u <usuario> -pmi_clave -P <puerto>
```
  
Esta es una forma muy insegura de conexión ya que estaríamos dejando la password escrita en texto plano… cualquiera con acceso a nuestro usuario podría obtenerla simplemente ejecutando el comando:
```
 $ history
 ```
Para esto el comando ‘mysql’ nos permite la opción de indicarle que debemos usar una contraseña, pero que queremos que nos la pregunte de forma segura a través de nuestro terminal. Para hacer esto el solo debemos indicarle el argumento ‘-p’; quedaría así:
 ```
$ mysql -h <dirección_de_nuestro_servidor> -u <usuario> -p -P <puerto>
Enter password: <aquí introducirías tu password>?
 ``` 
Una situación muy normal en entornos de producción es tener que acceder a nuestro servidor a través de un túnel SSH, es decir, primero accedemos a una máquina que podría ser la entrada de nuestro cluster y después, desde ahí, accederíamos a nuestro servidor MySQL.


![Alt text](https://www.tunnelsup.com/images/ssh-local2.png "DBMS")

Para hacer esto primero crearíamos nuestro túnel ssh y después nos conectaríamos a nuestro servidor MySQL de la siguiente forma:
```
$ ssh user@ssh.example.com -L 3307:mysql1.example.com:3306 -N -f
$ mysql -h 127.0.0.1 -P 3307
```
de esta forma lo que le estamos diciendo a nuestro ordenador es:

- Todo lo que yo envié al puerto 3307 de mi máquina local, saldrá por la máquina ssh.example.com (que ya esta dentro del cluster) en dirección a la máquina mysql1.example.com que escucha en el puerto 3306 (puerto por defecto)
- Ahora, me conector a través de mi ordenador (la IP 127.0.0.1 es mi pc) en el puerto 3307

Ahora ya te puedes conectar a tu servidor, no expuesto en internet, desde tu pc local.

Si hacéis esto último tenéis que tener en cuenta dos cosas:

1. La opción ‘-f’ en el comando que crea el túnel ssh, esta diciéndole a la máquina que deje el túnel creado y funcionando. Pero… ¿Y cómo cierro el túnel? Aquí hay dos opciones:
- Lo ejecutas sin la opción ‘-f’ en unaterminal, haces en otra terminal la conexión y lo que tengas que hacer y cuando acabes haces Ctrl+C para matar el comando y cerrar el túnel.

- Lo ejecutas con la opción -f y cuando lo quieras cerrar haces lo siguiente: (el comando ps con estos argumentos funciona bien en linux. Abría que mirar como es en mac)
```
$ ps -aux | grep ssh
```
Buscar el PID en la salida del comando que será de esta forma o similar:
```
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0  10440   588 ?        Ss   00:28   0:00 ssh user@ssh.example.com -L 3307:mysql1.examp...
```
y matas el proceso que se quedo corriendo el background de esta forma:
```
$ kill -9 <PID>
 ``` 
  
### Comandos:
```
show databases; -> lista las bases de datos que tiene el servidor

use name_database;-> selecciona o se conecta a la base de datos a trabajar

show tables; -> muestra las tablas que contiene la base de datos

select database(); -> muestra cual es la base de datos que tenemos seleccionada o en la que se esta trabajando.

Todos los comandos deben de terminar con “;”
```
