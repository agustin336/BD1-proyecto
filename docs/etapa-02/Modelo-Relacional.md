CREATE TABLE Gabinete (

ID\_Gabinete INT NOT NULL,

Altura FLOAT NOT NULL,

Ancho FLOAT NOT NULL,

Marca VARCHAR(100) NOT NULL,

Modelo VARCHAR(100) NOT NULL,

RGB VARCHAR(30) NOT NULL,

PRIMARY KEY (ID\_Gabinete)

)

CREATE TABLE Placa\_Video (

ID\_Placa\_Video INT NOT NULL,

Cantidad\_VRAM SMALLINT NOT NULL,

Tipo\_VRAM VARCHAR(100) NOT NULL,

Frecuencia\_VRAM FLOAT NOT NULL,

Marca VARCHAR(100) NOT NULL,

Modelo VARCHAR(100) NOT NULL,

Año SMALLINT NOT NULL,

PRIMARY KEY (ID\_Placa\_Video)

)

CREATE TABLE Almacenamiento (

ID\_Almacenamiento INT NOT NULL,

Marca VARCHAR(100) NOT NULL,

Velocidad\_Lectura INT NOT NULL,

Tamaño\_GB SMALLINT NOT NULL,

Tipo VARCHAR(100) NOT NULL,

Modelo VARCHAR(100) NOT NULL,

PRIMARY KEY (ID\_Almacenamiento)

)

CREATE TABLE Procesador (

ID\_Procesador INT NOT NULL,

Marca VARCHAR(100) NOT NULL,

Modelo VARCHAR(100) NOT NULL,

Generacion SMALLINT NOT NULL,

Cantidad\_Nucleos SMALLINT NOT NULL,

Velocidad FLOAT NOT NULL,

PRIMARY KEY (ID\_Procesador)

)

CREATE TABLE Fuente\_Poder (

ID\_Fuente\_Poder INT NOT NULL,

Modelo VARCHAR(100) NOT NULL,

Frecuencia\_W SMALLINT NOT NULL,

RGB VARCHAR(30) NOT NULL,

PRIMARY KEY (ID\_Fuente\_Poder)

)

CREATE TABLE Placa\_Madre (

ID\_Placa\_Madre INT NOT NULL,

Modelo VARCHAR(100) NOT NULL,

Marca VARCHAR(100) NOT NULL,

RGB VARCHAR(30) NOT NULL,

PRIMARY KEY (ID\_Placa\_Madre)

)

CREATE TABLE RAM (

ID\_RAM INT NOT NULL,

Tipo VARCHAR(100) NOT NULL,

Cantidad\_Memoria SMALLINT NOT NULL,

Frecuencia FLOAT NOT NULL,

Marca VARCHAR(100) NOT NULL,

RGB VARCHAR(30) NOT NULL,

PRIMARY KEY (ID\_RAM)

)

CREATE TABLE Computadora (

ID\_Computadora INT NOT NULL,

Nombre VARCHAR(100) NULL,

Marca VARCHAR(100)  NULL,

Taza\_Refresco SMALLINT NULL,

Refrigeracion VARCHAR(30) NOT NULL,

Stock INT NOT NULL,

ID\_Gabinete INT NOT NULL,

ID\_Placa\_Video INT NOT NULL,

ID\_Almacenamiento INT NOT NULL,

ID\_RAM INT NOT NULL,

ID\_Fuente\_Poder INT NOT NULL,

ID\_Procesador INT NOT NULL,

ID\_Placa\_Madre INT NOT NULL,

PRIMARY KEY (ID\_Computadora),

FOREIGN KEY (ID\_Gabinete) REFERENCES Gabinete(ID\_Gabinete),

FOREIGN KEY (ID\_Placa\_Video) REFERENCES Placa\_Video(ID\_Placa\_Video),

FOREIGN KEY (ID\_Almacenamiento) REFERENCES Almacenamiento(ID\_Almacenamiento),

FOREIGN KEY (ID\_RAM) REFERENCES RAM(ID\_RAM),

FOREIGN KEY (ID\_Fuente\_Poder) REFERENCES Fuente\_Poder(ID\_Fuente\_Poder),

FOREIGN KEY (ID\_Procesador) REFERENCES Procesador(ID\_Procesador),

FOREIGN KEY (ID\_Placa\_Madre) REFERENCES Placa\_Madre(ID\_Placa\_Madre)

)

CREATE TABLE Detalle\_Venta (

ID\_Detalle\_Venta INT NOT NULL,

Cantidad INT NOT NULL,

Subtotal FLOAT NOT NULL,

ID\_Computadora INT NOT NULL,

ID\_Cabecera\_venta INT NOT NULL,

PRIMARY KEY (ID\_Detalle\_Venta),

FOREIGN KEY (ID\_Computadora) REFERENCES Computadora(ID\_Computadora),

FOREIGN KEY (ID\_Cabecera\_venta) REFERENCES Cabecera\_Venta(ID\_Cabecera\_Venta)

)

CREATE TABLE Cabecera\_Venta (

ID\_Cabecera\_Venta INT NOT NULL,

Fecha DATE NOT NULL,

Total FLOAT NOT NULL,

ID\_Cliente INT NOT NULL,

ID\_Tipo\_Pago INT NOT NULL,

ID\_Usuario INT NOT NULL,

PRIMARY KEY (ID\_Cabecera\_Venta)

FOREIGN KEY (ID\_Cliente) REFERENCES Cliente(ID\_Cliente)

FOREIGN KEY (ID\_Tipo\_Pago) REFERENCES Tipo\_Pago(ID\_Tipo\_Pago)

FOREIGN KEY (ID\_Usuario) REFERENCES Usuario(ID\_Usuario)

)

CREATE TABLE Cliente (

ID\_Cliente INT NOT NULL,

Email VARCHAR(100) NOT NULL UNIQUE,

Nombre VARCHAR(100) NOT NULL,

Apellido VARCHAR(100) NOT NULL,

DNI VARCHAR(30) NOT NULL UNIQUE,

Telefono\_Contacto VARCHAR(30) NOT NULL,

PRIMARY KEY (ID\_Cliente)

)

CREATE TABLE Tipo\_Pago (

ID\_Tipo\_Pago INT NOT NULL,

Tipo VARCHAR(100) NOT NULL,

PRIMARY KEY (ID\_Tipo\_Pago)

)

CREATE TABLE Usuario (

ID\_Usuario INT NOT NULL,

DNI VARCHAR(30) NOT NULL UNIQUE,

User VARCHAR(100) NOT NULL UNIQUE,

Constraseña VARCHAR(100) NOT NULL,

Email VARCHAR(100) NOT NULL,

Nombre VARCHAR(100) NOT NULL,

Apellido VARCHAR(100) NOT NULL,

Telefono\_Contacto VARCHAR(30) NOT NULL,

ID\_Tipo\_Usuario INT NOT NULL,

PRIMARY KEY (ID\_Usuario)

FOREIGN KEY (ID\_Tipo\_Usuario) REFERENCES Tipo\_Usuario(ID\_Tipo\_Usuario)

)

CREATE TABLE Tipo\_Usuario (

ID\_Tipo\_Usuario INT NOT NULL,

Nombre/Rol VARCHAR(100) NOT NULL,

PRIMARY KEY (ID\_Tipo\_Usuario)

)
