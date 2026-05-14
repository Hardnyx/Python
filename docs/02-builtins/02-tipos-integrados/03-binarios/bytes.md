# `bytes`

## Propósito

`bytes` representa una secuencia inmutable de bytes. Se utiliza para trabajar con datos binarios, contenido codificado, archivos no textuales, protocolos, transmisiones y transformaciones entre texto y representación binaria.

## Naturaleza del tipo

`bytes` es un tipo secuencial, binario e inmutable.

Esto significa que:

- representa una secuencia ordenada de valores enteros entre `0` y `255`
- permite indexación y slicing
- no puede modificarse internamente después de crearse
- cualquier transformación produce un nuevo objeto

## Forma general

Un valor de tipo `bytes` puede escribirse como literal binario:

```python
b"abc"
b"\x41\x42\x43"
````

o construirse explícitamente con:

```python id="b9m2qw"
bytes(...)
```

## Literales de bytes

La forma más habitual es anteponer `b` a una cadena literal.

```python id="m4q7pk"
datos = b"abc"
print(datos)
print(type(datos))
```

Salida:

```text id="v1m5tv"
b'abc'
<class 'bytes'>
```

## Literales con secuencias hexadecimales

También pueden representarse bytes con escapes hexadecimales.

```python id="r8m1qw"
datos = b"\x41\x42\x43"
print(datos)
```

Salida:

```text id="m2q6pk"
b'ABC'
```

## Valor de retorno de `type()`

```python id="t5m9tv"
print(type(b"Hola"))
```

Salida:

```text id="n8m2qw"
<class 'bytes'>
```

## Inmutabilidad

`bytes` es inmutable.

```python id="p3m7pk"
datos = b"abc"
print(id(datos))

datos = datos + b"d"
print(datos)
print(id(datos))
```

Salida:

```text id="v6m4tv"
b'abcd'
```

La identidad cambia porque se crea un nuevo objeto.

## Longitud

La longitud se obtiene con `len()`.

```python id="r9m1qw"
print(len(b"abc"))
print(len(b""))
```

Salida:

```text id="m4q8pk"
3
0
```

## Indexación

Al indexar un objeto `bytes`, el resultado es un entero entre `0` y `255`, no otro objeto `bytes` de longitud 1.

```python id="t7m2tv"
datos = b"ABC"

print(datos[0])
print(datos[1])
print(datos[-1])
```

Salida:

```text id="n1m5qw"
65
66
67
```

Esto ocurre porque cada byte se interpreta como un número entero.

## Slicing

El slicing devuelve otro objeto `bytes`.

```python id="p4m8pk"
datos = b"Python"

print(datos[0:3])
print(datos[2:])
print(datos[::-1])
```

Salida:

```text id="v7m3tv"
b'Pyt'
b'thon'
b'nohtyP'
```

## Diferencia entre indexación y slicing

```python id="r2m6qw"
datos = b"ABC"

print(datos[0])
print(datos[0:1])

print(type(datos[0]))
print(type(datos[0:1]))
```

Salida:

```text id="m9q1pk"
65
b'A'
<class 'int'>
<class 'bytes'>
```

## Concatenación

Los objetos `bytes` pueden unirse con `+`.

```python id="t5m4tv"
a = b"AB"
b = b"CD"

print(a + b)
```

Salida:

```text id="n8m7qw"
b'ABCD'
```

## Repetición

También pueden repetirse con `*`.

```python id="p8m2pk"
print(b"AB" * 3)
```

Salida:

```text id="v1m5qw"
b'ABABAB'
```

## Pertenencia

Se puede verificar si un byte o sub-secuencia está presente con `in`.

```python id="r4m7tv"
datos = b"Python"

print(80 in datos)
print(b"Py" in datos)
print(b"Java" in datos)
```

Salida:

```text id="m7q4pk"
True
True
False
```

`80` corresponde al byte de la letra `P`.

## Iteración

Un objeto `bytes` es iterable, pero entrega enteros.

```python id="t1m8qw"
datos = b"ABC"

for valor in datos:
    print(valor)
```

Salida:

```text id="n4m2tv"
65
66
67
```

## Conversión con `bytes()`

La función `bytes()` permite construir objetos binarios de varias formas.

## Desde un iterable de enteros

```python id="p6m1pk"
datos = bytes([65, 66, 67])
print(datos)
```

Salida:

```text id="v9m8qw"
b'ABC'
```

Cada entero debe estar entre `0` y `255`.

## Desde un entero

Si se pasa un entero `n`, se crea una secuencia de `n` bytes nulos.

```python id="r3m5tv"
datos = bytes(5)
print(datos)
print(len(datos))
```

Salida:

```text id="m6q9pk"
b'\x00\x00\x00\x00\x00'
5
```

## Desde texto con codificación

```python id="t8m3qw"
datos = bytes("Hola", encoding="utf-8")
print(datos)
```

Salida:

```text id="n1q6tv"
b'Hola'
```

## Desde cadena vacía

```python id="p5m9pk"
print(bytes())
```

Salida:

```text id="v2m4qw"
b''
```

## Relación entre `str` y `bytes`

`str` representa texto. `bytes` representa datos binarios.

Para pasar de texto a bytes se usa codificación.

```python id="r7m1tv"
texto = "Hola"
datos = texto.encode("utf-8")

print(texto)
print(datos)
```

Salida:

```text id="m4q7pk"
Hola
b'Hola'
```

Para volver de bytes a texto se usa decodificación.

```python id="t2m6qw"
datos = b"Hola"
texto = datos.decode("utf-8")

print(datos)
print(texto)
```

Salida:

```text id="n8m1tv"
b'Hola'
Hola
```

## Codificación

La codificación transforma texto en bytes.

```python id="p9m4pk"
texto = "á"
datos = texto.encode("utf-8")

