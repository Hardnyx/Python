# `duckdb`

## Propósito

`duckdb` es una librería externa para usar DuckDB desde Python.

DuckDB es una base de datos SQL analítica en proceso. Se utiliza para consultar, transformar y analizar datos tabulares de forma local, rápida y sin necesidad de levantar un servidor de base de datos separado.

Es especialmente útil para trabajar con archivos como CSV, Parquet y JSON, así como con DataFrames de `pandas`, `polars` y datos en formato Arrow.

## Naturaleza de la librería

`duckdb` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install duckdb
```

La importación habitual es:

```python
import duckdb
```

DuckDB no requiere un servidor separado para empezar a trabajar. Se ejecuta dentro del mismo proceso de Python.

## Idea central

La idea principal de DuckDB es permitir consultas SQL analíticas sobre datos locales o en memoria.

Flujo típico:

```text
archivo / DataFrame / tabla
-> DuckDB
-> consulta SQL
-> resultado como tabla, DataFrame o archivo
```

Ejemplo mínimo:

```python
import duckdb

result = duckdb.sql("SELECT 42 AS value")

print(result.fetchall())
```

Salida conceptual:

```text
[(42,)]
```

## Cuándo usar DuckDB

Conviene usar DuckDB cuando se necesita:

```text
consultar archivos CSV con SQL
consultar archivos Parquet
analizar datos locales
trabajar sin servidor de base de datos
hacer consultas analíticas rápidas
combinar SQL con pandas
combinar SQL con Polars
crear una base local persistente
hacer prototipos de análisis
procesar datos tabulares medianos o grandes
convertir datos entre formatos
```

## Cuándo no usar DuckDB

No suele ser la mejor opción cuando se necesita:

```text
base transaccional multiusuario de producción
servidor central compartido por muchas aplicaciones
alta concurrencia de escritura
sistema OLTP clásico
autenticación y permisos complejos de servidor
aplicación web con muchos usuarios escribiendo al mismo tiempo
```

Para esos casos suelen corresponder bases como:

```text
PostgreSQL
MySQL
SQL Server
MongoDB
Redis
```

DuckDB destaca más como motor analítico local que como servidor transaccional tradicional.

## Instalación

Instalación básica:

```bash
python -m pip install duckdb
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
duckdb==1.x.x
```

La versión exacta puede variar según el entorno.

## Verificación

```python
import duckdb

print(duckdb.__version__)
```

## Consulta básica

```python
import duckdb

result = duckdb.sql("SELECT 1 + 1 AS result")

print(result.fetchall())
```

Salida conceptual:

```text
[(2,)]
```

## `duckdb.sql()`

`duckdb.sql()` permite ejecutar una consulta usando una base en memoria administrada por el módulo.

```python
import duckdb

duckdb.sql("SELECT 'Python' AS language").show()
```

Es una forma directa de hacer consultas rápidas.

## Resultado como relación

Una consulta de DuckDB devuelve una relación.

```python
result = duckdb.sql("SELECT 10 AS value")

print(type(result))
```

Una relación puede mostrarse, transformarse o convertirse a otros formatos.

## Mostrar resultado

```python
duckdb.sql("SELECT 10 AS value").show()
```

## Obtener datos como lista de tuplas

```python
result = duckdb.sql("SELECT 10 AS value")

rows = result.fetchall()

print(rows)
```

## Obtener un solo resultado

```python
result = duckdb.sql("SELECT 10 AS value")

row = result.fetchone()

print(row)
```

## Convertir resultado a pandas

```python
import duckdb

df = duckdb.sql("""
    SELECT
        1 AS id,
        'Laptop' AS product,
        3500 AS amount
""").df()

print(df)
```

## Convertir resultado a Arrow

```python
import duckdb

arrow_table = duckdb.sql("""
    SELECT
        1 AS id,
        'Laptop' AS product
""").arrow()

print(arrow_table)
```

## Base en memoria

Por defecto, DuckDB puede trabajar en memoria.

```python
import duckdb

connection = duckdb.connect()

connection.execute("""
    CREATE TABLE products (
        id INTEGER,
        name VARCHAR,
        price DOUBLE
    )
""")
```

La base en memoria desaparece cuando termina el proceso, salvo que los datos se exporten o se guarden en un archivo persistente.

## Base persistente

Para guardar datos en un archivo DuckDB:

```python
import duckdb

