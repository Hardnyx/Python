# `xlsxwriter`

## Propósito

`xlsxwriter` es una librería externa para crear archivos Excel `.xlsx` desde Python. Se utiliza para generar reportes, escribir tablas, aplicar formatos, crear fórmulas, insertar gráficos, agregar validaciones, usar formato condicional, configurar hojas y producir archivos Excel bien presentados desde cero.

A diferencia de `openpyxl`, `xlsxwriter` está orientada principalmente a la creación de archivos nuevos, no a la modificación de archivos existentes.

## Naturaleza de la librería

`xlsxwriter` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install XlsxWriter
```

La importación habitual es:

```python
import xlsxwriter
```

No suele usarse alias, porque el nombre ya es claro dentro del contexto de creación de archivos Excel.

## Idea central

La idea principal de `xlsxwriter` es construir un archivo Excel desde Python mediante tres objetos principales:

```text
Workbook -> Worksheet -> Format
```

Es decir:

```text
libro -> hoja -> formato
```

Ejemplo básico:

```python
import xlsxwriter

workbook = xlsxwriter.Workbook("reporte.xlsx")
worksheet = workbook.add_worksheet("Datos")

worksheet.write("A1", "Producto")
worksheet.write("B1", "Monto")

worksheet.write("A2", "Laptop")
worksheet.write("B2", 3500)

workbook.close()
```

## Diferencia principal frente a `openpyxl`

`xlsxwriter` crea archivos Excel nuevos.

`openpyxl` puede crear, leer y modificar archivos Excel existentes.

Regla práctica:

```text
xlsxwriter -> crear reportes Excel nuevos con formato
openpyxl   -> leer o modificar libros Excel existentes
```

## Cuándo usar `xlsxwriter`

Conviene usar `xlsxwriter` cuando se necesita:

- crear un archivo `.xlsx` desde cero
- generar reportes automatizados
- aplicar formatos detallados
- crear gráficos
- escribir fórmulas
- agregar validaciones de datos
- aplicar formato condicional
- insertar imágenes
- crear archivos Excel desde pandas
- producir salidas visualmente cuidadas

## Cuándo no usar `xlsxwriter`

No conviene usar `xlsxwriter` cuando se necesita abrir y modificar un archivo Excel existente.

Para ese caso suele corresponder:

```text
openpyxl
```

Tampoco es la mejor opción para transformar datos tabulares antes de exportar. Para limpieza, cruces, agrupaciones y cálculos tabulares suele ser mejor usar:

```text
pandas
polars
```

y luego exportar con `xlsxwriter`.

## Instalación

Instalación básica:

```bash
python -m pip install XlsxWriter
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
XlsxWriter==3.x.x
```

La versión exacta puede variar según el entorno.

## Importación

```python
import xlsxwriter
```

Verificación:

```python
import xlsxwriter

print(xlsxwriter.__version__)
```

## Crear un libro nuevo

```python
import xlsxwriter

workbook = xlsxwriter.Workbook("archivo.xlsx")

workbook.close()
```

El archivo se escribe correctamente cuando se llama a:

```python
workbook.close()
```

## Agregar una hoja

```python
import xlsxwriter

workbook = xlsxwriter.Workbook("archivo.xlsx")

worksheet = workbook.add_worksheet("Datos")

workbook.close()
```

Si no se indica nombre, Excel asigna uno automáticamente.

```python
worksheet = workbook.add_worksheet()
```

## Escribir en celdas

## Con notación A1

```python
worksheet.write("A1", "Producto")
worksheet.write("B1", "Monto")
```

## Con fila y columna

```python
worksheet.write(0, 0, "Producto")
worksheet.write(0, 1, "Monto")
```

En la notación por fila y columna, los índices empiezan en `0`.

```text
row=0, col=0 -> A1
row=0, col=1 -> B1
row=1, col=0 -> A2
```

## Escribir texto

```python
worksheet.write("A1", "Hola")
```

## Escribir números

```python
worksheet.write("A2", 100)
worksheet.write("A3", 250.75)
```

## Escribir booleanos

```python
worksheet.write("A4", True)
```

## Escribir valores vacíos

```python
worksheet.write_blank("A5", None)
```

## Escribir fórmulas

Las fórmulas se escriben como cadenas que empiezan con `=`.

```python
worksheet.write_formula("B4", "=SUM(B2:B3)")
```

Ejemplo completo:

```python
import xlsxwriter

