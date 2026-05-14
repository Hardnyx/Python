# Métodos de `str`

## Propósito

Los métodos de `str` permiten buscar, validar, transformar, formatear, dividir y recomponer texto. Constituyen una parte central del trabajo con datos textuales en Python, tanto en tareas simples de limpieza como en procesamiento más estructurado de cadenas.

## Alcance

Este bloque reúne los métodos públicos más importantes del tipo `str`. El enfoque principal recae en:

- transformación de texto
- validación de contenido
- búsqueda y conteo
- separación y unión
- limpieza y normalización
- alineación y relleno
- reemplazo y traducción
- formato y codificación

Los métodos especiales como `__add__`, `__len__`, `__contains__` o `__iter__` pertenecen al comportamiento general del tipo y se tratan mejor en el bloque de métodos especiales o en la documentación estructural del tipo `str`.

## Idea general

Una cadena es un objeto inmutable. Por eso, los métodos de `str` no modifican la cadena original, sino que devuelven una nueva cadena o un valor derivado.

Ejemplo:

```python
texto = "python"

resultado = texto.upper()

print(texto)
print(resultado)
````

Salida:

```text
python
PYTHON
```

## Criterios de organización

Los métodos de `str` conviene estudiarlos por familias funcionales.

## 1. Cambio de capitalización

Métodos orientados a modificar mayúsculas y minúsculas:

```python
capitalize
casefold
lower
swapcase
title
upper
```

## 2. Búsqueda, posición y conteo

Métodos orientados a localizar o contar fragmentos dentro de una cadena:

```python
count
find
index
rfind
rindex
startswith
endswith
```

## 3. Validación del contenido

Métodos que responden preguntas sobre la composición de la cadena:

```python
isalnum
isalpha
isascii
isdecimal
isdigit
isidentifier
islower
isnumeric
isprintable
isspace
istitle
isupper
```

## 4. Separación y partición

Métodos que dividen una cadena en partes:

```python
partition
rpartition
split
rsplit
splitlines
```

## 5. Unión y recomposición

Métodos usados para combinar texto o reconstruir cadenas:

```python
join
```

## 6. Limpieza y recorte

Métodos orientados a quitar espacios o caracteres en los extremos:

```python
strip
lstrip
rstrip
removeprefix
removesuffix
```

## 7. Reemplazo y traducción

Métodos para sustituir contenido o transformar caracteres según reglas:

```python
replace
translate
maketrans
```

## 8. Alineación y relleno

Métodos para ajustar ancho, justificar y rellenar texto:

```python
center
ljust
rjust
zfill
```

## 9. Expansión y ajuste de caracteres especiales

Métodos que transforman ciertas marcas de control o tabulación:

```python
expandtabs
```

## 10. Formato de cadenas

Métodos para interpolar o estructurar contenido textual:

```python
format
format_map
```

## 11. Codificación

Métodos para convertir texto a bytes mediante una codificación:

```python
encode
```

## Lista completa de métodos públicos de `str`

```python
capitalize
casefold
center
count
encode
endswith
expandtabs
find
format
format_map
index
isalnum
isalpha
isascii
isdecimal
isdigit
isidentifier
islower
isnumeric
isprintable
isspace
istitle
isupper
join
ljust
lower
lstrip
maketrans
partition
removeprefix
removesuffix
replace
rfind
rindex
rjust
rpartition
rsplit
rstrip
split
splitlines
startswith
strip
swapcase
title
translate
upper
zfill
```

## Orden de estudio recomendado

Para un aprendizaje más natural, conviene seguir este orden:

1. `lower()`, `upper()`, `capitalize()`, `title()`
2. `strip()`, `lstrip()`, `rstrip()`
3. `replace()`
4. `split()`, `rsplit()`, `splitlines()`
5. `join()`
6. `find()`, `index()`, `count()`
7. `startswith()`, `endswith()`
8. Métodos `is...`
9. `partition()`, `rpartition()`
10. `center()`, `ljust()`, `rjust()`, `zfill()`
11. `removeprefix()`, `removesuffix()`
12. `translate()`, `maketrans()`
13. `format()`, `format_map()`
14. `encode()`
15. `casefold()`, `expandtabs()`

## Relaciones importantes

Al estudiar estos métodos conviene tener presentes varias diferencias frecuentes:

### `find()` vs `index()`

Ambos buscan una subcadena, pero `find()` devuelve `-1` si no encuentra coincidencia, mientras que `index()` lanza error.

### `split()` vs `partition()`

`split()` divide en múltiples partes. `partition()` separa en exactamente tres componentes: izquierda, separador y derecha.

### `strip()` vs `replace()`

`strip()` trabaja solo en los extremos. `replace()` puede sustituir contenido interno.

### `lower()` vs `casefold()`

`casefold()` realiza una normalización más agresiva y suele ser más adecuada para comparaciones de texto insensibles a mayúsculas y minúsculas.

### `startswith()` y `endswith()`

Resultan más expresivos que comparar manualmente slices cuando solo interesa prefijo o sufijo.

## Resultado esperado del bloque

Al finalizar este bloque debería quedar claro:

* qué operaciones textuales resuelve cada familia de métodos
* qué métodos devuelven cadenas y cuáles devuelven valores booleanos o listas
* qué diferencias existen entre métodos parecidos
* cómo trabajar con texto de forma más idiomática y legible