connection = duckdb.connect("app.duckdb")
```

Esto crea o abre el archivo:

```text
app.duckdb
```

Ejemplo:

```python
import duckdb

connection = duckdb.connect("app.duckdb")

connection.execute("""
    CREATE TABLE IF NOT EXISTS products (
        id INTEGER,
        name VARCHAR,
        price DOUBLE
    )
""")

connection.close()
```

## `connect()`

`duckdb.connect()` crea una conexión.

```python
connection = duckdb.connect("app.duckdb")
```

Puede usarse:

```text
duckdb.connect()              -> base en memoria
duckdb.connect("app.duckdb")  -> base persistente en archivo
```

## Cerrar conexión

```python
connection.close()
```

Cerrar la conexión es una buena práctica cuando ya no se usará.

## Ejecutar SQL con conexión

```python
import duckdb

connection = duckdb.connect("app.duckdb")

result = connection.execute("SELECT 42 AS value").fetchall()

print(result)

connection.close()
```

## Crear tabla

```python
import duckdb

connection = duckdb.connect("app.duckdb")

connection.execute("""
    CREATE TABLE IF NOT EXISTS products (
        id INTEGER,
        name VARCHAR,
        price DOUBLE,
        stock INTEGER
    )
""")

connection.close()
```

## Insertar datos

```python
import duckdb

connection = duckdb.connect("app.duckdb")

connection.execute("""
    INSERT INTO products (id, name, price, stock)
    VALUES (1, 'Laptop', 3500, 5)
""")

connection.close()
```

## Insertar con parámetros

DuckDB permite usar parámetros para evitar concatenar valores dentro del SQL.

```python
import duckdb

connection = duckdb.connect("app.duckdb")

connection.execute(
    """
    INSERT INTO products (id, name, price, stock)
    VALUES (?, ?, ?, ?)
    """,
    [1, "Laptop", 3500, 5]
)

connection.close()
```

## Consultar datos

```python
import duckdb

connection = duckdb.connect("app.duckdb")

rows = connection.execute("""
    SELECT id, name, price, stock
    FROM products
    ORDER BY name
""").fetchall()

print(rows)

connection.close()
```

## Convertir consulta a DataFrame

```python
import duckdb

connection = duckdb.connect("app.duckdb")

df = connection.execute("""
    SELECT id, name, price, stock
    FROM products
""").df()

print(df)

connection.close()
```

## Leer CSV directamente

DuckDB puede consultar archivos CSV directamente desde SQL.

```python
import duckdb

df = duckdb.sql("""
    SELECT *
    FROM read_csv_auto('ventas.csv')
""").df()

print(df)
```

También puede consultarse el archivo directamente:

```python
df = duckdb.sql("""
    SELECT *
    FROM 'ventas.csv'
""").df()
```

## Leer CSV con opciones

```python
import duckdb

df = duckdb.sql("""
    SELECT *
    FROM read_csv(
        'ventas.csv',
        delim = ';',
        header = true
    )
""").df()

print(df)
```

## Crear tabla desde CSV

```python
import duckdb

connection = duckdb.connect("app.duckdb")

connection.execute("""
    CREATE TABLE ventas AS
    SELECT *
    FROM read_csv_auto('ventas.csv')
""")

connection.close()
```

## Leer Parquet

DuckDB es especialmente útil para trabajar con archivos Parquet.

```python
import duckdb

df = duckdb.sql("""
    SELECT *
    FROM 'ventas.parquet'
""").df()

print(df)
```

También puede usarse:

```python
df = duckdb.sql("""
    SELECT *
    FROM read_parquet('ventas.parquet')
""").df()
```

## Leer varios Parquet

```python
import duckdb

df = duckdb.sql("""
    SELECT *
    FROM read_parquet('data/*.parquet')
""").df()

print(df)
```

Este patrón es útil cuando una carpeta contiene varios archivos particionados.

## Leer JSON

```python
import duckdb

df = duckdb.sql("""
    SELECT *
    FROM read_json_auto('datos.json')
""").df()

print(df)
```

## Consultar DataFrame de pandas

DuckDB puede consultar directamente un DataFrame de pandas existente.

```python
import duckdb
import pandas as pd

df = pd.DataFrame({
    "product": ["Laptop", "Mouse", "Teclado"],
    "amount": [3500, 80, 150]
})

result = duckdb.sql("""
    SELECT
        product,
        amount
    FROM df
    WHERE amount > 100
""").df()