workbook = xlsxwriter.Workbook("formulas.xlsx")
worksheet = workbook.add_worksheet("Datos")

worksheet.write("A1", "Valor")
worksheet.write("A2", 10)
worksheet.write("A3", 20)
worksheet.write_formula("A4", "=SUM(A2:A3)")

workbook.close()
```

## Escribir hipervínculos

```python
worksheet.write_url("A1", "https://example.com")
```

Con texto visible:

```python
worksheet.write_url(
    "A1",
    "https://example.com",
    string="Sitio web"
)
```

## Escribir filas

## `write_row()`

```python
worksheet.write_row("A1", ["Producto", "Cantidad", "Precio"])
worksheet.write_row("A2", ["Laptop", 2, 3500])
```

Con fila y columna:

```python
worksheet.write_row(0, 0, ["Producto", "Cantidad", "Precio"])
```

## Escribir columnas

## `write_column()`

```python
worksheet.write_column("A1", ["Producto", "Laptop", "Mouse"])
```

Con fila y columna:

```python
worksheet.write_column(0, 0, ["Producto", "Laptop", "Mouse"])
```

## Formatos

Los formatos se crean desde el libro con `add_format()`.

```python
header_format = workbook.add_format({
    "bold": True,
    "bg_color": "#1F4E78",
    "font_color": "#FFFFFF"
})
```

Luego se aplican al escribir:

```python
worksheet.write("A1", "Producto", header_format)
```

## Formato de fuente

```python
bold_format = workbook.add_format({
    "bold": True
})
```

## Color de fondo

```python
fill_format = workbook.add_format({
    "bg_color": "#FFC000"
})
```

## Color de fuente

```python
font_format = workbook.add_format({
    "font_color": "#FF0000"
})
```

## Alineación

```python
center_format = workbook.add_format({
    "align": "center",
    "valign": "vcenter"
})
```

## Bordes

```python
border_format = workbook.add_format({
    "border": 1
})
```

## Formato numérico

```python
money_format = workbook.add_format({
    "num_format": "#,##0.00"
})
```

Porcentaje:

```python
percent_format = workbook.add_format({
    "num_format": "0.00%"
})
```

Fecha:

```python
date_format = workbook.add_format({
    "num_format": "dd/mm/yyyy"
})
```

## Formato de encabezados

```python
header_format = workbook.add_format({
    "bold": True,
    "bg_color": "#1F4E78",
    "font_color": "#FFFFFF",
    "align": "center",
    "valign": "vcenter",
    "border": 1
})

worksheet.write_row(
    "A1",
    ["Producto", "Cantidad", "Precio"],
    header_format
)
```

## Ancho de columnas

```python
worksheet.set_column("A:A", 20)
worksheet.set_column("B:C", 15)
```

Con formato aplicado a toda la columna:

```python
worksheet.set_column("C:C", 15, money_format)
```

## Alto de filas

```python
worksheet.set_row(0, 25)
```

Con formato:

```python
worksheet.set_row(0, 25, header_format)
```

## Congelar paneles

Congelar la primera fila:

```python
worksheet.freeze_panes(1, 0)
```

Congelar primera fila y primera columna:

```python
worksheet.freeze_panes(1, 1)
```

## Autofiltro

```python
worksheet.autofilter("A1:C10")
```

Ejemplo:

```python
worksheet.write_row("A1", ["Producto", "Cantidad", "Precio"])
worksheet.autofilter("A1:C1")
```

En tablas con datos, el rango debe cubrir encabezados y filas.

## Tablas

`xlsxwriter` permite agregar tablas de Excel.

```python
worksheet.add_table("A1:C4", {
    "columns": [
        {"header": "Producto"},
        {"header": "Cantidad"},
        {"header": "Precio"}
    ]
})
```

Ejemplo completo:

```python
import xlsxwriter

