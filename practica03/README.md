# Modelo Entidad/Relación - Tajinaste S.A. (Viveros)

## 1. Descripción de las Entidades

* **`VIVERO`**: Entidad fuerte que representa los centros y viveros pertenecientes a la red de Tajinaste S.A.[cite: 1]
* **`ZONA`**: Entidad débil cuya existencia depende de `VIVERO`. Representa las subdivisiones físicas dentro de un vivero (ej. almacén, zona exterior, zona de cajas)[cite: 1].
* **`PRODUCTO`**: Entidad fuerte que representa los artículos comercializados por la empresa (plantas, artículos de jardinería y elementos de decoración)[cite: 1].
* **`EMPLEADO`**: Entidad fuerte que contiene la información del personal contratado por Tajinaste S.A.[cite: 1]
* **`CLIENTE_PLUS`**: Entidad fuerte que representa a los clientes registrados dentro del programa de fidelización *Tajinaste Plus*[cite: 1].
* **`PEDIDO`**: Entidad fuerte que almacena las compras efectuadas por clientes *Tajinaste Plus* y gestionadas por un empleado[cite: 1].
* **`BONIFICACION`**: Entidad débil dependiente de `CLIENTE_PLUS` que registra los incentivos otorgados mensualmente en función del volumen de compras[cite: 1].

---

## 2. Descripción de Atributos y Ejemplos de Dominio

### Entidades

#### VIVERO[cite: 1]
* **`id_vivero`**: Clave Primaria (PK). Tipo cadena/alfanumérico único. *Ejemplo:* `"VIV-001"`[cite: 1].
* **`nombre`**: Cadena de caracteres. *Ejemplo:* `"Vivero Anaga"`[cite: 1].
* **`direccion`**: Cadena de caracteres. *Ejemplo:* `"Ctra. TF-12 Km 4, La Laguna"`[cite: 1].
* **`latitud`**: Número decimal (Float) en rango [-90.0, 90.0]. *Ejemplo:* `28.4811`[cite: 1].
* **`longitud`**: Número decimal (Float) en rango [-180.0, 180.0]. *Ejemplo:* `-16.3225`[cite: 1].

#### ZONA[cite: 1]
* **`cod_zona`**: Clave Parcial (PK local). Cadena de caracteres identificadora dentro del vivero. *Ejemplo:* `"ZON-EXT-01"`[cite: 1].
* **`nombre`**: Cadena de caracteres. *Ejemplo:* `"Zona Exterior Planta Sombra"`[cite: 1].
* **`latitud`**: Número decimal (Float) en rango [-90.0, 90.0]. *Ejemplo:* `28.4815`[cite: 1].
* **`longitud`**: Número decimal (Float) en rango [-180.0, 180.0]. *Ejemplo:* `-16.3221`[cite: 1].

#### PRODUCTO[cite: 1]
* **`id_producto`**: Clave Primaria (PK). Cadena/alfanumérico único. *Ejemplo:* `"PROD-1024"`[cite: 1].
* **`nombre`**: Cadena de caracteres. *Ejemplo:* `"Tajinaste Rojo (Echium wildpretii)"`[cite: 1].
* **`categoria`**: Cadena de caracteres / Enumerado (`Planta`, `Jardinería`, `Decoración`). *Ejemplo:* `"Planta"`[cite: 1].
* **`precio`**: Decimal positivo en euros (€). *Ejemplo:* `18.50`[cite: 1].
* **`descripcion`**: Texto explicativo. *Ejemplo:* `"Especie endémica de las Cañadas del Teide"`[cite: 1].

#### EMPLEADO[cite: 1]
* **`dni_empleado`**: Clave Primaria (PK). Cadena con formato DNI/NIE (8 dígitos + letra). *Ejemplo:* `"12345678A"`[cite: 1].
* **`nombre`**: Cadena de caracteres. *Ejemplo:* `"Laura"`[cite: 1].
* **`apellidos`**: Cadena de caracteres. *Ejemplo:* `"García Hernández"`[cite: 1].
* **`telefono`**: Cadena de caracteres telefónica. *Ejemplo:* `"+34 600112233"`[cite: 1].

#### CLIENTE_PLUS[cite: 1]
* **`id_cliente`**: Clave Primaria (PK). Cadena/alfanumérico único. *Ejemplo:* `"CLI-9041"`[cite: 1].
* **`nif`**: Cadena con formato NIF/NIE. *Ejemplo:* `"87654321B"`[cite: 1].
* **`nombre`**: Cadena de caracteres. *Ejemplo:* `"Carlos"`[cite: 1].
* **`apellidos`**: Cadena de caracteres. *Ejemplo:* `"Rodríguez Martín"`[cite: 1].
* **`fecha_ingreso`**: Fecha en formato `YYYY-MM-DD`. *Ejemplo:* `"2024-03-15"`[cite: 1].

#### PEDIDO[cite: 1]
* **`id_pedido`**: Clave Primaria (PK). Cadena/alfanumérico único. *Ejemplo:* `"PED-2026-0891"`[cite: 1].
* **`fecha`**: Fecha y hora en formato `YYYY-MM-DD HH:MM`. *Ejemplo:* `"2026-10-01 10:30"`[cite: 1].
* **`importe_total`**: Decimal positivo en euros (€). *Ejemplo:* `245.80`[cite: 1].