print(result)
```

El nombre `df` queda disponible para la consulta porque existe como variable de Python.

## Consultar DataFrame con conexión

```python
import duckdb
import pandas as pd

sales = pd.DataFrame({
    "product": ["Laptop", "Mouse", "Teclado"],
    "amount": [3500, 80, 150]
})

connection = duckdb.connect()

result = connection.execute("""
    SELECT *
    FROM sales
    WHERE amount >= 100
""").df()

print(result)

connection.close()
```

## Registrar DataFrame

También puede registrarse un DataFrame con un nombre explícito.

```python
import duckdb
import pandas as pd

sales = pd.DataFrame({
    "product": ["Laptop", "Mouse"],
    "amount": [3500, 80]
})

connection = duckdb.connect()

connection.register("sales_view", sales)

df = connection.execute("""
    SELECT *
    FROM sales_view
""").df()

print(df)

connection.close()
```

## Consultar Polars

DuckDB puede integrarse con Polars.

```python
import duckdb
import polars as pl

df = pl.DataFrame({
    "product": ["Laptop", "Mouse"],
    "amount": [3500, 80]
})

result = duckdb.sql("""
    SELECT *
    FROM df
    WHERE amount > 100
""").pl()

print(result)
```

## Exportar a CSV

```python
import duckdb

duckdb.sql("""
    COPY (
        SELECT
            'Laptop' AS product,
            3500 AS amount
    )
    TO 'salida.csv'
    WITH (HEADER, DELIMITER ';')
""")
```

## Exportar a Parquet

```python
import duckdb

duckdb.sql("""
    COPY (
        SELECT
            'Laptop' AS product,
            3500 AS amount
    )
    TO 'salida.parquet'
    (FORMAT PARQUET)
""")
```

## Crear tabla desde pandas

```python
import duckdb
import pandas as pd

df = pd.DataFrame({
    "product": ["Laptop", "Mouse"],
    "amount": [3500, 80]
})

connection = duckdb.connect("app.duckdb")

connection.execute("""
    CREATE TABLE sales AS
    SELECT *
    FROM df
""")

connection.close()
```

## Insertar desde DataFrame

```python
import duckdb
import pandas as pd

df = pd.DataFrame({
    "product": ["Laptop", "Mouse"],
    "amount": [3500, 80]
})

connection = duckdb.connect("app.duckdb")

connection.execute("""
    INSERT INTO sales
    SELECT *
    FROM df
""")

connection.close()
```

## Consultas analíticas

DuckDB permite usar SQL para análisis tabular.

## Agrupación

```python
import duckdb

result = duckdb.sql("""
    SELECT
        product,
        SUM(amount) AS total_amount
    FROM sales
    GROUP BY product
    ORDER BY total_amount DESC
""").df()

print(result)
```

## Filtros

```python
result = duckdb.sql("""
    SELECT *
    FROM sales
    WHERE amount >= 100
""").df()
```

## Ordenamiento

```python
result = duckdb.sql("""
    SELECT *
    FROM sales
    ORDER BY amount DESC
""").df()
```

## Joins

```python
import duckdb
import pandas as pd

sales = pd.DataFrame({
    "product_id": [1, 2, 1],
    "amount": [3500, 80, 2000]
})

products = pd.DataFrame({
    "product_id": [1, 2],
    "product": ["Laptop", "Mouse"]
})

result = duckdb.sql("""
    SELECT
        p.product,
        SUM(s.amount) AS total_amount
    FROM sales AS s
    INNER JOIN products AS p
        ON s.product_id = p.product_id
    GROUP BY p.product
""").df()

print(result)
```

## Window functions

DuckDB soporta funciones de ventana.

```python
import duckdb
import pandas as pd

sales = pd.DataFrame({
    "product": ["Laptop", "Laptop", "Mouse", "Mouse"],
    "month": ["Ene", "Feb", "Ene", "Feb"],
    "amount": [3500, 4200, 80, 120]
})

result = duckdb.sql("""
    SELECT
        product,
        month,
        amount,
        SUM(amount) OVER (
            PARTITION BY product
            ORDER BY month
        ) AS accumulated_amount
    FROM sales
""").df()

print(result)
```

## Fechas

```python
import duckdb
import pandas as pd

df = pd.DataFrame({
    "date": pd.to_datetime([
        "2026-01-01",
        "2026-01-02",
        "2026-02-01"
    ]),
    "amount": [100, 150, 200]
})

