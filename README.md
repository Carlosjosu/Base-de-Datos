# Ejercicios SQL y Álgebra Relacional  
**Carlos Josué Granda Cango**  
**Curso:** Tercer Ciclo  

---

## Contenido

- [Tienda Informática (SQL)](#tienda-informática-sql)
- [Tienda Discos (SQL)](#tienda-discos-sql)
- [Ejercicios de Álgebra Relacional](#ejercicios-de-álgebra-relacional)
- [Consultas SQL Relacionadas](#consultas-sql-relacionadas)
- [Imágenes del Modelado Entidad Relacion](#imágenes)

---

## Tienda Informática (SQL)

```sql
CREATE DATABASE tiendainformatica;
USE tiendainformatica;

CREATE TABLE cliente(
  codigo VARCHAR(25) PRIMARY KEY,
  nombre VARCHAR(25) NOT NULL,
  apellidos VARCHAR(25) NOT NULL,
  direccion TEXT,
  telefono VARCHAR(12),
  correo VARCHAR(50) UNIQUE
);

CREATE TABLE producto(
  codigoP VARCHAR(25) PRIMARY KEY,
  descripcion VARCHAR(25) NOT NULL,
  existencia INT NOT NULL,
  precio DOUBLE
);

CREATE TABLE proveedor(
  codigoPro VARCHAR(25) PRIMARY KEY,
  nombre VARCHAR(25) NOT NULL,
  apellidos VARCHAR(25) NOT NULL,
  telefono VARCHAR(12),
  provincia SET('LOJA', 'ZAMORA', 'EL ORO') NOT NULL,
  direccion TEXT
);

CREATE TABLE compra(
  idCompra INT AUTO_INCREMENT PRIMARY KEY,
  total_compra DOUBLE NOT NULL,
  fecha DATE NOT NULL,
  cedula_cliente VARCHAR(25) NOT NULL,
  codigo_producto VARCHAR(25) NOT NULL,
  FOREIGN KEY (cedula_cliente) REFERENCES cliente(codigo),
  FOREIGN KEY (codigo_producto) REFERENCES producto(codigoP)
);

CREATE TABLE suministra(
  idSuministra VARCHAR(25) PRIMARY KEY,
  codigo_producto VARCHAR(25),
  codigo_proveedor VARCHAR(25),
  FOREIGN KEY (codigo_producto) REFERENCES producto(codigoP),
  FOREIGN KEY (codigo_proveedor) REFERENCES proveedor(codigoPro)
);

-- Insertar datos
INSERT INTO cliente (codigo, nombre, apellidos, correo) VALUES ("C006", "Pablo",  "Sanchez", "pablo.sanchez@unl.edu.ec");
INSERT INTO cliente (codigo, nombre, apellidos, correo) VALUES ("C007", "Lucía",  "Gómez",   "lucia.gomez@unl.edu.ec");
INSERT INTO cliente (codigo, nombre, apellidos, correo) VALUES ("C008", "Juan",   "Pérez",   "juan.perez@unl.edu.ec");
INSERT INTO cliente (codigo, nombre, apellidos, correo) VALUES ("C009", "Ana",    "Torres",  "ana.torres@unl.edu.ec");
INSERT INTO cliente (codigo, nombre, apellidos, correo) VALUES ("C010", "Carlos", "Medina",  "carlos.medina@unl.edu.ec");

-- Mostrar todos los registros
SELECT * FROM cliente;

-- Eliminar
DELETE FROM cliente WHERE codigo = "C006";
DELETE FROM cliente WHERE codigo = "C006" OR correo = "pablo.sanchez@unl.edu.ec";

-- Seleccionar todo tras eliminación
SELECT * FROM cliente;

-- Ordenar
SELECT * FROM cliente ORDER BY nombre;
SELECT * FROM cliente ORDER BY apellidos, nombre DESC;
SELECT * FROM cliente ORDER BY apellidos, nombre DESC LIMIT 3;

-- Posiciones (LIKE)
SELECT * FROM cliente WHERE apellidos LIKE 'G%';
SELECT * FROM cliente WHERE nombre LIKE 'A%';
SELECT * FROM cliente WHERE nombre LIKE '_a%';
SELECT * FROM cliente WHERE apellidos LIKE '%z_';

-- Filtrado tipo SET
SELECT * FROM cliente WHERE apellidos IN("Gómez", "Sanchez");
SELECT * FROM cliente WHERE apellidos NOT IN("Gómez", "Sanchez");
```

---

## Tienda Discos (SQL)

```sql
CREATE DATABASE tienda_discos;
USE tienda_discos;

CREATE TABLE cancion(
  idCancion VARCHAR(25) PRIMARY KEY,
  titulo VARCHAR(50) UNIQUE NOT NULL
);

CREATE TABLE generoMusica(
  idGenero VARCHAR(25) PRIMARY KEY
);

CREATE TABLE cantante(
  idCantante VARCHAR(25) PRIMARY KEY,
  nombre VARCHAR(25) NOT NULL,
  pais SET('Ecuador', 'Peru', 'Colombia') NOT NULL
);

CREATE TABLE disco(
  idDisco VARCHAR(25) PRIMARY KEY,
  titulo VARCHAR(50) UNIQUE NOT NULL,
  precio FLOAT NOT NULL,
  genero VARCHAR(25) NOT NULL,
  cantante VARCHAR(25) NOT NULL,
  FOREIGN KEY (genero) REFERENCES generoMusica(idGenero),
  FOREIGN KEY (cantante) REFERENCES cantante(idCantante)
);

CREATE TABLE contiene(
  idContiene VARCHAR(25) PRIMARY KEY,
  posicion INT UNIQUE NOT NULL,
  id_cancion VARCHAR(25) NOT NULL,
  id_disco VARCHAR(25) NOT NULL,
  FOREIGN KEY (id_cancion) REFERENCES cancion(idCancion),
  FOREIGN KEY (id_disco) REFERENCES disco(idDisco)
);
```

---

## Ejercicios de Álgebra Relacional

```
π nombre, duracion (curso)
π ciudad (alumno)

σ duracion >= 15 (curso)
R1 = σ duracion >= 15 (curso)

R2 = π nombre(σ duracion >= 15 (curso))
R3 = (σ id_alumno == 11(apoderado))
R4 = π fono(σ id_alumno == 31(apoderado))

R5 = (inscrito) * (alumno) 

R6 = (ρ nombre ➡ nombreA, id ➡ idA (alumno)) * apoderado
R7 = σ idA == 31((ρ nombre ➡ nombreA, id ➡ idA (alumno)) * apoderado)
R8 = π nombre, fono(σ idA == id_alumno (σ nombreA == "Rosita"(ρ nombre ➡ nombreA, id ➡ idA(alumno)) * apoderado))
```

---

## Consultas SQL Relacionadas

```sql
SELECT titulo FROM libro WHERE estado = 'bajo';
CREATE VIEW view_libro AS SELECT titulo FROM libro WHERE estado = 'bajo';

SELECT nombres FROM hinchas WHERE equipo_futbol = 'Libertad';
CREATE VIEW view_hinchas AS SELECT nombres FROM hinchas WHERE equipo_futbol = 'Libertad';

SELECT * FROM hinchas WHERE equipo_futbol = 'Libertad';

SELECT * FROM equipos_primera WHERE categoria = 'Primera';

-- Inner Join
SELECT * FROM tabla1 INNER JOIN tabla2 ON condición;
SELECT * FROM hinchas INNER JOIN (SELECT * FROM equipos_primera WHERE categoria = 'Primera') ON equipos_futbol = nombre_equipo;
SELECT nombres, apellidos, equipo FROM hinchas INNER JOIN (SELECT * FROM equipos_primera WHERE categoria = 'Primera') ON equipos_futbol = nombre_equipo;
SELECT * FROM hinchas WHERE equipo_futbol IN (SELECT nombre_equipo FROM equipo_primera WHERE categoria = 'primera');

CREATE VIEW hinchas_primera AS 
SELECT nombres, apellidos, equipo 
FROM hinchas 
INNER JOIN (SELECT * FROM equipos_primera WHERE categoria = 'Primera') 
ON equipos_futbol = nombre_equipo;
```

---

## Imágenes

A continuación se muestran 5 imágenes relacionadas al curso y los ejercicios:

### Imagen 1
![image](https://github.com/user-attachments/assets/5a113831-8fe9-4a17-b4a8-cfcd32f4034d)

### Imagen 2
![image](https://github.com/user-attachments/assets/eb488c25-b3bf-45db-8d39-c927d55cac63)

### Imagen 3
![image](https://github.com/user-attachments/assets/4a544556-91e3-46e7-8786-8669f75b10b0)

### Imagen 4
![image](https://github.com/user-attachments/assets/640cd916-9f36-4d4c-8898-a585fa77f52d)

### Imagen 5
![Imagen 5](https://github.com/user-attachments/assets/74487942-b033-46ae-bbbd-5a45818b4149)

