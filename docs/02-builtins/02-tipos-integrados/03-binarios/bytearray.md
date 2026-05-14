# `bytearray`

## Propósito

`bytearray` representa una secuencia mutable de bytes. Se utiliza para trabajar con datos binarios cuando además se necesita modificar su contenido después de la creación.

## Naturaleza del tipo

`bytearray` es un tipo secuencial, binario y mutable.

Esto significa que:

- representa una secuencia ordenada de valores enteros entre `0` y `255`
- permite indexación y slicing
- puede modificarse después de crearse
- resulta apropiado cuando se necesita editar contenido binario en memoria

## Forma general

Un objeto `bytearray` puede construirse explícitamente con:

```python id="r6m1tv"
bytearray(...)
````

A diferencia de `bytes`, no existe una sintaxis literal específica como `ba"..."`.

## Construcción básica

## Desde bytes

```python id="n2m5qw"
datos = bytearray(b"abc")
print(datos)
print(type(datos))
```

Salida:

```text id="p8m4pk"
bytearray(b'abc')
<class 'bytearray'>
```

## Desde iterable de enteros

```python id="v1m7qw"
datos = bytearray([65, 66, 67])
print(datos)
```

Salida:

```text id="r4m2tv"
bytearray(b'ABC')
```

Cada entero debe estar entre `0` y `255`.

## Desde entero

Si se pasa un entero `n`, se crea una secuencia de `n` bytes nulos.

```python id="m7q1pk"
datos = bytearray(5)

print(datos)
print(len(datos))
```

Salida:

```text id="t1m6qw"
bytearray(b'\x00\x00\x00\x00\x00')
5
```

## Desde texto con codificación

```python id="n4m8tv"
datos = bytearray("Hola", encoding="utf-8")
print(datos)
```

Salida:

```text id="p9m3pk"
bytearray(b'Hola')
```

## Vacío

```python id="v6m1qw"
datos = bytearray()
print(datos)
```

Salida:

```text id="r8m5tv"
bytearray(b'')
```

## Valor de retorno de `type()`

```python id="m2q9pk"
print(type(bytearray(b"abc")))
```

Salida:

```text id="t5m2qw"
<class 'bytearray'>
```

## Mutabilidad

`bytearray` es mutable.

```python id="n9m6tv"
datos = bytearray(b"abc")
print(id(datos))

datos[0] = 65
print(datos)
print(id(datos))
```

Salida:

```text id="p2m8pk"
bytearray(b'Abc')
```

La identidad del objeto no cambia. Se modifica el mismo objeto.

## Longitud

La longitud se obtiene con `len()`.

```python id="v7m1qw"
print(len(bytearray(b"abc")))
print(len(bytearray()))
```

Salida:

```text id="r4m9tv"
3
0
```

## Indexación

Al indexar un `bytearray`, el resultado es un entero entre `0` y `255`.

```python id="m6q3pk"
datos = bytearray(b"ABC")

print(datos[0])
print(datos[1])
print(datos[-1])
```

Salida:

```text id="t8m5qw"
65
66
67
```

## Slicing

El slicing devuelve otro objeto `bytearray`.

```python id="n1m9tv"
datos = bytearray(b"Python")

print(datos[0:3])
print(datos[2:])
print(datos[::-1])
```

Salida:

```text id="p4m2pk"
bytearray(b'Pyt')
bytearray(b'thon')
bytearray(b'nohtyP')
```

## Diferencia entre indexación y slicing

```python id="v9m7qw"
datos = bytearray(b"ABC")

print(datos[0])
print(datos[0:1])

print(type(datos[0]))
print(type(datos[0:1]))
```

Salida:

```text id="r2m4tv"
65
bytearray(b'A')
<class 'int'>
<class 'bytearray'>
```

## Modificación por índice

Un `bytearray` permite reasignar bytes individuales.

```python id="m7q1pk"
datos = bytearray(b"ABC")
datos[0] = 90

print(datos)
```

Salida:

```text id="t1m6qw"
bytearray(b'ZBC')
```

`90` corresponde al byte de la letra `Z`.

## Modificación por slicing

También puede reemplazarse un segmento.

```python id="n4m8tv"
datos = bytearray(b"abcdef")
datos[1:4] = b"XYZ"

