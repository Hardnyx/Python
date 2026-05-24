# `pypdf`

## Propósito

`pypdf` es una librería externa para leer, dividir, unir, extraer texto, modificar páginas, manejar metadatos, cifrar y descifrar archivos PDF desde Python.

Se utiliza para automatizar tareas documentales relacionadas con PDFs, como combinar varios archivos, separar páginas específicas, extraer texto de documentos digitales, leer metadatos, rotar páginas, agregar marcas de agua simples o generar un PDF nuevo a partir de páginas existentes.

## Naturaleza de la librería

`pypdf` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install pypdf
```

Importaciones frecuentes:

```python
from pypdf import PdfReader
from pypdf import PdfWriter
```

`PdfReader` se usa para leer archivos PDF.

`PdfWriter` se usa para crear o escribir archivos PDF nuevos a partir de páginas, archivos o modificaciones.

## Idea central

La idea principal de `pypdf` es trabajar con PDFs a nivel de documento y páginas.

```text
PDF de entrada -> PdfReader -> páginas -> PdfWriter -> PDF de salida
```

Ejemplo básico:

```python
from pypdf import PdfReader

reader = PdfReader("documento.pdf")

print(len(reader.pages))
print(reader.pages[0].extract_text())
```

## Cuándo usar `pypdf`

Conviene usar `pypdf` cuando se necesita:

- leer archivos PDF
- contar páginas
- extraer texto de PDFs digitales
- unir varios PDFs
- separar páginas específicas
- crear un PDF nuevo desde páginas existentes
- rotar páginas
- recortar páginas
- leer metadatos
- agregar metadatos
- cifrar o descifrar PDFs
- aplicar marcas de agua simples
- automatizar flujos documentales

## Cuándo no usar `pypdf`

No conviene usar `pypdf` cuando se necesita:

- convertir imágenes escaneadas a texto
- hacer OCR
- renderizar visualmente páginas como imágenes
- extraer tablas complejas con precisión
- preservar estructura visual compleja
- editar texto interno de un PDF como si fuera Word
- convertir PDF a Word o Excel con alta fidelidad

Para OCR suelen requerirse herramientas como motores OCR.

Para extracción tabular compleja pueden requerirse librerías especializadas.

Para renderizar páginas como imágenes se necesitan herramientas orientadas a renderizado de PDF.

## Instalación

Instalación básica:

```bash
python -m pip install pypdf
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
pypdf==6.x.x
```

La versión exacta puede variar según el entorno.

## Importación

```python
from pypdf import PdfReader, PdfWriter
```

Verificación:

```python
import pypdf

print(pypdf.__version__)
```

## Objetos principales

## `PdfReader`

`PdfReader` permite abrir y leer un PDF.

```python
from pypdf import PdfReader

reader = PdfReader("documento.pdf")
```

Desde un `PdfReader` se puede acceder a:

- páginas
- metadatos
- estado de cifrado
- campos de formulario
- texto extraído
- objetos internos del PDF

## `PdfWriter`

`PdfWriter` permite crear un PDF de salida.

```python
from pypdf import PdfWriter

writer = PdfWriter()
```

Desde un `PdfWriter` se puede:

- agregar páginas
- unir archivos
- insertar páginas
- escribir un archivo final
- agregar metadatos
- cifrar el PDF
- manipular ciertas preferencias del documento

## Abrir un PDF

```python
from pypdf import PdfReader

reader = PdfReader("documento.pdf")

print(reader)
```

## Contar páginas

```python
from pypdf import PdfReader

reader = PdfReader("documento.pdf")

page_count = len(reader.pages)

print(page_count)
```

## Acceder a una página

```python
from pypdf import PdfReader

reader = PdfReader("documento.pdf")

first_page = reader.pages[0]

print(first_page)
```

Las páginas se indexan desde `0`.

```text
reader.pages[0] -> primera página
reader.pages[1] -> segunda página
reader.pages[-1] -> última página
```

## Extraer texto

## Texto de una página

```python
from pypdf import PdfReader

reader = PdfReader("documento.pdf")

page = reader.pages[0]
text = page.extract_text()

print(text)
```

## Texto de todas las páginas

```python
from pypdf import PdfReader

reader = PdfReader("documento.pdf")

texts = []

for page in reader.pages:
    text = page.extract_text()
    texts.append(text)

full_text = "\n".join(texts)

print(full_text)
```

## Guardar texto extraído

```python
from pathlib import Path
from pypdf import PdfReader

reader = PdfReader("documento.pdf")

texts = []

