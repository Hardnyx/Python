# `pyxlsb`

## Propósito

`pyxlsb` es una librería externa para leer archivos Excel en formato binario `.xlsb`.

Se utiliza cuando se necesita extraer datos desde libros binarios de Excel, especialmente en casos donde herramientas como `openpyxl` no pueden abrir el archivo porque están orientadas principalmente a formatos como `.xlsx` y `.xlsm`.

## Naturaleza de la librería

`pyxlsb` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install pyxlsb
```

La importación habitual es:

```python
from pyxlsb import open_workbook
```

Su función principal es abrir archivos `.xlsb` y recorrer sus hojas para extraer datos. :contentReference[oaicite:1]{index=1}

## Idea central

La idea principal de `pyxlsb` es leer un archivo binario de Excel y recorrer sus filas.

Flujo típico:

```text
archivo .xlsb -> open_workbook() -> hoja -> filas -> celdas -> valores
```

Ejemplo básico:

```python
from pyxlsb import open_workbook

with open_workbook("archivo.xlsb") as workbook:
    with workbook.get_sheet(1) as sheet:
        for row in sheet.rows():
            values = [cell.v for cell in row]
            print(values)
```

## Cuándo usar `pyxlsb`

Conviene usar `pyxlsb` cuando se necesita:

- leer archivos `.xlsb`
- extraer datos básicos desde hojas de Excel binario
- convertir contenido de `.xlsb` a listas, diccionarios, CSV, pandas o Excel moderno
- trabajar con archivos que no pueden abrirse con `openpyxl`
- procesar reportes exportados en formato binario de Excel

## Cuándo no usar `pyxlsb`

No conviene usar `pyxlsb` cuando se necesita:

- crear archivos Excel
- modificar archivos Excel existentes
- aplicar estilos
- escribir fórmulas
- conservar formatos
- manipular gráficos
- trabajar con macros
- editar plantillas

Para esos casos suele corresponder:

```text
openpyxl
xlsxwriter
```

`pyxlsb` está pensado principalmente para lectura y extracción básica de datos. :contentReference[oaicite:2]{index=2}

## Instalación

Instalación básica:

```bash
python -m pip install pyxlsb
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
pyxlsb==1.0.10
```

La versión exacta puede variar según el entorno.

## Importación

```python
from pyxlsb import open_workbook
```

Verificación:

```python
import pyxlsb

print(pyxlsb.__version__)
```

## Abrir un libro

Para abrir un archivo `.xlsb` se usa `open_workbook()`.

```python
from pyxlsb import open_workbook

with open_workbook("archivo.xlsb") as workbook:
    print(workbook)
```

El uso con `with` es recomendable porque asegura el cierre correcto del archivo.

## Acceder a una hoja

La forma básica documentada es usar `get_sheet()`.

```python
from pyxlsb import open_workbook

with open_workbook("archivo.xlsb") as workbook:
    with workbook.get_sheet(1) as sheet:
        print(sheet)
```

En `pyxlsb`, el índice de hoja usado por `get_sheet()` es de base `1`, según el ejemplo del propio proyecto. :contentReference[oaicite:3]{index=3}

## Recorrer filas

Las filas se recorren con `rows()`.

```python
from pyxlsb import open_workbook

with open_workbook("archivo.xlsb") as workbook:
    with workbook.get_sheet(1) as sheet:
        for row in sheet.rows():
            print(row)
```

Cada fila contiene objetos celda.

## Extraer valores de celdas

El valor de una celda se obtiene con `.v`.

```python
from pyxlsb import open_workbook

with open_workbook("archivo.xlsb") as workbook:
    with workbook.get_sheet(1) as sheet:
        for row in sheet.rows():
            values = [cell.v for cell in row]
            print(values)
```

## Convertir una hoja a lista de listas

```python
from pyxlsb import open_workbook


def read_xlsb_sheet(file_path, sheet_index=1):
    rows = []

    with open_workbook(file_path) as workbook:
        with workbook.get_sheet(sheet_index) as sheet:
            for row in sheet.rows():
                values = [cell.v for cell in row]
                rows.append(values)

    return rows


data = read_xlsb_sheet("archivo.xlsb", sheet_index=1)

print(data[:5])
```

## Convertir una hoja a lista de diccionarios

Cuando la primera fila contiene encabezados, puede usarse como nombres de campos.

```python
from pyxlsb import open_workbook


def read_xlsb_as_records(file_path, sheet_index=1):
    rows = []

    with open_workbook(file_path) as workbook:
        with workbook.get_sheet(sheet_index) as sheet:
            for row in sheet.rows():
                values = [cell.v for cell in row]
                rows.append(values)

    if not rows:
        return []

    headers = rows[0]
    data_rows = rows[1:]

    records = []

    for row in data_rows:
        record = dict(zip(headers, row))
        records.append(record)

    return records


records = read_xlsb_as_records("archivo.xlsb")

print(records[:5])
```

## Uso con pandas

`pandas` puede leer archivos `.xlsb` usando el motor `pyxlsb`.

```python
import pandas as pd