print(datos)
```

Salida:

```text id="p9m3pk"
bytearray(b'aXYZef')
```

## Concatenación

Los objetos `bytearray` pueden unirse con `+`.

```python id="v6m1qw"
a = bytearray(b"AB")
b = bytearray(b"CD")

print(a + b)
```

Salida:

```text id="r8m5tv"
bytearray(b'ABCD')
```

## Repetición

También pueden repetirse con `*`.

```python id="m2q9pk"
print(bytearray(b"AB") * 3)
```

Salida:

```text id="t5m2qw"
bytearray(b'ABABAB')
```

## Pertenencia

Se puede verificar si un byte o sub-secuencia está presente con `in`.

```python id="n9m6tv"
datos = bytearray(b"Python")

print(80 in datos)
print(bytearray(b"Py") in datos)
print(b"Py" in datos)
```

Salida:

```text id="p2m8pk"
True
True
True
```

## Iteración

Un `bytearray` es iterable y entrega enteros.

```python id="v7m1qw"
datos = bytearray(b"ABC")

for valor in datos:
    print(valor)
```

Salida:

```text id="r4m9tv"
65
66
67
```

## Relación con `bytes`

`bytes` y `bytearray` representan secuencias binarias, pero difieren en mutabilidad.

## `bytes`

* inmutable
* adecuado cuando no se necesita modificar el contenido

## `bytearray`

* mutable
* adecuado cuando el contenido binario debe cambiar

Ejemplo comparativo:

```python id="m6q3pk"
datos_bytes = b"ABC"
datos_bytearray = bytearray(b"ABC")

print(type(datos_bytes))
print(type(datos_bytearray))
```

Salida:

```text id="t8m5qw"
<class 'bytes'>
<class 'bytearray'>
```

## Conversión entre `bytes` y `bytearray`

## De `bytes` a `bytearray`

```python id="n1m9tv"
datos = b"ABC"
mutable = bytearray(datos)

print(mutable)
```

## De `bytearray` a `bytes`

```python id="p4m2pk"
mutable = bytearray(b"ABC")
inmutable = bytes(mutable)

print(inmutable)
```

Salida:

```text id="v9m7qw"
b'ABC'
```

## Relación entre `str`, `bytes` y `bytearray`

* `str` representa texto
* `bytes` representa binario inmutable
* `bytearray` representa binario mutable

Para pasar de texto a `bytearray`, puede hacerse:

```python id="r2m4tv"
texto = "Hola"
datos = bytearray(texto, encoding="utf-8")

print(datos)
```

Salida:

```text id="m7q1pk"
bytearray(b'Hola')
```

Para volver a texto:

```python id="t1m6qw"
datos = bytearray(b"Hola")
texto = datos.decode("utf-8")

print(texto)
```

Salida:

```text id="n4m8tv"
Hola
```

## Métodos principales

`bytearray` tiene varios métodos útiles, muchos de ellos similares a los de `bytes`.

Ejemplos:

```python id="p9m3pk"
datos = bytearray(b"python")

print(datos.upper())
print(datos.replace(b"py", b"ty"))
print(datos.startswith(b"py"))
```

Salida:

```text id="v6m1qw"
bytearray(b'PYTHON')
bytearray(b'tython')
True
```

Además, por ser mutable, admite métodos como `append()`, `extend()`, `insert()`, `pop()` y otros análogos a secuencias mutables.

```python id="r8m5tv"
datos = bytearray(b"ABC")

datos.append(68)
print(datos)

