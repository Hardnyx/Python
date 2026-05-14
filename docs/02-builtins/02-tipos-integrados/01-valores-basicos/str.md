# `str`

## Propósito

`str` representa texto en Python. Se usa para nombres, mensajes, rutas, contenido de archivos, identificadores, fechas en formato textual, datos serializados y, en general, cualquier secuencia de caracteres Unicode.

## Naturaleza del tipo

`str` es un tipo secuencial, textual e inmutable.

Esto significa que:

- representa una secuencia ordenada de caracteres
- permite indexación y slicing
- no puede modificarse internamente después de crearse
- cualquier transformación produce una nueva cadena

## Forma general

Una cadena puede escribirse como literal:

```python
"Hola"
'Python'
"""Texto"""
````

o construirse explícitamente con:

```python id="m7tk5c"
str(valor)
```

## Literales de cadena

Las formas más comunes de escribir texto son con comillas simples o dobles.

```python id="xr6b1h"
a = "Hola"
b = 'Python'

print(a)
print(b)
```

Salida:

```text id="c9n4pz"
Hola
Python
```

## Comillas triples

Permiten escribir texto multilínea.

```python id="o2q7wd"
mensaje = """Línea 1
Línea 2
Línea 3"""

print(mensaje)
```

Salida:

```text id="e5w8uj"
Línea 1
Línea 2
Línea 3
```

## Cadena vacía

```python id="s4m1ky"
texto = ""
print(texto)
print(len(texto))
```

Salida:

```text id="u7p3xn"
0
```

## Valor de retorno de `type()`

```python id="k1v8qe"
print(type("Hola"))
```

Salida:

```text id="z4r2mc"
<class 'str'>
```

## Mutabilidad

`str` es inmutable.

```python id="r6q3th"
texto = "hola"
print(id(texto))

texto = texto.upper()
print(texto)
print(id(texto))
```

Salida:

```text id="g8m1pc"
HOLA
```

La identidad cambia porque se crea una nueva cadena. La original no se modifica.

## Longitud

La longitud de una cadena se obtiene con `len()`.

```python id="j2p9wa"
print(len("Python"))
print(len(""))
print(len("Hola mundo"))
```

Salida:

```text id="q7v4nk"
6
0
10
```

## Indexación

Una cadena permite acceder a caracteres por posición.

```python id="y5m8cx"
texto = "Python"

print(texto[0])
print(texto[1])
print(texto[-1])
```

Salida:

```text id="n3q6vt"
P
y
n
```

## Índices negativos

Los índices negativos cuentan desde el final.

```python id="h1w7rb"
texto = "Python"

print(texto[-1])
print(texto[-2])
```

Salida:

```text id="k4p2mf"
n
o
```

## Error por índice fuera de rango

```python id="t9r4xz"
texto = "Hola"
print(texto[10])
```

Error típico:

```text id="f6m1qv"
IndexError: string index out of range
```

## Slicing

El slicing permite extraer subcadenas.

Forma general:

```python
texto[inicio:fin:paso]
```

## Segmento simple

```python id="w3p8nk"
texto = "Python"

print(texto[0:3])
print(texto[2:6])
```

Salida:

```text id="v1q5mc"
Pyt
thon
```

## Omitiendo límites

```python id="x6m2pr"
texto = "Python"

print(texto[:3])
print(texto[3:])
print(texto[:])
```

Salida:

```text id="r8q1tv"
Pyt
hon
Python
```

## Paso en slicing

```python id="b4p9xe"
texto = "Python"

print(texto[::2])
print(texto[::-1])
```

Salida:

```text id="m2v7rk"
Pto
nohtyP
```

## Concatenación

Las cadenas se pueden unir con `+`.

```python id="n7m3qw"
nombre = "Ana"
apellido = "Pérez"

print(nombre + " " + apellido)
```

Salida:

```text id="u4r8pc"
Ana Pérez
```

## Repetición

Las cadenas pueden repetirse con `*`.

```python id="p1v6mt"
print("Ha" * 3)
print("-" * 10)
```

Salida:

```text id="y9q2kw"
HaHaHa
----------
```

## Pertenencia

Se puede verificar si una subcadena está presente con `in`.

```python id="g5r1xn"
texto = "Python"

