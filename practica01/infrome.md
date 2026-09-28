# Registro Completo de Ejecución - Práctica 1 (PostgreSQL)

## Sección  1: Arranque del servicio y creación de la base de datos


```bash
usuario@ubuntu:~$ sudo -u postgres psql

postgres=# CREATE DATABASE biblioteca;
CREATE DATABASE

postgres-# \c biblioteca 
You are now connected to database "biblioteca" as user "postgres".
```
## Sección  2: Creación de usuarios

### Usuarios:
```bash
biblioteca=# CREATE USER admin_biblio WITH PASSWORD 'password_admin';
CREATE ROLE

biblioteca=# ALTER DATABASE biblioteca OWNER TO admin_biblio;
ALTER DATABASE

biblioteca=# CREATE USER usuario_biblio WITH PASSWORD 'password_usuario';
CREATE ROLE

```
### Roles y privilegios:
```bash
biblioteca=# CREATE ROLE lectores;
CREATE ROLE

biblioteca=# GRANT CONNECT ON DATABASE biblioteca TO lectores;
GRANT

biblioteca=# GRANT USAGE ON SCHEMA public TO lectores;
GRANT

biblioteca=# GRANT SELECT ON ALL TABLES IN SCHEMA public TO lectores;
GRANT

biblioteca=# ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO lectores;
ALTER DEFAULT PRIVILEGES

biblioteca=# GRANT lectores TO usuario_biblio;
GRANT ROLE

biblioteca=# SELECT rolname, rolsuper, rolcreaterole, rolcanlogin 
FROM pg_roles 
WHERE rolname IN ('admin_biblio', 'usuario_biblio', 'lectores');
    rolname     | rolsuper | rolcreaterole | rolcanlogin 
----------------+----------+---------------+-------------
 admin_biblio   | f        | f             | t
 usuario_biblio | f        | f             | t
 lectores       | f        | f             | f
(3 rows)

biblioteca=# ALTER USER usuario_biblio WITH PASSWORD 'nueva_password_usuario';
ALTER ROLE

biblioteca=# REVOKE DELETE ON ALL TABLES IN SCHEMA public FROM usuario_biblio;
REVOKE

biblioteca=# REVOKE DELETE ON ALL TABLES IN SCHEMA public FROM lectores;
REVOKE
```
## Sección  3: Creación tablas, datos, consultas y exportación
```bash
biblioteca=# CREATE TABLE autores (
    id_autor SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    nacionalidad VARCHAR(50)
);
CREATE TABLE

biblioteca=# CREATE TABLE libros (
    id_libro SERIAL PRIMARY KEY,
    titulo VARCHAR(150) NOT NULL,
    anio_publicacion INT,
    id_autor INT,
    CONSTRAINT fk_autor FOREIGN KEY (id_autor) 
        REFERENCES autores(id_autor) 
        ON DELETE CASCADE
);
CREATE TABLE

biblioteca=# CREATE TABLE prestamos (
    id_prestamo SERIAL PRIMARY KEY,
    id_libro INT NOT NULL,
    fecha_prestamo DATE NOT NULL DEFAULT CURRENT_DATE,
    fecha_devolucion DATE,
    usuario_prestatario VARCHAR(100) NOT NULL,
    CONSTRAINT fk_libro FOREIGN KEY (id_libro) 
        REFERENCES libros(id_libro) 
        ON DELETE CASCADE
);
CREATE TABLE

biblioteca=# INSERT INTO autores (nombre, nacionalidad) VALUES
('Gabriel García Márquez', 'Colombiana'),
('Isabel Allende', 'Chilena'),
('Miguel de Cervantes', 'Española'),
('George Orwell', 'Británica'),
('J.K. Rowling', 'Británica');
INSERT 0 5

biblioteca=# INSERT INTO libros (titulo, anio_publicacion, id_autor) VALUES
('Cien años de soledad', 1967, 1),
('El amor en los tiempos del cólera', 1985, 1),
('La casa de los espíritus', 1982, 2),
('Don Quijote de la Mancha', 1605, 3),
('1984', 1949, 4),
('Rebelión en la granja', 1945, 4),
('Harry Potter y la piedra filosofal', 1997, 5),
('Harry Potter y la cámara secreta', 1998, 5);
INSERT 0 8

biblioteca=# INSERT INTO prestamos (id_libro, fecha_prestamo, fecha_devolucion, usuario_prestatario) VALUES
(1, '2026-09-01', '2026-09-15', 'Ana López'),
(2, '2026-09-05', NULL, 'Carlos Gómez'),
(5, '2026-09-10', '2026-09-20', 'Ana López'),
(5, '2026-09-21', NULL, 'María Rodríguez'),
(7, '2026-09-15', NULL, 'Carlos Gómez');
INSERT 0 5

```
## Sección  4: consultas y exportación