#### BONIFICACION[cite: 1]
* **`mes`**: Clave Parcial. Entero en rango `[1..12]`. *Ejemplo:* `9`[cite: 1].
* **`año`**: Clave Parcial. Entero de 4 dígitos. *Ejemplo:* `2026`[cite: 1].
* **`volumen_compras`**: Decimal positivo acumulado (€). *Ejemplo:* `520.00`[cite: 1].
* **`monto_bonif`**: Decimal positivo otorgado (€). *Ejemplo:* `26.00`[cite: 1].

---

### Atributos de Relaciones[cite: 1]

#### STOCK_ZONA[cite: 1]
* **`stock_disponible`**: Entero no negativo (≥ 0). Cantidad disponible de un producto en una zona. *Ejemplo:* `45`[cite: 1].

#### HISTORICO_PUESTO[cite: 1]
* **`fecha_inicio`**: Fecha en formato `YYYY-MM-DD`. *Ejemplo:* `"2026-01-15"`[cite: 1].
* **`fecha_fin`**: Fecha en formato `YYYY-MM-DD` (o `NULL` si el puesto sigue activo). *Ejemplo:* `"2026-06-30"`[cite: 1].
* **`tarea`**: Cadena de caracteres descriptiva de la función desempeñada. *Ejemplo:* `"Atención en almacén y reposición"`[cite: 1].
* **`productividad`**: Valor numérico / Métrica de desempeño. *Ejemplo:* `8.75`[cite: 1].

---

## 3. Descripción y Cardinalidades de las Relaciones

1. **`TIENE` (`VIVERO` — `ZONA`)**[cite: 1]:
   * **Tipo**: Relación débil / identificadora[cite: 1].
   * **Cardinalidad**: `VIVERO (1,1) --- (1,N) ZONA`[cite: 1].
   * **Explicación**: Un vivero debe tener asignada al menos una zona y puede tener muchas[cite: 1]. Cada zona pertenece de forma única y obligatoria a un solo vivero[cite: 1].

2. **`STOCK_ZONA` (`ZONA` — `PRODUCTO`)**[cite: 1]:
   * **Tipo**: Relación $N:M$ (Muchos a Muchos)[cite: 1].
   * **Cardinalidad**: `ZONA (0,N) --- (0,N) PRODUCTO`[cite: 1].
   * **Explicación**: Una zona puede almacenar varios productos o ninguno de forma temporal[cite: 1]. Un producto puede estar disponible en cero o múltiples zonas de la red de viveros[cite: 1]. Contiene el atributo propio `stock_disponible`[cite: 1].

3. **`HISTORICO_PUESTO` (`EMPLEADO` — `ZONA`)**[cite: 1]:
   * **Tipo**: Relación $N:M$ con registro histórico[cite: 1].
   * **Cardinalidad**: `EMPLEADO (1,N) --- (0,N) ZONA`[cite: 1].
   * **Explicación**: Un empleado realiza funciones en distintas zonas/viveros según la época del año, manteniendo un histórico[cite: 1]. En una zona trabajan diversos empleados a lo largo del tiempo[cite: 1]. Incluye información de las fechas (`fecha_inicio`, `fecha_fin`), la `tarea` y la `productividad`[cite: 1].

4. **`GESTIONA` (`EMPLEADO` — `PEDIDO`)**[cite: 1]:
   * **Tipo**: Relación $1:N$ (Uno a Muchos)[cite: 1].
   * **Cardinalidad**: `EMPLEADO (0,N) --- (1,1) PEDIDO`[cite: 1].
   * **Explicación**: Un empleado puede ser el responsable de gestionar cero o muchos pedidos[cite: 1]. Cada pedido gestionado tiene obligatoriamente **un único empleado responsable**[cite: 1].

5. **`REALIZA` (`CLIENTE_PLUS` — `PEDIDO`)**[cite: 1]:
   * **Tipo**: Relación $1:N$ (Uno a Muchos)[cite: 1].
   * **Cardinalidad**: `CLIENTE_PLUS (0,N) --- (1,1) PEDIDO`[cite: 1].
   * **Explicación**: Un cliente registrado en el programa *Tajinaste Plus* puede realizar múltiples pedidos en el tiempo o ninguno[cite: 1]. Todo pedido pertenece a un único cliente[cite: 1].

6. **`RECIBE` (`CLIENTE_PLUS` — `BONIFICACION`)**[cite: 1]:
   * **Tipo**: Relación débil / identificadora[cite: 1].
   * **Cardinalidad**: `CLIENTE_PLUS (1,1) --- (0,N) BONIFICACION`[cite: 1].
   * **Explicación**: Un cliente puede acumular mensualmente bonificaciones en función de sus compras[cite: 1]. Cada registro de bonificación pertenece a un cliente específico[cite: 1].

---

## 4. Restricciones Semánticas Propuestas

1. **Unicidad de Destino Simultáneo**: Para cualquier fecha $T$, un mismo empleado no puede tener simultáneamente dos registros activos en `HISTORICO_PUESTO`[cite: 1].
2. **Jerarquía Geográfica**: La ubicación georreferenciada (`latitud`, `longitud`) de cada `ZONA` debe estar contenida dentro de los límites geográficos del `VIVERO` al que está asignada[cite: 1].
3. **Coherencia de Pedidos Tajinaste Plus**: La `fecha` de cualquier registro de `PEDIDO` debe ser obligatoriamente igual o posterior a la `fecha_ingreso` del cliente en el programa *Tajinaste Plus*[cite: 1].
4. **Cálculo de Bonificación Mensual**: El `volumen_compras` asignado a una `BONIFICACION` mensual debe corresponder exactamente a la suma de los importes (`importe_total`) de todos los pedidos efectuados por dicho cliente durante ese mes y año[cite: 1].