Ejercicio 1

-- ============================================
-- PROYECTO: clothing_store (tienda de ropa)
-- Código practicado con Claude
-- ============================================

-- Si quieres empezar de cero (borra todo y permite re-ejecutar el script):
-- DROP DATABASE clothing_store;

-- 1. Crear la base de datos
CREATE DATABASE IF NOT EXISTS clothing_store;

-- 2. Seleccionar la base de datos
USE clothing_store;

-- 3. Tabla de registro de clientes
CREATE TABLE clients (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    registration_date DATE NOT NULL
);

-- 4. Tablas de items
--    ENUM: solo acepta los valores de la lista
CREATE TABLE shirts (
    id INT AUTO_INCREMENT PRIMARY KEY,
    size ENUM('S','M','L','XL'),
    cost DECIMAL(5,2) NOT NULL
);

CREATE TABLE pants (
    id INT AUTO_INCREMENT PRIMARY KEY,
    size ENUM('S','M','L','XL'),
    cost DECIMAL(5,2) NOT NULL
);

-- 5. Marcas disponibles
CREATE TABLE brand (
    id INT AUTO_INCREMENT PRIMARY KEY,
    mark VARCHAR(50) NOT NULL
);

-- 6. Insertar los datos
INSERT INTO clients (name, registration_date) VALUES
('Luis Colon', '2023-04-20'),
('Carlos Gozalez', '2025-05-10'),
('Mimi Flores', '2025-02-22');

INSERT INTO shirts (size, cost) VALUES
('S', 24.99),
('M', 35.00),
('L', 59.99),
('XL', 74.99);

INSERT INTO pants (size, cost) VALUES
('S', 54.99),
('M', 65.00),
('L', 89.99),
('XL', 94.99);

INSERT INTO brand (mark) VALUES
('Polo Ralph Lauren'),
('Lacoste'),
('Hugo Boos'),
('Zara');

-- 7. Consultar los datos
SELECT * FROM clients;
SELECT * FROM shirts;
SELECT * FROM pants;
SELECT * FROM brand;

-- ============================================
-- LECCIONES DE ESTE PROYECTO
-- ============================================
-- Las fechas deben existir: '2025-02-30' da error (febrero no tiene 30).
-- Si un INSERT de varias filas tiene UN error, NINGUNA fila se inserta.
-- Si el script queda a medias, lo más limpio: DROP DATABASE y re-ejecutar todo.
-- Mejora pendiente (idea): unir shirts y pants en una sola tabla items
--   con columna type ENUM('shirt','pants'), y conectar brand con brand_id
--   + FOREIGN KEY como en sports_shop.


Ejercicio 2

-- ============================================
-- PROYECTO: sistem_movies
-- Código practicado con Claude
-- ============================================

-- 1. Crear la base de datos y seleccionarla
CREATE DATABASE IF NOT EXISTS sistem_movies;
USE sistem_movies;

-- 2. Tabla de usuarios
CREATE TABLE usuarios (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL
);

-- 3. Tabla de películas
CREATE TABLE movies (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(100) NOT NULL,
    year YEAR NOT NULL
);

-- 4. Insertar datos
INSERT INTO usuarios (nombre, email) VALUES
('Carlos', 'carlos@gmail.com'),
('Maria', 'maria@gmail.com'),
('Juan', 'juan@gmail.com'),
('Gerrardo', 'gerrardo@gmail.com');

INSERT INTO movies (title, year) VALUES
('Inception', 2010),
('Titanic', 1997);

-- 5. Ver los datos
SELECT * FROM usuarios;
SELECT * FROM movies;

-- 6. Borrar un duplicado (cambia el id por el de la fila repetida)
-- DELETE FROM usuarios WHERE id = 5;

-- 7. Evitar emails duplicados en el futuro
-- ALTER TABLE usuarios ADD UNIQUE (email);