result = duckdb.sql("""
    SELECT
        DATE_TRUNC('month', date) AS month,
        SUM(amount) AS total_amount
    FROM df
    GROUP BY month
    ORDER BY month
""").df()

print(result)
```

## Crear vistas

```python
import duckdb

connection = duckdb.connect("app.duckdb")

connection.execute("""
    CREATE VIEW active_products AS
    SELECT *
    FROM products
    WHERE stock > 0
""")

connection.close()
```

## Crear tabla temporal

```python
import duckdb

connection = duckdb.connect()

connection.execute("""
    CREATE TEMP TABLE temp_sales AS
    SELECT
        'Laptop' AS product,
        3500 AS amount
""")

df = connection.execute("""
    SELECT *
    FROM temp_sales
""").df()

print(df)

connection.close()
```

## Consultar metadatos

## Ver tablas

```python
import duckdb

connection = duckdb.connect("app.duckdb")

tables = connection.execute("""
    SHOW TABLES
""").fetchall()

print(tables)

connection.close()
```

## Describir tabla

```python
import duckdb

connection = duckdb.connect("app.duckdb")

description = connection.execute("""
    DESCRIBE products
""").df()

print(description)

connection.close()
```

## Tipos de datos frecuentes

Tipos comunes en DuckDB:

```text
INTEGER
BIGINT
DOUBLE
DECIMAL
VARCHAR
BOOLEAN
DATE
TIMESTAMP
```

Ejemplo:

```sql
CREATE TABLE products (
    id INTEGER,
    name VARCHAR,
    price DECIMAL(10, 2),
    stock INTEGER,
    is_active BOOLEAN,
    created_at TIMESTAMP
)
```

## Transacciones

DuckDB permite usar transacciones.

```python
import duckdb

connection = duckdb.connect("app.duckdb")

try:
    connection.execute("BEGIN TRANSACTION")

    connection.execute("""
        INSERT INTO products (id, name, price, stock)
        VALUES (?, ?, ?, ?)
    """, [1, "Laptop", 3500, 5])

    connection.execute("COMMIT")
except Exception:
    connection.execute("ROLLBACK")
    raise
finally:
    connection.close()
```

## Uso con SQL parametrizado

Los valores deben pasarse como parámetros.

Correcto:

```python
connection.execute(
    """
    SELECT *
    FROM products
    WHERE id = ?
    """,
    [product_id]
)
```

Menos recomendable:

```python
connection.execute(
    f"""
    SELECT *
    FROM products
    WHERE id = {product_id}
    """
)
```

La parametrización evita problemas de formato y reduce riesgos al construir consultas.

## Uso con rutas de archivos

Conviene usar `pathlib` para construir rutas.

```python
from pathlib import Path

import duckdb

input_path = Path("data") / "ventas.parquet"

df = duckdb.sql(f"""
    SELECT *
    FROM read_parquet('{input_path.as_posix()}')
""").df()

print(df)
```

Si la ruta viene de una fuente externa, debe validarse antes de construir una consulta.

## Uso con archivos particionados

DuckDB puede consultar varios archivos a la vez.

```python
import duckdb

df = duckdb.sql("""
    SELECT *
    FROM read_parquet('data/sales/*.parquet')
""").df()

print(df)
```

## Uso con carpetas de datos

```python
import duckdb

result = duckdb.sql("""
    SELECT
        product,
        SUM(amount) AS total_amount
    FROM read_parquet('data/sales/*.parquet')
    GROUP BY product
    ORDER BY total_amount DESC
""").df()

print(result)
```

## Uso con pandas para análisis

```python
import duckdb
import pandas as pd

df = pd.read_excel("ventas.xlsx")

summary = duckdb.sql("""
    SELECT
        Producto,
        SUM(Monto) AS MontoTotal
    FROM df
    GROUP BY Producto
    ORDER BY MontoTotal DESC
""").df()

print(summary)
```

## Uso con Excel

DuckDB no reemplaza directamente a `openpyxl` para editar archivos Excel.

Flujo frecuente:

```text
pandas lee Excel
DuckDB consulta DataFrame
pandas exporta resultado
```

Ejemplo:

```python
import duckdb
import pandas as pd

df = pd.read_excel("ventas.xlsx")

summary = duckdb.sql("""
    SELECT
        Producto,
        SUM(Monto) AS MontoTotal
    FROM df
    GROUP BY Producto
""").df()

