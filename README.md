# Practica 4
## Integrantes:
- Daniel Jaramillo
- Nayeli Leiva
- Bryan Maldonado
- Donoban Ramón
- Ricardo Villareal
## Ejercicio 21
## 1. Cargar el CSV como tabla

Se importó el archivo `products.csv` en Excel y se dejó como tabla con las columnas:

<img width="1285" height="718" alt="Screenshot 2026-05-05 190606" src="https://github.com/user-attachments/assets/41390275-ec01-4ae4-bce7-59433adb258d" />


## 2. Crear la dimensión `dim_category`

Se creó una nueva hoja llamada `dim_category`.

Se copió la columna `category` desde la tabla original.

Luego se aplicó:

**Quitar duplicados**

Después se agregó un identificador numérico:

<img width="571" height="460" alt="Screenshot 2026-05-05 190956" src="https://github.com/user-attachments/assets/62998de4-8c9d-45ec-9861-910e31bffec7" />


## 3. Crear la dimensión `dim_subcategory`

Se creó una nueva hoja llamada `dim_subcategory`.

Se copió la columna `sub_category` desde la tabla original.

Luego se aplicó:

**Quitar duplicados**

Después se agregó un identificador numérico:

<img width="514" height="684" alt="Screenshot 2026-05-05 191153" src="https://github.com/user-attachments/assets/01a3fbb7-e538-4416-927c-0a10a32a879d" />


## 4. Crear la tabla `fact_products`

Se creó una hoja llamada `fact_products`.

Inicialmente se copió la tabla original y luego se reemplazaron los textos de categoría y subcategoría por sus IDs.

La tabla final quedó así:

| product_id | product_name | category_id | subcategory_id |
|---|---|---|---|

Para traer el ID de categoría se usó:

```excel
=BUSCARX(C2;dim_category!$B:$B;dim_category!$A:$A)
```

Para traer el ID de subcategoría se usó:

```excel
=BUSCARX(D2;dim_subcategory!$B:$B;dim_subcategory!$A:$A)
```

Luego se eliminaron las columnas de texto:

<img width="1127" height="669" alt="Screenshot 2026-05-05 194025" src="https://github.com/user-attachments/assets/3a27bbe2-0adf-4d19-976b-3248cb72bbad" />


## 5. Agregar las tablas al modelo de datos

Se agregaron al modelo de datos las siguientes tablas:

| Tabla |
|---|
| fact_products |
| dim_category |
| dim_subcategory |

Ruta usada:

**Power Pivot → Agregar al modelo de datos**


## 5. Crear relaciones

Se crearon las siguientes relaciones:

| Tabla dimensión | Campo | Tabla de hechos | Campo |
|---|---|---|---|
| dim_category | category_id | fact_products | category_id |
| dim_subcategory | subcategory_id | fact_products | subcategory_id |

El modelo estrella declara que fact_products es la tabla central y se relaciona con dos dimensiones: dim_category y dim_subcategory.

La relación indica que una categoría puede tener muchos productos y una subcategoría también puede tener muchos productos

<img width="1159" height="632" alt="Screenshot 2026-05-05 194825" src="https://github.com/user-attachments/assets/6bbc0468-02a4-469f-86a5-fac098679323" />




### Ejercicio 22
Aqui explicar los scripts que usamos para sacar las tablas de dimensiones, hechos y como las poblamos
## 22.a Normalización y Diagrama
---

## 1. Carga de datos inicial

1. Abrir Excel  
2. Ir a **Datos → Obtener datos → Desde texto/CSV**  
3. Seleccionar el archivo: `Tabla_Desnormalizada_Ventas.csv`  
4. En la ventana de carga:
   - Seleccionar: **Transformar datos**

<img width="1149" height="863" alt="image" src="https://github.com/user-attachments/assets/c1126112-4aea-480a-8dbb-8f1977bd9df3" />


---

## 2. Preparación en Power Query

Una vez dentro del Editor de Power Query:

- Verificar tipos de datos
- Promover encabezados (si no está hecho)

<img width="1291" height="800" alt="image" src="https://github.com/user-attachments/assets/e93a8bdf-e545-46a2-ba5f-6b8eb701367c" />


---

## 3. Creación de tablas del modelo estrella

