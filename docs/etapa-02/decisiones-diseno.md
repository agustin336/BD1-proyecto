# Decisiones de Diseño — Diagrama Entidad-Relación (DER) y Modelo Relacional (3FN)

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

---

## 5. Pasaje del DER al Modelo Relacional

En esta parte explicamos cómo convertimos el diagrama entidad-relación en el conjunto de tablas relacionales que definimos en el archivo de diseño, aplicando las reglas de transformación:

* **Conversión de entidades a tablas:**  
  Cada una de las entidades del DER pasó a ser una tabla independiente en el modelo relacional. A cada una le asignamos una clave primaria (`ID_*`) para identificar a cada registro de manera unívoca.

* **Mapeo de relaciones 1 a N mediante claves foráneas (FK):**  
  Todas las relaciones de nuestro modelo son del tipo uno a muchos (1 a M). Por regla de transformación, la clave primaria de la entidad que está del lado "1" viaja como clave foránea a la tabla que está del lado "M":
  * **`Cliente` — `Cabecera_Venta`:** agregamos `ID_Cliente` en `Cabecera_Venta`.
  * **`Tipo_Pago` — `Cabecera_Venta`:** agregamos `ID_Tipo_Pago` en `Cabecera_Venta`.
  * **`Usuario` — `Cabecera_Venta`:** agregamos `ID_Usuario` en `Cabecera_Venta`.
  * **`Tipo_Usuario` — `Usuario`:** agregamos `ID_Tipo_Usuario` en `Usuario`.
  * **`Cabecera_Venta` — `Detalle_Venta`:** agregamos `ID_Cabecera_venta` en `Detalle_Venta`.
  * **`Computadora` — `Detalle_Venta`:** agregamos `ID_Computadora` en `Detalle_Venta`.
  * **Componentes — `Computadora`:** como la cardinalidad es 1 a M desde los componentes hacia `Computadora`, trasladamos los IDs de cada una de las 7 piezas como claves foráneas dentro de `Computadora` (`ID_Gabinete`, `ID_Placa_Video`, `ID_Almacenamiento`, `ID_RAM`, `ID_Fuente_Poder`, `ID_Procesador` e `ID_Placa_Madre`).

* **Ajustes de nombres y atributos:**  
  * En el DER habíamos llamado a la entidad `Cabecera_Factura`, mientras que en el modelo relacional pasó a llamarse `Cabecera_Venta` (y su clave `ID_Cabecera_Venta`), unificando el término con `Detalle_Venta`.
  * En la tabla `Computadora` sumamos el atributo `Precio`, indispensable para poder calcular los importes y subtotales en las líneas de venta.

---

## 6. Justificación de la Normalización (1FN, 2FN y 3FN)

El diseño relacional resultante cumple de manera directa con las tres primeras formas normales:

* **Primera Forma Normal (1FN):**  
  * Todos los atributos almacenan valores atómicos (indivisibles). No usamos listas, arreglos ni textos compuestos dentro de una sola celda.
  * No existen grupos repetitivos. Los componentes de hardware no se agregaron como columnas reiteradas dentro de `Computadora`, sino como tablas separadas. Del mismo modo, los distintos ítems de una compra no están en `Cabecera_Venta`, sino que cada uno ocupa un registro individual en `Detalle_Venta`.
  * Todas las tablas cuentan con una clave primaria definida que identifica cada fila.

* **Segunda Forma Normal (2FN):**  
  * Cumple con la 1FN.
  * Exige que todos los atributos que no forman parte de la clave dependan por completo de la clave primaria, eliminando dependencias parciales.
  * En nuestro modelo, todas las tablas tienen claves primarias simples de una sola columna (`ID_*`), incluida `Detalle_Venta`. Al no tener claves compuestas en ninguna tabla, no existe la posibilidad de que un atributo dependa de solo una parte de la clave, por lo que la 2FN se satisface automáticamente en todo el esquema.

* **Tercera Forma Normal (3FN):**  
  * Cumple con la 2FN.
  * Exige que no existan dependencias transitivas, es decir, que ningún atributo no clave dependa funcionalmente de otro atributo no clave.
  * Esto se garantiza mediante la separación de conceptos que aplicamos:
    * **Roles de usuario:** los roles están en `Tipo_Usuario`. Si hubiéramos dejado el nombre del rol dentro de `Usuario`, ese dato dependería del tipo de usuario y no directamente del usuario (`ID_Usuario -> ID_Tipo_Usuario -> Nombre/Rol`).
    * **Tipos de pago:** están en `Tipo_Pago`. Dejar la descripción del medio de pago en `Cabecera_Venta` generaría redundancia y dependencia transitiva.
    * **Datos del cliente:** en `Cabecera_Venta` solo se almacena `ID_Cliente`. La información personal (`Nombre`, `Apellido`, `DNI`, `Email`, `Telefono_Contacto`) vive exclusivamente en `Cliente`.
    * **Detalles técnicos de componentes:** las características de hardware (como frecuencia, núcleos, tamaño o velocidad) se mantienen en sus tablas específicas y no dentro de `Computadora`, evitando que dependan transitivamente del ID del equipo.

---

## 7. Justificación de Decisiones del Modelo Relacional

* **Por qué usamos claves subrogadas (`ID`) en lugar de claves naturales:**  
  Tanto en `Cliente` como en `Usuario` existen atributos naturales candidatos a clave, como `DNI`, `Email` y el nombre de `User`. Elegimos usar identificadores numéricos artificiales (`ID`) como clave primaria para simplificar las relaciones con claves foráneas y optimizar los índices de búsqueda. Además, esto evita problemas si un cliente modifica su correo electrónico o si hay que corregir un número de documento mal cargado. Para garantizar que esos datos no se dupliquen en el sistema, les aplicamos la restricción de unicidad (`UNIQUE`).

* **Por qué `Detalle_Venta` tiene su propia clave primaria (`ID_Detalle_Venta`):**  
  En vez de usar una clave compuesta formada por `(ID_Cabecera_Venta, ID_Computadora)`, preferimos asignarle un ID propio a cada fila de detalle. Esto simplifica las consultas y las claves foráneas, y además permite registrar más de una vez el mismo equipo en una venta si fuera necesario (por ejemplo, si se vende con condiciones, promociones o garantías diferentes).

* **Manejo de valores nulos en `Computadora`:**  
  Los atributos `Nombre`, `Marca` y `Taza_Refresco` se configuraron para aceptar valores nulos (`NULL`). Esto responde a la variedad de equipos que maneja la tienda: una PC de escritorio armada a medida no suele tener marca comercial ni pantalla integrada, mientras que una notebook o una All in One sí cuenta con esos datos. El resto de los atributos técnicos y las 7 claves foráneas a los componentes son obligatorios (`NOT NULL`).


