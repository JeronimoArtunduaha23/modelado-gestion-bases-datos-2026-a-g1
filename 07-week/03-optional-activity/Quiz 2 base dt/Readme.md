#  TALLER – SENTENCIAS DML APLICADAS A MODELO DE DATOS

---

##  1. Introducción

Las sentencias DML (Data Manipulation Language) permiten manipular los datos dentro de una base de datos mediante operaciones como inserción, consulta y eliminación.

---

##  2. Sentencias utilizadas

- **INSERT** → Insertar datos  
- **SELECT** → Consultar datos  
- **DELETE** → Eliminar datos  

---

#  Definición de Sentencias DML

Las **sentencias DML (Data Manipulation Language)** son un conjunto de instrucciones del lenguaje SQL utilizadas para **gestionar y manipular los datos almacenados en una base de datos**.

Estas sentencias permiten realizar operaciones fundamentales como:

- Insertar datos nuevos  
- Consultar información existente  
- Actualizar registros  
- Eliminar datos  

---
#  Definición de Sentencias DML

Las **sentencias DML (Data Manipulation Language)** son un conjunto de instrucciones del lenguaje SQL utilizadas para **gestionar y manipular los datos almacenados en una base de datos**.

Estas sentencias permiten realizar operaciones fundamentales como:

- Insertar datos nuevos  
- Consultar información existente  
- Actualizar registros  
- Eliminar datos  

---

##  Principales sentencias DML

#  Definición de Sentencias DML

Las **sentencias DML (Data Manipulation Language)** son un conjunto de instrucciones del lenguaje SQL utilizadas para **gestionar y manipular los datos almacenados en una base de datos**.

Estas sentencias permiten realizar operaciones fundamentales como:

- Insertar datos nuevos  
- Consultar información existente  
- Actualizar registros  
- Eliminar datos  

---

##  Principales sentencias DML

###  INSERT

#  Definición de Sentencias DML

Las **sentencias DML (Data Manipulation Language)** son un conjunto de instrucciones del lenguaje SQL utilizadas para **gestionar y manipular los datos almacenados en una base de datos**.

Estas sentencias permiten realizar operaciones fundamentales como:

- Insertar datos nuevos  
- Consultar información existente  
- Actualizar registros  
- Eliminar datos  

---

## Definicion de sentencias DML

Las **sentencias DML (Data Manipulation Language)** son un conjunto de instrucciones del 
lenguaje SQL utilizadas para **gestionar y manipular los datos almacenados en una base de datos**.

Estas sentencias permiten realizar operaciones fundamentales como:

- Insertar datos nuevos  
- Consultar información existente  
- Actualizar registros  
- Eliminar datos  


---

##  Principales sentencias DML

###  INSERT

Se utiliza para **agregar nuevos registros** a una tabla.

```sql
INSERT INTO tabla (columna1, columna2)
VALUES ('valor1', 'valor2');
```
### SELECT
Permite consultar o recuperar datos de una o varias tablas.

```sql
SELECT * FROM tabla;
```

### UPDATE
Se usa para modificar datos existentes en una tabla.

```sql
UPDATE tabla
SET columna1 = 'nuevo_valor'
WHERE condicion;
```

### DELECT
Se ustiliza para eliminar registros de una tabla

```sql
DELETE FROM tabla
WHERE condicion;
```

---

# TABLA DE BD

