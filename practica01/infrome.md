Registro de Ejecución - Práctica 1 (PostgreSQL)A continuación se detalla el registro completo de la ejecución de la práctica, extraído directamente del script de la terminal.   1. Sesión de arranque y creación de base de datosInicio del script: 28 de septiembre de 2026 a las 09:27:11   Arranque del servicio: Se inició PostgreSQL correctamente con el comando sudo service postgresql start.   Intentos de conexión: Se intentó acceder con psql -u admin (generando un error de opción inválida) y se consultó la ayuda con psql --help. Finalmente, se accedió con éxito mediante sudo -u postgres psql.   Creación de BD: Dentro de psql, se ejecutó CREATE DATABASE biblioteca; obteniendo una respuesta exitosa (CREATE DATABASE).   Esta primera sesión finalizó a las 09:31:22.   2. Conexión y gestión de usuarios y rolesInicio de la segunda sesión: 09:35:19   Tras algunos errores tipográficos iniciales en la terminal (psql -d bib lioteca, psql biblioteca;, l, list), se logró conectar a la base de datos con el comando \c biblioteca.   Se procedió con la creación y asignación de permisos de los usuarios:   SQLCREATE USER admin_biblio WITH PASSWORD 'password_admin';
ALTER DATABASE biblioteca OWNER TO admin_biblio;
CREATE USER usuario_biblio WITH PASSWORD 'password_usuario';

CREATE ROLE lectores;
GRANT CONNECT ON DATABASE biblioteca TO lectores;
GRANT USAGE ON SCHEMA public TO lectores;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO lectores;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO lectores;

GRANT lectores TO usuario_biblio;
Todas las instrucciones anteriores devolvieron sus respectivos mensajes de éxito (CREATE ROLE, ALTER DATABASE, GRANT, etc.).   Comprobación de roles creados:   SQLSELECT rolname, rolsuper, rolcreaterole, rolcanlogin 
FROM pg_roles 
WHERE rolname IN ('admin_biblio', 'usuario_biblio', 'lectores');
Salida:   Plaintext    rolname     | rolsuper | rolcreaterole | rolcanlogin 
----------------+----------+---------------+-------------
 admin_biblio   | f        | f             | t
 usuario_biblio | f        | f             | t
 lectores       | f        | f             | f
(3 rows)
Modificación de credenciales y permisos:   SQLALTER USER usuario_biblio WITH PASSWORD 'nueva_password_usuario';
REVOKE DELETE ON ALL TABLES IN SCHEMA public FROM usuario_biblio;
REVOKE DELETE ON ALL TABLES IN SCHEMA public FROM lectores;
3. Creación de tablas e inserción de datosSe crearon las tres tablas principales con sus respectivas claves foráneas configuradas en ON DELETE CASCADE:   SQLCREATE TABLE autores (
    id_autor SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    nacionalidad VARCHAR(50)
);

CREATE TABLE libros (
    id_libro SERIAL PRIMARY KEY,
    titulo VARCHAR(150) NOT NULL,
    anio_publicacion INT,
    id_autor INT,
    CONSTRAINT fk_autor FOREIGN KEY (id_autor) 
        REFERENCES autores(id_autor) 
        ON DELETE CASCADE
);

CREATE TABLE prestamos (
    id_prestamo SERIAL PRIMARY KEY,
    id_libro INT NOT NULL,
    fecha_prestamo DATE NOT NULL DEFAULT CURRENT_DATE,
    fecha_devolucion DATE,
    usuario_prestatario VARCHAR(100) NOT NULL,
    CONSTRAINT fk_libro FOREIGN KEY (id_libro) 
        REFERENCES libros(id_libro) 
        ON DELETE CASCADE
);
Inserción de registros:   Se insertaron 5 autores exitosamente (INSERT 0 5).   Se insertaron 8 libros exitosamente (INSERT 0 8).   Se insertaron 5 préstamos exitosamente (INSERT 0 5).   4. Consultas y modificacionesListar libros con su autor:   SQLSELECT l.titulo, l.anio_publicacion, a.nombre AS autor
FROM libros l
JOIN autores a ON l.id_autor = a.id_autor;
(Devolvió correctamente los 8 libros asociados a sus autores).   Préstamos sin fecha de devolución:   SQLSELECT * FROM prestamos WHERE fecha_devolucion IS NULL;
(Devolvió 3 filas: los préstamos de los libros 2, 5 y 7).   Autores con más de un libro:   SQLSELECT a.nombre, COUNT(l.id_libro) AS total_libros
FROM autores a
JOIN libros l ON a.id_autor = l.id_autor
GROUP BY a.id_autor, a.nombre
HAVING COUNT(l.id_libro) > 1;
(Devolvió a J.K. Rowling, George Orwell y Gabriel García Márquez con 2 libros cada uno).   Estadísticas de préstamos:   El total de préstamos fue de 5.   La agrupación por usuario arrojó 2 para Carlos Gómez, 1 para María Rodríguez y 2 para Ana López.   Modificación de un préstamo y borrado en cascada:   SQLUPDATE prestamos SET fecha_devolucion = '2026-09-28' WHERE id_prestamo = 2;
-- Salida: UPDATE 1

DELETE FROM libros WHERE id_libro = 7;
-- Salida: DELETE 1

SELECT * FROM prestamos WHERE id_libro = 7;
-- Salida: (0 rows)
5. Vistas, Funciones y ExportaciónCreación de vista y permisos:   SQLCREATE VIEW vista_libros_prestados AS
SELECT l.titulo, a.nombre AS autor, p.usuario_prestatario
FROM prestamos p
JOIN libros l ON p.id_libro = l.id_libro
JOIN autores a ON l.id_autor = a.id_autor;

GRANT SELECT ON vista_libros_prestados TO usuario_biblio;
Creación de función de búsqueda:   SQLCREATE OR REPLACE FUNCTION obtener_libros_por_autor(p_nombre_autor VARCHAR)
RETURNS TABLE(id_libro INT, titulo VARCHAR, anio_publicacion INT) AS $$
BEGIN
    RETURN QUERY
    SELECT l.id_libro, l.titulo, l.anio_publicacion
    FROM libros l
    JOIN autores a ON l.id_autor = a.id_autor
    WHERE a.nombre ILIKE '%' || p_nombre_autor || '%';
END;
$$ LANGUAGE plpgsql;
Al ejecutar SELECT * FROM obtener_libros_por_autor('George Orwell');, devolvió los libros "1984" y "Rebelión en la granja".   Los 3 libros más prestados:   SQLSELECT l.titulo, COUNT(p.id_prestamo) AS veces_prestado
FROM libros l
JOIN prestamos p ON l.id_libro = p.id_libro
GROUP BY l.id_libro, l.titulo
ORDER BY veces_prestado DESC
LIMIT 3;
   (Devolvió "1984" con 2 préstamos, y "El amor en los tiempos del cólera" y "Cien años de soledad" con 1 préstamo cada uno).   Exportación e importación de archivos CSV:   La exportación de la tabla funcionó correctamente guardando 7 registros (COPY 7):   SQL\copy libros TO '~/libros.csv' WITH (FORMAT csv, HEADER true, DELIMITER ',');
La importación falló mostrando un error debido a que el archivo no existía en esa ruta:   SQL\copy autores(nombre, nacionalidad) FROM '~/autores_nuevos.csv' WITH (FORMAT csv, HEADER true, DELIMITER ',');
-- Salida de error: /var/lib/postgresql/autores_nuevos.csv: No such file or directory
Fin del script: 09:51:07   