df = pd.read_excel(
    "archivo.xlsb",
    engine="pyxlsb"
)

print(df.head())
```

Leer una hoja específica:

```python
df = pd.read_excel(
    "archivo.xlsb",
    sheet_name="Datos",
    engine="pyxlsb"
)
```

También puede usarse índice de hoja:

```python
df = pd.read_excel(
    "archivo.xlsb",
    sheet_name=0,
    engine="pyxlsb"
)
```

## Relación con pandas

En muchos flujos prácticos, `pyxlsb` se usa de forma indirecta a través de `pandas`.

```python
import pandas as pd

df = pd.read_excel(
    "archivo.xlsb",
    engine="pyxlsb"
)
```

Este enfoque es más cómodo cuando el objetivo es obtener una tabla para limpiar, filtrar, cruzar o exportar.

## Relación con openpyxl

`openpyxl` se usa principalmente para archivos `.xlsx` y `.xlsm`.

`pyxlsb` se usa para leer archivos `.xlsb`.

Regla práctica:

```text
.xlsx / .xlsm -> openpyxl
.xlsb         -> pyxlsb
```

## Relación con xlsxwriter

`xlsxwriter` sirve para crear archivos `.xlsx` nuevos.

`pyxlsb` sirve para leer archivos `.xlsb`.

No cumplen la misma función.

```text
pyxlsb     -> lectura de .xlsb
xlsxwriter -> creación de .xlsx
```

## Lectura básica y exportación a CSV

```python
import csv

from pyxlsb import open_workbook


def xlsb_to_csv(input_path, output_path, sheet_index=1):
    with open_workbook(input_path) as workbook:
        with workbook.get_sheet(sheet_index) as sheet:
            with open(output_path, "w", newline="", encoding="utf-8") as file:
                writer = csv.writer(file)

                for row in sheet.rows():
                    values = [cell.v for cell in row]
                    writer.writerow(values)


xlsb_to_csv(
    "archivo.xlsb",
    "archivo_convertido.csv"
)

print("Archivo convertido correctamente")
```

## Lectura básica y exportación a XLSX con pandas

```python
import pandas as pd

df = pd.read_excel(
    "archivo.xlsb",
    engine="pyxlsb"
)

df.to_excel(
    "archivo_convertido.xlsx",
    index=False
)

print("Archivo convertido correctamente")
```

## Tratamiento de encabezados

Si el archivo no tiene encabezados claros, puede indicarse a pandas:

```python
import pandas as pd

df = pd.read_excel(
    "archivo.xlsb",
    engine="pyxlsb",
    header=None
)

print(df.head())
```

Si los encabezados están en otra fila:

```python
df = pd.read_excel(
    "archivo.xlsb",
    engine="pyxlsb",
    header=2
)
```

## Selección de columnas

Con pandas:

```python
import pandas as pd

df = pd.read_excel(
    "archivo.xlsb",
    engine="pyxlsb",
    usecols=["Fecha", "Cliente", "Monto"]
)

print(df.head())
```

Con letras de Excel:

```python
df = pd.read_excel(
    "archivo.xlsb",
    engine="pyxlsb",
    usecols="A:D"
)
```

## Control de tipos

Cuando hay códigos, documentos o identificadores con ceros a la izquierda, conviene leerlos como texto.

```python
import pandas as pd

df = pd.read_excel(
    "archivo.xlsb",
    engine="pyxlsb",
    dtype={
        "document_id": str,
        "client_code": str
    }
)
```

## Lectura de varias hojas con pandas

```python
import pandas as pd

sheets = pd.read_excel(
    "archivo.xlsb",
    sheet_name=None,
    engine="pyxlsb"
)

for sheet_name, df in sheets.items():
    print(sheet_name)
    print(df.head())
```

`sheet_name=None` devuelve un diccionario donde las claves son nombres de hojas y los valores son DataFrames.

## Limpieza posterior

Después de leer un `.xlsb`, normalmente el trabajo de limpieza se hace con `pandas`.

```python
import pandas as pd

df = pd.read_excel(
    "archivo.xlsb",
    engine="pyxlsb"
)

df.columns = df.columns.str.strip()

df = df.dropna(how="all")

print(df.head())
```

## Exportación limpia

```python
import pandas as pd

df = pd.read_excel(
    "archivo.xlsb",
    engine="pyxlsb"
)

df = df.dropna(how="all")

df.to_excel(
    "salida.xlsx",
    index=False
)
```

## Casos de uso frecuentes

## Leer una hoja `.xlsb`

```python
from pyxlsb import open_workbook

with open_workbook("archivo.xlsb") as workbook:
    with workbook.get_sheet(1) as sheet:
        for row in sheet.rows():
            print([cell.v for cell in row])
```

## Leer `.xlsb` como DataFrame

```python
import pandas as pd

df = pd.read_excel(
    "archivo.xlsb",
    engine="pyxlsb"
)
```

## Convertir `.xlsb` a `.xlsx`

```python
import pandas as pd