### Consulta 1:
```bash
biblioteca=# SELECT l.titulo, l.anio_publicacion, a.nombre AS autor
FROM libros l
JOIN autores a ON l.id_autor = a.id_autor;
               titulo               | anio_publicacion |         autor          
------------------------------------+------------------+------------------------
 Cien años de soledad               |             1967 | Gabriel García Márquez
 El amor en los tiempos del cólera  |             1985 | Gabriel García Márquez
 La casa de los espíritus           |             1982 | Isabel Allende
 Don Quijote de la Mancha           |             1605 | Miguel de Cervantes
 1984                               |             1949 | George Orwell
 Rebelión en la granja              |             1945 | George Orwell
 Harry Potter y la piedra filosofal |             1997 | J.K. Rowling
 Harry Potter y la cámara secreta   |             1998 | J.K. Rowling
(8 rows)
```
### Consulta 2:
```bash
biblioteca=# SELECT * 
FROM prestamos 
WHERE fecha_devolucion IS NULL;
 id_prestamo | id_libro | fecha_prestamo | fecha_devolucion | usuario_prestatario 
-------------+----------+----------------+------------------+---------------------
           2 |        2 | 2026-09-05     |                  | Carlos Gómez
           4 |        5 | 2026-09-21     |                  | María Rodríguez
           5 |        7 | 2026-09-15     |                  | Carlos Gómez
(3 rows)
```
### Consulta 3:
```bash
biblioteca=# SELECT a.nombre, COUNT(l.id_libro) AS total_libros
FROM autores a
JOIN libros l ON a.id_autor = l.id_autor
GROUP BY a.id_autor, a.nombre
HAVING COUNT(l.id_libro) > 1;
        nombre         | total_libros 
------------------------+--------------
 J.K. Rowling           |            2
 George Orwell          |            2
 Gabriel García Márquez |            2
(3 rows)
```
### Consulta 4:
```bash
biblioteca=# SELECT COUNT(*) AS total_prestamos 
FROM prestamos;
 total_prestamos 
-----------------
               5
(1 row)
```
### Consulta 5:
```bash
biblioteca=# SELECT usuario_prestatario, COUNT(*) AS libros_prestados
FROM prestamos
GROUP BY usuario_prestatario;
 usuario_prestatario | libros_prestados 
---------------------+------------------
 Carlos Gómez        |                2
 María Rodríguez     |                1
 Ana López           |                2
(3 rows)
```
### Update 1:
```bash
biblioteca=# UPDATE prestamos
SET fecha_devolucion = '2026-09-28'
WHERE id_prestamo = 2;
UPDATE 1

```
### Delete y confirmación:
```bash
biblioteca=# DELETE FROM libros WHERE id_libro = 7;
DELETE 1

biblioteca=# SELECT * FROM prestamos WHERE id_libro = 7;
 id_prestamo | id_libro | fecha_prestamo | fecha_devolucion | usuario_prestatario 
-------------+----------+----------------+------------------+---------------------
(0 rows)

```
### Vista 1:
```bash
biblioteca=# CREATE VIEW vista_libros_prestados AS
SELECT l.titulo, a.nombre AS autor, p.usuario_prestatario
FROM prestamos p
JOIN libros l ON p.id_libro = l.id_libro
JOIN autores a ON l.id_autor = a.id_autor;
CREATE VIEW

biblioteca=# GRANT SELECT ON vista_libros_prestados TO usuario_biblio;
GRANT
```
### Funcion 1:
```bash
biblioteca=# CREATE OR REPLACE FUNCTION obtener_libros_por_autor(p_nombre_autor VARCHAR)
RETURNS TABLE(id_libro INT, titulo VARCHAR, anio_publicacion INT) AS $$ BEGIN     RETURN QUERY     SELECT l.id_libro, l.titulo, l.anio_publicacion     FROM libros l     JOIN autores a ON l.id_autor = a.id_autor     WHERE a.nombre ILIKE '\%' \vert{}\vert{} p_nombre_autor \vert{}\vert{} '\%'; END; $$ LANGUAGE plpgsql;
CREATE FUNCTION

biblioteca=# SELECT * FROM obtener_libros_por_autor('George Orwell');
 id_libro |        titulo         | anio_publicacion 
----------+-----------------------+------------------
        5 | 1984                  |             1949
        6 | Rebelión en la granja |             1945
(2 rows)
```
### Consulta 6:
```bash
biblioteca=# SELECT l.titulo, COUNT(p.id_prestamo) AS veces_prestado
FROM libros l
JOIN prestamos p ON l.id_libro = p.id_libro
GROUP BY l.id_libro, l.titulo
ORDER BY veces_prestado DESC
LIMIT 3;
              titulo               | veces_prestado 
-----------------------------------+----------------
 1984                              |              2
 El amor en los tiempos del cólera |              1
 Cien años de soledad              |              1
(3 rows)


```
### Exportación he importación:
```bash
biblioteca=# \copy libros TO '~/libros.csv' WITH (FORMAT csv, HEADER true, DELIMITER ',');
COPY 7

biblioteca=# \copy autores(nombre, nacionalidad) FROM '~/autores_nuevos.csv' WITH (FORMAT csv, HEADER true, DELIMITER ',');

