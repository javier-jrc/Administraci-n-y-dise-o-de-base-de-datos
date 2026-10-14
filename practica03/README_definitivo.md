# Modelo entidad/relación — Viveros Tajinaste S.A.

**Asignatura:** Administración y Diseño de Bases de Datos (Grado en Ingeniería Informática, ULL) — curso 2026-2027
**Autores:** _Nombre Apellido 1_ · _Nombre Apellido 2_ <!-- completar; si es individual, dejar un solo nombre -->

## Contenido del repositorio

| Fichero | Descripción |
|---|---|
| [`modelo-er-viveros.drawio`](modelo-er-viveros.drawio) | Modelo E/R editable en [draw.io](https://app.diagrams.net) |
| [`modelo-er-viveros.png`](modelo-er-viveros.png) | Imagen del modelo E/R |
| `README.md` | Este documento |

![Modelo entidad/relación](modelo-er-viveros.png)

## Notación

- **Rectángulo**: entidad. **Rectángulo con doble borde**: entidad débil.
- **Rombo**: relación. **Rombo con doble borde**: relación identificadora, la que une una entidad débil con su entidad propietaria.
- **Atributo subrayado**: clave primaria. **Subrayado discontinuo**: clave parcial (discriminante) de una entidad débil.
- **Elipse discontinua**: atributo derivado.
- **Triángulo «ES UN»**: especialización (jerarquía supertipo/subtipo). El subtipo hereda los atributos y la clave del supertipo.
- **Cardinalidad `(mín,máx)`** (convención *look-across*): en una relación A–B, la etiqueta que hay junto a la entidad **B** indica cuántas ocurrencias de B, como mínimo y como máximo, se asocian a **cada ocurrencia de A**. Se lee desde el extremo opuesto. Por ejemplo, en VIVERO–TIENE–ZONA, el `(1,N)` junto a ZONA significa que cada vivero tiene entre 1 y N zonas, y el `(1,1)` junto a VIVERO que cada zona pertenece a un único vivero. Dentro del rombo aparece además el tipo de correspondencia (1:N, N:M).

---

## 1. Entidades

| Entidad | Tipo | Descripción |
|---|---|---|
| **VIVERO** | Fuerte | Cada uno de los centros de venta de la red de Tajinaste S.A. Se conoce su ubicación postal y su georreferenciación. |
| **ZONA** | Débil (depende de VIVERO) | Área física dentro de un vivero donde se colocan productos y trabajan empleados: zona exterior, almacén, invernadero, caja… Solo existe dentro de su vivero y se identifica por el vivero más un código local, porque el código "ALM" puede repetirse en varios viveros. También está georreferenciada. |
| **PRODUCTO** | Fuerte | Artículo del catálogo: plantas, productos de jardinería y decoración. |
| **EMPLEADO** | Fuerte | Persona que trabaja para Tajinaste S.A. Según la época del año puede estar destinada en distintos viveros y zonas. |
| **CLIENTE** | Fuerte (supertipo) | Cliente identificado de la empresa: se conocen sus datos personales. Los clientes anónimos, que compran sin identificarse, **no** se registran como ocurrencias de esta entidad. |
| **CLIENTE_PLUS** | Subtipo de CLIENTE | Cliente adherido al programa de fidelización *Tajinaste Plus*. Hereda los atributos de CLIENTE y añade la fecha de ingreso en el programa. Es el único tipo de cliente que recibe bonificaciones. |
| **PEDIDO** | Fuerte | Compra hecha en la empresa. Puede hacerla un cliente Plus, un cliente normal o un cliente anónimo (pedido sin cliente asociado). Cada pedido contiene uno o varios productos y tiene un único empleado responsable. |
| **BONIFICACION** | Débil (depende de CLIENTE_PLUS) | Bonificación mensual asignada a un cliente Plus según su volumen de compras de ese mes. Se identifica por el cliente más el año y el mes. |

## 2. Atributos y dominios

### VIVERO

| Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|
| <u>id_vivero</u> | Identificador único del vivero (clave primaria) | Entero positivo | `3` |
| nombre | Nombre comercial | Cadena (máx. 100) | `Tajinaste La Laguna` |
| dirección | Dirección postal | Cadena (máx. 200) | `Ctra. General del Norte, km 4, 38206 La Laguna` |
| latitud | Latitud en grados decimales (WGS84) | Real en [-90, 90], 6 decimales | `28.487400` |
| longitud | Longitud en grados decimales (WGS84) | Real en [-180, 180], 6 decimales | `-16.315900` |

### ZONA (entidad débil)

| Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|
| <ins>cod_zona</ins> (parcial) | Código de la zona dentro de su vivero. Clave completa: (id_vivero, cod_zona) | Cadena (máx. 10) | `EXT`, `ALM`, `INV1` |
| nombre | Nombre descriptivo | Cadena (máx. 60) | `Zona exterior`, `Almacén` |
| latitud | Latitud de la zona (WGS84) | Real en [-90, 90] | `28.487520` |
| longitud | Longitud de la zona (WGS84) | Real en [-180, 180] | `-16.315700` |

### PRODUCTO

| Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|
| <u>id_producto</u> | Identificador único (clave primaria) | Entero positivo | `1024` |
| nombre | Nombre del producto | Cadena (máx. 100) | `Drago canario 40 cm` |
| categoría | Familia del producto | {`Planta`, `Jardinería`, `Decoración`} | `Planta` |
| precio | Precio de venta unitario (€) | Decimal(8,2) > 0 | `24.95` |
| descripción | Descripción libre | Texto | `Dracaena draco en maceta de 18 cm` |

### EMPLEADO

| Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|
| <u>dni</u> | DNI/NIE del empleado (clave primaria) | Cadena de 9 caracteres: 8 dígitos + letra, o NIE | `43821567K` |
| nombre | Nombre | Cadena (máx. 50) | `Laura` |
| apellidos | Apellidos | Cadena (máx. 100) | `Hernández Pérez` |

### CLIENTE (supertipo)

| Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|
| <u>id_cliente</u> | Número de cliente (clave primaria) | Entero positivo | `50231` |
| nif | NIF del cliente (clave alternativa, único) | Cadena de 9 caracteres | `78654321B` |
| nombre | Nombre | Cadena (máx. 50) | `Carlos` |
| apellidos | Apellidos | Cadena (máx. 100) | `Rodríguez Díaz` |

### CLIENTE_PLUS (subtipo de CLIENTE)

Hereda `id_cliente` (que también es su clave), `nif`, `nombre` y `apellidos`.

| Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|
| fecha_ingreso | Fecha de alta en el programa Tajinaste Plus | Fecha ≤ fecha actual | `2025-03-15` |

### PEDIDO

| Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|
| <u>id_pedido</u> | Identificador único del pedido (clave primaria) | Entero positivo | `880145` |
| fecha | Fecha en que se hizo el pedido | Fecha | `2026-05-08` |
| importe_total *(derivado)* | Importe total del pedido (€). Se calcula como Σ (`cantidad` × `precio_unitario`) de sus líneas | Decimal(10,2) ≥ 0 | `189.41` |

### BONIFICACION (entidad débil)

| Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|
| <ins>año</ins> (parcial) | Año al que corresponde la bonificación | Entero ≥ 2000 | `2026` |
| <ins>mes</ins> (parcial) | Mes al que corresponde la bonificación. Clave completa: (id_cliente, año, mes) | Entero en [1, 12] | `5` |
| volumen_compras *(derivado)* | Suma del `importe_total` de los pedidos del cliente en ese mes | Decimal(10,2) ≥ 0 | `642.75` |
| importe_bonificacion | Bonificación asignada (€) según el volumen de compras | Decimal(8,2) ≥ 0 | `32.14` |

### Atributos de las relaciones

| Relación | Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|---|
| STOCK | cantidad_disponible | Unidades del producto disponibles en esa zona | Entero ≥ 0 | `35` |
| LINEA_PEDIDO | cantidad | Unidades del producto incluidas en el pedido | Entero > 0 | `3` |
| LINEA_PEDIDO | precio_unitario | Precio por unidad aplicado en ese pedido (€). Se guarda aparte porque `PRODUCTO.precio` puede cambiar después o llevar descuento | Decimal(8,2) ≥ 0 | `24.95` |
| OCUPA_PUESTO | <u>fecha_inicio</u> | Fecha en que el empleado empieza en esa zona. Junto con el empleado y la zona identifica cada periodo, así que un empleado puede volver a la misma zona en otra temporada | Fecha | `2026-06-01` |
| OCUPA_PUESTO | fecha_fin | Fecha en que termina ese destino. `NULL` si es el destino actual | Fecha ≥ fecha_inicio, o nulo | `2026-09-30` |
| OCUPA_PUESTO | tarea | Tarea que hace el empleado en esa zona durante el periodo | Cadena (máx. 60) | `Riego y mantenimiento`, `Atención al cliente`, `Caja` |

## 3. Relaciones y cardinalidades

### TIENE (VIVERO — ZONA) · 1:N · identificadora

Une cada zona con el vivero al que pertenece. Es identificadora porque ZONA es débil: su clave es `(id_vivero, cod_zona)`.

- **(1,N) junto a ZONA** (leída desde VIVERO): un vivero tiene al menos una zona y puede tener muchas (exterior, almacén, invernadero…).
- **(1,1) junto a VIVERO** (leída desde ZONA): cada zona pertenece a un solo vivero. Participación total y dependencia de identificación.

### STOCK (ZONA — PRODUCTO) · N:M

Indica en qué zonas está asignado cada producto y **cuántas unidades hay disponibles** en cada una (atributo `cantidad_disponible`).

- **(0,N) junto a PRODUCTO** (leída desde ZONA): una zona puede no tener productos (por ejemplo, una zona de caja u oficina) o tener muchos.
- **(0,N) junto a ZONA** (leída desde PRODUCTO): un producto puede estar asignado a varias zonas de uno o varios viveros, o a ninguna (un producto del catálogo sin existencias o recién dado de alta).

*Ejemplo:* el producto 1024 está en la zona `EXT` del vivero 3 con 35 unidades y en la zona `ALM` del mismo vivero con 120.

### OCUPA_PUESTO (EMPLEADO — ZONA) · N:M

Guarda el **histórico de puestos**: en qué zona (y, por tanto, en qué vivero) ha trabajado cada empleado, entre qué fechas y haciendo qué tarea. Con este histórico se puede analizar la productividad de cada zona a lo largo del tiempo y la de cada empleado.

- **(0,N) junto a ZONA** (leída desde EMPLEADO): con el tiempo un empleado puede pasar por muchas zonas de distintos viveros. Puede tener 0 si acaba de incorporarse y aún no tiene destino.
- **(0,N) junto a EMPLEADO** (leída desde ZONA): por una zona pueden pasar muchos empleados a lo largo del tiempo, o ninguno.

Que un empleado "nunca tenga dos destinos" a la vez no se puede expresar con cardinalidades, porque en el histórico sí tiene muchos destinos. Se recoge como restricción semántica (R5 y R6).

### LINEA_PEDIDO (PEDIDO — PRODUCTO) · N:M

Indica qué productos incluye cada pedido, en qué cantidad y a qué precio unitario. A partir de estas líneas se calcula el `importe_total` del pedido.

- **(1,N) junto a PRODUCTO** (leída desde PEDIDO): un pedido contiene al menos un producto y puede contener muchos.
- **(0,N) junto a PEDIDO** (leída desde PRODUCTO): un producto puede aparecer en muchos pedidos o en ninguno.

*Ejemplo:* el pedido 880145 tiene 3 unidades del producto 1024 a 24,95 € y 2 unidades del producto 2210 a 57,28 €, así que su `importe_total` es 189,41 €.

### GESTIONA (EMPLEADO — PEDIDO) · 1:N

Asigna a cada pedido el empleado que lo gestiona. Sirve para medir si los empleados alcanzan sus objetivos de venta. Los análisis de productividad del enunciado se centran en los pedidos de clientes Plus, pero todos los pedidos tienen un responsable.

- **(0,N) junto a PEDIDO** (leída desde EMPLEADO): un empleado puede gestionar muchos pedidos o ninguno (no todos los puestos son de venta).
- **(1,1) junto a EMPLEADO** (leída desde PEDIDO): cada pedido tiene **exactamente un responsable**, tal como dice el enunciado.

### REALIZA (CLIENTE — PEDIDO) · 1:N

Une cada pedido con el cliente identificado que lo ha hecho. Como CLIENTE_PLUS es subtipo de CLIENTE, también participa en esta relación. Así se pueden consultar los pedidos de un cliente Plus desde su ingreso en el programa para preparar campañas.

- **(0,N) junto a PEDIDO** (leída desde CLIENTE): un cliente (normal o Plus) puede haber hecho muchos pedidos o ninguno.
- **(0,1) junto a CLIENTE** (leída desde PEDIDO): un pedido lo hace como mucho un cliente identificado. El mínimo es 0 para recoger los **pedidos de clientes anónimos**, que no se asocian a ningún cliente.

### ES UN (CLIENTE — CLIENTE_PLUS) · especialización

CLIENTE_PLUS es una especialización de CLIENTE.

- **Parcial**: no todos los clientes son Plus. Un cliente normal es un CLIENTE que no pertenece al subtipo.
- Como solo hay un subtipo, no hace falta indicar si es disjunta o solapada.
- Un cliente normal que se apunte al programa pasa a ser también CLIENTE_PLUS: conserva su `id_cliente` y su histórico de pedidos y se le añade `fecha_ingreso`.

### RECIBE (CLIENTE_PLUS — BONIFICACION) · 1:N · identificadora

Une cada bonificación mensual con su cliente Plus. Solo los clientes Tajinaste Plus reciben bonificaciones. Es identificadora porque BONIFICACION es débil: su clave es `(id_cliente, año, mes)`.

- **(0,N) junto a BONIFICACION** (leída desde CLIENTE_PLUS): un cliente Plus acumula como mucho una bonificación por mes (R10). Puede no tener ninguna si aún no ha comprado lo suficiente.
- **(1,1) junto a CLIENTE_PLUS** (leída desde BONIFICACION): cada bonificación pertenece a un único cliente y no existe sin él.

## 4. Restricciones semánticas

| # | Restricción |
|---|---|
| R1 | `latitud` ∈ [-90, 90] y `longitud` ∈ [-180, 180] en VIVERO y ZONA. |
| R2 | Las coordenadas de una zona deben quedar dentro del recinto (o muy cerca) de su vivero. |
| R3 | `cantidad_disponible` ≥ 0. `precio` > 0. `importe_total` ≥ 0. En LINEA_PEDIDO, `cantidad` > 0 y `precio_unitario` ≥ 0. |
| R4 | En OCUPA_PUESTO, `fecha_fin` es nula o mayor o igual que `fecha_inicio`. |
| R5 | **Un empleado nunca tiene dos destinos a la vez**: los periodos `[fecha_inicio, fecha_fin]` de un mismo empleado no pueden solaparse. |
| R6 | Como consecuencia de R5, cada empleado tiene como máximo un periodo abierto (`fecha_fin` nula), que es su destino actual. |
| R7 | Para campañas y bonificaciones de un cliente Plus solo cuentan sus pedidos con `PEDIDO.fecha` ≥ `CLIENTE_PLUS.fecha_ingreso`. Los anteriores, hechos como cliente normal, se conservan pero no cuentan para el programa. |
| R8 | El empleado que gestiona un pedido debe estar destinado en algún vivero (tener un periodo de OCUPA_PUESTO vigente) en la fecha del pedido. |
| R9 | `volumen_compras` de una bonificación = suma de `importe_total` de los pedidos del cliente Plus cuyo año y mes coinciden con los de la bonificación. |
| R9b | `importe_total` de un pedido = Σ (`cantidad` × `precio_unitario`) de sus líneas. |
| R10 | Un cliente tiene como mucho una bonificación por (año, mes). Esto ya lo garantiza la clave de la entidad débil. El mes de la bonificación no puede ser anterior al de `fecha_ingreso` ni posterior al mes actual. |
| R11 | `importe_bonificacion` ≥ 0 y nunca mayor que `volumen_compras`. Se calcula con la política de tramos que fije la empresa. |
| R12 | `nif` de CLIENTE es único (clave alternativa). |
| R13 | Un pedido anónimo (sin cliente en REALIZA) no cuenta para ninguna bonificación ni campaña de fidelización. |
| R14 | Un producto aparece como mucho una vez en cada pedido: si se compran varias unidades se usa `cantidad`. Esto ya lo garantiza que LINEA_PEDIDO sea una relación N:M. |
