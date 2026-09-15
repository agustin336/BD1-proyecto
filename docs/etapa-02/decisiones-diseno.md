# Decisiones de Diseño — Diagrama Entidad-Relación (DER)

**Asignatura:** Bases de Datos I — FaCENA (UNNE)  
**Equipo:** 20

---

## 1. Introducción y Enfoque del Modelo Conceptual

En este documento explicamos las decisiones que tomamos para armar el Diagrama Entidad-Relación (DER) del sistema de ventas de la tienda de computación. Todo lo que describimos acá sale directamente del diagrama (`ERD Proyecto`), respetando las entidades, atributos, opcionalidades y cardinalidades tal como quedaron definidas ahí.

---

## 2. Identificación y Justificación de Entidades y Atributos

### Módulo Comercial, Facturación y Seguridad

* **`Cabecera_Factura`:**  
  Es el comprobante o la orden de venta que se emite cuando el cliente compra algo. La pensamos como una entidad transaccional porque agrupa los datos generales de esa operación puntual.
  * `Id`: clave que identifica a cada factura.
  * `Fecha`: cuándo se emitió.
  * `Total`: el monto final de la operación.

* **`Detalle_venta`:**  
  Acá van las líneas de cada factura, es decir, qué computadora (o computadoras) se vendieron en esa operación puntual y en qué cantidad. La separamos de `Cabecera_Factura` porque una misma venta puede incluir varios ítems distintos.
  * `Id`: clave de cada línea de detalle.
  * `Cantidad`: cuántas unidades se vendieron en esa línea.
  * `Subtotal`: el importe que corresponde a esa línea en particular.

* **`Cliente`:**  
  Son las personas (físicas o jurídicas) que compran en el local. La modelamos como entidad fuerte porque existe de forma independiente, sin depender de ninguna otra entidad para tener sentido. En el diagrama aparecen tres atributos candidatos a clave: `ID`, `DNI` y `Email`. Elegimos `ID` como clave primaria por ser un identificador interno simple y estable, mientras que `DNI` y `Email` quedan como atributos únicos (no se pueden repetir entre clientes, pero no son la clave principal de la entidad).
  * `ID` (clave primaria), `DNI` (único), `Email` (único), `Nombre`, `Apellido`, `Telefono_Contacto`.

* **`Tipo_Pago`:**  
  Guarda las distintas formas en que se puede pagar una compra: efectivo, tarjeta, transferencia, etc. La separamos en su propia entidad para no repetir ese dato como texto suelto en cada factura.
  * `ID` (clave), `Tipo`.

* **`Usuario`:**  
  Representa a las personas que trabajan en el sistema y necesitan iniciar sesión para operarlo (vendedores, administradores, etc.). Igual que en `Cliente`, acá también hay varios atributos candidatos a clave: `ID`, `User`, `Email` y `DNI`. Elegimos `ID` como clave primaria, y dejamos `User`, `Email` y `DNI` como atributos únicos, ya que ninguno de los tres se puede repetir entre distintos usuarios pero no son la clave principal.
  * `ID` (clave primaria), `User` (único), `Email` (único), `DNI` (único), `Contraseña`, `Nombre`, `Apellido`, `Telefono_Contacto`.

* **`Tipo_usuario`:**  
  Define qué rol o nivel de acceso tiene cada usuario dentro del sistema (por ejemplo, si es vendedor o administrador). La separamos de `Usuario` por el mismo motivo que separamos `Tipo_Pago`: evitar repetir texto y poder agregar roles nuevos sin tocar la entidad principal.
  * `ID` (clave), `Nombre/Rol`.

---

### Módulo de Equipos y Componentes Hardware

* **`Computadora`:**  
  Es el producto principal que se vende: un equipo ya armado. Tiene algunos atributos que no todas las computadoras necesitan, así que los dejamos como opcionales para poder representar tanto PCs de escritorio armadas a medida como notebooks o equipos All in One.
  * `ID`: clave del equipo.
  * `Stock`: cuántas unidades hay disponibles.
  * `Refrigeracion`: qué sistema de enfriamiento tiene.
  * `Taza_Refresco (O)`: opcional, pensado para equipos con pantalla integrada (notebooks, All in One).
  * `Marca (O)`: opcional, para cuando el armado corresponde a una marca comercial.
  * `Nombre (O)`: opcional, para el nombre comercial del modelo, si lo tiene.

