El siguiente archivo debería ser:

```text
docs/04-external-libraries/05-archivos-y-formatos/pillow.md
```

Base factual principal: `Pillow` es la continuación moderna de PIL para trabajar con imágenes en Python. Su documentación oficial muestra el uso de `Image.open()`, `rotate()`, `show()`, `thumbnail()` y `save()`, además del patrón de importación `from PIL import Image`. ([Pillow (PIL Fork)][1])

Contenido propuesto:

````markdown
# `pillow`

## Propósito

`pillow` es una librería externa para abrir, leer, modificar, crear y guardar imágenes desde Python. Se utiliza para automatizar tareas de procesamiento de imágenes, redimensionar archivos, convertir formatos, recortar, rotar, aplicar filtros, trabajar con transparencia, crear miniaturas, agregar texto, preparar imágenes para reportes o generar recursos visuales para documentos.

## Naturaleza de la librería

`pillow` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install pillow
```

La importación habitual es:

```python
from PIL import Image
```

El nombre de instalación es `pillow`, pero el paquete se importa desde `PIL`.

Esto ocurre porque Pillow conserva compatibilidad con la interfaz histórica de Python Imaging Library.

## Idea central

La idea principal de Pillow es trabajar con imágenes mediante objetos `Image`.

Flujo típico:

```text
archivo de imagen -> Image.open() -> objeto Image -> transformación -> save()
```

Ejemplo básico:

```python
from PIL import Image

image = Image.open("imagen.jpg")

print(image.size)
print(image.mode)
print(image.format)

image.save("copia.png")
```

## Cuándo usar `pillow`

Conviene usar `pillow` cuando se necesita:

- abrir imágenes
- convertir formatos
- redimensionar imágenes
- crear miniaturas
- recortar regiones
- rotar o voltear imágenes
- trabajar con transparencia
- aplicar filtros simples
- agregar texto a una imagen
- insertar marcas visuales
- preparar imágenes para Word, PowerPoint o Excel
- procesar carpetas completas de imágenes

## Cuándo no usar `pillow`

No conviene usar `pillow` cuando se necesita:

- procesamiento avanzado de visión por computadora
- detección de objetos
- OCR
- entrenamiento de modelos de imágenes
- análisis profundo de video
- edición profesional compleja
- manipulación vectorial de SVG como formato principal

Para visión por computadora suele usarse `opencv-python`.

Para OCR se requieren herramientas especializadas.

Para gráficos vectoriales o diagramas, pueden corresponder otras herramientas.

## Instalación

Instalación básica:

```bash
python -m pip install pillow
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
Pillow==12.x.x
```

La versión exacta puede variar según el entorno.

## Importación

Importación principal:

```python
from PIL import Image
```

Importaciones frecuentes adicionales:

```python
from PIL import ImageFilter
from PIL import ImageEnhance
from PIL import ImageDraw
from PIL import ImageFont
from PIL import ImageOps
```

Verificación:

```python
import PIL

print(PIL.__version__)
```

## Objeto principal: `Image`

El objeto principal de Pillow es `Image`.

```python
from PIL import Image

image = Image.open("imagen.jpg")

print(type(image))
```

Un objeto `Image` contiene información como:

- ancho y alto
- modo de color
- formato original
- datos de píxeles
- metadatos disponibles
- métodos de transformación

## Abrir una imagen

```python
from PIL import Image

image = Image.open("imagen.jpg")

print(image)
```

Uso recomendado con `with`:

```python
from PIL import Image

with Image.open("imagen.jpg") as image:
    print(image.size)
    print(image.mode)
```

El uso de `with` ayuda a cerrar el archivo correctamente después de leerlo.

## Propiedades principales

## `size`

Devuelve una tupla con ancho y alto.

```python
from PIL import Image

with Image.open("imagen.jpg") as image:
    print(image.size)
```

Salida conceptual:

```text
(width, height)
```

## `width` y `height`

```python
print(image.width)
print(image.height)
```

## `mode`

Indica el modo de color.

```python
print(image.mode)
```

Modos frecuentes:

```text
RGB
RGBA
L
P
CMYK
```

## `format`

Indica el formato leído desde el archivo.

```python
print(image.format)
```

Ejemplos:

```text
JPEG
PNG
WEBP
BMP
TIFF
```

## Guardar una imagen

```python
from PIL import Image

