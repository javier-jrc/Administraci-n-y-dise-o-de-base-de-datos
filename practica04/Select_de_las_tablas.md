# Reporte de Verificación de Datos - Base de Datos `viveros`

**Fecha de ejecución:** 14 de Octubre de 2026  
**Usuario del sistema:** `usuario@ubuntu`  
**Base de Datos:** `viveros`  
**Estado:** Confirmación e inspección de inserción exitosa.

---

## 💻 Comando Ejecutado

```bash
sudo -u postgres psql -d viveros -c "
SELECT '--- VIVERO ---' AS tabla; SELECT * FROM vivero;
SELECT '--- ZONA ---' AS tabla; SELECT * FROM zona;
SELECT '--- PRODUCTO ---' AS tabla; SELECT * FROM producto;
SELECT '--- STOCK ---' AS tabla; SELECT * FROM stock;
SELECT '--- EMPLEADO ---' AS tabla; SELECT * FROM empleado;
SELECT '--- OCUPA_PUESTO ---' AS tabla; SELECT * FROM ocupa_puesto;
SELECT '--- CLIENTE ---' AS tabla; SELECT * FROM cliente;
SELECT '--- CLIENTE_PLUS ---' AS tabla; SELECT * FROM cliente_plus;
SELECT '--- PEDIDO ---' AS tabla; SELECT * FROM pedido;
SELECT '--- LINEA_PEDIDO ---' AS tabla; SELECT * FROM linea_pedido;
SELECT '--- BONIFICACION ---' AS tabla; SELECT * FROM bonificacion;
"
```

---

## 📊 Registros por Tabla

### 1. Vivero
| id_vivero | nombre | direccion | latitud | longitud |
| :---: | :--- | :--- | :---: | :---: |
| 1 | Vivero Central Anaga | Carretera General del Norte 45, La Laguna | 28.485300 | -16.320100 |
| 2 | Jardines del Sur | Avenida de Chayofita 12, Los Cristianos | 28.053100 | -16.716200 |
| 3 | Vivero Orotava | Camino del Botánico 8, La Orotava | 28.411200 | -16.523400 |
| 4 | Plantas Tajinaste | Calle San Clemente 3, Santa Cruz | 28.468200 | -16.254600 |
| 5 | El Vergel del Teide | Avenida Los Menceyes 102, Candelaria | 28.354100 | -16.371200 |

### 2. Zona
| id_vivero | cod_zona | nombre | latitud | longitud |
| :---: | :---: | :--- | :---: | :---: |
| 1 | ZN01 | Exterior Arbustos | 28.485400 | -16.320200 |
| 1 | ZN02 | Invernadero Flores | 28.485500 | -16.320300 |
| 2 | ZN01 | Zona Cactus y Suculentas | 28.053200 | -16.716300 |
| 3 | ZN01 | Herramientas y Abonos | 28.411300 | -16.523500 |
| 4 | ZN01 | Decoración e Interiores | 28.468300 | -16.254700 |

### 3. Producto
| id_producto | nombre | categoria | precio (€) | descripcion |
| :---: | :--- | :---: | :---: | :--- |
| 1 | Tajinaste Rojo (Echium wildpretii) | Planta | 18.50 | Planta endémica de las Islas Canarias de flores rojas. |
| 2 | Palmera Canaria (Phoenix canariensis) | Planta | 45.00 | Ejemplar joven en maceta de 10 litros. |
| 3 | Tijeras de Podar Profesionales | Jardinería | 24.99 | Tijeras de acero inoxidable con mango ergonómico. |
| 4 | Macetero de Cerámica Artesanal | Decoración | 32.00 | Macetero decorativo pintado a mano. |
| 5 | Sustrato Universal 50L | Jardinería | 12.50 | Sustrato rico en nutrientes apto para todo tipo de plantas. |

### 4. Stock
| id_vivero | cod_zona | id_producto | cantidad_disponible |
| :---: | :---: | :---: | :---: |
| 1 | ZN01 | 1 | 50 |
| 1 | ZN02 | 2 | 20 |
| 2 | ZN01 | 1 | 15 |
| 3 | ZN01 | 3 | 100 |
| 4 | ZN01 | 4 | 30 |

### 5. Empleado
| dni | nombre | apellidos |
| :---: | :--- | :--- |
| 12345678A | Carlos | González Pérez |
| 23456789B | María | Rodríguez Hernández |
| 34567890C | Juan | Martín Delgado |
| 45678901D | Laura | Díaz Castro |
| 56789012E | Pedro | Álvarez Morales |

### 6. Ocupa Puesto
| dni | id_vivero | cod_zona | fecha_inicio | fecha_fin | tarea |
| :---: | :---: | :---: | :---: | :---: | :--- |
| 12345678A | 1 | ZN01 | 2024-01-15 | *NULL* | Encargado de Riego y Mantenimiento |
| 23456789B | 1 | ZN02 | 2024-02-01 | *NULL* | Especialista en Botánica de Invernadero |
| 34567890C | 2 | ZN01 | 2024-03-10 | *NULL* | Atención al Cliente y Ventas |
| 45678901D | 3 | ZN01 | 2024-05-01 | *NULL* | Control de Inventario y Almacén |
| 56789012E | 4 | ZN01 | 2024-06-15 | *NULL* | Mantenimiento de Exposición |

### 7. Cliente
| id_cliente | nif | nombre | apellidos |
| :---: | :---: | :--- | :--- |
| 1 | 78901234F | Ana | Sánchez Torres |
| 2 | 89012345G | David | López Ramos |
| 3 | 90123456H | Elena | Benítez Suárez |
| 4 | 01234567J | Jorge | Gómez Santana |
| 5 | 11223344K | Marta | Cabrera Fuentes |

### 8. Cliente Plus
| id_cliente | fecha_ingreso |
| :---: | :---: |
| 1 | 2024-01-10 |
| 2 | 2024-02-15 |
| 3 | 2024-03-20 |
| 4 | 2024-04-05 |
| 5 | 2024-05-12 |

### 9. Pedido
| id_pedido | fecha | importe_total (€) | dni_empleado | id_cliente |
| :---: | :---: | :---: | :---: | :---: |
| 1 | 2026-10-01 | 37.00 | 12345678A | 1 |
| 2 | 2026-10-02 | 45.00 | 23456789B | 2 |
| 3 | 2026-10-03 | 24.99 | 34567890C | 3 |
| 4 | 2026-10-04 | 64.00 | 45678901D | 4 |
| 5 | 2026-10-05 | 25.00 | 56789012E | 5 |

### 10. Línea Pedido
| id_pedido | id_producto | cantidad | precio_unitario (€) | subtotal (€) |
| :---: | :---: | :---: | :---: | :---: |
| 1 | 1 | 2 | 18.50 | 37.00 |
| 2 | 2 | 1 | 45.00 | 45.00 |
| 3 | 3 | 1 | 24.99 | 24.99 |
| 4 | 4 | 2 | 32.00 | 64.00 |
| 5 | 5 | 2 | 12.50 | 25.00 |

### 11. Bonificación
| id_cliente | anio | mes | volumen_compras (€) | importe_bonificacion (€) |
| :---: | :---: | :---: | :---: | :---: |
| 1 | 2026 | 9 | 150.00 | 15.00 |
| 2 | 2026 | 9 | 200.00 | 20.00 |
| 3 | 2026 | 9 | 80.00 | 8.00 |
| 4 | 2026 | 9 | 310.00 | 31.00 |
| 5 | 2026 | 9 | 120.00 | 12.00 |