df = pd.read_excel(
    "archivo.xlsb",
    engine="pyxlsb"
)

df.to_excel(
    "archivo.xlsx",
    index=False
)
```

## Extraer datos de una hoja específica

```python
df = pd.read_excel(
    "archivo.xlsb",
    sheet_name="Datos",
    engine="pyxlsb"
)
```

## Errores comunes

## Usar openpyxl para abrir `.xlsb`

Problemático:

```python
from openpyxl import load_workbook

workbook = load_workbook("archivo.xlsb")
```

`openpyxl` no está orientado a abrir `.xlsb`.

Para `.xlsb` corresponde usar `pyxlsb` o `pandas` con `engine="pyxlsb"`.

## Esperar que `pyxlsb` modifique archivos

`pyxlsb` sirve para leer datos. No debe usarse como herramienta de edición de libros Excel.

## No usar `with`

Menos recomendable:

```python
workbook = open_workbook("archivo.xlsb")
```

Más seguro:

```python
with open_workbook("archivo.xlsb") as workbook:
    ...
```

## Confundir índice de hoja

Con `pyxlsb.get_sheet()`, el ejemplo de uso del proyecto muestra índice de base `1`. :contentReference[oaicite:4]{index=4}

```python
with workbook.get_sheet(1) as sheet:
    ...
```

En cambio, en pandas, `sheet_name=0` suele referirse a la primera hoja.

## No extraer `.v` de la celda

Problemático:

```python
values = [cell for cell in row]
```

Eso produce objetos celda.

Para obtener valores:

```python
values = [cell.v for cell in row]
```

## No revisar tipos después de leer

Al leer archivos Excel, fechas, códigos y montos pueden interpretarse de forma no esperada.

Con pandas:

```python
print(df.dtypes)
```

## Perder ceros a la izquierda

Si una columna contiene códigos o documentos, puede ser necesario leerla como texto.

```python
df = pd.read_excel(
    "archivo.xlsb",
    engine="pyxlsb",
    dtype={"document_id": str}
)
```

## Suponer que conserva formatos

`pyxlsb` está orientado a extracción de datos, no a preservar estilos, formatos visuales, fórmulas, gráficos o macros.

## Buenas prácticas

## Usar `pyxlsb` para lectura básica de `.xlsb`

```python
from pyxlsb import open_workbook
```

## Usar pandas cuando se necesite análisis tabular

```python
df = pd.read_excel(
    "archivo.xlsb",
    engine="pyxlsb"
)
```

## Validar encabezados y tipos

```python
print(df.head())
print(df.dtypes)
```

## Leer identificadores como texto

```python
dtype={"document_id": str}
```

## Convertir a formatos más manejables

```python
df.to_excel("salida.xlsx", index=False)
df.to_csv("salida.csv", index=False, sep=";")
```

## Usar `with` para abrir libros y hojas

```python
with open_workbook("archivo.xlsb") as workbook:
    with workbook.get_sheet(1) as sheet:
        ...
```

## Separar extracción y transformación

```python
raw_rows = read_xlsb_sheet(path)
records = build_records(raw_rows)
```

## Ejemplo integrado

```python
from pathlib import Path

import pandas as pd


def load_xlsb_data(input_path, sheet_name):
    df = pd.read_excel(
        input_path,
        sheet_name=sheet_name,
        engine="pyxlsb",
        dtype={
            "document_id": str,
            "client_code": str
        }
    )

    return df


def clean_data(df):
    result = df.copy()

    result.columns = result.columns.str.strip()

    result = result.dropna(how="all")

    if "amount" in result.columns:
        result["amount"] = pd.to_numeric(
            result["amount"],
            errors="coerce"
        )

    return result


def export_data(df, output_path):
    df.to_excel(
        output_path,
        index=False
    )


input_path = Path("reporte.xlsb")
output_path = Path("reporte_limpio.xlsx")

data = load_xlsb_data(
    input_path,
    sheet_name="Datos"
)

cleaned_data = clean_data(data)

export_data(
    cleaned_data,
    output_path
)

print("Archivo procesado correctamente")
```

## Relación con otras librerías

`pyxlsb` se relaciona especialmente con:

- `pandas`, para leer `.xlsb` como DataFrame
- `openpyxl`, como alternativa para `.xlsx` y `.xlsm`
- `xlsxwriter`, para crear archivos `.xlsx` nuevos
- `csv`, para exportar datos extraídos a texto delimitado
- `pathlib`, para manejar rutas
- `polars`, cuando los datos extraídos se convierten a CSV o DataFrame compatible
- `pyarrow`, cuando se convierten datos a formatos columnares como Parquet

## Orden didáctico interno

```text
1. Propósito de pyxlsb
2. Instalación e importación
3. Diferencia frente a openpyxl y xlsxwriter
4. open_workbook()
5. Acceso a hojas con get_sheet()
6. Recorrido de filas con rows()
7. Extracción de valores con cell.v
8. Uso con pandas
9. Conversión a CSV o XLSX
10. Errores comunes
11. Buenas prácticas
```