```sql
- =========================================
-- EXTENSIONS
-- =========================================
-- Para generación de UUID
id CHAR(36) PRIMARY KEY DEFAULT (UUID())

-- =========================================
-- SCHEMAS
-- =========================================
CREATE SCHEMA security;
CREATE SCHEMA inventory;
CREATE SCHEMA bill;

-- =========================================
-- SECURITY
-- =========================================

CREATE TABLE security.role (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    name VARCHAR(100) NOT NULL,
    description TEXT,

    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    created_by VARCHAR(100),
    updated_at TIMESTAMP,
    updated_by VARCHAR(100),
    state VARCHAR(20) NOT NULL DEFAULT 'ACTIVE'
);

CREATE TABLE security.user (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    username VARCHAR(100) NOT NULL UNIQUE,
    email VARCHAR(150) NOT NULL UNIQUE,
    password TEXT NOT NULL,
    role_id UUID NOT NULL,

    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    created_by VARCHAR(100),
    updated_at TIMESTAMP,
    updated_by VARCHAR(100),
    state VARCHAR(20) NOT NULL DEFAULT 'ACTIVE',

    CONSTRAINT fk_user_role
        FOREIGN KEY (role_id)
        REFERENCES security.role(id)
);

CREATE TABLE security.form (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    name VARCHAR(100) NOT NULL,
    route VARCHAR(200),

    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    created_by VARCHAR(100),
    updated_at TIMESTAMP,
    updated_by VARCHAR(100),
    state VARCHAR(20) NOT NULL DEFAULT 'ACTIVE'
);

-- =========================================
-- INVENTORY
-- =========================================

CREATE TABLE inventory.category (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    name VARCHAR(100) NOT NULL,
    description TEXT,

    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    created_by VARCHAR(100),
    updated_at TIMESTAMP,
    updated_by VARCHAR(100),
    state VARCHAR(20) NOT NULL DEFAULT 'ACTIVE'
);

CREATE TABLE inventory.product (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    name VARCHAR(150) NOT NULL,
    description TEXT,
    price NUMERIC(12,2) NOT NULL,
    category_id UUID NOT NULL,

    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    created_by VARCHAR(100),
    updated_at TIMESTAMP,
    updated_by VARCHAR(100),
    state VARCHAR(20) NOT NULL DEFAULT 'ACTIVE',

    CONSTRAINT fk_product_category
        FOREIGN KEY (category_id)
        REFERENCES inventory.category(id)
);

CREATE TABLE inventory.inventory (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    product_id UUID NOT NULL,
    quantity INTEGER NOT NULL,

    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    created_by VARCHAR(100),
    updated_at TIMESTAMP,
    updated_by VARCHAR(100),
    state VARCHAR(20) NOT NULL DEFAULT 'ACTIVE',

    CONSTRAINT fk_inventory_product
        FOREIGN KEY (product_id)
        REFERENCES inventory.product(id)
);

-- =========================================
-- BILLING
-- =========================================

CREATE TABLE bill.bill (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    user_id UUID NOT NULL,
    total NUMERIC(14,2) NOT NULL,

    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    created_by VARCHAR(100),
    updated_at TIMESTAMP,
    updated_by VARCHAR(100),
    state VARCHAR(20) NOT NULL DEFAULT 'ACTIVE',

    CONSTRAINT fk_bill_user
        FOREIGN KEY (user_id)
        REFERENCES security."user"(id)
);

CREATE TABLE bill.bill_item (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    bill_id UUID NOT NULL,
    product_id UUID NOT NULL,
    quantity INTEGER NOT NULL,
    unit_price NUMERIC(12,2) NOT NULL,
    total NUMERIC(14,2) NOT NULL,

    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    created_by VARCHAR(100),
    updated_at TIMESTAMP,
    updated_by VARCHAR(100),
    state VARCHAR(20) NOT NULL DEFAULT 'ACTIVE',

    CONSTRAINT fk_bill_item_bill
        FOREIGN KEY (bill_id)
        REFERENCES bill.bill(id),

    CONSTRAINT fk_bill_item_product
        FOREIGN KEY (product_id)
        REFERENCES inventory.product(id)
);

INSERT INTO security.role (name, description) VALUES
('Admin', 'Administrador del sistema'),
('Vendedor', 'Encargado de ventas'),
('Cajero', 'Maneja caja'),
('Supervisor', 'Supervisa procesos'),
('Inventario', 'Gestiona inventario'),
('Cliente', 'Usuario cliente');

INSERT INTO security.user (username, email, password, role_id) VALUES
('admin1', 'admin1@mail.com', '1234', (SELECT id FROM security.role WHERE name='Admin')),
('vendedor1', 'vend1@mail.com', '1234', (SELECT id FROM security.role WHERE name='Vendedor')),
('cajero1', 'caj1@mail.com', '1234', (SELECT id FROM security.role WHERE name='Cajero')),
('super1', 'super1@mail.com', '1234', (SELECT id FROM security.role WHERE name='Supervisor')),
('invent1', 'inv1@mail.com', '1234', (SELECT id FROM security.role WHERE name='Inventario')),
('cliente1', 'cli1@mail.com', '1234', (SELECT id FROM security.role WHERE name='Cliente'));

INSERT INTO security.form (name, route) VALUES
('Login', '/login'),
('Dashboard', '/dashboard'),
('Usuarios', '/users'),
('Productos', '/products'),
('Facturacion', '/billing'),
('Inventario', '/inventory');

INSERT INTO inventory.product (name, description, price, category_id) VALUES
('Coca Cola', 'Bebida gaseosa', 5000, (SELECT id FROM inventory.category WHERE name='Bebidas')),
('Papas', 'Papas fritas', 3000, (SELECT id FROM inventory.category WHERE name='Snacks')),
('Leche', 'Leche entera', 4000, (SELECT id FROM inventory.category WHERE name='Lacteos')),
('Carne Res', 'Carne roja', 15000, (SELECT id FROM inventory.category WHERE name='Carnes')),
('Jabon', 'Jabon de baño', 2500, (SELECT id FROM inventory.category WHERE name='Aseo')),
('Laptop', 'Computador portatil', 2500000, (SELECT id FROM inventory.category WHERE name='Tecnologia'));

INSERT INTO inventory.inventory (product_id, quantity) VALUES
((SELECT id FROM inventory.product WHERE name='Coca Cola'), 100),
((SELECT id FROM inventory.product WHERE name='Papas'), 200),
((SELECT id FROM inventory.product WHERE name='Leche'), 150),
((SELECT id FROM inventory.product WHERE name='Carne Res'), 80),
((SELECT id FROM inventory.product WHERE name='Jabon'), 300),
((SELECT id FROM inventory.product WHERE name='Laptop'), 20);

INSERT INTO bill.bill (user_id, total) VALUES
((SELECT id FROM security.user WHERE username='admin1'), 20000),
((SELECT id FROM security.user WHERE username='vendedor1'), 15000),
((SELECT id FROM security.user WHERE username='cajero1'), 30000),
((SELECT id FROM security.user WHERE username='super1'), 10000),
((SELECT id FROM security.user WHERE username='invent1'), 50000),
((SELECT id FROM security.user WHERE username='cliente1'), 25000);

INSERT INTO bill.bill_item (bill_id, product_id, quantity, unit_price, total) VALUES
(
 (SELECT id FROM bill.bill LIMIT 1),
 (SELECT id FROM inventory.product WHERE name='Coca Cola'),
 2, 5000, 10000
),
(
 (SELECT id FROM bill.bill OFFSET 1 LIMIT 1),
 (SELECT id FROM inventory.product WHERE name='Papas'),
 3, 3000, 9000
),
(
 (SELECT id FROM bill.bill OFFSET 2 LIMIT 1),
 (SELECT id FROM inventory.product WHERE name='Leche'),
 2, 4000, 8000
),
(
 (SELECT id FROM bill.bill OFFSET 3 LIMIT 1),
 (SELECT id FROM inventory.product WHERE name='Carne Res'),
 1, 15000, 15000
),
(
 (SELECT id FROM bill.bill OFFSET 4 LIMIT 1),
 (SELECT id FROM inventory.product WHERE name='Jabon'),
 4, 2500, 10000
),
(
 (SELECT id FROM bill.bill OFFSET 5 LIMIT 1),
 (SELECT id FROM inventory.product WHERE name='Laptop'),
 1, 2500000, 2500000
);

DELETE FROM bill.bill_item
WHERE quantity = 2;

DELETE FROM bill.bill_item
WHERE total > 10000;

DELETE FROM bill.bill_item
WHERE unit_price = 3000;

DELETE FROM bill.bill
WHERE total < 15000;

DELETE FROM bill.bill
WHERE total > 40000;

DELETE FROM bill.bill
WHERE total = 20000;

DELETE FROM inventory.inventory
WHERE quantity < 50;

DELETE FROM inventory.inventory
WHERE quantity > 200;

DELETE FROM inventory.inventory
WHERE quantity = 100;

DELETE FROM inventory.product
WHERE price < 3000;

DELETE FROM inventory.product
WHERE price > 1000000;

DELETE FROM inventory.product
WHERE name = 'Leche';

DELETE FROM inventory.category
WHERE name = 'Aseo';

DELETE FROM inventory.category
WHERE name = 'Tecnologia';

DELETE FROM inventory.category
WHERE name = 'Carnes';

DELETE FROM security.user
WHERE username = 'cliente1';

DELETE FROM security.user
WHERE username = 'invent1';

DELETE FROM security.user
WHERE username = 'super1';

DELETE FROM security.form
WHERE name = 'Login';

DELETE FROM security.form
WHERE name = 'Dashboard';

DELETE FROM security.form
WHERE name = 'Usuarios';

DELETE FROM security.role
WHERE name = 'Cliente';

DELETE FROM security.role
WHERE name = 'Inventario';

DELETE FROM security.role
WHERE name = 'Supervisor';

-- Ver todos los roles
SELECT * FROM security.role;

-- Roles activos
SELECT name, description
FROM security.role
WHERE state = 'ACTIVE';

-- Buscar rol específico
SELECT * 
FROM security.role
WHERE name = 'Admin';

-- Ver todos los usuarios
SELECT * FROM security.user;

-- Usuarios con su rol (JOIN 🔥)
SELECT u.username, u.email, r.name AS role
FROM security.user u
JOIN security.role r ON u.role_id = r.id;

-- Buscar usuario por correo
SELECT * 
FROM security.user
WHERE email LIKE '%mail.com';

-- Todos los formularios
SELECT * FROM security.form;

-- Formularios activos
SELECT name, route
FROM security.form
WHERE state = 'ACTIVE';

-- Buscar por ruta
SELECT * 
FROM security.form
WHERE route LIKE '%dash%';

-- Todas las categorías
SELECT * FROM inventory.category;

-- Categorías activas
SELECT name
FROM inventory.category
WHERE state = 'ACTIVE';

-- Buscar categoría específica
SELECT *
FROM inventory.category
WHERE name = 'Bebidas';

-- Todos los productos
SELECT * FROM inventory.product;

-- Productos con categoría (JOIN 🔥)
SELECT p.name, p.price, c.name AS category
FROM inventory.product p
JOIN inventory.category c ON p.category_id = c.id;

-- Productos caros
SELECT *
FROM inventory.product
WHERE price > 10000;

-- Todo el inventario
SELECT * FROM inventory.inventory;

-- Inventario con producto (JOIN 🔥)
SELECT p.name, i.quantity
FROM inventory.inventory i
JOIN inventory.product p ON i.product_id = p.id;

-- Productos con poco stock
SELECT *
FROM inventory.inventory
WHERE quantity < 100;

-- Todas las facturas
SELECT * FROM bill.bill;

-- Facturas con usuario (JOIN 🔥)
SELECT b.id, u.username, b.total
FROM bill.bill b
JOIN security.user u ON b.user_id = u.id;

-- Facturas altas
SELECT *
FROM bill.bill
WHERE total > 20000;

-- Todos los items
SELECT * FROM bill.bill_item;

-- Detalle completo (JOIN PRO 🔥🔥)
SELECT b.id AS factura, p.name AS producto, bi.quantity, bi.total
FROM bill.bill_item bi
JOIN bill.bill b ON bi.bill_id = b.id
JOIN inventory.product p ON bi.product_id = p.id;

-- Items con cantidad alta
SELECT *
FROM bill.bill_item
WHERE quantity >= 2;
```