workbook = xlsxwriter.Workbook("tabla.xlsx")
worksheet = workbook.add_worksheet("Datos")

data = [
    ["Producto", "Cantidad", "Precio"],
    ["Laptop", 2, 3500],
    ["Mouse", 10, 80],
    ["Teclado", 5, 150]
]

for row_index, row in enumerate(data):
    worksheet.write_row(row_index, 0, row)

worksheet.add_table("A1:C4", {
    "style": "Table Style Medium 9",
    "columns": [
        {"header": "Producto"},
        {"header": "Cantidad"},
        {"header": "Precio"}
    ]
})

workbook.close()
```

## Celdas combinadas

```python
title_format = workbook.add_format({
    "bold": True,
    "font_size": 14,
    "align": "center",
    "valign": "vcenter"
})

worksheet.merge_range(
    "A1:D1",
    "Reporte de ventas",
    title_format
)
```

## Formato condicional

El formato condicional permite aplicar estilos según reglas.

Ejemplo básico:

```python
highlight_format = workbook.add_format({
    "bg_color": "#FFC7CE",
    "font_color": "#9C0006"
})

worksheet.conditional_format("B2:B10", {
    "type": "cell",
    "criteria": ">",
    "value": 1000,
    "format": highlight_format
})
```

## Escala de colores

```python
worksheet.conditional_format("B2:B10", {
    "type": "3_color_scale"
})
```

## Barras de datos

```python
worksheet.conditional_format("B2:B10", {
    "type": "data_bar"
})
```

## Validación de datos

La validación de datos permite limitar o sugerir valores en una celda.

## Lista desplegable

```python
worksheet.data_validation("A2:A100", {
    "validate": "list",
    "source": ["Pendiente", "Aprobado", "Rechazado"]
})
```

## Número entero

```python
worksheet.data_validation("B2:B100", {
    "validate": "integer",
    "criteria": ">=",
    "value": 0
})
```

## Fecha

```python
worksheet.data_validation("C2:C100", {
    "validate": "date",
    "criteria": "between",
    "minimum": "2026-01-01",
    "maximum": "2026-12-31"
})
```

## Comentarios

```python
worksheet.write_comment("A1", "Comentario de ejemplo")
```

## Imágenes

```python
worksheet.insert_image("A1", "logo.png")
```

Con opciones:

```python
worksheet.insert_image("A1", "logo.png", {
    "x_scale": 0.5,
    "y_scale": 0.5
})
```

## Gráficos

## Crear gráfico de columnas

```python
chart = workbook.add_chart({
    "type": "column"
})
```

## Agregar serie

```python
chart.add_series({
    "name": "Ventas",
    "categories": "=Datos!$A$2:$A$4",
    "values": "=Datos!$B$2:$B$4"
})
```

## Insertar gráfico

```python
worksheet.insert_chart("D2", chart)
```

## Ejemplo de gráfico completo

```python
import xlsxwriter

workbook = xlsxwriter.Workbook("grafico.xlsx")
worksheet = workbook.add_worksheet("Datos")

data = [
    ["Producto", "Ventas"],
    ["Laptop", 10],
    ["Mouse", 25],
    ["Teclado", 15]
]

for row_index, row in enumerate(data):
    worksheet.write_row(row_index, 0, row)

chart = workbook.add_chart({
    "type": "column"
})

chart.add_series({
    "name": "Ventas",
    "categories": "=Datos!$A$2:$A$4",
    "values": "=Datos!$B$2:$B$4"
})

chart.set_title({
    "name": "Ventas por producto"
})

chart.set_x_axis({
    "name": "Producto"
})

chart.set_y_axis({
    "name": "Ventas"
})