with Image.open("imagen.jpg") as image:
    image.save("salida.png")
```

Pillow puede inferir el formato por la extensión del archivo.

También puede indicarse explícitamente:

```python
image.save("salida.jpg", format="JPEG")
```

## Convertir formato

```python
from PIL import Image

with Image.open("imagen.png") as image:
    rgb_image = image.convert("RGB")
    rgb_image.save("imagen_convertida.jpg")
```

La conversión a `RGB` suele ser necesaria cuando se guarda como JPEG una imagen que tiene transparencia.

## Modos de color

## `RGB`

Imagen con tres canales:

```text
rojo
verde
azul
```

## `RGBA`

Imagen con cuatro canales:

```text
rojo
verde
azul
alpha
```

El canal `alpha` representa transparencia.

## `L`

Imagen en escala de grises.

## `CMYK`

Modo usado en algunos flujos de impresión.

## Convertir a escala de grises

```python
from PIL import Image

with Image.open("imagen.jpg") as image:
    gray_image = image.convert("L")
    gray_image.save("imagen_gris.jpg")
```

## Convertir a RGB

```python
rgb_image = image.convert("RGB")
```

## Convertir a RGBA

```python
rgba_image = image.convert("RGBA")
```

## Redimensionar imágenes

## `resize()`

`resize()` devuelve una nueva imagen con el tamaño indicado.

```python
from PIL import Image

with Image.open("imagen.jpg") as image:
    resized = image.resize((800, 600))
    resized.save("imagen_redimensionada.jpg")
```

El tamaño se indica como:

```text
(width, height)
```

## Redimensionar conservando proporción

Para conservar proporción, debe calcularse el nuevo tamaño.

```python
from PIL import Image

with Image.open("imagen.jpg") as image:
    target_width = 800

    ratio = target_width / image.width
    target_height = int(image.height * ratio)

    resized = image.resize((target_width, target_height))
    resized.save("imagen_redimensionada.jpg")
```

## `thumbnail()`

`thumbnail()` modifica la imagen en sitio y conserva la proporción dentro de un tamaño máximo.

```python
from PIL import Image

with Image.open("imagen.jpg") as image:
    image.thumbnail((300, 300))
    image.save("miniatura.jpg")
```

Diferencia importante:

```text
resize()    -> devuelve una nueva imagen con tamaño exacto
thumbnail() -> modifica la imagen y conserva proporción dentro de un límite
```

## Copiar antes de modificar

Como algunos métodos modifican el objeto, puede convenir usar `copy()`.

```python
from PIL import Image

with Image.open("imagen.jpg") as image:
    thumbnail = image.copy()
    thumbnail.thumbnail((300, 300))
    thumbnail.save("miniatura.jpg")
```

## Recortar imágenes

## `crop()`

`crop()` recorta una región rectangular.

```python
from PIL import Image

with Image.open("imagen.jpg") as image:
    cropped = image.crop((100, 100, 500, 400))
    cropped.save("recorte.jpg")
```

La caja se define como:

```text
(left, upper, right, lower)
```

Interpretación:

```text
left  -> coordenada x inicial
upper -> coordenada y inicial
right -> coordenada x final
lower -> coordenada y final
```

## Recortar el centro

```python
from PIL import Image


def crop_center(image, crop_width, crop_height):
    left = (image.width - crop_width) // 2
    upper = (image.height - crop_height) // 2
    right = left + crop_width
    lower = upper + crop_height

    return image.crop((left, upper, right, lower))


with Image.open("imagen.jpg") as image:
    cropped = crop_center(image, 500, 500)
    cropped.save("recorte_central.jpg")
```

## Recortar a cuadrado

```python
from PIL import Image


def crop_square(image):
    side = min(image.width, image.height)

    left = (image.width - side) // 2
    upper = (image.height - side) // 2
    right = left + side
    lower = upper + side

    return image.crop((left, upper, right, lower))


with Image.open("imagen.jpg") as image:
    square = crop_square(image)
    square.save("imagen_cuadrada.jpg")
```

## Rotar imágenes

## `rotate()`

```python
from PIL import Image

with Image.open("imagen.jpg") as image:
    rotated = image.rotate(90)
    rotated.save("imagen_rotada.jpg")
