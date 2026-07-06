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