worksheet.insert_chart("D2", chart)

workbook.close()
```

## Hojas múltiples

```python
worksheet_data = workbook.add_worksheet("Datos")
worksheet_summary = workbook.add_worksheet("Resumen")
```

Ejemplo:

```python
worksheet_data.write("A1", "Datos")
worksheet_summary.write("A1", "Resumen")
```

## Nombres definidos

```python
workbook.define_name(
    "TotalVentas",
    "=Resumen!$B$2"
)
```

Esto crea un nombre definido en Excel.

## Ocultar hojas

```python
worksheet.hide()
```

## Proteger hojas

```python
worksheet.protect()
```

Con opciones:

```python
worksheet.protect("clave")
```

La protección de hoja no debe interpretarse como seguridad fuerte. Está orientada principalmente a control de edición en Excel.

## Configuración de página

## Orientación

```python
worksheet.set_landscape()
```

o:

```python
worksheet.set_portrait()
```

## Tamaño de papel

```python
worksheet.set_paper(9)
```

## Márgenes

```python
worksheet.set_margins(
    left=0.5,
    right=0.5,
    top=0.75,
    bottom=0.75
)
```

## Repetir filas al imprimir

```python
worksheet.repeat_rows(0)
```

## Ajustar impresión a página

```python
worksheet.fit_to_pages(1, 0)
```

## Encabezado y pie de página

```python
worksheet.set_header("&CReporte")
worksheet.set_footer("&C&P de &N")
```

## Modo de memoria constante

Para archivos grandes, `xlsxwriter` ofrece opciones de optimización de memoria.

```python
workbook = xlsxwriter.Workbook(
    "archivo_grande.xlsx",
    {
        "constant_memory": True
    }
)
```

Este modo puede ayudar cuando se escriben muchas filas, aunque puede imponer restricciones sobre algunas operaciones.

## Uso con pandas

`xlsxwriter` puede usarse como motor de escritura en `pandas`.

```python
import pandas as pd

df = pd.DataFrame({
    "Producto": ["Laptop", "Mouse"],
    "Monto": [3500, 80]
})

with pd.ExcelWriter(
    "reporte.xlsx",
    engine="xlsxwriter"
) as writer:
    df.to_excel(
        writer,
        sheet_name="Datos",
        index=False
    )
```

## Aplicar formato con pandas y xlsxwriter

```python
import pandas as pd

df = pd.DataFrame({
    "Producto": ["Laptop", "Mouse"],
    "Monto": [3500, 80]
})

with pd.ExcelWriter(
    "reporte.xlsx",
    engine="xlsxwriter"
) as writer:
    df.to_excel(
        writer,
        sheet_name="Datos",
        index=False
    )

    workbook = writer.book
    worksheet = writer.sheets["Datos"]

    money_format = workbook.add_format({
        "num_format": "#,##0.00"
    })

    worksheet.set_column("A:A", 20)
    worksheet.set_column("B:B", 15, money_format)
```

## Uso con Polars

Polars también puede integrarse con flujos de exportación a Excel, y `xlsxwriter` puede ser relevante cuando se busca una salida `.xlsx` con formato.

Ejemplo conceptual:

```python
import polars as pl

df = pl.DataFrame({
    "Producto": ["Laptop", "Mouse"],
    "Monto": [3500, 80]
})

df.write_excel("reporte.xlsx")
```

Para necesidades muy específicas de formato, puede ser conveniente generar o ajustar el archivo con herramientas especializadas.

## Diferencia entre escribir y leer

`xlsxwriter` no está diseñado para leer archivos Excel.

Esto significa que no se usa para:

```text
abrir un archivo existente
leer celdas
modificar una plantilla ya creada
conservar macros
editar hojas ya existentes
```

Para esos casos normalmente corresponde `openpyxl`.

## Comparación con `openpyxl`

## `xlsxwriter`

- crea archivos `.xlsx` nuevos
- tiene muy buen soporte de formatos y gráficos
- se integra bien con pandas para exportación
- no lee archivos Excel existentes
- no modifica libros ya existentes

## `openpyxl`

- crea archivos `.xlsx`
- lee archivos `.xlsx`
- modifica archivos existentes
- puede trabajar con plantillas
- puede conservar macros en ciertos flujos con `.xlsm`
- tiene soporte de estilos y estructuras de Excel

## Regla práctica

Para generar reportes nuevos desde cero:

```text
xlsxwriter
```

Para abrir y modificar archivos existentes:

```text
openpyxl
```

## Casos de uso frecuentes

## Crear reporte desde cero

```python
import xlsxwriter