```

## Rotar expandiendo el lienzo

```python
rotated = image.rotate(45, expand=True)
```

`expand=True` ajusta el tamaño del lienzo para evitar recortes innecesarios.

## Voltear imágenes

Pillow permite invertir imágenes mediante `transpose()`.

```python
from PIL import Image

with Image.open("imagen.jpg") as image:
    flipped = image.transpose(Image.Transpose.FLIP_LEFT_RIGHT)
    flipped.save("imagen_volteada.jpg")
```

Voltear verticalmente:

```python
flipped = image.transpose(Image.Transpose.FLIP_TOP_BOTTOM)
```

## Transponer y rotar por constantes

```python
rotated = image.transpose(Image.Transpose.ROTATE_90)
```

También existen opciones como:

```python
Image.Transpose.ROTATE_180
Image.Transpose.ROTATE_270
```

## Transparencia

## Trabajar con RGBA

Para trabajar con transparencia, suele usarse modo `RGBA`.

```python
from PIL import Image

with Image.open("imagen.png") as image:
    rgba_image = image.convert("RGBA")
    print(rgba_image.mode)
```

## Crear imagen transparente

```python
from PIL import Image

image = Image.new(
    "RGBA",
    (400, 300),
    (255, 255, 255, 0)
)

image.save("transparente.png")
```

El color se define como:

```text
(R, G, B, A)
```

Donde `A` es el canal alpha.

```text
0   -> completamente transparente
255 -> completamente opaco
```

## Pegar una imagen sobre otra

```python
from PIL import Image

background = Image.new("RGB", (800, 600), "white")

with Image.open("logo.png") as logo:
    logo = logo.convert("RGBA")
    background.paste(logo, (50, 50), logo)

background.save("composicion.jpg")
```

El tercer argumento en `paste()` usa la transparencia de la imagen pegada como máscara.

## Crear imágenes nuevas

## `Image.new()`

```python
from PIL import Image

image = Image.new("RGB", (800, 600), "white")

image.save("lienzo.jpg")
```

Con color RGB:

```python
image = Image.new("RGB", (800, 600), (255, 255, 255))
```

Con transparencia:

```python
image = Image.new("RGBA", (800, 600), (255, 255, 255, 0))
```

## Dibujar sobre imágenes

Para dibujar se usa `ImageDraw`.

```python
from PIL import Image, ImageDraw

image = Image.new("RGB", (800, 400), "white")
draw = ImageDraw.Draw(image)

draw.rectangle(
    (50, 50, 300, 200),
    outline="black",
    width=3
)

draw.text(
    (50, 250),
    "Texto de ejemplo",
    fill="black"
)

image.save("dibujo.png")
```

## Dibujar líneas

```python
draw.line(
    (50, 50, 300, 300),
    fill="blue",
    width=5
)
```

## Dibujar rectángulos

```python
draw.rectangle(
    (50, 50, 300, 200),
    fill="lightgray",
    outline="black"
)
```

## Dibujar círculos o elipses

```python
draw.ellipse(
    (100, 100, 300, 300),
    fill="red"
)
```

## Agregar texto

```python
from PIL import Image, ImageDraw

image = Image.new("RGB", (800, 300), "white")
draw = ImageDraw.Draw(image)

draw.text(
    (50, 100),
    "Reporte generado",
    fill="black"
)

image.save("texto.png")
```

## Fuente personalizada

```python
from PIL import Image, ImageDraw, ImageFont

image = Image.new("RGB", (800, 300), "white")
draw = ImageDraw.Draw(image)

font = ImageFont.truetype("arial.ttf", 32)

draw.text(
    (50, 100),
    "Reporte generado",
    font=font,
    fill="black"
)

image.save("texto_fuente.png")
```

Debe tenerse cuidado con rutas de fuentes. No conviene depender de una fuente que no exista en el entorno de ejecución.

## Filtros

Los filtros se importan desde `ImageFilter`.

```python
from PIL import Image, ImageFilter

with Image.open("imagen.jpg") as image:
    blurred = image.filter(ImageFilter.BLUR)
    blurred.save("imagen_blur.jpg")