for page in reader.pages:
    text = page.extract_text()

    if text is not None:
        texts.append(text)

Path("texto_extraido.txt").write_text(
    "\n\n".join(texts),
    encoding="utf-8"
)

print("Texto extraído correctamente")
```

## Limitaciones de la extracción de texto

La extracción de texto en PDFs puede ser difícil porque el PDF no siempre almacena el contenido como párrafos ordenados.

Problemas frecuentes:

- texto dividido en fragmentos
- saltos de línea inesperados
- columnas mezcladas
- tablas sin estructura clara
- encabezados y pies repetidos
- texto rotado
- caracteres especiales
- documentos escaneados como imagen

Si el PDF es escaneado, `pypdf` no puede extraer texto real porque no hay capa textual. En ese caso se requiere OCR.

## Extraer texto con validación

```python
from pathlib import Path
from pypdf import PdfReader


def extract_pdf_text(input_path):
    reader = PdfReader(input_path)

    texts = []

    for page_number, page in enumerate(reader.pages, start=1):
        text = page.extract_text()

        if not text:
            continue

        texts.append(f"Página {page_number}")
        texts.append(text)

    return "\n\n".join(texts)


text = extract_pdf_text("documento.pdf")

Path("salida.txt").write_text(
    text,
    encoding="utf-8"
)

print("Extracción finalizada")
```

## Leer metadatos

```python
from pypdf import PdfReader

reader = PdfReader("documento.pdf")

metadata = reader.metadata

print(metadata)
```

## Metadatos frecuentes

```python
print(metadata.title)
print(metadata.author)
print(metadata.subject)
print(metadata.creator)
print(metadata.producer)
```

Algunos campos pueden devolver `None` si no existen.

## Crear un PDF nuevo

```python
from pypdf import PdfWriter

writer = PdfWriter()

with open("salida.pdf", "wb") as file:
    writer.write(file)
```

Un PDF vacío no suele ser útil por sí mismo, pero el patrón muestra que `PdfWriter` escribe el archivo final.

## Copiar páginas a un PDF nuevo

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("documento.pdf")
writer = PdfWriter()

for page in reader.pages:
    writer.add_page(page)

with open("copia.pdf", "wb") as file:
    writer.write(file)

print("PDF copiado correctamente")
```

## Extraer una página

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("documento.pdf")
writer = PdfWriter()

writer.add_page(reader.pages[0])

with open("primera_pagina.pdf", "wb") as file:
    writer.write(file)
```

## Extraer un rango de páginas

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("documento.pdf")
writer = PdfWriter()

start_page = 0
end_page = 3

for page in reader.pages[start_page:end_page]:
    writer.add_page(page)

with open("paginas_1_a_3.pdf", "wb") as file:
    writer.write(file)
```

El índice superior no se incluye.

```text
pages[0:3] -> páginas 1, 2 y 3
```

## Separar todas las páginas

```python
from pathlib import Path
from pypdf import PdfReader, PdfWriter


def split_pdf(input_path, output_dir):
    output_dir.mkdir(parents=True, exist_ok=True)

    reader = PdfReader(input_path)

    for index, page in enumerate(reader.pages, start=1):
        writer = PdfWriter()
        writer.add_page(page)

        output_path = output_dir / f"pagina_{index:03}.pdf"

        with output_path.open("wb") as file:
            writer.write(file)


split_pdf(
    Path("documento.pdf"),
    Path("paginas")
)

print("PDF separado correctamente")
```

## Unir PDFs

## Unión básica con `append()`

```python
from pypdf import PdfWriter

writer = PdfWriter()

for pdf_path in ["archivo_1.pdf", "archivo_2.pdf", "archivo_3.pdf"]:
    writer.append(pdf_path)

with open("unido.pdf", "wb") as file:
    writer.write(file)

print("PDF unido correctamente")
```

## Unir PDFs desde rutas

```python
from pathlib import Path
from pypdf import PdfWriter


def merge_pdfs(input_paths, output_path):
    writer = PdfWriter()

    for input_path in input_paths:
        writer.append(str(input_path))

    with output_path.open("wb") as file:
        writer.write(file)


merge_pdfs(
    [
        Path("archivo_1.pdf"),
        Path("archivo_2.pdf"),
        Path("archivo_3.pdf")
    ],
    Path("unido.pdf")
)
```

## Agregar solo algunas páginas de un PDF

```python
from pypdf import PdfWriter

writer = PdfWriter()

writer.append(
    "documento.pdf",
    pages=(0, 3)
)

with open("primeras_tres_paginas.pdf", "wb") as file:
    writer.write(file)
```

