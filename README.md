# curso-sql-mysql

MySQL es un sistema de gestión de base de datos relacional en SQL, mientras que SQL (Structured Query Language) es un lenguaje estándar de programación que sirve para administrar (los sistemas de gestión de) las bases de datos, como MySQL.

Existen muchos tipos de bases de datos, desde un simple archivo hasta sistemas relacionales orientados a objetos. MySQL, como base de datos relacional, utiliza múltiples tablas para almacenar y organizar la información.

Fue escrito en C y C++ y se integra perfectamente con los lenguajes de programación más usados en todo el mundo.

![base de datos](https://kkslinuxinfo.files.wordpress.com/2016/02/database.png)

## Los RDMBS

Un sistema de gestión de bases de datos relacionales (RDBMS) es un programa que te permite crear, actualizar y administrar una base de datos relacional. La mayoría de los RDBMS comerciales utilizan el lenguaje de consultas estructuradas (SQL) para acceder a la base de datos, aunque SQL fue inventado después del desarrollo del modelo relacional y no es necesario para su uso.

## Intallar Mysql Server Ubuntu

En Ubuntu, en consola escriben:
```ssh
  sudo apt-get update
  sudo apt install mysql-server
  sudo mysql_secure_installation
```
Para correr la consola de MySQL, simplemente corren el comando:
```ssh
  sudo mysql
```
Si durante la instalacion cambiaron la contraseña por defecto, pueden correr el mismo commando con la bandera --pasword
```ssh
  sudo mysql --password 
```
## Comandos principales

```ssh
  # conectar a mysql
  mysql -u root -h localhost -p
  
  # mostrar bases de datos
  show databases;
  
  # abrir una base de datos
  use <nombre>;
  
  # mostrar tablas de una base de datos seleccionada
  show tables;
  
  # Ver la base de datos donde estamos 
  select database();
```
## Las bases de datos

Una base de datos es un conjunto de datos pertenecientes a un mismo contexto y almacenados sistemáticamente para su posterior uso. En este sentido; una biblioteca puede considerarse una base de datos compuesta en su mayoría por documentos y textos impresos en papel e indexados para su consulta. Actualmente, y debido al desarrollo tecnológico de campos como la informática y la electrónica, la mayoría de las bases de datos están en formato digital, siendo este un componente electrónico, por tanto se ha desarrollado y se ofrece un amplio rango de soluciones al problema del almacenamiento de datos.

Hay programas denominados sistemas gestores de bases de datos, abreviado SGBD (del inglés Database Management System o DBMS), que permiten almacenar y posteriormente acceder a los datos de forma rápida y estructurada. Las propiedades de estos DBMS, así como su utilización y administración, se estudian dentro del ámbito de la informática.

Las aplicaciones más usuales son para la gestión de empresas e instituciones públicas; También son ampliamente utilizadas en entornos científicos con el objeto de almacenar la información experimental.

## Tablas

Tabla en las bases de datos, se refiere al tipo de modelado de datos donde se guardan los datos recogidos por un programa. Su estructura general se asemeja a la vista general de un programa de tablas

Las tablas se componen de dos estructuras:

Campo: Corresponde al nombre de la columna. Debe ser único y además de tener un tipo de dato asociado.
Registro: Corresponde a cada fila que compone la tabla. Ahí se componen los datos y los registros. Eventualmente pueden ser nulos en su almacenamiento.
En la definición de cada campo, debe existir un nombre único, con su tipo de dato correspondiente. Esto es útil a la hora de manejar varios campos en la tabla, ya que cada nombre de campo debe ser distinto entre sí.

A los campos se les puede asignar, además, propiedades especiales que afectan a los registros insertados. El campo puede ser definido como índice o autoincrementable, lo cual permite que los datos de ese campo cambien solos o sean el principal indicar a la hora de ordenar los datos contenidos.

## Relaciones entre tablas

En pocas palabras, una «relación» es una asociación que se crea entre tablas, con el fin de vincularlas y garantizar la integridad referencial de sus datos.

Para que una relación entre dos tablas exista, la tabla que deseas relacionar debe poseer una clave primaria, mientras que la tabla donde estará el lado dependiente de la relación debe poseer una clave foránea, pero:

## Tipos de tablas

### MyISAM: 

1. Bloqueo de tabla
2. Aumento del rendimiento si nuestra aplicación realiza un elevado número de consultas “Select”.
3. Las tablas pueden llegar a dar problemas en la recuperación de datos.
4. Permite hacer búsquedas full-text
5. Menor consumo memoria RAM
5. Mayor velocidad en general a la hora de recuperar datos.
7. Ausencia de características de atomicidad ya que no tiene que hacer comprobaciones de la integridad referencial, ni bloquear las tablas para realizar las operaciones, esto nos lleva como los anteriores puntos a una mayor velocidad.

### InnoDB

1. Bloqueo de registros
2. Soporte de transacciones
3. Rendimiento
4. Concurrencia
5. Confiabilidad
6. Permite hacer búsquedas full-text (mysql >= 5.6)
7. Encontré este enlace con una buena discusión al respecto: MyISAM vs InnoDB (Ventajas y diferencias)

## Tipos de datos en SQL

<div style="margin-top: 100px;"></div>
<table border=1px >
<tr>
<td><b>Tipo de Datos</b></td><td><b>Longitud</b></td><td><b>Descripción</b></td>
</tr>
<tr>
<td>BINARY</td><td>1 byte</td><td>Para consultas sobre tabla adjunta de productos de bases de datos que definen un tipo de datos Binario.</td> 
</tr>
<tr>
<td>BIT</td><td>1 byte</td><td>Valores Si/No ó True/False</td> 
</tr>
<tr>
<td>BYTE</td><td>1 byte</td><td>Un valor entero entre 0 y 255.</td>
</tr>
<tr>
<td>COUNTER</td><td>4 bytes</td><td>Un número incrementado automáticamente (de tipo Long)</td>
</tr>
<tr>
<td>CURRENCY</td><td>8 bytes</td><td>Un entero escalable entre 922.337.203.685.477,5808 y 922.337.203.685.477,5807.</td>
</tr>
<tr>
<td>DATETIME</td><td>8 bytes</td><td>Un valor de fecha u hora entre los años 100 y 9999.</td>
</tr>
<tr>
<td>SINGLE</td><td>4 bytes</td><td>Un valor en punto flotante de precisión simple con un rango de - 3.402823*1038 a -1.401298*10-45 para valores negativos, 1.401298*10- 45 a 3.402823*1038 para valores positivos, y 0.</td>
</tr>
<tr>
<td>DOUBLE</td><td>8 bytes</td><td>Un valor en punto flotante de doble precisión con un rango de - 1.79769313486232*10308 a -4.94065645841247*10-324 para valores negativos, 4.94065645841247*10-324 a 1.79769313486232*10308 para valores positivos, y 0.</td>
</tr>
<tr>
<td>SHORT</td><td>2 bytes</td><td>Un entero corto entre -32,768 y 32,767.</td>
</tr>
<tr>
<td>LONG</td><td>4 bytes</td><td>Un entero largo entre -2,147,483,648 y 2,147,483,647.</td>
</tr>
<tr>
<td>LONGTEXT</td><td>1 byte por carácter</td><td>De cero a un máximo de 1.2 gigabytes.</td>
</tr>
<tr>
<td>LONGBINARY</td><td>Según se necesite</td><td>De cero 1 gigabyte.  Utilizado para objetos OLE.</td>
</tr>
<tr>
<td>TEXT</td><td>1 byte por carácter</td><td>De cero a 255 caracteres.</td> 
</tr>
</table>
<br>
La siguiente tabla recoge los sinónimos de los tipos de datos definidos:
<br>
<br>
<table border=1px>
<tr>
<td><b>Tipo de Dato</b></td><td><b>Sinónimos</b></td>	
</tr>
<tr>
<td>BINARY</td><td>VARBINARY</td>	
</tr>
<tr>
<td>BIT</td><td>BOOLEAN <br>LOGICAL <br>LOGICAL1 <br>YESNO</td>	
</tr>
<tr>
<td>BYTE</td><td>INTEGER1</td>	
</tr>
<tr>
<td>COUNTER</td><td>AUTOINCREMENT</td>	
</tr>
<tr>
<td>CURRENCY</td><td>MONEY</td>
</tr>
<tr>
<td>DATETIME</td><td>DATE <br>TIME <br>TIMESTAMP</td>	
</tr>
<tr>
<td>SINGLE</td><td>FLOAT4 <br>IEEESINGLE <br>REAL</td>	
</tr>
<tr>
<td>DOUBLE</td><td>FLOAT<br>FLOAT8 <br>IEEEDOUBLE <br>NUMBER <br>NUMERIC</td>	
</tr>
<tr>
<td>SHORT</td><td>INTEGER2 <br>SMALLINT</td>	
</tr>
<tr>
<td>LONG</td><td>INT <br>INTEGER <br>INTEGER4</td>	
</tr>
<tr>
<td>LONGBINARY</td><td>GENERAL <br>OLEOBJECT</td>	
</tr>
<tr>
<td>LONGTEXT</td><td>LONGCHAR<br>MEMO <br>
NOTE</td>	
</tr>
<tr>
<td>TEXT</td>	<td>ALPHANUMERIC <br>CHAR - CHARACTER <br>STRING - VARCHAR</td>
</tr>
<tr>
<td>VARIANT (No Admitido)</td><td>VALUE</td>
</tr></table>
  </div>


## CREATE





