workbook = xlsxwriter.Workbook("reporte.xlsx")
worksheet = workbook.add_worksheet("Datos")

worksheet.write_row("A1", ["Producto", "Monto"])
worksheet.write_row("A2", ["Laptop", 3500])

workbook.close()
```

## Exportar DataFrame con formato

```python
with pd.ExcelWriter("reporte.xlsx", engine="xlsxwriter") as writer:
    df.to_excel(writer, sheet_name="Datos", index=False)
```

## Crear gráfico

```python
chart = workbook.add_chart({"type": "column"})
```

## Aplicar formato condicional

```python
worksheet.conditional_format("B2:B100", {
    "type": "cell",
    "criteria": ">",
    "value": 1000,
    "format": highlight_format
})
```

## Errores comunes

## Olvidar cerrar el libro

Problemático:

```python
workbook = xlsxwriter.Workbook("reporte.xlsx")
worksheet = workbook.add_worksheet()
worksheet.write("A1", "Dato")
```

Falta:

```python
workbook.close()
```

Si no se cierra el libro, el archivo puede quedar incompleto o no escribirse correctamente.

## Intentar leer archivos con `xlsxwriter`

Problemático:

```python
workbook = xlsxwriter.load_workbook("archivo.xlsx")
```

`xlsxwriter` no sirve para leer libros existentes.

## Confundir índices con notación Excel

Con fila y columna:

```python
worksheet.write(0, 0, "A1")
```

Con notación A1:

```python
worksheet.write("A1", "A1")
```

No deben mezclarse sin cuidado.

## Usar `xlsxwriter` para modificar plantillas

Si el flujo requiere abrir una plantilla y conservar contenido previo, corresponde evaluar `openpyxl`.

## Esperar que Excel esté instalado

`xlsxwriter` genera el archivo `.xlsx` sin necesitar abrir Excel.

No se requiere Excel para crear el archivo.

## Escribir fórmulas con separadores regionales

Las fórmulas de Excel escritas por `xlsxwriter` suelen usar sintaxis inglesa y separadores esperados por el formato interno del archivo, no necesariamente los nombres localizados de Excel.

Ejemplo:

```python
worksheet.write_formula("A1", "=SUM(B1:B10)")
```

No:

```python
worksheet.write_formula("A1", "=SUMA(B1:B10)")
```

## No controlar rutas de salida

Si se escribe:

```python
workbook = xlsxwriter.Workbook("reporte.xlsx")
```

el archivo se crea en el directorio actual de ejecución. Para mayor claridad puede usarse `pathlib`.

## Buenas prácticas

## Cerrar siempre el libro

```python
workbook.close()
```

## Usar `with` cuando se use desde pandas

```python
with pd.ExcelWriter("reporte.xlsx", engine="xlsxwriter") as writer:
    ...
```

## Separar datos, formatos y escritura

```python
records = build_records()
formats = create_formats(workbook)
write_report(worksheet, records, formats)
```

## Crear formatos una vez y reutilizarlos

```python
header_format = workbook.add_format({...})
money_format = workbook.add_format({...})
```

## Usar nombres de hojas claros

```python
workbook.add_worksheet("Datos")
workbook.add_worksheet("Resumen")
```

## Usar tablas cuando el reporte sea tabular

```python
worksheet.add_table("A1:D100", {...})
```

## Aplicar formatos por columna cuando sea posible

```python
worksheet.set_column("B:B", 15, money_format)
```

## Preferir pandas para preparar datos tabulares

```python
df = prepare_dataframe()
```

y `xlsxwriter` para generar el archivo final con presentación.

## Ejemplo integrado

```python
from pathlib import Path