print("Py" in texto)
print("Java" in texto)
```

Salida:

```text id="c3m8pv"
True
False
```

## Comparación

Las cadenas pueden compararse lexicográficamente.

```python id="q8v4rb"
print("Ana" == "Ana")
print("Ana" < "Luis")
print("b" > "a")
```

Salida:

```text id="s1p7mk"
True
True
True
```

La comparación se basa en el orden de los caracteres.

## Truthiness

En contexto booleano:

* la cadena vacía se evalúa como `False`
* cualquier cadena no vacía se evalúa como `True`

```python id="t4q9wn"
print(bool(""))
print(bool("Hola"))
print(bool(" "))
```

Salida:

```text id="m7r2px"
False
True
True
```

Una cadena con un espacio sigue siendo no vacía.

## Conversión con `str()`

La función `str()` convierte valores a representación textual.

## Desde entero

```python id="v2m6qc"
print(str(10))
```

Salida:

```text id="k8p1rv"
10
```

## Desde flotante

```python id="f5q3nx"
print(str(3.14))
```

Salida:

```text id="r4m9tk"
3.14
```

## Desde booleano

```python id="j7p2wc"
print(str(True))
print(str(False))
```

Salida:

```text id="n1v8qm"
True
False
```

## Desde colecciones

```python id="c6m4py"
print(str([1, 2, 3]))
print(str({"a": 1}))
```

Salida:

```text id="u3q7wd"
[1, 2, 3]
{'a': 1}
```

## Secuencias de escape

Las cadenas admiten secuencias especiales.

| Secuencia | Significado     |
| --------- | --------------- |
| `\n`      | salto de línea  |
| `\t`      | tabulación      |
| `\\`      | barra invertida |
| `\"`      | comilla doble   |
| `\'`      | comilla simple  |

Ejemplo:

```python id="m1p8qk"
print("Línea 1\nLínea 2")
print("A\tB\tC")
print("Ruta: C:\\Users\\Nombre")
```

Salida:

```text id="x4v2rm"
Línea 1
Línea 2
A	B	C
Ruta: C:\Users\Nombre
```

## Raw strings

Las raw strings reducen el efecto de ciertas secuencias de escape.

```python id="r9m3tw"
ruta = r"C:\Users\Nombre\Documentos"
print(ruta)
```

Salida:

```text id="h2q7pc"
C:\Users\Nombre\Documentos
```

Son útiles en rutas y expresiones regulares.

## Formateo de cadenas

## Concatenación manual

```python id="y6p1nx"
nombre = "Ana"
edad = 20

print("Nombre: " + nombre + ", Edad: " + str(edad))
```

## f-strings

Es la forma más clara y moderna en la mayoría de casos.

```python id="p4q8mv"
nombre = "Ana"
edad = 20

print(f"Nombre: {nombre}, Edad: {edad}")
```

Salida:

```text id="w7m2rk"
Nombre: Ana, Edad: 20
```

## `format()`

```python id="g3v9qc"
nombre = "Ana"
edad = 20

print("Nombre: {}, Edad: {}".format(nombre, edad))
```

Salida:

```text id="t1p6mx"
Nombre: Ana, Edad: 20
```

## Iteración sobre cadenas

Una cadena es iterable.

```python id="n8q4pw"
texto = "Hola"

for caracter in texto:
    print(caracter)
```

Salida:

```text id="q5m1rv"
H
o
l
a
```

## Desempaquetado

```python id="m6v3qx"
a, b, c = "Sol"

print(a)
print(b)
print(c)
```

Salida:

```text id="r2p9wk"
S
o
l
```

## Métodos de cadena

`str` tiene muchos métodos propios, documentados aparte en la sección de métodos de tipos.

Ejemplos:

```python id="v9q1tm"
texto = "python"

print(texto.upper())
print(texto.capitalize())
print(texto.replace("py", "ty"))
```

Salida:

```text id="k4m8pr"
PYTHON
Python
tython
```

## Casos de uso frecuentes

## Nombres y etiquetas

```python id="p2v7wn"
nombre = "Ana"
print(nombre)
```

## Mensajes