```

Filtros frecuentes:

```python
ImageFilter.BLUR
ImageFilter.CONTOUR
ImageFilter.DETAIL
ImageFilter.EDGE_ENHANCE
ImageFilter.SHARPEN
ImageFilter.SMOOTH
```

## Desenfoque gaussiano

```python
from PIL import Image, ImageFilter

with Image.open("imagen.jpg") as image:
    blurred = image.filter(
        ImageFilter.GaussianBlur(radius=3)
    )

    blurred.save("imagen_gaussian_blur.jpg")
```

## Ajustes de imagen

Los ajustes se hacen con `ImageEnhance`.

## Brillo

```python
from PIL import Image, ImageEnhance

with Image.open("imagen.jpg") as image:
    enhancer = ImageEnhance.Brightness(image)
    result = enhancer.enhance(1.3)
    result.save("imagen_mas_brillante.jpg")
```

Valores habituales:

```text
1.0 -> sin cambio
>1  -> aumenta
<1  -> reduce
```

## Contraste

```python
enhancer = ImageEnhance.Contrast(image)
result = enhancer.enhance(1.5)
```

## Color

```python
enhancer = ImageEnhance.Color(image)
result = enhancer.enhance(1.2)
```

## Nitidez

```python
enhancer = ImageEnhance.Sharpness(image)
result = enhancer.enhance(2.0)
```

## Operaciones con `ImageOps`

`ImageOps` contiene operaciones comunes de transformación.

```python
from PIL import Image, ImageOps

with Image.open("imagen.jpg") as image:
    grayscale = ImageOps.grayscale(image)
    grayscale.save("gris.jpg")
```

## Invertir colores

```python
inverted = ImageOps.invert(image.convert("RGB"))
```

## Ajustar a tamaño con recorte

```python
from PIL import Image, ImageOps

with Image.open("imagen.jpg") as image:
    fitted = ImageOps.fit(image, (500, 500))
    fitted.save("ajustada.jpg")
```

`ImageOps.fit()` ajusta una imagen a un tamaño objetivo recortando si es necesario.

## Bordes

```python
bordered = ImageOps.expand(
    image,
    border=20,
    fill="white"
)
```

## Trabajar con píxeles

## Leer píxel

```python
from PIL import Image

with Image.open("imagen.jpg") as image:
    pixel = image.getpixel((10, 10))
    print(pixel)
```

## Modificar píxel

```python
from PIL import Image

image = Image.new("RGB", (100, 100), "white")

image.putpixel((10, 10), (255, 0, 0))

image.save("pixel.png")
```

Para operaciones masivas sobre píxeles, suele ser más eficiente usar `numpy`.

## Uso con NumPy

Pillow puede convertirse a arreglos NumPy.

```python
from PIL import Image
import numpy as np

with Image.open("imagen.jpg") as image:
    array = np.array(image)

print(array.shape)
```

Convertir arreglo a imagen:

```python
from PIL import Image
import numpy as np

array = np.zeros((300, 400, 3), dtype=np.uint8)
array[:, :] = [255, 255, 255]

image = Image.fromarray(array)

image.save("desde_numpy.png")
```

## Procesar una carpeta de imágenes

```python
from pathlib import Path
from PIL import Image


def create_thumbnails(input_dir, output_dir, size=(300, 300)):
    output_dir.mkdir(parents=True, exist_ok=True)

    for image_path in input_dir.glob("*.jpg"):
        with Image.open(image_path) as image:
            thumbnail = image.copy()
            thumbnail.thumbnail(size)

            output_path = output_dir / f"{image_path.stem}_thumb.jpg"
            thumbnail.save(output_path)


create_thumbnails(
    Path("imagenes"),
    Path("miniaturas")
)

print("Miniaturas generadas correctamente")
```

## Convertir imágenes por lote

```python
from pathlib import Path
from PIL import Image


def convert_png_to_jpg(input_dir, output_dir):
    output_dir.mkdir(parents=True, exist_ok=True)

    for image_path in input_dir.glob("*.png"):
        with Image.open(image_path) as image:
            rgb_image = image.convert("RGB")
            output_path = output_dir / f"{image_path.stem}.jpg"
            rgb_image.save(output_path, quality=90)


convert_png_to_jpg(
    Path("png"),
    Path("jpg")
)