Se debe duplicar la tabla base para crear:

- DimProduct  
- DimCustomer  
- DimDate  
- DimDateShip  
- FactSales  

### Duplicar tabla
1. Clic derecho sobre la consulta
2. Seleccionar **Duplicar**

<img width="376" height="364" alt="image" src="https://github.com/user-attachments/assets/f9f2b4f9-3929-4615-ba0e-0baaf72f47fd" />

---

## 4. Construcción de DimProduct

### Columnas:
- ProductKey  
- Product Code  
- Product Name  
- List Price  
- Color  
- Size  
- Category  
- Subcategory  

### Pasos:
1. Eliminar columnas innecesarias  
2. Seleccionar `ProductKey`  
3. **Inicio → Quitar duplicados**

<img width="914" height="322" alt="image" src="https://github.com/user-attachments/assets/93e0836f-1dce-4ec9-8192-27550d6feac6" />


---

## 5. Construcción de DimCustomer

### Columnas:
- CustomerKey  
- Birth Date  
- Marital Status  
- Gender  
- Income  
- Children  
- Home Owner  
- Cars  

### Pasos:
1. Eliminar columnas no relacionadas  
2. Quitar duplicados por `CustomerKey`

---

## 6. Construcción de DimDate

Ir a: **Agregar columna → Columna personalizada**

- Año:
    Date.Year([Order Date])

- Mes:
    Date.Month([Order Date])

- Nombre del mes:
    Date.MonthName([Order Date])


---

## 7. Construcción de DimDateShip

### Columnas:
- ShipDateKey  
- Ship Date  

### Columnas adicionales:

- Año:
    Date.Year([Ship Date])

- Mes:
    Date.Month([Ship Date])

---

## 8. Construcción de FactSales

### Columnas:
- OrderNumber  
- OrderLineNumber  
- ProductKey  
- CustomerKey  
- OrderDateKey  
- ShipDateKey  
- Quantity  
- UnitPrice  
- ProductCost  
- SalesAmount  

### Pasos:
- Eliminar todas las columnas descriptivas  
- Mantener solo claves y métricas  

---

## 9. Cargar al modelo de datos

Para cada tabla:

1. Clic en **Cerrar y cargar**  
2. Seleccionar:
   - Solo crear conexión  
   - Agregar al modelo de datos  

![Carga al modelo](https://github.com/user-attachments/assets/22bb6f8a-28c5-49b7-a234-eb68c7780f89)

---

## 10. Creación del modelo en Power Pivot

1. Ir a **Power Pivot → Administrar**  
2. Cambiar a **Vista de diagrama**

---

## 11. Crear relaciones

Arrastrar:

- FactSales[ProductKey] → DimProduct[ProductKey]  
- FactSales[CustomerKey] → DimCustomer[CustomerKey]  
- FactSales[OrderDateKey] → DimDate[OrderDateKey]  
- FactSales[ShipDateKey] → DimDateShip[ShipDateKey]  

---

## 12. Resultado final

El modelo debe tener forma de estrella:

![Modelo estrella](https://github.com/user-attachments/assets/20b86740-0c0c-437c-a532-62157aa1d577)

---

##  22.b Contestar las siguientes preguntas en SQL

### 1. ¿Cuántas ventas se realizaron por categoría de producto y mes?
![Consulta 1](https://github.com/user-attachments/assets/75323d99-c35f-47d3-8ac9-af3ec306310f)

---

### 2. ¿Cuál es el ingreso total (ventas) por cliente y género?
![Consulta 2](https://github.com/user-attachments/assets/ed628bab-6cce-4f47-8ca1-8fa265fce8da)

---

### 3. ¿Cuál es la cantidad total vendida por producto?
![Consulta 3](https://github.com/user-attachments/assets/fef94b9f-7557-4cfd-b890-76fa0180ae82)

---

### 4. ¿Cuál fue la cantidad enviada por mes de envío?
![Consulta 4](https://github.com/user-attachments/assets/1de1b786-cba2-472c-b095-107cce6d02b4)

---

### 5. ¿Cuánto se vendió por tamaño de producto y por estado civil del cliente?
![Consulta 5](https://github.com/user-attachments/assets/31bd8afa-5c08-4750-9bba-3da29965cc0c)

