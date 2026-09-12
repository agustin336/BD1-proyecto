CREATE TABLE Gabinete (

ID_Gabinete INT NOT NULL,

Altura FLOAT NOT NULL,

Ancho FLOAT NOT NULL,

Marca VARCHAR(100) NOT NULL,

Modelo VARCHAR(100) NOT NULL,

RGB VARCHAR(30) NOT NULL,

PRIMARY KEY (ID_Gabinete)

)

CREATE TABLE Placa_Video (

ID_Placa_Video INT NOT NULL,

Cantidad_VRAM SMALLINT NOT NULL,

Tipo_VRAM VARCHAR(100) NOT NULL,

Frecuencia_VRAM FLOAT NOT NULL,

Marca VARCHAR(100) NOT NULL,

Modelo VARCHAR(100) NOT NULL,

Año SMALLINT NOT NULL,

PRIMARY KEY (ID_Placa_Video)

)

CREATE TABLE Almacenamiento (

ID_Almacenamiento INT NOT NULL,

Marca VARCHAR(100) NOT NULL,

Velocidad_Lectura INT NOT NULL,

Tamaño_GB SMALLINT NOT NULL,

Tipo VARCHAR(100) NOT NULL,

Modelo VARCHAR(100) NOT NULL,

PRIMARY KEY (ID_Almacenamiento)

)

CREATE TABLE Procesador (

ID_Procesador INT NOT NULL,

Marca VARCHAR(100) NOT NULL,

Modelo VARCHAR(100) NOT NULL,

Generacion SMALLINT NOT NULL,

Cantidad_Nucleos SMALLINT NOT NULL,

Velocidad FLOAT NOT NULL,

PRIMARY KEY (ID_Procesador)

)

CREATE TABLE Fuente_Poder (

ID_Fuente_Poder INT NOT NULL,

Modelo VARCHAR(100) NOT NULL,

Marca VARCHAR(100) NOT NULL;

Frecuencia_W SMALLINT NOT NULL,

RGB VARCHAR(30) NOT NULL,

PRIMARY KEY (ID_Fuente_Poder)

)

CREATE TABLE Placa_Madre (

ID_Placa_Madre INT NOT NULL,

Modelo VARCHAR(100) NOT NULL,

Marca VARCHAR(100) NOT NULL,

RGB VARCHAR(30) NOT NULL,

PRIMARY KEY (ID_Placa_Madre)

)

CREATE TABLE RAM (

ID_RAM INT NOT NULL,

Tipo VARCHAR(100) NOT NULL,

Cantidad_Memoria SMALLINT NOT NULL,

Frecuencia FLOAT NOT NULL,

Marca VARCHAR(100) NOT NULL,

RGB VARCHAR(30) NOT NULL,

PRIMARY KEY (ID_RAM)

)

CREATE TABLE Computadora (

ID_Computadora INT NOT NULL,

Nombre VARCHAR(100) NULL,

Marca VARCHAR(100) NULL,

Taza_Refresco SMALLINT NULL,

Refrigeracion VARCHAR(30) NOT NULL,

Stock INT NOT NULL,

ID_Gabinete INT NOT NULL,

ID_Placa_Video INT NOT NULL,

ID_Almacenamiento INT NOT NULL,

ID_RAM INT NOT NULL,

ID_Fuente_Poder INT NOT NULL,

ID_Procesador INT NOT NULL,

ID_Placa_Madre INT NOT NULL,

PRIMARY KEY (ID_Computadora),

FOREIGN KEY (ID_Gabinete) REFERENCES Gabinete(ID_Gabinete),

FOREIGN KEY (ID_Placa_Video) REFERENCES Placa_Video(ID_Placa_Video),

FOREIGN KEY (ID_Almacenamiento) REFERENCES Almacenamiento(ID_Almacenamiento),

FOREIGN KEY (ID_RAM) REFERENCES RAM(ID_RAM),

FOREIGN KEY (ID_Fuente_Poder) REFERENCES Fuente_Poder(ID_Fuente_Poder),

FOREIGN KEY (ID_Procesador) REFERENCES Procesador(ID_Procesador),

FOREIGN KEY (ID_Placa_Madre) REFERENCES Placa_Madre(ID_Placa_Madre)

)

CREATE TABLE Detalle_Venta (

ID_Detalle_Venta INT NOT NULL,

Cantidad INT NOT NULL,

Subtotal FLOAT NOT NULL,

ID_Computadora INT NOT NULL,

ID_Cabecera_venta INT NOT NULL,

PRIMARY KEY (ID_Detalle_Venta),

FOREIGN KEY (ID_Computadora) REFERENCES Computadora(ID_Computadora),

FOREIGN KEY (ID_Cabecera_venta) REFERENCES Cabecera_Venta(ID_Cabecera_Venta)

)

CREATE TABLE Cabecera_Venta (

ID_Cabecera_Venta INT NOT NULL,

Fecha DATE NOT NULL,

Total FLOAT NOT NULL,

ID_Cliente INT NOT NULL,

ID_Tipo_Pago INT NOT NULL,

ID_Usuario INT NOT NULL,

PRIMARY KEY (ID_Cabecera_Venta)

FOREIGN KEY (ID_Cliente) REFERENCES Cliente(ID_Cliente)

FOREIGN KEY (ID_Tipo_Pago) REFERENCES Tipo_Pago(ID_Tipo_Pago)

FOREIGN KEY (ID_Usuario) REFERENCES Usuario(ID_Usuario)

)

CREATE TABLE Cliente (

ID_Cliente INT NOT NULL,

Email VARCHAR(100) NOT NULL UNIQUE,

Nombre VARCHAR(100) NOT NULL,

Apellido VARCHAR(100) NOT NULL,

DNI VARCHAR(30) NOT NULL UNIQUE,

Telefono_Contacto VARCHAR(30) NOT NULL,

PRIMARY KEY (ID_Cliente)

)

CREATE TABLE Tipo_Pago (

ID_Tipo_Pago INT NOT NULL,

Tipo VARCHAR(100) NOT NULL,

PRIMARY KEY (ID_Tipo_Pago)

)

CREATE TABLE Usuario (

ID_Usuario INT NOT NULL,

DNI VARCHAR(30) NOT NULL UNIQUE,

User VARCHAR(100) NOT NULL UNIQUE,

Constraseña VARCHAR(100) NOT NULL,

Email VARCHAR(100) NOT NULL,

Nombre VARCHAR(100) NOT NULL,

Apellido VARCHAR(100) NOT NULL,

Telefono_Contacto VARCHAR(30) NOT NULL,

ID_Tipo_Usuario INT NOT NULL,

PRIMARY KEY (ID_Usuario)

FOREIGN KEY (ID_Tipo_Usuario) REFERENCES Tipo_Usuario(ID_Tipo_Usuario)

)

CREATE TABLE Tipo_Usuario (

ID_Tipo_Usuario INT NOT NULL,

Nombre/Rol VARCHAR(100) NOT NULL,

PRIMARY KEY (ID_Tipo_Usuario)

)