print("Conversión finalizada")
```

## Calidad al guardar JPEG

Al guardar JPEG puede indicarse calidad.

```python
image.save("salida.jpg", quality=90)
```

Valores más altos suelen conservar más detalle, pero generan archivos más pesados.

## Optimización

```python
image.save(
    "salida.jpg",
    quality=85,
    optimize=True
)
```

## Guardar PNG optimizado

```python
image.save(
    "salida.png",
    optimize=True
)
```

## Metadatos básicos

Algunas imágenes pueden contener metadatos.

```python
from PIL import Image

with Image.open("imagen.jpg") as image:
    print(image.info)
```

Los metadatos disponibles dependen del formato y del archivo.

## EXIF

Algunas imágenes JPEG pueden contener información EXIF.

```python
from PIL import Image

with Image.open("imagen.jpg") as image:
    exif = image.getexif()
    print(exif)
```

La interpretación detallada de EXIF puede requerir procesamiento adicional.

## Manejo de orientación EXIF

Algunas fotos tienen orientación guardada en metadatos. Puede usarse `ImageOps.exif_transpose()` para aplicar esa orientación.

```python
from PIL import Image, ImageOps

with Image.open("foto.jpg") as image:
    corrected = ImageOps.exif_transpose(image)
    corrected.save("foto_corregida.jpg")
```

## Uso con Word y PowerPoint

Pillow puede preparar imágenes antes de insertarlas en documentos o presentaciones.

Flujo típico:

```text
pillow -> imagen procesada -> python-docx / python-pptx
```

Ejemplo:

```python
from PIL import Image
from docx import Document
from docx.shared import Inches

with Image.open("grafico.png") as image:
    image.thumbnail((1000, 1000))
    image.save("grafico_reducido.png")

document = Document()
document.add_picture("grafico_reducido.png", width=Inches(5))
document.save("reporte.docx")
```

## Uso con Excel

Las imágenes preparadas con Pillow pueden insertarse luego con `openpyxl` o `xlsxwriter`.

Ejemplo conceptual:

```text
pillow -> logo redimensionado -> openpyxl -> Excel
```

## Uso con matplotlib

`matplotlib` puede generar imágenes que luego Pillow puede ajustar.

```python
from pathlib import Path
from PIL import Image

image_path = Path("grafico.png")

with Image.open(image_path) as image:
    image.thumbnail((800, 600))
    image.save("grafico_reducido.png")
```

## Uso con PDFs

Pillow puede manipular imágenes extraídas o preparadas para documentos, pero no reemplaza a una librería de PDF.

Para PDFs corresponde usar herramientas como:

```text
pypdf
```

Para renderizar páginas PDF como imágenes se requieren herramientas específicas adicionales.

## Casos de uso frecuentes

## Redimensionar una imagen

```python
with Image.open("imagen.jpg") as image:
    resized = image.resize((800, 600))
    resized.save("salida.jpg")
```

## Crear miniatura

```python
with Image.open("imagen.jpg") as image:
    image.thumbnail((300, 300))
    image.save("miniatura.jpg")
```

## Convertir PNG a JPEG

```python
with Image.open("imagen.png") as image:
    image.convert("RGB").save("imagen.jpg")
```

## Recortar centro

```python
cropped = crop_center(image, 500, 500)
```

## Agregar texto

```python
draw = ImageDraw.Draw(image)
draw.text((50, 50), "Texto", fill="black")
```

## Procesar carpeta completa

```python
for image_path in input_dir.glob("*.jpg"):
    ...
```

## Errores comunes

## Instalar `PIL` en lugar de `pillow`

Problemático:

```bash
python -m pip install PIL
```

Lo correcto es:

```bash
python -m pip install pillow
```

La importación se hace desde `PIL`:

```python
from PIL import Image
```

## Guardar RGBA como JPEG

Problemático:

```python
image.save("salida.jpg")
```

si la imagen está en modo `RGBA`.

JPEG no maneja transparencia. Conviene convertir a `RGB`.

```python
image.convert("RGB").save("salida.jpg")
```

## No cerrar archivos

Menos recomendable:

```python
image = Image.open("imagen.jpg")
```

Más seguro:

```python
with Image.open("imagen.jpg") as image:
    ...