* **Componentes de hardware asociados:**  
  Cada computadora se arma a partir de distintos componentes, y decidimos modelar cada tipo de componente como su propia entidad en lugar de meter todo como atributos sueltos dentro de `Computadora`. Así evitamos tener atributos multivaluados y podemos describir cada pieza con el detalle técnico que corresponde.

  1. **`Gabinete`:** `Id` (clave), `Ancho`, `Altura`, `Marca`, `Modelo`, `RGB`.
  2. **`Placa_Video`:** `ID` (clave), `Cant_VRAM`, `Tipo_VRAM`, `FRECUENCIA_VRAM`, `Marca`, `Modelo`, `Año`.
  3. **`Almacenamiento`:** `ID` (clave), `marca`, `Velocidad_Lectura`, `Velocidad_Escritura`, `Tamaño_GB`, `Tipo`, `Modelo`.
  4. **`Fuente_Poder`:** `ID` (clave), `Modelo`, `Marca`, `Potencia_W`, `RGB`.
  5. **`RAM`:** `ID` (clave), `Tipo`, `Cantidad_Memoria`, `Frecuencia`, `Marca`, `RGB`.
  6. **`Placa_Madre`:** `ID` (clave), `Modelo`, `RGB`, `Marca`.
  7. **`Procesador`:** `ID` (clave), `Marca`, `Modelo`, `Generacion`, `Nucleos`, `Velocidad`.

---

## 3. Relaciones y Cardinalidades del DER

Así quedaron definidas las relaciones entre entidades, con sus cardinalidades:

1. **`Cliente` — `Cabecera_Factura`** (relación `Tiene`):  
   Un cliente puede tener muchas facturas (1 a M), pero cada factura le corresponde a un único cliente.

2. **`Usuario` — `Cabecera_Factura`** (relación `Posee`):  
   Un usuario puede haber emitido varias facturas (1 a M); cada factura, sin embargo, quedó a cargo de un solo usuario.

3. **`Tipo_usuario` — `Usuario`** (relación `Tiene`):  
   Un tipo de usuario agrupa a varios usuarios (1 a M).

4. **`Tipo_Pago` — `Cabecera_Factura`** (relación `Posee`):  
   Un mismo tipo de pago puede estar asociado a muchas facturas distintas (1 a M).

5. **`Cabecera_Factura` — `Detalle_venta`** (relación `Tiene`):  
   Cada factura tiene al menos un renglón de detalle, y puede tener varios (1 a M).

6. **`Computadora` — `Detalle_venta`** (relación `Tiene`):  
   Una misma computadora puede aparecer en varios detalles de venta distintos (1 a M).

7. **`Computadora` — Componentes de hardware:**  
   Cada componente pertenece a un armado específico, así que la cardinalidad es M del lado de `Computadora` y 1 del lado del componente en todos los casos:
   * `Gabinete` (`Posee`)
   * `Placa_Video` (`Posee`)
   * `Almacenamiento` (`Posee`)
   * `Fuente_Poder` (`Posee`)
   * `RAM` (`Tiene`)
   * `Placa_Madre` (`Posee`)
   * `Procesador` (`Posee`)

---

## 4. Justificación de Decisiones de Modelado Conceptual

* **Por qué separamos `Cabecera_Factura` de `Detalle_venta`:** nos pareció más ordenado tener el total y los datos generales de la operación en un lado, y las cantidades e importes de cada ítem vendido en otro. Si lo hubiéramos puesto todo junto, una factura con varios productos hubiera necesitado repetir los datos generales en cada línea.

* **Por qué separamos los componentes de hardware en entidades propias:** en vez de cargar `Computadora` con un montón de atributos según cada pieza (procesador, RAM, placa de video, etc.), preferimos que cada componente tenga su propia entidad. Esto nos permite describir cada uno con sus características técnicas reales y evita que `Computadora` termine con atributos multivaluados.

* **Sobre los atributos opcionales de `Computadora`:** marcamos `Taza_Refresco`, `Marca` y `Nombre` como opcionales porque no todos los equipos los necesitan de la misma manera. Una PC armada a medida, por ejemplo, no siempre tiene una marca comercial asociada, mientras que una notebook o un All in One sí necesita el dato de la pantalla integrada.