## Insertar páginas en una posición

```python
from pypdf import PdfWriter

writer = PdfWriter()

writer.append("base.pdf")
writer.merge(
    position=1,
    fileobj="insertar.pdf",
    pages=(0, 1)
)

with open("resultado.pdf", "wb") as file:
    writer.write(file)
```

`merge()` permite insertar páginas en una posición específica del PDF de salida.

## Rotar páginas

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("documento.pdf")
writer = PdfWriter()

page = reader.pages[0]
page.rotate(90)

writer.add_page(page)

with open("pagina_rotada.pdf", "wb") as file:
    writer.write(file)
```

## Rotar todas las páginas

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("documento.pdf")
writer = PdfWriter()

for page in reader.pages:
    page.rotate(90)
    writer.add_page(page)

with open("documento_rotado.pdf", "wb") as file:
    writer.write(file)
```

## Recortar páginas

Un PDF maneja cajas de página, como `mediabox` y `cropbox`.

Ejemplo simple:

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("documento.pdf")
writer = PdfWriter()

page = reader.pages[0]

page.cropbox.lower_left = (50, 50)
page.cropbox.upper_right = (500, 750)

writer.add_page(page)

with open("pagina_recortada.pdf", "wb") as file:
    writer.write(file)
```

Este tipo de operación requiere conocer coordenadas del sistema de página del PDF.

## Agregar marca de agua

Una marca de agua simple puede aplicarse fusionando una página sobre otra.

```python
from pypdf import PdfReader, PdfWriter

content_reader = PdfReader("documento.pdf")
watermark_reader = PdfReader("marca_agua.pdf")

watermark_page = watermark_reader.pages[0]

writer = PdfWriter()

for page in content_reader.pages:
    page.merge_page(watermark_page)
    writer.add_page(page)

with open("documento_con_marca.pdf", "wb") as file:
    writer.write(file)
```

La marca de agua debe estar preparada previamente como PDF.

## Agregar metadatos

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("documento.pdf")
writer = PdfWriter()

for page in reader.pages:
    writer.add_page(page)

writer.add_metadata({
    "/Title": "Reporte generado",
    "/Author": "Python",
    "/Subject": "Documento procesado con pypdf"
})

with open("documento_con_metadata.pdf", "wb") as file:
    writer.write(file)
```

## Cifrar un PDF

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("documento.pdf")
writer = PdfWriter()

for page in reader.pages:
    writer.add_page(page)

writer.encrypt("clave")

with open("documento_cifrado.pdf", "wb") as file:
    writer.write(file)
```

## Leer un PDF cifrado

```python
from pypdf import PdfReader

reader = PdfReader("documento_cifrado.pdf")

if reader.is_encrypted:
    reader.decrypt("clave")

print(len(reader.pages))
```

## Descifrar y guardar una copia sin cifrado

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("documento_cifrado.pdf")

if reader.is_encrypted:
    reader.decrypt("clave")

writer = PdfWriter()

for page in reader.pages:
    writer.add_page(page)

with open("documento_descifrado.pdf", "wb") as file:
    writer.write(file)
```

Este patrón requiere tener autorización para abrir y procesar el archivo.

## Formularios PDF

Algunos PDFs contienen campos de formulario.

`pypdf` puede leer ciertos campos de formulario.

```python
from pypdf import PdfReader

reader = PdfReader("formulario.pdf")

fields = reader.get_fields()

print(fields)
```

También pueden existir limitaciones según la estructura del formulario y el tipo de PDF.

## Leer adjuntos

Algunos PDFs pueden contener archivos adjuntos. `pypdf` tiene soporte para trabajar con ciertos adjuntos, aunque este uso es más especializado.

En documentación inicial, conviene priorizar:

```text
lectura de páginas
extracción de texto
unión
separación
metadatos
cifrado
```

## Uso con `pathlib`

```python
from pathlib import Path
from pypdf import PdfReader

input_path = Path("documento.pdf")

reader = PdfReader(input_path)

print(len(reader.pages))
```

Para escribir:

```python
from pathlib import Path
from pypdf import PdfReader, PdfWriter

input_path = Path("documento.pdf")
output_path = Path("salida.pdf")

reader = PdfReader(input_path)
writer = PdfWriter()

for page in reader.pages:
    writer.add_page(page)

with output_path.open("wb") as file:
    writer.write(file)
```

## Uso con archivos abiertos

También se pueden abrir archivos en modo binario.

```python
from pypdf import PdfReader

with open("documento.pdf", "rb") as file:
    reader = PdfReader(file)
    print(len(reader.pages))
```