```

## Sobrescribir archivos originales

Problemático:

```python
image.save("imagen.jpg")
```

si se quiere conservar la imagen original.

Más seguro:

```python
image.save("imagen_procesada.jpg")
```

## Deformar imágenes al usar `resize()`

```python
image.resize((800, 800))
```

puede deformar una imagen rectangular si se fuerza un tamaño cuadrado.

Para conservar proporción, usar cálculo proporcional, `thumbnail()` o `ImageOps.fit()` según el caso.

## Confundir `resize()` con `thumbnail()`

```text
resize()    -> crea una nueva imagen con tamaño exacto
thumbnail() -> modifica el objeto y conserva proporción
```

## Usar operaciones píxel por píxel para grandes imágenes

Modificar píxeles individualmente puede ser lento.

Para procesamiento numérico masivo, suele ser mejor convertir a NumPy.

## Depender de fuentes no disponibles

```python
ImageFont.truetype("arial.ttf", 32)
```

puede fallar si la fuente no existe en el entorno.

Conviene validar la ruta o usar una fuente incluida en el proyecto cuando sea apropiado.

## Buenas prácticas

## Usar `with Image.open()`

```python
with Image.open("imagen.jpg") as image:
    ...
```

## Guardar salidas con nombres distintos

```python
imagen_procesada.jpg
```

## Convertir modo antes de guardar

```python
image.convert("RGB").save("salida.jpg")
```

## Conservar proporción al redimensionar

```python
image.thumbnail((300, 300))
```

## Usar `pathlib` para rutas

```python
from pathlib import Path
```

## Separar funciones por tarea

```python
resize_image()
create_thumbnail()
convert_image()
crop_center()
```

## Usar NumPy para operaciones numéricas masivas

```python
array = np.array(image)
```

## Aplicar orientación EXIF cuando se procesan fotos

```python
image = ImageOps.exif_transpose(image)
```

## Ejemplo integrado

```python
from pathlib import Path

from PIL import Image, ImageDraw, ImageFont, ImageOps


def prepare_image(input_path, output_path, max_size=(1200, 1200)):
    with Image.open(input_path) as image:
        image = ImageOps.exif_transpose(image)
        image = image.convert("RGB")

        image.thumbnail(max_size)

        image.save(
            output_path,
            quality=90,
            optimize=True
        )


def add_text_label(input_path, output_path, label):
    with Image.open(input_path) as image:
        image = image.convert("RGB")

        draw = ImageDraw.Draw(image)

        draw.rectangle(
            (20, 20, 420, 80),
            fill="white",
            outline="black"
        )

        draw.text(
            (40, 40),
            label,
            fill="black"
        )

        image.save(output_path, quality=90)


def process_folder(input_dir, output_dir):
    output_dir.mkdir(parents=True, exist_ok=True)

    for input_path in input_dir.glob("*.jpg"):
        prepared_path = output_dir / f"{input_path.stem}_prepared.jpg"
        labeled_path = output_dir / f"{input_path.stem}_labeled.jpg"

        prepare_image(input_path, prepared_path)

        add_text_label(
            prepared_path,
            labeled_path,
            label=input_path.stem
        )


process_folder(
    Path("imagenes"),
    Path("imagenes_procesadas")
)

print("Imágenes procesadas correctamente")
```

## Relación con otras librerías

`pillow` se relaciona especialmente con:

- `pathlib`, para manejar rutas de imágenes
- `numpy`, para convertir imágenes a arreglos y hacer operaciones numéricas
- `matplotlib`, para generar o guardar gráficos como imágenes
- `openpyxl`, para insertar imágenes procesadas en Excel
- `xlsxwriter`, para insertar imágenes en reportes Excel nuevos
- `python-docx`, para insertar imágenes en documentos Word
- `python-pptx`, para insertar imágenes en presentaciones
- `pypdf`, cuando se trabaja en flujos documentales que también incluyen PDFs
- `opencv-python`, cuando se requiere visión por computadora más avanzada

## Orden didáctico interno

```text
1. Propósito de pillow
2. Instalación e importación
3. Objeto Image
4. Abrir y guardar imágenes
5. Propiedades: size, mode y format
6. Conversión de formatos y modos
7. Redimensionamiento y miniaturas
8. Recorte, rotación y volteo
9. Transparencia y composición
10. Dibujo, texto y formas
11. Filtros y ajustes
12. Procesamiento por lotes
13. Uso con NumPy y documentos
14. Errores comunes
15. Buenas prácticas
```