import xlsxwriter


def create_formats(workbook):
    return {
        "title": workbook.add_format({
            "bold": True,
            "font_size": 14,
            "align": "center",
            "valign": "vcenter"
        }),
        "header": workbook.add_format({
            "bold": True,
            "bg_color": "#1F4E78",
            "font_color": "#FFFFFF",
            "align": "center",
            "valign": "vcenter",
            "border": 1
        }),
        "money": workbook.add_format({
            "num_format": "#,##0.00",
            "border": 1
        }),
        "text": workbook.add_format({
            "border": 1
        })
    }


def write_sales_report(records, output_path):
    workbook = xlsxwriter.Workbook(output_path)
    worksheet = workbook.add_worksheet("Ventas")

    formats = create_formats(workbook)

    worksheet.merge_range(
        "A1:D1",
        "Reporte de ventas",
        formats["title"]
    )

    headers = ["Producto", "Cantidad", "Precio", "Total"]

    worksheet.write_row("A3", headers, formats["header"])

    for row_index, record in enumerate(records, start=3):
        quantity = record["quantity"]
        price = record["price"]
        total = quantity * price

        worksheet.write(row_index, 0, record["product"], formats["text"])
        worksheet.write(row_index, 1, quantity, formats["text"])
        worksheet.write(row_index, 2, price, formats["money"])
        worksheet.write(row_index, 3, total, formats["money"])

    last_row = len(records) + 3

    worksheet.add_table(2, 0, last_row - 1, 3, {
        "columns": [
            {"header": "Producto"},
            {"header": "Cantidad"},
            {"header": "Precio"},
            {"header": "Total"}
        ],
        "style": "Table Style Medium 9"
    })

    worksheet.set_column("A:A", 20)
    worksheet.set_column("B:B", 12)
    worksheet.set_column("C:D", 15, formats["money"])
    worksheet.freeze_panes(3, 0)
    worksheet.autofilter(2, 0, last_row - 1, 3)

    chart = workbook.add_chart({
        "type": "column"
    })

    chart.add_series({
        "name": "Total",
        "categories": ["Ventas", 3, 0, last_row - 1, 0],
        "values": ["Ventas", 3, 3, last_row - 1, 3]
    })

    chart.set_title({
        "name": "Total por producto"
    })

    worksheet.insert_chart("F3", chart)

    workbook.close()


records = [
    {"product": "Laptop", "quantity": 2, "price": 3500},
    {"product": "Mouse", "quantity": 10, "price": 80},
    {"product": "Teclado", "quantity": 5, "price": 150}
]

write_sales_report(
    records,
    Path("reporte_ventas.xlsx")
)

print("Reporte generado correctamente")
```

## Relación con otras librerías

`xlsxwriter` se relaciona especialmente con:

- `pandas`, para exportar `DataFrame` a Excel con formato
- `polars`, por flujos modernos de exportación tabular
- `openpyxl`, como alternativa cuando se requiere leer o modificar archivos existentes
- `pyxlsb`, cuando se necesita leer archivos binarios `.xlsb`
- `pathlib`, para manejar rutas de salida
- `datetime`, para escribir fechas con formato
- `matplotlib` o `plotly`, cuando se generan imágenes de gráficos para insertar en reportes

## Orden didáctico interno

```text
1. Propósito de xlsxwriter
2. Instalación e importación
3. Workbook, Worksheet y Format
4. Crear libros y hojas
5. Escribir celdas, filas y columnas
6. Formatos
7. Fórmulas e hipervínculos
8. Tablas y autofiltros
9. Formato condicional y validación
10. Gráficos e imágenes
11. Uso con pandas
12. Diferencia frente a openpyxl
13. Errores comunes
14. Buenas prácticas
```