Para escribir:

```python
with open("salida.pdf", "wb") as file:
    writer.write(file)
```

## Procesar una carpeta de PDFs

```python
from pathlib import Path
from pypdf import PdfWriter


def merge_folder_pdfs(input_dir, output_path):
    pdf_paths = sorted(input_dir.glob("*.pdf"))

    writer = PdfWriter()

    for pdf_path in pdf_paths:
        writer.append(str(pdf_path))

    with output_path.open("wb") as file:
        writer.write(file)


merge_folder_pdfs(
    Path("pdfs"),
    Path("pdfs_unidos.pdf")
)

print("PDFs unidos correctamente")
```

## Extraer texto de una carpeta de PDFs

```python
from pathlib import Path
from pypdf import PdfReader


def extract_text_from_pdf(pdf_path):
    reader = PdfReader(pdf_path)

    parts = []

    for page in reader.pages:
        text = page.extract_text()

        if text:
            parts.append(text)

    return "\n\n".join(parts)


input_dir = Path("pdfs")
output_dir = Path("textos")
output_dir.mkdir(parents=True, exist_ok=True)

for pdf_path in input_dir.glob("*.pdf"):
    text = extract_text_from_pdf(pdf_path)

    output_path = output_dir / f"{pdf_path.stem}.txt"

    output_path.write_text(
        text,
        encoding="utf-8"
    )

print("Textos extraídos correctamente")
```

## Relación con PDFs escaneados

Un PDF escaneado suele contener imágenes de páginas, no texto real.

En ese caso:

```python
page.extract_text()
```

puede devolver:

```text
None
```

o una cadena vacía.

Para estos casos se necesita OCR.

`pypdf` no reemplaza a un motor OCR.

## Relación con extracción tabular

`pypdf` puede extraer texto, pero no siempre conserva estructura tabular.

Para tablas complejas, puede ser necesario usar herramientas especializadas de extracción de tablas PDF.

En flujos simples, puede extraerse texto y luego limpiarlo manualmente, pero no debe asumirse que las columnas quedarán bien alineadas.

## Relación con imágenes

`pypdf` puede trabajar con ciertos recursos de imágenes del PDF, pero no es una librería general de procesamiento de imágenes.

Para manipulación de imágenes suele corresponder:

```text
pillow
```

Para renderizar páginas PDF como imágenes, normalmente se requieren herramientas distintas.

## Relación con pandas

`pypdf` puede extraer texto que luego se procesa con `pandas`, pero no convierte automáticamente un PDF en un DataFrame.

Flujo típico:

```text
pypdf -> texto -> limpieza -> pandas
```

Ejemplo conceptual:

```python
from pathlib import Path
import pandas as pd
from pypdf import PdfReader

reader = PdfReader("documento.pdf")

rows = []

for page_number, page in enumerate(reader.pages, start=1):
    text = page.extract_text()

    if text is None:
        continue

    rows.append({
        "page": page_number,
        "text": text
    })

df = pd.DataFrame(rows)

df.to_excel("texto_pdf.xlsx", index=False)
```

## Relación con python-docx y python-pptx

`pypdf` sirve para procesar PDFs.

`python-docx` sirve para crear o modificar documentos Word.

`python-pptx` sirve para crear o modificar presentaciones PowerPoint.

Un flujo documental puede combinar estas herramientas, pero cada una opera sobre formatos distintos.

## Casos de uso frecuentes

## Contar páginas

```python
reader = PdfReader("documento.pdf")
print(len(reader.pages))
```

## Extraer texto

```python
text = reader.pages[0].extract_text()
```

## Unir archivos

```python
writer = PdfWriter()
writer.append("archivo_1.pdf")
writer.append("archivo_2.pdf")
writer.write("unido.pdf")
```

## Separar páginas

```python
writer = PdfWriter()
writer.add_page(reader.pages[0])
writer.write("pagina_1.pdf")
```

## Rotar páginas

```python
page.rotate(90)
```

## Cifrar archivo

```python
writer.encrypt("clave")
```

## Errores comunes

## Esperar OCR automático

Problemático:

```python
text = page.extract_text()
```

en un PDF escaneado.

Si el PDF contiene solo imágenes, no hay texto real para extraer.

## No abrir archivos de salida en modo binario

Problemático:

```python
with open("salida.pdf", "w") as file:
    writer.write(file)
```

Correcto:

```python
with open("salida.pdf", "wb") as file:
    writer.write(file)
```

## Confundir índices de página

```python
reader.pages[0]
```

es la primera página, no la página cero del documento desde el punto de vista del usuario final.