```python id="c8q3mx"
mensaje = "Proceso completado"
print(mensaje)
```

## Rutas y extensiones

```python id="w1m9rk"
archivo = "reporte.xlsx"
print(archivo.endswith(".xlsx"))
```

## Validaciones simples

```python id="g7q2pv"
correo = "usuario@dominio.com"
print("@" in correo)
```

## Errores comunes

## Intentar modificar un carácter directamente

Problemático:

```python id="y4p8mc"
texto = "hola"
texto[0] = "H"
```

Esto genera `TypeError` porque las cadenas son inmutables.

Si se necesita una variante, debe construirse una nueva cadena:

```python id="n3q6tw"
texto = "hola"
texto = "H" + texto[1:]
print(texto)
```

## Confundir índice con slicing

```python id="u8m1qx"
texto = "Python"

print(texto[0])
print(texto[0:1])
```

Salida:

```text id="r5p9mv"
P
P
```

Pero no son exactamente lo mismo:

* `texto[0]` devuelve un carácter de tipo `str`
* `texto[0:1]` devuelve una subcadena de longitud 1

## Olvidar convertir tipos al concatenar

Problemático:

```python id="t2q7wn"
edad = 20
print("Edad: " + edad)
```

Esto genera `TypeError`.

Forma correcta:

```python id="m9v4rk"
edad = 20
print("Edad: " + str(edad))
```

o mejor:

```python id="x6p2mq"
edad = 20
print(f"Edad: {edad}")
```

## Suponer que una cadena con espacios está vacía

```python id="f1q8pv"
texto = " "
print(bool(texto))
```

Salida:

```text id="k7m3rw"
True
```

La cadena contiene un carácter, aunque sea un espacio.

## Buenas prácticas

## Usar f-strings para construir mensajes

```python id="v4p6mx"
nombre = "Ana"
print(f"Hola, {nombre}")
```

## Usar slicing para extraer subcadenas de forma clara

```python id="q9m2rv"
codigo = "ABC123"
prefijo = codigo[:3]
```

## No intentar modificar cadenas directamente

Cuando se necesite cambiar el contenido, debe generarse una nueva cadena.

## Usar métodos de cadena en lugar de lógica manual cuando exista una operación apropiada

Por ejemplo, suele ser mejor usar `upper()`, `lower()`, `strip()` o `replace()` que construir esas transformaciones manualmente.

## Tener cuidado con comparaciones sensibles a mayúsculas y minúsculas

```python id="h6q1pw"
print("Ana" == "ana")
```

Salida:

```text id="m3v8rk"
False
```

Si se quiere ignorar diferencias de capitalización, conviene normalizar antes.

## Ejemplo integrado

```python id="w8p4mq"
def show_user_summary(name, city, age):
    full_message = f"Nombre: {name}\nCiudad: {city}\nEdad: {age}"
    separator = "-" * 30

    print("Resumen de usuario")
    print(separator)
    print(full_message)
    print(separator)
    print("Inicial del nombre:", name[0])
    print("Nombre en mayúsculas:", name.upper())
    print("Ciudad contiene 'i':", "i" in city.lower())


show_user_summary("Ana", "Lima", 20)
```

Salida aproximada:

```text id="n1q7tv"
Resumen de usuario
------------------------------
Nombre: Ana
Ciudad: Lima
Edad: 20
------------------------------
Inicial del nombre: A
Nombre en mayúsculas: ANA
Ciudad contiene 'i': True
```

## Relación con otros elementos integrados

`str` se relaciona especialmente con:

* `print()` e `input()` en entrada y salida textual
* `len()` para longitud de texto
* `type()` e `isinstance()` para inspección
* `list()` y `tuple()` cuando se convierte texto en secuencia de caracteres
* métodos de cadena, documentados en la sección correspondiente de métodos de tipos

## Orden didáctico interno

```text id="j5m9qx"
1. Propósito de str
2. Naturaleza del tipo
3. Literales y construcción con str()
4. Longitud, indexación y slicing
5. Concatenación y repetición
6. Pertenencia y comparación
7. Truthiness
8. Secuencias de escape y raw strings
9. Formateo de cadenas
10. Iteración
11. Errores comunes
12. Buenas prácticas
```