-- ============================================
-- CONCEPTOS APRENDIDOS
-- ============================================
-- VARCHAR(n): texto de longitud VARIABLE, n = máximo de caracteres.
--   Guarda solo lo que uses ("Ana" en VARCHAR(50) ocupa 3).
-- CHAR(n): longitud FIJA, siempre ocupa n (rellena con espacios).
-- Ambos tienen límite: más de n caracteres = error o truncado.
-- INT: enteros (sin decimales; 19.99 se redondea a 20).
-- DECIMAL(10,2): números con decimales, ideal para dinero.
-- DATE: fechas en formato 'AAAA-MM-DD'.
-- NOT NULL: la columna es obligatoria en todo INSERT.
-- AUTO_INCREMENT: el id se genera solo (1, 2, 3...).
-- PRIMARY KEY: identificador único de cada fila.

Ejercico 3

-- ============================================
-- PROYECTO: sports_shop (tienda de deporte)
-- Código practicado con Claude
-- ============================================

-- 1. Crear la base de datos y seleccionarla
CREATE DATABASE IF NOT EXISTS sports_shop;
USE sports_shop;

-- 2. Tabla de equipos deportivos
--    (versión final, ya con el nombre correcto)
CREATE TABLE equips (
    id INT AUTO_INCREMENT PRIMARY KEY,
    type VARCHAR(50) NOT NULL,      -- texto de hasta 50 caracteres
    year YEAR NOT NULL,             -- año
    cost DECIMAL(5,2) NOT NULL      -- hasta 999.99 (5 dígitos, 2 decimales)
);

-- 3. Tabla de clientes
CREATE TABLE client (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    registration_date DATE          -- sin NOT NULL = opcional (puede quedar NULL)
);

-- 4. Tabla de marcas
CREATE TABLE brand (
    id INT AUTO_INCREMENT PRIMARY KEY,
    mark VARCHAR(50) NOT NULL
);

-- 5. Conectar equips con brand (clave foránea)
--    Cada equipo guarda el id de su marca
ALTER TABLE equips ADD brand_id INT,
ADD FOREIGN KEY (brand_id) REFERENCES brand(id);

-- 6. Insertar datos
-- Las fechas siempre en formato 'AAAA-MM-DD'
INSERT INTO client (name, registration_date) VALUES
('Pedro Matos', '2025-05-23'),
('Carlos Vidal', '2025-03-08'),
('Luis Diaz', '2026-12-30');

INSERT INTO equips (type, year, cost) VALUES
('Basketball', 2014, 59.99),
('Baseball', 2025, 99.99),
('Football', 2026, 299.99);

INSERT INTO brand (mark) VALUES
('Nike'),
('Puma'),
('Adidas');

-- 7. Asignar marca a cada equipo
UPDATE equips SET brand_id = 1 WHERE id = 1;  -- Basketball -> Nike
UPDATE equips SET brand_id = 2 WHERE id = 2;  -- Baseball  -> Puma
UPDATE equips SET brand_id = 3 WHERE id = 3;  -- Football  -> Adidas

-- 8. Ver los datos
SELECT * FROM client;
SELECT * FROM equips;
SELECT * FROM brand;

-- 9. JOIN: equipos con el nombre de su marca
SELECT equips.type, brand.mark, equips.cost
FROM equips
JOIN brand ON equips.brand_id = brand.id;

-- ============================================
-- COMANDOS ÚTILES APRENDIDOS
-- ============================================
-- SHOW DATABASES;                  -- listar bases de datos
-- SHOW TABLES;                     -- listar tablas de la base activa
-- DESCRIBE equips;                 -- ver estructura de una tabla
-- RENAME TABLE viejo TO nuevo;     -- renombrar tabla
-- DROP TABLE nombre;               -- borrar tabla
-- DROP DATABASE nombre;            -- borrar base de datos
-- DELETE FROM tabla WHERE id = X;  -- borrar una fila
-- CURDATE()                        -- fecha de hoy en un INSERT

-- ============================================
-- ERRORES QUE VIMOS Y SU CAUSA
-- ============================================
-- Error 1046 "No database selected": falta ejecutar USE sports_shop;
-- Error 1050 "Table already exists": la tabla ya fue creada antes
-- Error 1146 "Table doesn't exist": la tabla no existe (o typo en el nombre)
-- "0 row(s) affected" en CREATE TABLE: es NORMAL, no es error
-- "0 row(s) returned" en SELECT: la tabla esta vacia, falta el INSERT
-- Recuerda: en Workbench, selecciona todo (Ctrl+A) y luego el rayo
-- para ejecutar el script completo.





