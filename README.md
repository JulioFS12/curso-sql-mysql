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

## Errores de login 
```ssh
  Some systems like Ubuntu, mysql is using by default the UNIX auth_socket plugin.

  Basically means that: db_users using it, will be "auth" by the system user credentias. You can see if your root user is set up like this by doing the following:

  $ sudo mysql -u root # I had to use "sudo" since is new installation

  mysql> USE mysql;
  mysql> SELECT User, Host, plugin FROM mysql.user;

  +------------------+-----------------------+
  | User             | plugin                |
  +------------------+-----------------------+
  | root             | auth_socket           |
  | mysql.sys        | mysql_native_password |
  | debian-sys-maint | mysql_native_password |
  +------------------+-----------------------+
  As you can see in the query, the root user is using the auth_socket plugin

  There are 2 ways to solve this:

  You can set the root user to use the mysql_native_password plugin
  You can create a new db_user with you system_user (recommended)
  Option 1:

  $ sudo mysql -u root # I had to use "sudo" since is new installation

  mysql> USE mysql;
  mysql> UPDATE user SET plugin='mysql_native_password' WHERE User='root';
  mysql> FLUSH PRIVILEGES;
  mysql> exit;

  $ sudo service mysql restart
  Option 2: (replace YOUR_SYSTEM_USER with the username you have)

  $ sudo mysql -u root # I had to use "sudo" since is new installation

  mysql> USE mysql;
  mysql> CREATE USER 'YOUR_SYSTEM_USER'@'localhost' IDENTIFIED BY 'YOUR_PASSWD';
  mysql> GRANT ALL PRIVILEGES ON *.* TO 'YOUR_SYSTEM_USER'@'localhost';
  mysql> UPDATE user SET plugin='auth_socket' WHERE User='YOUR_SYSTEM_USER';
  mysql> FLUSH PRIVILEGES;
  mysql> exit;

  $ sudo service mysql restart
  Remember that if you use option #2 you'll have to connect to mysql as your system username (mysql -u YOUR_SYSTEM_USER)

  Note: On some systems (e.g., Debian stretch) 'auth_socket' plugin is called 'unix_socket', so the corresponding SQL command should be: UPDATE user SET plugin='unix_socket' WHERE User='YOUR_SYSTEM_USER';

  Update: from @andy's comment seems that mysql 8.x.x updated/replaced the auth_socket for caching_sha2_password I don't have a system setup with mysql 8.x.x to test this, however the steps above should help you to understand the issue. Here's the reply:

  One change as of MySQL 8.0.4 is that the new default authentication plugin is 'caching_sha2_password'. The new 'YOUR_SYSTEM_USER' will have this auth plugin and you can login from the bash shell now with "mysql -u YOUR_SYSTEM_USER -p" and provide the password for this user on the prompt. No need for the "UPDATE user SET plugin" step. For the 8.0.4 default auth plugin update see, https://mysqlserverteam.com/mysql-8-0-4-new-default-authentication-plugin-caching_sha2_password/
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

```sql 

  /* crear base de datos */
  CREATE DATABASE IF NOT EXISTS <nombre de la base de datos>;
  
  /* crear una tabla books*/
  
  CREATE TABLE IF NOT EXISTS books (
    book_id INTEGER UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    author INTEGER UNSIGNED ,
    /* no es lo mismo un dato null que un dato vacio*/
    title VARCHAR(100) NOT NULL,
    `year` INTEGER UNSIGNED NOT NULL DEFAULT 1990,
    language VARCHAR(2) NOT NULL DEFAULT 'es' COMMENT 'ISO 639-1 Language',
    /* es recomendado no guardar las imagenes en la base de datos*/
    cover_url VARCHAR(500),
    price DOUBLE(6,2) NOT NULL DEFAULT 10.0,
    sellable TINYINT(1) DEFAULT 1,
    copies INTEGER NOT NULL DEFAULT 1,
    description TEXT
  );
  
  CREATE TABLE IF NOT EXISTS authors (
    author_id INTEGER UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    nationality VARCHAR(3)
  );  
  
  CREATE TABLE IF NOT EXISTS clients (
    client_id INTEGER UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    `name` VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    birthdate DATETIME,
    gender ENUM('M', 'F', 'ND') NOT NULL,
    active TINYINT(1) NOT NULL DEFAULT 1,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
  );
  
  CREATE TABLE IF NOT EXISTS `transactions` (
  `transaction_id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `book_id` int(10) unsigned NOT NULL,
  `client_id` int(10) unsigned NOT NULL,
  `type` enum('lend','sell') NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `modified_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  `finished` tinyint(1) NOT NULL DEFAULT '0',
  PRIMARY KEY (`transaction_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
  
```
[more about datetime and timestamp](https://dev.mysql.com/doc/refman/8.0/en/datetime.html)
```ssh
  /*mostrar errores al crear tablas*/
  show warnings;
  
  /*eliminar tabla*/
  drop table <nombre de la tabla>;
  
  /*mostrar a detalle los campos de una tabla*/
  describe <nombre de la tabla>
  
  /*muestra con muchos mas detalles todas las tablas con cada tupla*/
  show full columns from <nombre de la tabla>;

```

## INSERT 

```sql  
  /*insercion en tablas*/
  INSERT INTO authors (name, nationality)
  VALUES('Juan Rulfo', 'MEX');
  
  INSERT INTO authors (name, nationality)
  VALUES('Gabriel Garcia Marquez', 'COL');
  
  INSERT INTO authors (name, nationality)
  VALUES('Gabriel Garcia Marquez', 'COL');
  
  /*insercion simultanea*/
  
  INSERT INTO authors (name, nationality)
  VALUES('julio flores', 'VE'),
  ('Elonk Musk', 'CA'),
  ('Javier Santaolalla', 'ESP'),
  ('Freddy Vegass', 'COL');
  
  /*Nos permite sustituir el valor que enviamos como primary key y evita el error de duplicidad*/
  ON DUPLICATE KEY UPDATE <dubla o celda> = VALUES(<variable de valor enviado);
  
  /*Insertar con query anidado - nos permite buscar en otra tabla un valor para insertarlo en la tabla donde estamos almacenando los datos*/
  INSERT INTO books (title, author_id, `year`) VALUES ('this is the title', ( SELECT FROM  < nombre de la tabla> WHERE name = 'Nombre que recibimos), '1906);
```
## SELECT

### select, from, like, where, functions

```sql
  
  /*Traer solo una columna especifica (en este caso la columna “name”) */
  SELECT name FROM clients;

  /*Traer varias columnas especificas (en este caso la columna “name” y “gender”)*/
   SELECT name, email, gender FROM clients 
   
  /* Limitar el numero de resultados (en este caso maximo 10 resultados) */
  SELECT name, email, gender FROM clients LIMIT 10;
  
  /*Condicionar los resultados a una caracteristica (en este caso todos los resultados que   tengan gender con el valor “M”)*/
  SELECT name, email, gender FROM clients WHERE gender='M';
  
  /*Utilizar funciones para obtener datos especificos (en este caso todas las Mujeres que   en su nombre tengan la cadena “Lop”)*/
  SELECT name, email, YEAR(NOW()) - YEAR(birthdate) AS edad, gender
  FROM clients
  WHERE gender='F'
    AND name LIKE '%Lop%';
```
[Funciones en SQL](https://diego.com.es/principales-funciones-en-sql)

### INNER JOIN, IN, ON 

![joins](https://static.platzi.com/media/user_upload/FB_IMG_1464718822700-8fbde636-f214-40fd-8478-84114dddc1d2.jpg)
![joins](https://static.platzi.com/media/user_upload/FB_IMG_1464718826308-8604c822-a5de-4857-8265-61480fd706dd.jpg)
![joins](https://static.platzi.com/media/user_upload/FB_IMG_1464718830425-4b6b0a43-e326-4656-b7fe-f66ea93f431f.jpg)
![joins](https://static.platzi.com/media/user_upload/FB_IMG_1464718833847-68334673-7ee9-4118-be6a-3320276d2e13.jpg)
![joins](https://static.platzi.com/media/user_upload/FB_IMG_1464718837175-0039cf85-87ca-4407-9c15-baf7efa6106b.jpg)

```sql
  
  -- Listar todos los autores con ID entre 1 y 5 con los filtro mayor y menor igual
SELECT * FROM authors WHERE author_id > 0 AND author_id <= 5;

-- Listar todos los autores con ID entre 1 y 5 con el filtro BETWEEN
SELECT * FROM authors WHERE author_id BETWEEN 1 AND 5;

-- Listar los libros con filtro de author_id entre 1 y 5
SELECT book_id, author_id, title FROM books WHERE author_id BETWEEN 1 AND 5;

-- Listar nombre y titulo de libros mediante el JOIN de las tablas books y authors
SELECT b.book_id, a.name, a.author_id, b.title
FROM books AS b
JOIN authors AS a
  ON a.author_id = b.author_id
WHERE a.author_id BETWEEN 1 AND 5;

-- Listar transactions con detalle de nombre, titulo y tipo. Con los filtro genero = F y tipo = Vendido.
-- Haciendo join entre transactions, books y clients.
SELECT c.name, b.title, t.type
FROM transactions AS t
JOIN books AS b
  ON t.book_id = b.book_id
JOIN clients AS c
  ON t.client_id = c.client_id
WHERE c.gender = 'F'
  AND t.type = 'sell';

-- Listar transactions con detalle de nombre, titulo, autoor y tipo. Con los filtro genero = M y de tipo = Vendido y Devuelto.
-- Haciendo join entre transactions, books, clients y authors.
SELECT c.name, b.title, a.name, t.type
FROM transactions AS t
JOIN books AS b
  ON t.book_id = b.book_id
JOIN clients AS c
  ON t.client_id = c.client_id
JOIN authors AS a
  ON b.author_id = a.author_id
WHERE c.gender = 'M'
  AND t.type IN ('sell', 'lend');

  
```

### Left JOIN, ORDER BY, DESC

Siempre que se trabaje con funciones como son Sum(), Count(), Avg(), Max(), Min(), deben agregar la funcion group cuando en el select incluyan otra columna que desean mostrar aparte de la funcion de agrupamiento otra columna

```sql
  -- Uso del JOIN implícito
SELECT b.title, a.name
FROM authors AS a, books AS b
WHERE a.author_id = b.author_id
LIMIT 10;

-- Uso del JOIN explícito
SELECT b.title, a.name
FROM books AS b
INNER JOIN authors AS a
  ON a.author_id = b.author_id
LIMIT 10;

--  JOIN y order by (por defecto es ASC)
SELECT a.author_id, a.name, a.nationality, b.title
FROM authors AS a
JOIN books AS b
  ON b.author_id = a.author_id
WHERE a.author_id BETWEEN 1 AND 5
ORDER BY a.author_id DESC;

-- LEFT JOIN para traer datos incluso que no existen, como el caso del author_id = 4 que no tene ningún libro registrado.
SELECT a.author_id, a.name, a.nationality, b.title
FROM authors AS a
LEFT JOIN books AS b
  ON b.author_id = a.author_id
WHERE a.author_id BETWEEN 1 AND 5
ORDER BY a.author_id;

-- Contar número de libros tiene un autor.
-- Con COUNT (contar), es necesario tener un GROUP BY (agrupado por un criterio)
SELECT a.author_id, a.name, a.nationality, COUNT(b.book_id)
FROM authors AS a
LEFT JOIN books AS b
  ON b.author_id = a.author_id
WHERE a.author_id BETWEEN 1 AND 5
GROUP BY a.author_id
ORDER BY a.author_id;
```
### DISTINT, IS NOT NULL, IS NOT IN, 

```sql
  -- 1. ¿Qué nacionalidades hay?
  -- Mediante la clausula DISTINCT trae solo los elementos distintos
  SELECT DISTINCT nationality 
  FROM authors
  ORDER BY 1;

  -- 2. ¿Cuántos escritores hay de cada nacionalidad?
  -- IS NOT NULL para traer solo los valores diferentes de nulo
  -- NOT IN para traer valores que no sean los declarados (RUS y AUT)
  SELECT nationality, COUNT(author_id) AS c_authors
  FROM authors
  WHERE nationality IS NOT NULL
    AND nationality NOT IN ('RUS','AUT')
  GROUP BY nationality
  ORDER BY c_authors DESC, nationality ASC;
```

### More Query
```sql
  -- 4. ¿Cuál es el promedio/desviación standard del precio de libros?
  SELECT a.nationality,  
    AVG(b.price) AS promedio, 
    STDDEV(b.price) AS std 
  FROM books AS b
  JOIN authors AS a
    ON a.author_id = b.author_id
  GROUP BY a.nationality
  ORDER BY promedio DESC;

  -- 5. ¿Cuál es el promedio/desviación standard del precio de libros por nacionalidad?
  -- Agrupar por la columna pivot
  SELECT a.nationality,
    COUNT(b.book_id) AS libros,  
    AVG(b.price) AS promedio, 
    STDDEV(b.price) AS std 
  FROM books AS b
  JOIN authors AS a
    ON a.author_id = b.author_id
  GROUP BY a.nationality
  ORDER BY libros DESC;

  -- 6. ¿Cuál es el precio máximo/mínimo de un libro?
  SELECT nationality, MAX(price), MIN(price)
  FROM books AS b
  JOIN authors AS a
    ON a.author_id = b.author_id
  GROUP BY nationality;

  -- 7. ¿cómo quedaría el reporte de préstamos?
  -- CONCAT: para concatenar en cadenas de texto.
  -- TO_DAYS: recibe un timestamp ó un datetime
  SELECT c.name, t.type, b.title, 
    CONCAT(a.name, " (", a.nationality, ")") AS autor,
    TO_DAYS(NOW()) - TO_DAYS(t.created_at)
  FROM transactions AS t
  LEFT JOIN clients AS c
    ON c.client_id = t.client_id
  LEFT JOIN books AS b
    ON b.book_id = t.book_id
  LEFT JOIN authors AS a
    ON b.author_id = a.author_id;
```

### UPDATE AND DELETED

```sql
  -- Clase 19 UPDATE Y DELETE

  -- Ejemplos de Delete y Update

  DELETE FROM authors WHERE author_id = 161 LIMIT 1;

  SELECT client_id, name FROM clients WHERE active <> 1;

  UPDATE clients SET active = 0 WHERE client_id = 80 LIMIT 1;

  UPDATE clients
  SET 
      email = 'javier@gmail.com'
  WHERE
      client_id = 7
  LIMIT 1;

  --Desactivar clientes con ID especifica y apellido lopez

  UPDATE clients
  SET 
      active = 0
  WHERE
      client_id IN (1,6,8,27)
      OR name like '%Lopez%';

  -- Buscar con ID y Lopez

  SELECT client_id, name
  FROM clients
  WHERE
      client_id IN (1,6,8,27)
      OR name like '%Lopez%';
```

### SUPER  QUERY

```sql
  SELECT DISTINCT nationality FROM authors;

  UPDATE authors SET nationality = 'GBR' WHERE nationality = 'ENG';

  SELECT COUNT(*) FROM books;

  SELECT COUNT(book_id) FROM books;

  SELECT COUNT(book_id) , SUM(1) FROM books;

  SELECT SUM(price) FROM books WHERE sellable = 1; 

  SELECT SUM(price*copies) FROM books WHERE sellable = 1;

  SELECT sellable, SUM(price*copies) FROM books GROUP BY sellable;

  SELECT COUNT(book_id) , SUM(IF(YEAR < 1950,1,0)) AS '< 1950' FROM books;

  SELECT COUNT(book_id)   FROM books WHERE YEAR < 1950;

  SELECT COUNT(book_id) , SUM(IF(YEAR < 1950,1,0)) AS '< 1950' , SUM(IF(YEAR <= 1950,0,1)) AS '>= 1950'  FROM books;

  SELECT COUNT(book_id) , SUM(IF(YEAR < 1950,1,0)) AS '< 1950' , SUM(IF(YEAR >= 1950 AND YEAR < 1990,1,0)) AS '< 1990',  
  SUM(IF(YEAR >= 1990 AND YEAR < 2000,1,0)) AS '< 2000'
  FROM books; 

  SELECT COUNT(book_id) , SUM(IF(YEAR < 1950,1,0)) AS '< 1950' , SUM(IF(YEAR >= 1950 AND YEAR < 1990,1,0)) AS '< 1990',  
  SUM(IF(YEAR >= 1990 AND YEAR < 2000,1,0)) AS '< 2000' , SUM(IF(YEAR >= 2000 ,1,0)) AS '> 2000' 
  FROM books; 

  SELECT nationality,COUNT(book_id) , SUM(IF(YEAR < 1950,1,0)) AS '< 1950' , SUM(IF(YEAR >= 1950 AND YEAR < 1990,1,0)) AS '< 1990',  
  SUM(IF(YEAR >= 1990 AND YEAR < 2000,1,0)) AS '< 2000' , SUM(IF(YEAR >= 2000 ,1,0)) AS '> 2000' 
  FROM books AS b INNER JOIN authors AS a ON
  b.author_id = a.author_id
  WHERE nationality IS NOT NULL
  GROUP BY nationality ;  
```

## Comandos del curso

```sql
  SHOW databases; - muestra las bases de datos existentes.
  USE database_name; - selecciona una base de datos específica.
  SHOW tables; - muestras las tablas de la base de datos.
  SELECT database(); - me muestra el nombre de la base de datos seleccionada.
  CREATE database database_name; - crea una nueva base de datos.
  CREATE DATABASE IF NOT EXISTS database_name; - crea una base de datos si no existe.
  SHOW warnings; - muestra las advertencias.
  DROP table table_name; - Elimina permanentemente una tabla.
  DESCRIBE table_name; - Nos indica las columnas que tenemos en una tabla.
  SHOW FULL COLUMNS FROM table_name; - es parecido al comando DESCRIBE pero muestra mas datos.
  INSERT INTO table_name(columns) VALUES(values); - inserta una tupla.
  ON DUPLICATE KEY IGNORE ALL - esta sentencia ignora las resticciones al insertar una tupla
  con un valor repetido y que esta restringido en una columna con UNIQUE (Nota: nunca utilizarlo).
  ON DUPLICATE KEY UPDATE column = VALUES(value) - al insertar una tupla con un campo duplicado
  actualiza un el valor de un campo específico con un nuevo valor tomado de los datos insertados.
  SELECT * FROM table_name WHERE column_value = 1\G - en lugar de cerrar la sentencia con ;
  se utiliza \G, lo cual muestra los datos de una manera mas legible.
  mysql -u root -p < all_schema.sql - con este comando podemos ejecutar un script SQL inmediatamente
  despues de acceder a la base de datos.
  mysql -u root -p -D database_name < all_schema.sql - este comando es parecido al anterior solo
  que con la bandera -D indicamos el nombre de la base de datos sobre la que queremos ejecutar el script.
  SELECT YEAR(NOW()); - esta sentencia me muestra el año de la fecha actual utilizando las funciones YEAR() y NOW().
  SELECT * FROM table_name WHERE column_value like ‘%value%’; - esta sentencia nos muestra las tuplas que en un
  campo específico contengan un valor, el wildcard % indica que no nos importa que valor existan antes o despues del
  dato que especificamos.
  SELECT COUNT(*) FROM table_name; - devuelve el número de tuplas de una tabla.
  SELECT * FROM table_name WHERE column_value BETWEEN value AND value; - nos devuelve las tuplas que se encuentren
  en medio de los valores indicados.
  DELETE FROM table_name WHERE column_value = value; - elimina una tupla de una tabla.
  UPDATE table_name SET [column_value = value, …] WHERE column_value = value; - actualiza una tupla de una tabla.
  TRUNCATE table_name; - borra todo el contenido de una tabla.
  mysqldump -u user -p database_name > esquema.sql - guarda el esquema de una base de datos con todo y datos en un
  archivo sql.
  mysqldump -u user -p -d database_name es parecido al comando anterior solo que aquí no se guardan los datos.
```

## Script del curso

```sql
  DROP DATABASE IF EXISTS pruebaplatzi;

  CREATE DATABASE pruebaplatzi;
  USE pruebaplatzi;

  CREATE TABLE `authors` (
    `author_id` int(10) unsigned NOT NULL AUTO_INCREMENT,
    `name` varchar(100) NOT NULL,
    `nationality` varchar(100) DEFAULT NULL,
    PRIMARY KEY (`author_id`),
    UNIQUE KEY `uniq_author` (`name`)
  ) ENGINE=InnoDB AUTO_INCREMENT=193 DEFAULT CHARSET=utf8;

  INSERT INTO `authors` VALUES (1,'Sam Altman','USA'),
  (2,'Freddy Vega','COL'),
  (3,'Arthur Conan Doyle','GBR'),
  (4,'Chuck Palahniuk','USA'),
  (5,'Juan Rulfo','MEX'),
  (6,'Henning Mankel','SWE'),
  (7,'Jaideva Goswami','USA'),
  (8,'John Foreman','USA'),
  (9,'Stephen Hawking','USA'),
  (10,'Stephen Dubner','USA'),
  (11,'Edward Said','USA'),
  (12,'Vladimir Vapnik','RUS'),
  (13,'V P Menon','IND'),
  (14,'Leonard Mlodinow','USA'),
  (15,'Frank Shih','JAP'),
  (16,'Maria Konnikova','RUS'),
  (17,'Sebastian Gutierrez','ESP'),
  (18,'Kurt Vonnegut','USA'),
  (19,'Cedric Villani','FRA'),
  (20,'Gerald Sussman','USA'),
  (21,'Abraham Eraly','IND'),
  (22,'Frank Kafka','AUT'),
  (23,'John Pratt','USA'),
  (24,'Robert Nisbet','USA'),
  (25,'H. G. Wells',"ENG"),
  (26,'Werner Heisenberg','DEU'),
  (27,'Andy Oram',NULL),
  (28,'Terence Tao',"AUS"),
  (29,'Drew Conway',"USA"),
  (30,'Nate Silver',"USA"),
  (31,'Wes McKinney',"USA"),
  (32,'Thomas Cormen', "USA"),
  (33,'Siddhartha Deb',"IND"),
  (34,'Albert Camus',"FRA"),
  (37,'Adam Smith',"ENG"),
  (38,'Ken Follett',"ENG"),
  (39,'Fritjof Capra',"AUT"),
  (40,'Richard Feynman',"USA"),
  (41,'Ernest Hemingway',"USA"),
  (42,'Frederick Forsyth',"ENG"),
  (43,'Jeffery Archer',"ENG"),
  (44,'Randy Pausch',"USA"),
  (45,'Ayn Rand',"RUS"),
  (46,'Michael Crichton',"USA"),
  (47,'John Steinbeck',"USA"),
  (48,'Edgar Allen Poe',"USA"),
  (51,'Will Durant',NULL),
  (52,'P L Deshpande',NULL),
  (56,'John Grisham',"USA"),
  (57,'V. S. Naipaul',NULL),
  (58,'Joseph Heller',NULL),
  (59,'BBC',NULL),
  (60,'Bob Dylan',"USA"),
  (61,'Madan Gupta',"IND"),
  (62,'Alfred Stonier',NULL),
  (63,'W. H. Greene',NULL),
  (64,'Gary Bradsky',NULL),
  (65,'Andrew Tanenbaum',NULL),
  (66,'David Forsyth',NULL),
  (67,'Schilling Taub',NULL),
  (68,'Yashwant Kanetkar',NULL),
  (69,'Jonathan Stroud',NULL),
  (70,'Fyodor Dostoevsky',"RUS"),
  (71,'Dan Brown',"USA"),
  (72,'Amartya Sen',NULL),
  (73,'Amitav Ghosh',NULL),
  (75,'Lorraine Hansberry',NULL),
  (76,'Bob Woodward',NULL),
  (78,'Kuldip Nayar',NULL),
  (79,'Sunita Deshpande',NULL),
  (80,'William Dalrymple',NULL),
  (81,'Various',NULL),
  (85,'Sanjay Garg',NULL),
  (86,'V P Kale',NULL),
  (87,'Shashi Tharoor',"IND"),
  (89,'Dominique Lapierre',NULL),
  (93,'Bertrand Russell',"ENG"),
  (94,'Sam Harris',NULL),
  (96,'Earle Stanley Gardner',NULL),
  (98,'Peter Drucker',NULL),
  (99,'David Bodanis',NULL),
  (100,'Victor Hugo',"FRA"),
  (103,'Richard Gordon',NULL),
  (104,'George Orwell',NULL),
  (107,'Lee Iacoca',"USA"),
  (108,'William S Maugham',NULL),
  (111,'Robert Pirsig',NULL),
  (112,'Robert Fisk',NULL),
  (114,'Amir Aczel',NULL),
  (117,'Samuel Huntington',NULL),
  (119,'Richard Bach',NULL),
  (120,'Braithwaite',NULL),
  (121,'V S Naipaul',NULL),
  (122,'Jawaharlal Nehru',NULL),
  (128,'Gerald Durrell',NULL),
  (133,'Simon Singh',"ENG"),
  (134,'Hart Duda',NULL),
  (135,'Thomas Friedman',NULL),
  (138,'Keith Devlin',NULL),
  (140,'James Gleick',NULL),
  (141,'Joy Thomas',NULL),
  (142,'Muhammad Rashid',NULL),
  (143,'Ned Mohan',NULL),
  (144,'Simon Haykin',NULL),
  (148,'Alex Rutherford',NULL),
  (153,'Michael Baz-Zohar',NULL),
  (154,'Jim Corbett',NULL),
  (155,'Jules Verne',NULL),
  (156,'Deshpande P L',NULL),
  (160,'Eric Raymond',NULL),
  (161,'Sergio Franco',NULL),
  (162,'Allen Downey',NULL),
  (163,'Morris West',NULL),
  (166,'Phillip Janert',NULL),
  (167,'Carl Sagan',"USA"),
  (168,'E T Bell',NULL),
  (169,'Richard Dawkins',NULL),
  (170,'Sudhanshu Ranjan',"IND"),
  (171,'Kautiyla',NULL),
  (172,'Palkhivala',NULL),
  (174,'Sorabjee',NULL),
  (175,'Hussain Zaidi',NULL),
  (176,'Peter Ackroyd',NULL),
  (178,'Nariman',NULL),
  (179,'Jean Sassoon',NULL),
  (180,'Peter Dickinson',NULL),
  (182,'Machiavelli',NULL),
  (183,'Aldous Huxley',"ENG"),
  (184,'J K Rowling',"ENG"),
  (185,'Steig Larsson',"SWE"),
  (189,'Steve Eddins', NULL),
  (192,'Charles Dickens',"ENG");


  CREATE TABLE `books` (
    `book_id` int(10) unsigned NOT NULL AUTO_INCREMENT,
    `author_id` int(10) unsigned DEFAULT NULL,
    `title` varchar(100) NOT NULL,
    `year` int(11) NOT NULL DEFAULT '1900',
    `language` varchar(2) NOT NULL COMMENT 'ISO 639-1 Language code (2 chars)',
    `cover_url` varchar(500) DEFAULT NULL,
    `price` double(6,2) DEFAULT NULL,
    `sellable` tinyint(1) NOT NULL DEFAULT '0',
    `copies` int(11) NOT NULL DEFAULT '1',
    `description` text,
    PRIMARY KEY (`book_id`),
    UNIQUE KEY `book_language` (`title`,`language`)
  ) ENGINE=InnoDB AUTO_INCREMENT=199 DEFAULT CHARSET=utf8;

  INSERT INTO `books` VALUES (1,1,'The Startup Playbook',2013,'en',NULL,10.00,1,5,'Advice from the experts'),
  (2,1,'The Startup Playbook',2014,'es',NULL,10.00,1,5,'Consejo de los expertos, traducido por Platzi'),
  (3,3,'Estudio en escarlata',1887,'es',NULL,5.00,1,10,'La primera novela de Sherlock Holmes'),
  (4,6,'Wallander: Asesinos sin rostro',1991,'es',NULL,15.00,1,3,''),
  (5,6,'Wallander: Los perros de Riga',1992,'es',NULL,15.00,1,3,''),
  (6,6,'Wallander: La leona blanca',1993,'es',NULL,15.00,1,3,''),
  (7,6,'Wallander: El hombre sonriente',1994,'es',NULL,15.00,1,3,''),
  (8,6,'Wallander: La falsa pista',1995,'es',NULL,15.00,1,3,''),
  (9,6,'Wallander: La quinta mujer',1996,'es',NULL,15.00,1,3,''),
  (10,6,'Wallander: Pisando los talones',1997,'es',NULL,15.00,1,3,''),
  (11,6,'Wallander: Cortafuegos',1998,'es',NULL,15.00,1,3,''),
  (12,5,'El llano en llamas',1953,'es',NULL,10.00,0,1,'Cuentos mexicanos'),
  (13,7,'Fundamentals of Wavelets',1900,'en',NULL,NULL,1,4,NULL),
  (14,8,'Data Smart',1900,'en',NULL,NULL,1,4,NULL),
  (15,9,'God Created the Integers',1900,'en',NULL,NULL,1,4,NULL),
  (16,10,'Superfreakonomics',1900,'en',NULL,NULL,1,4,NULL),
  (17,11,'Orientalism',1900,'en',NULL,NULL,1,4,NULL),
  (18,12,'The Nature of Statistical Learning Theory',1900,'en',NULL,NULL,1,4,NULL),
  (19,13,'Integration of the Indian States',1900,'en',NULL,NULL,1,4,NULL),
  (20,14,'The Drunkard\'s Walk',1900,'en',NULL,NULL,1,4,NULL),
  (21,15,'Image Processing & Mathematical Morphology',1900,'en',NULL,NULL,1,4,NULL),
  (22,16,'How to Think Like Sherlock Holmes',1900,'en',NULL,NULL,1,4,NULL),
  (23,17,'Data Scientists at Work',1900,'en',NULL,NULL,1,4,NULL),
  (24,18,'Slaughterhouse Five',1900,'en',NULL,NULL,1,4,NULL),
  (25,19,'Birth of a Theorem',1900,'en',NULL,NULL,1,4,NULL),
  (26,20,'Structure & Interpretation of Computer Programs',1900,'en',NULL,NULL,1,4,NULL),
  (27,21,'The Age of Wrath',1900,'en',NULL,NULL,1,4,NULL),
  (28,22,'The Trial',1900,'en',NULL,NULL,1,4,NULL),
  (29,23,'Statistical Decision Theory',1900,'en',NULL,NULL,1,4,NULL),
  (30,24,'Data Mining Handbook',1900,'en',NULL,NULL,1,4,NULL),
  (31,25,'The New Machiavelli',1900,'en',NULL,NULL,1,4,NULL),
  (32,26,'Physics & Philosophy',1900,'en',NULL,NULL,1,4,NULL),
  (33,27,'Making Software',1900,'en',NULL,NULL,1,4,NULL),
  (34,28,'Vol I Analysis',1900,'en',NULL,NULL,1,4,NULL),
  (35,29,'Machine Learning for Hackers',1900,'en',NULL,NULL,1,4,NULL),
  (36,30,'The Signal and the Noise',1900,'en',NULL,NULL,1,4,NULL),
  (37,31,'Python for Data Analysis',1900,'en',NULL,NULL,1,4,NULL),
  (38,32,'Introduction to Algorithms',1900,'en',NULL,NULL,1,4,NULL),
  (39,33,'The Beautiful and the Damned',1900,'en',NULL,NULL,1,4,NULL),
  (40,34,'The Outsider',1900,'en',NULL,NULL,1,4,NULL),
  (41,3,'The - Vol I Complete Sherlock Holmes',1900,'en',NULL,NULL,1,4,NULL),
  (42,3,'The - Vol II Complete Sherlock Holmes',1900,'en',NULL,NULL,1,4,NULL),
  (43,37,'The Wealth of Nations',1900,'en',NULL,NULL,1,4,NULL),
  (44,38,'The Pillars of the Earth',1900,'en',NULL,NULL,1,4,NULL),
  (45,39,'The Tao of Physics',1900,'en',NULL,NULL,1,4,NULL),
  (46,40,'Surely You\'s re Joking Mr Feynman',1900,'en',NULL,NULL,1,4,NULL),
  (47,41,'A Farewell to Arms',1900,'en',NULL,NULL,1,4,NULL),
  (48,42,'The Veteran',1900,'en',NULL,NULL,1,4,NULL),
  (49,43,'False Impressions',1900,'en',NULL,NULL,1,4,NULL),
  (50,44,'The Last Lecture',1900,'en',NULL,NULL,1,4,NULL),
  (51,45,'Return of the Primitive',1900,'en',NULL,NULL,1,4,NULL),
  (52,46,'Jurassic Park',1900,'en',NULL,NULL,1,4,NULL),
  (53,47,'A Russian Journal',1900,'en',NULL,NULL,1,4,NULL),
  (54,48,'Tales of Mystery and Imagination',1900,'en',NULL,NULL,1,4,NULL),
  (55,10,'Freakonomics',1900,'en',NULL,NULL,1,4,NULL),
  (56,39,'The Hidden Connections',1900,'en',NULL,NULL,1,4,NULL),
  (57,51,'The Story of Philosophy',1900,'en',NULL,NULL,1,4,NULL),
  (58,52,'Asami Asami',1900,'en',NULL,NULL,1,4,NULL),
  (59,47,'Journal of a Novel',1900,'en',NULL,NULL,1,4,NULL),
  (60,47,'Once There Was a War',1900,'en',NULL,NULL,1,4,NULL),
  (61,47,'The Moon is Down',1900,'en',NULL,NULL,1,4,NULL),
  (62,56,'The Brethren',1900,'en',NULL,NULL,1,4,NULL),
  (63,57,'In a Free State',1900,'en',NULL,NULL,1,4,NULL),
  (64,58,'Catch 22',1900,'en',NULL,NULL,1,4,NULL),
  (65,59,'The Complete Mastermind',1900,'en',NULL,NULL,1,4,NULL),
  (66,60,'Dylan on Dylan',1900,'en',NULL,NULL,1,4,NULL),
  (67,61,'Soft Computing & Intelligent Systems',1900,'en',NULL,NULL,1,4,NULL),
  (68,62,'Textbook of Economic Theory',1900,'en',NULL,NULL,1,4,NULL),
  (69,63,'Econometric Analysis',1900,'en',NULL,NULL,1,4,NULL),
  (70,64,'Learning OpenCV',1900,'en',NULL,NULL,1,4,NULL),
  (71,65,'Data Structures Using C & C++',1900,'en',NULL,NULL,1,4,NULL),
  (72,66,'A Modern Approach Computer Vision',1900,'en',NULL,NULL,1,4,NULL),
  (73,67,'Principles of Communication Systems',1900,'en',NULL,NULL,1,4,NULL),
  (74,68,'Let Us C',1900,'en',NULL,NULL,1,4,NULL),
  (75,69,'The Amulet of Samarkand',1900,'en',NULL,NULL,1,4,NULL),
  (76,70,'Crime and Punishment',1900,'en',NULL,NULL,1,4,NULL),
  (77,71,'Angels & Demons',1900,'en',NULL,NULL,1,4,NULL),
  (78,72,'The Argumentative Indian',1900,'en',NULL,NULL,1,4,NULL),
  (79,73,'Sea of Poppies',1900,'en',NULL,NULL,1,4,NULL),
  (80,72,'The Idea of Justice',1900,'en',NULL,NULL,1,4,NULL),
  (81,75,'A Raisin in the Sun',1900,'en',NULL,NULL,1,4,NULL),
  (82,76,'All the President\'s Men',1900,'en',NULL,NULL,1,4,NULL),
  (83,43,'A Prisoner of Birth',1900,'en',NULL,NULL,1,4,NULL),
  (84,78,'Scoop!',1900,'en',NULL,NULL,1,4,NULL),
  (85,79,'Ahe Manohar Tari',1900,'en',NULL,NULL,1,4,NULL),
  (86,80,'The Last Mughal',1900,'en',NULL,NULL,1,4,NULL),
  (87,81,'Vol 39 No. 1 Social Choice & Welfare',1900,'en',NULL,NULL,1,4,NULL),
  (88,52,'Radiowaril Bhashane & Shrutika',1900,'en',NULL,NULL,1,4,NULL),
  (89,52,'Gun Gayin Awadi',1900,'en',NULL,NULL,1,4,NULL),
  (90,52,'Aghal Paghal',1900,'en',NULL,NULL,1,4,NULL),
  (91,85,'Maqta-e-Ghalib',1900,'en',NULL,NULL,1,4,NULL),
  (92,86,'Manasa',1900,'en',NULL,NULL,1,4,NULL),
  (93,87,'India from Midnight to Milennium',1900,'en',NULL,NULL,1,4,NULL),
  (94,87,'The Great Indian Novel',1900,'en',NULL,NULL,1,4,NULL),
  (95,89,'O Jerusalem!',1900,'en',NULL,NULL,1,4,NULL),
  (96,89,'The City of Joy',1900,'en',NULL,NULL,1,4,NULL),
  (97,89,'Freedom at Midnight',1900,'en',NULL,NULL,1,4,NULL),
  (98,47,'The Winter of Our Discontent',1900,'en',NULL,NULL,1,4,NULL),
  (99,93,'On Education',1900,'en',NULL,NULL,1,4,NULL),
  (100,94,'Free Will',1900,'en',NULL,NULL,1,4,NULL),
  (101,87,'Bookless in Baghdad',1900,'en',NULL,NULL,1,4,NULL),
  (102,96,'The Case of the Lame Canary',1900,'en',NULL,NULL,1,4,NULL),
  (103,9,'The Theory of Everything',1900,'en',NULL,NULL,1,4,NULL),
  (104,98,'New Markets & Other Essays',1900,'en',NULL,NULL,1,4,NULL),
  (105,99,'Electric Universe',1900,'en',NULL,NULL,1,4,NULL),
  (106,100,'The Hunchback of Notre Dame',1900,'en',NULL,NULL,1,4,NULL),
  (107,47,'Burning Bright',1900,'en',NULL,NULL,1,4,NULL),
  (108,98,'The Age of Discontuinity',1900,'en',NULL,NULL,1,4,NULL),
  (109,103,'Doctor in the Nude',1900,'en',NULL,NULL,1,4,NULL),
  (110,104,'Down and Out in Paris & London',1900,'en',NULL,NULL,1,4,NULL),
  (111,72,'Identity & Violence',1900,'en',NULL,NULL,1,4,NULL),
  (112,80,'Beyond the Three Seas',1900,'en',NULL,NULL,1,4,NULL),
  (113,107,'Talking Straight',1900,'en',NULL,NULL,1,4,NULL),
  (114,108,'Vol 3 Maugham\'s Collected Short Stories',1900,'en',NULL,NULL,1,4,NULL),
  (115,42,'The Phantom of Manhattan',1900,'en',NULL,NULL,1,4,NULL),
  (116,108,'Ashenden of The British Agent',1900,'en',NULL,NULL,1,4,NULL),
  (117,111,'Zen & The Art of Motorcycle Maintenance',1900,'en',NULL,NULL,1,4,NULL),
  (118,112,'The Great War for Civilization',1900,'en',NULL,NULL,1,4,NULL),
  (119,45,'We the Living',1900,'en',NULL,NULL,1,4,NULL),
  (120,114,'The Artist and the Mathematician',1900,'en',NULL,NULL,1,4,NULL),
  (121,93,'History of Western Philosophy',1900,'en',NULL,NULL,1,4,NULL),
  (122,72,'Rationality & Freedom',1900,'en',NULL,NULL,1,4,NULL),
  (123,117,'Clash of Civilizations and Remaking of the World Order',1900,'en',NULL,NULL,1,4,NULL),
  (124,39,'Uncommon Wisdom',1900,'en',NULL,NULL,1,4,NULL),
  (125,119,'One',1900,'en',NULL,NULL,1,4,NULL),
  (126,120,'To Sir With Love',1900,'en',NULL,NULL,1,4,NULL),
  (127,121,'Half A Life',1900,'en',NULL,NULL,1,4,NULL),
  (128,122,'The Discovery of India',1900,'en',NULL,NULL,1,4,NULL),
  (129,52,'Apulki',1900,'en',NULL,NULL,1,4,NULL),
  (130,93,'Unpopular Essays',1900,'en',NULL,NULL,1,4,NULL),
  (131,42,'The Deceiver',1900,'en',NULL,NULL,1,4,NULL),
  (132,76,'Veil: Secret Wars of the CIA',1900,'en',NULL,NULL,1,4,NULL),
  (133,52,'Char Shabda',1900,'en',NULL,NULL,1,4,NULL),
  (134,128,'Rosy is My Relative',1900,'en',NULL,NULL,1,4,NULL),
  (135,108,'The Moon and Sixpence',1900,'en',NULL,NULL,1,4,NULL),
  (136,130,'A Short History of the World',1900,'en',NULL,NULL,1,4,NULL),
  (137,108,'The Trembling of a Leaf',1900,'en',NULL,NULL,1,4,NULL),
  (138,103,'Doctor on the Brain',1900,'en',NULL,NULL,1,4,NULL),
  (139,133,'Simpsons & Their Mathematical Secrets',1900,'en',NULL,NULL,1,4,NULL),
  (140,134,'Pattern Classification',1900,'en',NULL,NULL,1,4,NULL),
  (141,135,'From Beirut to Jerusalem',1900,'en',NULL,NULL,1,4,NULL),
  (142,133,'The Code Book',1900,'en',NULL,NULL,1,4,NULL),
  (143,112,'The Age of the Warrior',1900,'en',NULL,NULL,1,4,NULL),
  (144,138,'The Numbers Behind Numb3rs',1900,'en',NULL,NULL,1,4,NULL),
  (145,47,'A Life in Letters',1900,'en',NULL,NULL,1,4,NULL),
  (146,140,'The Information',1900,'en',NULL,NULL,1,4,NULL),
  (147,141,'Elements of Information Theory',1900,'en',NULL,NULL,1,4,NULL),
  (148,142,'Power Electronics - Rashid',1900,'en',NULL,NULL,1,4,NULL),
  (149,143,'Power Electronics - Mohan',1900,'en',NULL,NULL,1,4,NULL),
  (150,144,'Neural Networks',1900,'en',NULL,NULL,1,4,NULL),
  (151,47,'The Grapes of Wrath',1900,'en',NULL,NULL,1,4,NULL),
  (152,52,'Vyakti ani Valli',1900,'en',NULL,NULL,1,4,NULL),
  (153,12,'Statistical Learning Theory',1900,'en',NULL,NULL,1,4,NULL),
  (154,148,'Empire of the Mughal - The Tainted Throne',1900,'en',NULL,NULL,1,4,NULL),
  (155,148,'Empire of the Mughal - Brothers at War',1900,'en',NULL,NULL,1,4,NULL),
  (156,148,'Empire of the Mughal - Ruler of the World',1900,'en',NULL,NULL,1,4,NULL),
  (157,148,'Empire of the Mughal - The Serpent\'s Tooth',1900,'en',NULL,NULL,1,4,NULL),
  (158,148,'Empire of the Mughal - Raiders from the North',1900,'en',NULL,NULL,1,4,NULL),
  (159,153,'Mossad',1900,'en',NULL,NULL,1,4,NULL),
  (160,154,'Jim Corbett Omnibus',1900,'en',NULL,NULL,1,4,NULL),
  (161,155,'20000 Leagues Under the Sea',1900,'en',NULL,NULL,1,4,NULL),
  (162,156,'Batatyachi Chal',1900,'en',NULL,NULL,1,4,NULL),
  (163,156,'Hafasavnuk',1900,'en',NULL,NULL,1,4,NULL),
  (164,156,'Urlasurla',1900,'en',NULL,NULL,1,4,NULL),
  (165,68,'Pointers in C',1900,'en',NULL,NULL,1,4,NULL),
  (166,160,'The Cathedral and the Bazaar',1900,'en',NULL,NULL,1,4,NULL),
  (167,161,'Design with OpAmps',1900,'en',NULL,NULL,1,4,NULL),
  (168,162,'Think Complexity',1900,'en',NULL,NULL,1,4,NULL),
  (169,163,'The Devil\'s Advocate',1900,'en',NULL,NULL,1,4,NULL),
  (170,45,'Ayn Rand Answers',1900,'en',NULL,NULL,1,4,NULL),
  (171,45,'Philosophy: Who Needs It',1900,'en',NULL,NULL,1,4,NULL),
  (172,166,'Data Analysis with Open Source Tools',1900,'en',NULL,NULL,1,4,NULL),
  (173,167,'Broca\'s Brain',1900,'en',NULL,NULL,1,4,NULL),
  (174,168,'Men of Mathematics',1900,'en',NULL,NULL,1,4,NULL),
  (175,169,'Oxford book of Modern Science Writing',1900,'en',NULL,NULL,1,4,NULL),
  (176,170,'Judiciary and Democracy Justice',1900,'en',NULL,NULL,1,4,NULL),
  (177,171,'The Arthashastra',1900,'en',NULL,NULL,1,4,NULL),
  (178,172,'We the People',1900,'en',NULL,NULL,1,4,NULL),
  (179,172,'We the Nation',1900,'en',NULL,NULL,1,4,NULL),
  (180,174,'The Courtroom Genius',1900,'en',NULL,NULL,1,4,NULL),
  (181,175,'Dongri to Dubai',1900,'en',NULL,NULL,1,4,NULL),
  (182,176,'Foundation History of England',1900,'en',NULL,NULL,1,4,NULL),
  (183,80,'City of Djinns',1900,'en',NULL,NULL,1,4,NULL),
  (184,178,'India\'s Legal System',1900,'en',NULL,NULL,1,4,NULL),
  (185,179,'More Tears to Cry',1900,'en',NULL,NULL,1,4,NULL),
  (186,180,'The Ropemaker',1900,'en',NULL,NULL,1,4,NULL),
  (188,182,'The Prince',1900,'en',NULL,NULL,1,4,NULL),
  (189,183,'Eyeless in Gaza',1900,'en',NULL,NULL,1,4,NULL),
  (190,184,'Tales of Beedle the Bard',1900,'en',NULL,NULL,1,4,NULL),
  (191,185,'Girl with the Dragon Tattoo',1900,'en',NULL,NULL,1,4,NULL),
  (192,185,'Girl who kicked the Hornet\'s Nest',1900,'en',NULL,NULL,1,4,NULL),
  (193,185,'Girl who played with Fire',1900,'en',NULL,NULL,1,4,NULL),
  (194,28,'Structure and Randomness',1900,'en',NULL,NULL,1,4,NULL),
  (195,189,'Image Processing with MATLAB',1900,'en',NULL,NULL,1,4,NULL),
  (196,104,'Animal Farm',1900,'en',NULL,NULL,1,4,NULL),
  (197,70,'The Idiot',1900,'en',NULL,NULL,1,4,NULL),
  (198,192,'A Christmas Carol',1900,'en',NULL,NULL,1,4,NULL);

  CREATE TABLE `clients` (
    `client_id` int(10) unsigned NOT NULL AUTO_INCREMENT,
    `name` varchar(50) DEFAULT NULL,
    `email` varchar(100) NOT NULL,
    `birthdate` date DEFAULT NULL,
    `gender` enum('M','F') DEFAULT NULL,
    `active` tinyint(1) NOT NULL DEFAULT '1',
    `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (`client_id`),
    UNIQUE KEY `email` (`email`)
  ) ENGINE=InnoDB AUTO_INCREMENT=101 DEFAULT CHARSET=utf8;

  INSERT INTO `clients` VALUES (1,'Maria Dolores Gomez','Maria Dolores.95983222J@random.names','1971-06-06','F',1,'2018-04-09 16:51:30'),
  (2,'Adrian Fernandez','Adrian.55818851J@random.names','1970-04-09','M',1,'2018-04-09 16:51:30'),
  (3,'Maria Luisa Marin','Maria Luisa.83726282A@random.names','1957-07-30','F',1,'2018-04-09 16:51:30'),
  (4,'Pedro Sanchez','Pedro.78522059J@random.names','1992-01-31','M',1,'2018-04-09 16:51:30'),
  (5,'Pablo Saavedra','Pablo.93733268B@random.names','1960-07-21','M',1,'2018-04-09 16:51:30'),
  (6,'Marta Carrillo','Marta.55741882W@random.names','1981-06-15','F',1,'2018-04-09 16:51:30'),
  (7,'Javier Barrio','Javier.54966248C@random.names','1971-04-24','M',1,'2018-04-09 16:51:30'),
  (8,'Milagros Garcia','Milagros.90074144A@random.names','1964-12-05','F',1,'2018-04-09 16:51:30'),
  (9,'Carlos Quiros','Carlos.63291957M@random.names','1954-08-28','M',1,'2018-04-09 16:51:30'),
  (10,'Carmen De la Torre','Carmen.57408761W@random.names','1966-05-14','F',1,'2018-04-09 16:51:30'),
  (11,'Laura Moron','Laura.57774589S@random.names','1954-03-02','F',1,'2018-04-09 16:51:30'),
  (12,'Maria Dolores Larrea','Maria Dolores.71788005Z@random.names','1990-09-04','F',1,'2018-04-09 16:51:30'),
  (13,'Maria Dolores Sanz','Maria Dolores.30948169J@random.names','2001-08-30','F',1,'2018-04-09 16:51:30'),
  (14,'Jose Maria Bermudez','Jose Maria.24963969E@random.names','1998-05-23','M',1,'2018-04-09 16:51:30'),
  (15,'Carlos Blanco','Carlos.94783133H@random.names','1952-08-07','M',1,'2018-04-09 16:51:30'),
  (16,'Juan Carlos Jurado','Juan Carlos.71650477A@random.names','1990-12-12','M',1,'2018-04-09 16:51:30'),
  (17,'David Gonzalez','David.54332034P@random.names','1976-05-03','M',1,'2018-04-09 16:51:30'),
  (18,'Antonia Aranda','Antonia.91560262E@random.names','1979-10-25','F',1,'2018-04-09 16:51:30'),
  (19,'Maria Moreno','Maria.58935447V@random.names','1997-01-12','F',1,'2018-04-09 16:51:30'),
  (20,'David Casals','David.06746883V@random.names','1999-07-13','M',1,'2018-04-09 16:51:30'),
  (21,'Mario Romero','Mario.46091382A@random.names','1985-03-29','M',1,'2018-04-09 16:51:30'),
  (22,'Maria angeles Alba','Maria angeles.91808919A@random.names','1959-09-14','F',1,'2018-04-09 16:51:30'),
  (23,'Rafael Espinola','Rafael.67605541P@random.names','1998-10-11','M',1,'2018-04-09 16:51:30'),
  (24,'Montserrat alvarez','Montserrat.31114289G@random.names','1994-11-06','F',1,'2018-04-09 16:51:30'),
  (25,'Maria Carmen Gomez','Maria Carmen.64351051H@random.names','1968-06-30','F',1,'2018-04-09 16:51:30'),
  (26,'Maria Cruz Morillas','Maria Cruz.81385695B@random.names','1978-10-29','F',1,'2018-04-09 16:51:30'),
  (27,'Josefa Roldan','Josefa.51417560W@random.names','1993-08-09','F',1,'2018-04-09 16:51:30'),
  (28,'Monica Pla','Monica.18992324M@random.names','1972-06-08','F',1,'2018-04-09 16:51:30'),
  (29,'Juana Maria Lopez','Juana Maria.51072683X@random.names','1990-07-15','F',1,'2018-04-09 16:51:30'),
  (30,'Maria Carmen Ponce','Maria Carmen.41619980P@random.names','1984-07-26','F',1,'2018-04-09 16:51:30'),
  (31,'Juan Carlos Rios','Juan Carlos.45673504N@random.names','1967-05-04','M',1,'2018-04-09 16:51:30'),
  (32,'Isabel Alfaro','Isabel.77316882J@random.names','1980-07-25','F',1,'2018-04-09 16:51:30'),
  (33,'Maria Isabel Armas','Maria Isabel.42010289F@random.names','1950-11-21','F',1,'2018-04-09 16:51:30'),
  (34,'Maria Teresa Castillo','Maria Teresa.91228389Q@random.names','2002-11-08','F',1,'2018-04-09 16:51:30'),
  (35,'Andres Planells','Andres.09981449R@random.names','1992-06-19','M',1,'2018-04-09 16:51:30'),
  (36,'Silvia Perez','Silvia.91812407H@random.names','1969-02-15','F',1,'2018-04-09 16:51:30'),
  (37,'Pablo Gonzalez','Pablo.11605676Z@random.names','2000-10-11','M',1,'2018-04-09 16:51:30'),
  (38,'Maria Antonia Jimenez','Maria Antonia.98071058R@random.names','1998-06-23','F',1,'2018-04-09 16:51:31'),
  (39,'Jesus Rodriguez','Jesus.86679475L@random.names','1961-01-17','M',1,'2018-04-09 16:51:31'),
  (40,'Carmen Rodriguez','Carmen.81799536J@random.names','1973-02-17','F',1,'2018-04-09 16:51:31'),
  (41,'Maria Dolores Rodriguez','Maria Dolores.75444599E@random.names','1962-08-14','F',1,'2018-04-09 16:51:31'),
  (42,'Jordi Campos','Jordi.76000917Q@random.names','1956-09-23','M',1,'2018-04-09 16:51:31'),
  (43,'Carlos Caceres','Carlos.97628163V@random.names','1993-05-16','M',1,'2018-04-09 16:51:31'),
  (44,'Carmen Robles','Carmen.29258188A@random.names','1955-06-19','F',1,'2018-04-09 16:51:31'),
  (45,'Sara Rodriguez','Sara.16181250Z@random.names','2001-06-07','F',1,'2018-04-09 16:51:31'),
  (46,'Maria Carmen Rivera','Maria Carmen.59955426S@random.names','2000-05-27','F',1,'2018-04-09 16:51:31'),
  (47,'Alberto Cabanas','Alberto.40633755T@random.names','1991-10-27','M',1,'2018-04-09 16:51:31'),
  (48,'Jose Sanchez','Jose.52243847Z@random.names','1976-12-06','M',1,'2018-04-09 16:51:31'),
  (49,'Isabel Martinez','Isabel.90843261T@random.names','1962-07-01','F',1,'2018-04-09 16:51:31'),
  (50,'David Sanchez','David.14910073R@random.names','1975-05-18','M',1,'2018-04-09 16:51:31'),
  (51,'Sergio Sebastian','Sergio.09345984A@random.names','1959-08-30','M',1,'2018-04-09 16:51:31'),
  (52,'Manuel Cabrera','Manuel.38738750B@random.names','1993-08-23','M',1,'2018-04-09 16:51:31'),
  (53,'Marina Gabaldon','Marina.12101665P@random.names','1959-03-25','F',1,'2018-04-09 16:51:31'),
  (54,'Rafael Galvez','Rafael.87947175M@random.names','1988-09-02','M',1,'2018-04-09 16:51:31'),
  (55,'Francisco Villar','Francisco.13922268T@random.names','1952-04-25','M',1,'2018-04-09 16:51:31'),
  (56,'Francisco Garcia','Francisco.34242509V@random.names','1989-01-22','M',1,'2018-04-09 16:51:31'),
  (57,'Esther Pina','Esther.36300729J@random.names','1977-11-07','F',1,'2018-04-09 16:51:31'),
  (58,'Maria Jesus Noya','Maria Jesus.95839533M@random.names','1996-08-07','F',1,'2018-04-09 16:51:31'),
  (59,'Paula Ropero','Paula.53630073F@random.names','1998-09-04','F',1,'2018-04-09 16:51:31'),
  (60,'Maria Carmen Iglesias','Maria Carmen.24044144J@random.names','1977-06-12','F',1,'2018-04-09 16:51:31'),
  (61,'Albert Galvez','Albert.10067957Y@random.names','1971-05-17','M',1,'2018-04-09 16:51:31'),
  (62,'Carmen Lopez','Carmen.09399409E@random.names','1987-03-07','F',1,'2018-04-09 16:51:31'),
  (63,'Francisco Jose Leon','Francisco Jose.07598657D@random.names','1965-12-11','M',1,'2018-04-09 16:51:31'),
  (64,'Francisca Gonzalez','Francisca.19675393C@random.names','1957-12-23','F',1,'2018-04-09 16:51:31'),
  (65,'Daniel Garcia','Daniel.01386486T@random.names','1979-05-29','M',1,'2018-04-09 16:51:31'),
  (66,'Ana Maria Martinez','Ana Maria.91340418N@random.names','1980-09-14','F',1,'2018-04-09 16:51:31'),
  (67,'Maria Aguilar','Maria.41749884P@random.names','2000-07-31','F',1,'2018-04-09 16:51:31'),
  (68,'oscar Penas','oscar.31681177B@random.names','1981-10-02','M',1,'2018-04-09 16:51:31'),
  (69,'Adrian Vela','Adrian.66561884E@random.names','1978-12-10','M',1,'2018-04-09 16:51:31'),
  (70,'Francisco Alcalde','Francisco.52899588W@random.names','1967-03-11','M',1,'2018-04-09 16:51:31'),
  (71,'Maria Dolores Perez','Maria Dolores.47800073R@random.names','2003-11-10','F',1,'2018-04-09 16:51:31'),
  (72,'Juan Jose Tejada','Juan Jose.30429668R@random.names','1990-06-15','M',1,'2018-04-09 16:51:31'),
  (73,'Cristobal Nogues','Cristobal.01001763K@random.names','2003-10-01','M',1,'2018-04-09 16:51:31'),
  (74,'Maria Luisa Sanchez','Maria Luisa.91748033K@random.names','2000-02-03','F',1,'2018-04-09 16:51:31'),
  (75,'Adrian Orta','Adrian.11458937S@random.names','1952-03-20','M',1,'2018-04-09 16:51:31'),
  (76,'Maria Pilar Martin','Maria Pilar.93607154Y@random.names','1996-08-29','F',1,'2018-04-09 16:51:31'),
  (77,'Jesus Perez','Jesus.91931655B@random.names','1954-06-01','M',1,'2018-04-09 16:51:31'),
  (78,'Jesus Perez','Jesus.15757299E@random.names','1956-08-29','M',1,'2018-04-09 16:51:31'),
  (79,'Esther Capdevila','Esther.96440550D@random.names','1970-10-12','F',1,'2018-04-09 16:51:31'),
  (80,'David Nieves','David.40697907M@random.names','1965-04-02','M',1,'2018-04-09 16:51:31'),
  (81,'Antonia Giron','Antonia.32080105G@random.names','1983-01-22','F',1,'2018-04-09 16:51:31'),
  (82,'Juan Casero','Juan.94063877H@random.names','1974-06-29','M',1,'2018-04-09 16:51:31'),
  (83,'Manuel De Pablo','Manuel.01279669H@random.names','1973-03-23','M',1,'2018-04-09 16:51:31'),
  (84,'angel Palomo','angel.28890315S@random.names','1991-07-04','M',1,'2018-04-09 16:51:31'),
  (85,'Laura Herrera','Laura.98555932N@random.names','1966-12-12','F',1,'2018-04-09 16:51:31'),
  (86,'Maria Josefa Benitez','Maria Josefa.36743977M@random.names','1987-04-17','F',1,'2018-04-09 16:51:31'),
  (87,'Luis Saez','Luis.08103734Y@random.names','1983-03-28','M',1,'2018-04-09 16:51:31'),
  (88,'Susana Nevado','Susana.09442372K@random.names','1961-12-26','F',1,'2018-04-09 16:51:31'),
  (89,'Miguel Gomez','Miguel.01631964E@random.names','1965-05-16','M',1,'2018-04-09 16:51:31'),
  (90,'Julio Mayordomo','Julio.77582185B@random.names','1968-06-05','M',1,'2018-04-09 16:51:31'),
  (91,'Sonia Mari','Sonia.06246888L@random.names','1994-10-13','F',1,'2018-04-09 16:51:31'),
  (92,'Antonia Beltran','Antonia.96371304Q@random.names','1967-11-17','F',1,'2018-04-09 16:51:31'),
  (93,'Mercedes Perez','Mercedes.80944345P@random.names','1958-11-05','F',1,'2018-04-09 16:51:31'),
  (94,'Concepcion Velez','Concepcion.56896097P@random.names','1964-04-05','F',1,'2018-04-09 16:51:31'),
  (95,'Diego Correa','Diego.44862413Q@random.names','1999-09-15','M',1,'2018-04-09 16:51:31'),
  (96,'Juan Antonio Galan','Juan Antonio.95710220K@random.names','1982-11-20','M',1,'2018-04-09 16:51:31'),
  (97,'Manuel Cerezo','Manuel.25853412D@random.names','1991-03-12','M',1,'2018-04-09 16:51:31'),
  (98,'Rosa Maria Singh','Rosa Maria.41642169W@random.names','1956-12-31','F',1,'2018-04-09 16:51:31'),
  (99,'angeles Mena','angeles.88859550Q@random.names','1987-09-22','F',1,'2018-04-09 16:51:31'),
  (100,'Jose Hidalgo','Jose.05903641R@random.names','1973-08-13','M',1,'2018-04-09 16:51:31');

  CREATE TABLE `transactions` (
    `transaction_id` int(10) unsigned NOT NULL AUTO_INCREMENT,
    `book_id` int(10) unsigned NOT NULL,
    `client_id` int(10) unsigned NOT NULL,
    `type` enum('lend','sell') NOT NULL,
    `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
    `modified_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    `finished` tinyint(1) NOT NULL DEFAULT '0',
    PRIMARY KEY (`transaction_id`)
  ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```