## Sobrescribir archivos originales

Problemático:

```python
with open("documento.pdf", "wb") as file:
    writer.write(file)
```

si `documento.pdf` era el archivo original.

Más seguro:

```python
with open("documento_procesado.pdf", "wb") as file:
    writer.write(file)
```

## No validar PDFs cifrados

Problemático:

```python
reader = PdfReader("documento.pdf")
print(len(reader.pages))
```

si el archivo está cifrado.

Mejor:

```python
if reader.is_encrypted:
    reader.decrypt("clave")
```

## Esperar extracción tabular perfecta

El texto de una tabla puede salir desordenado o sin estructura clara.

Para tablas complejas deben evaluarse herramientas específicas.

## Procesar archivos muy grandes sin control

La extracción de texto puede consumir mucha memoria en PDFs complejos.

Conviene procesar por páginas, validar tamaños y manejar errores.

## No manejar excepciones

Los PDFs pueden estar dañados, cifrados, incompletos o tener estructuras no estándar.

Conviene manejar errores en flujos por lotes.

## Buenas prácticas

## Usar `PdfReader` para leer y `PdfWriter` para escribir

```python
reader = PdfReader("entrada.pdf")
writer = PdfWriter()
```

## Guardar con nombre de salida distinto

```python
salida.pdf
```

en lugar de sobrescribir el archivo original.

## Usar `pathlib` para rutas

```python
from pathlib import Path
```

## Validar texto extraído

```python
if text:
    ...
```

## Procesar por páginas

```python
for page in reader.pages:
    ...
```

## Manejar PDFs cifrados explícitamente

```python
if reader.is_encrypted:
    reader.decrypt(password)
```

## Usar OCR solo cuando el PDF sea escaneado

`pypdf` no extrae texto de imágenes escaneadas.

## Separar funciones por tarea

```python
extract_text()
merge_pdfs()
split_pdf()
encrypt_pdf()
```

## Ejemplo integrado

```python
from pathlib import Path

from pypdf import PdfReader, PdfWriter


def extract_text(input_path):
    reader = PdfReader(input_path)

    if reader.is_encrypted:
        raise ValueError("El PDF está cifrado y requiere contraseña")

    parts = []

    for page_number, page in enumerate(reader.pages, start=1):
        text = page.extract_text()

        if not text:
            continue

        parts.append(f"Página {page_number}")
        parts.append(text)

    return "\n\n".join(parts)


def extract_pages(input_path, output_path, start_page, end_page):
    reader = PdfReader(input_path)
    writer = PdfWriter()

    for page in reader.pages[start_page:end_page]:
        writer.add_page(page)

    with output_path.open("wb") as file:
        writer.write(file)


def merge_pdfs(input_paths, output_path):
    writer = PdfWriter()

    for input_path in input_paths:
        writer.append(str(input_path))

    with output_path.open("wb") as file:
        writer.write(file)


input_pdf = Path("documento.pdf")
text_output = Path("documento.txt")
pages_output = Path("primeras_paginas.pdf")
merged_output = Path("documentos_unidos.pdf")

text = extract_text(input_pdf)

text_output.write_text(
    text,
    encoding="utf-8"
)

extract_pages(
    input_pdf,
    pages_output,
    start_page=0,
    end_page=3
)

merge_pdfs(
    [
        Path("archivo_1.pdf"),
        Path("archivo_2.pdf")
    ],
    merged_output
)

print("Procesamiento PDF finalizado correctamente")
```

## Relación con otras librerías

`pypdf` se relaciona especialmente con:

- `pathlib`, para manejar rutas de archivos
- `pandas`, cuando el texto extraído se estructura en tablas
- `python-docx`, cuando se generan reportes Word a partir de contenido extraído
- `python-pptx`, cuando se generan presentaciones a partir de resultados
- `pillow`, cuando se trabaja con imágenes, aunque no como renderizador PDF
- herramientas OCR, cuando el PDF contiene páginas escaneadas como imagen
- librerías especializadas de extracción tabular, cuando se necesitan tablas desde PDFs

## Orden didáctico interno

```text
1. Propósito de pypdf
2. Instalación e importación
3. PdfReader y PdfWriter
4. Abrir PDFs y contar páginas
5. Extraer texto
6. Leer metadatos
7. Crear PDFs desde páginas existentes
8. Separar páginas
9. Unir PDFs
10. Rotar y recortar páginas
11. Marcas de agua simples
12. Cifrado y descifrado
13. Limitaciones de OCR y tablas
14. Errores comunes
15. Buenas prácticas
```