summary.to_excel("resumen.xlsx", index=False)
```

## Uso con Parquet como formato intermedio

DuckDB es especialmente útil cuando los datos se guardan en Parquet.

```python
import duckdb
import pandas as pd

df = pd.read_csv("ventas.csv", sep=";")

duckdb.sql("""
    COPY df TO 'ventas.parquet' (FORMAT PARQUET)
""")
```

Luego puede consultarse directamente:

```python
result = duckdb.sql("""
    SELECT *
    FROM 'ventas.parquet'
""").df()
```

## Uso en pipelines de datos

DuckDB puede funcionar como motor de transformación local.

Flujo conceptual:

```text
CSV / Parquet / JSON
-> DuckDB SQL
-> resultado filtrado o agregado
-> Parquet / CSV / DataFrame
```

Ejemplo:

```python
import duckdb

duckdb.sql("""
    COPY (
        SELECT
            product,
            SUM(amount) AS total_amount
        FROM read_parquet('data/*.parquet')
        GROUP BY product
    )
    TO 'output/summary.parquet'
    (FORMAT PARQUET)
""")
```

## Relación con bases de datos tradicionales

DuckDB no requiere servidor para trabajar localmente.

Comparación conceptual:

```text
PostgreSQL / MySQL / SQL Server
-> bases cliente-servidor, multiusuario, transaccionales

DuckDB
-> base analítica local, embebida, orientada a consultas sobre datos
```

Esto no significa que DuckDB sea inferior, sino que está optimizado para otro tipo de uso.

## Relación con SQLite

SQLite y DuckDB son bases embebidas.

Diferencia práctica:

```text
SQLite -> más orientado a almacenamiento transaccional ligero
DuckDB -> más orientado a análisis columnar y consultas analíticas
```

Para una aplicación pequeña con muchas operaciones CRUD simples, SQLite puede ser suficiente.

Para análisis de archivos, agregaciones y consultas sobre datos tabulares, DuckDB suele ser más cómodo.

## Relación con pandas

DuckDB puede complementar a pandas.

Pandas es muy útil para manipulación en memoria con API de DataFrame.

DuckDB permite expresar transformaciones con SQL.

Flujo frecuente:

```text
pandas carga datos
DuckDB consulta con SQL
pandas recibe resultado
```

## Relación con Polars

DuckDB y Polars pueden coexistir en pipelines de análisis.

```text
Polars -> manipulación DataFrame rápida
DuckDB -> consultas SQL sobre datos
```

## Relación con PyArrow

DuckDB puede trabajar con datos Arrow.

Esto facilita interoperabilidad con herramientas modernas de datos.

## Errores comunes

## Creer que DuckDB necesita servidor

DuckDB se ejecuta dentro del proceso de Python.

No se necesita iniciar un servidor para consultas locales básicas.

## No distinguir memoria y archivo persistente

```python
duckdb.connect()
```

crea una base en memoria.

```python
duckdb.connect("app.duckdb")
```

usa una base persistente en archivo.

## Usar DuckDB como si fuera PostgreSQL de producción multiusuario

DuckDB no está pensado como reemplazo directo de una base cliente-servidor transaccional para muchas conexiones concurrentes de escritura.

## Concatenar valores dentro del SQL

Menos recomendable:

```python
query = f"SELECT * FROM products WHERE id = {product_id}"
```

Mejor:

```python
connection.execute(
    "SELECT * FROM products WHERE id = ?",
    [product_id]
)
```

## No cerrar conexiones

Más seguro:

```python
connection.close()
```

cuando ya no se necesita la conexión.

## Leer archivos con rutas incorrectas

Si DuckDB no encuentra el archivo, la consulta falla.

Conviene validar rutas cuando se construyen dinámicamente.

## Convertir resultados grandes a pandas sin revisar memoria

```python
df = connection.execute("SELECT * FROM big_table").df()
```

puede consumir mucha memoria si el resultado es demasiado grande.

Conviene filtrar, agrupar o exportar antes.

## Usar pandas para todo cuando SQL es más claro

Algunas transformaciones complejas pueden ser más legibles en SQL.

DuckDB permite escribirlas directamente.

## Usar SQL para todo cuando pandas es más simple

Para operaciones pequeñas o muy específicas de DataFrame, pandas puede ser más directo.

Ambas herramientas pueden complementarse.

## Buenas prácticas

## Usar conexión explícita en proyectos

```python
connection = duckdb.connect("app.duckdb")
```

## Usar archivos persistentes cuando se necesita conservar datos

```python
duckdb.connect("datawarehouse.duckdb")
```

## Usar parámetros para valores

```python
connection.execute(
    "SELECT * FROM products WHERE id = ?",
    [product_id]
)
```

## Usar Parquet para datos analíticos

```python
read_parquet()
COPY ... FORMAT PARQUET
```

## Filtrar antes de convertir a pandas

```python
df = duckdb.sql("""
    SELECT *
    FROM 'large_file.parquet'
    WHERE year = 2026
""").df()
```

## Separar SQL largo en archivos si crece demasiado

Estructura posible:

```text
queries/
└─ sales_summary.sql
```

Leer SQL:

```python
from pathlib import Path