print(datos)
```

Salida posible:

```text id="v6m9qw"
b'\xc3\xa1'
```

## Decodificación

La decodificación transforma bytes en texto.

```python id="r1m5tv"
datos = b"\xc3\xa1"
texto = datos.decode("utf-8")

print(texto)
```

Salida:

```text id="m7q2pk"
á
```

## Error por codificación o decodificación incorrecta

Si los bytes no corresponden a la codificación indicada, puede producirse error.

```python id="t4m8qw"
datos = b"\xff"
print(datos.decode("utf-8"))
```

Error típico:

```text id="n2m5tv"
UnicodeDecodeError
```

## Métodos principales

`bytes` tiene varios métodos útiles, muchos de ellos análogos a los de `str`, pero operando en dominio binario.

Ejemplos:

```python id="p7m1pk"
datos = b"python"

print(datos.upper())
print(datos.replace(b"py", b"ty"))
print(datos.startswith(b"py"))
```

Salida:

```text id="v3m6qw"
b'PYTHON'
b'tython'
True
```

Los métodos se documentan aparte en la sección correspondiente de métodos de tipos.

## No modificación por índice

No se puede reasignar un byte individual.

```python id="r8m4tv"
datos = b"ABC"
datos[0] = 90
```

Error típico:

```text id="m1q7pk"
TypeError: 'bytes' object does not support item assignment
```

## Truthiness

En contexto booleano:

* `b""` se evalúa como `False`
* cualquier secuencia no vacía de bytes se evalúa como `True`

```python id="t5m2qw"
print(bool(b""))
print(bool(b"A"))
print(bool(b"\x00"))
```

Salida:

```text id="n9m6tv"
False
True
True
```

Aunque el byte sea `0`, la secuencia no está vacía.

## Comparación

Los objetos `bytes` pueden compararse lexicográficamente.

```python id="p2m8pk"
print(b"ABC" == b"ABC")
print(b"ABC" < b"ABD")
print(b"b" > b"a")
```

Salida:

```text id="v7m1qw"
True
True
True
```

## Casos de uso frecuentes

## Representar contenido binario

```python id="r4m9tv"
firma = b"\x89PNG"
print(firma)
```

## Convertir texto a bytes

```python id="m6q3pk"
mensaje = "Hola"
datos = mensaje.encode("utf-8")

print(datos)
```

## Leer o escribir contenido binario

```python id="t8m5qw"
with open("archivo.bin", "wb") as archivo:
    archivo.write(b"ABC")
```

## Trabajar con protocolos o formatos

```python id="n1m9tv"
cabecera = b"GET / HTTP/1.1\r\n"
print(cabecera)
```

## Errores comunes

## Confundir `str` con `bytes`

Problemático:

```python id="p4m2pk"
texto = "Hola"
datos = b"Hola"

print(texto == datos)
```

Salida:

```text id="v9m7qw"
False
```

Aunque visualmente parezcan similares, pertenecen a tipos distintos.

## Intentar concatenar `str` con `bytes`

Problemático:

```python id="r2m4tv"
print("Hola" + b" mundo")
```

Esto genera `TypeError`.

## Intentar modificar un byte por índice

Problemático:

```python id="m7q1pk"
datos = b"ABC"
datos[0] = 65
```

Esto genera `TypeError` porque `bytes` es inmutable.

## Usar enteros fuera de rango en `bytes([...])`

Problemático:

```python id="t1m6qw"
bytes([300])
```

Esto genera `ValueError` porque los valores deben estar entre `0` y `255`.

## Decodificar con la codificación incorrecta

Problemático:

```python id="n4m8tv"
datos = b"\xff"
datos.decode("utf-8")
```

Esto puede generar `UnicodeDecodeError`.

## Buenas prácticas

## Usar `bytes` cuando el contenido sea binario y no deba modificarse

```python id="p9m3pk"
firma = b"\x89PNG"
```

## Distinguir claramente entre texto y binario

* `str` para texto
* `bytes` para datos binarios

## Codificar y decodificar explícitamente

```python id="v6m1qw"
datos = texto.encode("utf-8")
texto = datos.decode("utf-8")
```

## Usar `bytearray` si se necesita mutabilidad binaria

Si el contenido binario debe modificarse, suele corresponder `bytearray`, no `bytes`.

## Ejemplo integrado

```python id="r8m5tv"
def show_binary_summary(text):
    data = text.encode("utf-8")

    print("Resumen binario")
    print("-" * 30)
    print("Texto original:", text)
    print("Bytes:", data)
    print("Longitud:", len(data))
    print("Primer byte:", data[0] if data else "Sin datos")
    print("Slice inicial:", data[:2])
    print("Reconstruido:", data.decode("utf-8"))


show_binary_summary("Hola")
```

Salida aproximada:

```text id="m2q9pk"
Resumen binario
------------------------------
Texto original: Hola
Bytes: b'Hola'
Longitud: 4
Primer byte: 72
Slice inicial: b'Ho'
Reconstruido: Hola
```

## Relación con otros elementos integrados

`bytes` se relaciona especialmente con:

* `str`, por la conversión entre texto y binario
* `bytearray`, como variante mutable de secuencia binaria
* `len()` para contar bytes
* `open()` en modo binario
* métodos de bytes, documentados en la sección correspondiente

## Orden didáctico interno

```text id="v4m1qw"
1. Propósito de bytes
2. Naturaleza del tipo
3. Literales y construcción con bytes()
4. Longitud, indexación y slicing
5. Concatenación, repetición y pertenencia
6. Iteración
7. Relación entre str y bytes
8. Codificación y decodificación
9. Errores comunes
10. Buenas prácticas
```