datos.extend(b"EF")
print(datos)
```

Salida:

```text id="m2q9pk"
bytearray(b'ABCD')
bytearray(b'ABCDEF')
```

Los métodos se documentan aparte en la sección correspondiente de métodos de tipos.

## Valores permitidos

Cada elemento individual debe estar en el rango:

```text id="t5m2qw"
0 a 255
```

Ejemplo válido:

```python id="n9m6tv"
datos = bytearray([0, 65, 255])
print(datos)
```

Ejemplo inválido:

```python id="p2m8pk"
datos = bytearray([300])
```

Esto genera `ValueError`.

## Truthiness

En contexto booleano:

* `bytearray(b"")` se evalúa como `False`
* cualquier secuencia no vacía se evalúa como `True`

```python id="v7m1qw"
print(bool(bytearray()))
print(bool(bytearray(b"A")))
print(bool(bytearray(b"\x00")))
```

Salida:

```text id="r4m9tv"
False
True
True
```

Aunque el byte sea `0`, la secuencia no está vacía.

## Comparación

Los objetos `bytearray` pueden compararse lexicográficamente.

```python id="m6q3pk"
print(bytearray(b"ABC") == bytearray(b"ABC"))
print(bytearray(b"ABC") < bytearray(b"ABD"))
print(bytearray(b"b") > bytearray(b"a"))
```

Salida:

```text id="t8m5qw"
True
True
True
```

## Casos de uso frecuentes

## Modificar contenido binario en memoria

```python id="n1m9tv"
datos = bytearray(b"ABC")
datos[1] = 90

print(datos)
```

## Construir buffers binarios

```python id="p4m2pk"
buffer = bytearray()
buffer.extend(b"HEAD")
buffer.extend(b"DATA")

print(buffer)
```

## Preparar datos para escritura binaria

```python id="v9m7qw"
datos = bytearray(b"ABC")
datos.append(68)

with open("archivo.bin", "wb") as archivo:
    archivo.write(datos)
```

## Errores comunes

## Creer que existe literal propio para `bytearray`

Problemático:

```python id="r2m4tv"
datos = ba"ABC"
```

Eso no existe en Python. Debe usarse `bytearray(...)`.

## Usar valores fuera de rango

Problemático:

```python id="m7q1pk"
bytearray([300])
```

Esto genera `ValueError`.

## Confundir `bytearray` con `bytes`

Problemático:

```python id="t1m6qw"
datos = bytearray(b"ABC")
print(type(datos) == bytes)
```

Salida:

```text id="n4m8tv"
False
```

Son tipos distintos.

## Decodificar con codificación incorrecta

Problemático:

```python id="p9m3pk"
datos = bytearray([255])
datos.decode("utf-8")
```

Esto puede generar `UnicodeDecodeError`.

## Suponer que la iteración devuelve bytes de longitud 1

Problemático:

```python id="v6m1qw"
datos = bytearray(b"ABC")

for valor in datos:
    print(type(valor))
```

Salida:

```text id="r8m5tv"
<class 'int'>
<class 'int'>
<class 'int'>
```

## Buenas prácticas

## Usar `bytearray` cuando se necesite mutabilidad binaria

```python id="m2q9pk"
datos = bytearray(b"ABC")
datos[0] = 90
```

## Usar `bytes` cuando no se necesite modificación

Para contenido binario fijo, suele ser más apropiado `bytes`.

## Distinguir claramente entre texto y binario mutable

* `str` para texto
* `bytes` para binario inmutable
* `bytearray` para binario mutable

## Validar o controlar la codificación al convertir a texto

```python id="t5m2qw"
texto = datos.decode("utf-8")
```

## Ejemplo integrado

```python id="n9m6tv"
def show_mutable_binary_summary(text):
    data = bytearray(text, encoding="utf-8")

    print("Resumen binario mutable")
    print("-" * 30)
    print("Original:", data)
    print("Longitud:", len(data))

    if data:
        data[0] = ord("H")

    data.extend(b"!")
    print("Modificado:", data)
    print("Reconstruido:", data.decode("utf-8"))


show_mutable_binary_summary("hola")
```

Salida aproximada:

```text id="p2m8pk"
Resumen binario mutable
------------------------------
Original: bytearray(b'hola')
Longitud: 4
Modificado: bytearray(b'Hola!')
Reconstruido: Hola!
```

## Relación con otros elementos integrados

`bytearray` se relaciona especialmente con:

* `bytes`, como variante binaria inmutable
* `str`, por la conversión entre texto y binario
* `len()` para contar bytes
* `open()` en modo binario
* métodos de `bytearray`, documentados en la sección correspondiente

## Orden didáctico interno

```text id="v7m1qw"
1. Propósito de bytearray
2. Naturaleza del tipo
3. Construcción con bytearray()
4. Longitud, indexación y slicing
5. Mutabilidad y modificación
6. Relación con bytes y str
7. Codificación y decodificación
8. Errores comunes
9. Buenas prácticas
```