query = Path("queries/sales_summary.sql").read_text(encoding="utf-8")
```

## Mantener nombres claros de tablas y columnas

```text
sales
products
customers
total_amount
created_at
```

## Usar DuckDB como motor analítico local

No tratarlo automáticamente como reemplazo de todas las bases de datos.

## Ejemplo integrado

```python
from pathlib import Path

import duckdb
import pandas as pd


def load_sales(input_path):
    if input_path.suffix == ".xlsx":
        return pd.read_excel(input_path)

    if input_path.suffix == ".csv":
        return pd.read_csv(input_path, sep=";")

    raise ValueError("Formato de archivo no soportado")


def create_sales_summary(df):
    return duckdb.sql("""
        SELECT
            Producto,
            SUM(Monto) AS MontoTotal,
            COUNT(*) AS Operaciones
        FROM df
        GROUP BY Producto
        ORDER BY MontoTotal DESC
    """).df()


def save_outputs(summary, output_dir):
    output_dir.mkdir(parents=True, exist_ok=True)

    summary.to_excel(
        output_dir / "resumen_ventas.xlsx",
        index=False
    )

    duckdb.sql("""
        COPY summary
        TO 'output/resumen_ventas.parquet'
        (FORMAT PARQUET)
    """)


input_path = Path("ventas.xlsx")
output_dir = Path("output")

sales = load_sales(input_path)
summary = create_sales_summary(sales)

save_outputs(summary, output_dir)

print("Resumen generado correctamente")
```

## Ejemplo con base persistente

```python
from pathlib import Path

import duckdb


database_path = Path("analytics.duckdb")
input_path = Path("data") / "ventas.parquet"

connection = duckdb.connect(database_path)

try:
    connection.execute("""
        CREATE TABLE IF NOT EXISTS ventas AS
        SELECT *
        FROM read_parquet(?)
    """, [input_path.as_posix()])

    summary = connection.execute("""
        SELECT
            product,
            SUM(amount) AS total_amount
        FROM ventas
        GROUP BY product
        ORDER BY total_amount DESC
    """).df()

    print(summary)

finally:
    connection.close()
```

## Relación con otras librerías

`duckdb` se relaciona especialmente con:

```text
pandas
polars
pyarrow
parquet
csv
json
pathlib
sqlalchemy
jupyter
```

## Relación con pandas

DuckDB puede consultar DataFrames existentes y devolver resultados como DataFrames.

```python
duckdb.sql("SELECT * FROM df").df()
```

## Relación con Polars

DuckDB puede consultar datos de Polars y devolver resultados compatibles.

```python
duckdb.sql("SELECT * FROM df").pl()
```

## Relación con PyArrow

DuckDB puede trabajar con datos Arrow, lo cual facilita interoperabilidad entre herramientas de análisis modernas.

## Relación con SQLAlchemy

DuckDB puede integrarse con SQLAlchemy mediante dialectos o extensiones disponibles en el ecosistema, aunque para muchos usos analíticos directos basta con la API nativa de `duckdb`.

## Relación con archivos

DuckDB es especialmente útil para consultar directamente:

```text
CSV
Parquet
JSON
```

sin cargar todo manualmente en estructuras intermedias.

## Orden didáctico interno

```text
1. Propósito de duckdb
2. Instalación
3. Consulta básica
4. duckdb.sql()
5. connect()
6. Base en memoria y base persistente
7. Crear tablas
8. Insertar y consultar datos
9. Leer CSV, Parquet y JSON
10. Consultar pandas y Polars
11. Exportar a CSV y Parquet
12. Consultas analíticas
13. Joins y window functions
14. Transacciones
15. Uso con pipelines de datos
16. Relación con pandas, Polars, Arrow y SQLite
17. Errores comunes
18. Buenas prácticas
```