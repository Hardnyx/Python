# `memoryview`

## Propósito

`memoryview` representa una vista sobre los datos binarios de otro objeto que expone la interfaz de buffer. Su utilidad principal es permitir acceso, lectura y, cuando corresponde, escritura sobre esos datos sin copiar el contenido subyacente. :contentReference[oaicite:0]{index=0}

## Naturaleza del tipo

`memoryview` es un tipo integrado orientado a datos binarios. No es una secuencia textual como `str`, ni una secuencia binaria propietaria como `bytes` o `bytearray`, sino una vista sobre la memoria de otro objeto compatible. En la documentación de tipos integrados aparece dentro de los tipos de secuencias binarias junto con `bytes` y `bytearray`. :contentReference[oaicite:1]{index=1}

## Forma general

Un objeto `memoryview` se construye con:

```python
memoryview(obj)
````

El objeto `obj` debe exponer la interfaz de buffer. En la práctica, suele usarse con objetos como `bytes`, `bytearray`, `array.array` y otros exportadores de buffer. ([Python documentation][1])

## Idea central

La idea más importante de `memoryview` es que permite trabajar sobre el buffer subyacente sin copiarlo. Esto lo vuelve útil cuando se necesita eficiencia o cuando se quiere manipular una porción de datos binarios manteniendo una vista sobre el mismo contenido original. ([Python documentation][2])

## Construcción básica

## Desde `bytes`

```python id="y2r5bn"
datos = memoryview(b"abc")

print(datos)
print(type(datos))
```

Salida posible:

```text id="bcj7tu"
<memory at 0x...>
<class 'memoryview'>
```

Un `memoryview` creado desde `bytes` será de solo lectura, porque `bytes` es inmutable. ([Python documentation][2])

## Desde `bytearray`

```python id="q6m1zs"
datos = memoryview(bytearray(b"abc"))

print(datos)
print(type(datos))
```

Salida posible:

```text id="n4u8px"
<memory at 0x...>
<class 'memoryview'>
```

Un `memoryview` creado desde `bytearray` puede ser escribible, porque `bytearray` es mutable. ([Python documentation][3])

## Valor de retorno de `type()`

```python id="n1t9ra"
print(type(memoryview(b"abc")))
```

Salida:

```text id="r8m3he"
<class 'memoryview'>
```

## Longitud

`memoryview` tiene longitud y funciona con `len()`. La longitud cuenta elementos de la vista. ([Python documentation][4])

```python id="m5p2dk"
vista = memoryview(b"abc")
print(len(vista))
```

Salida:

```text id="7e8jns"
3
```

## Indexación

Una vista de memoria admite indexación. En vistas unidimensionales sobre datos binarios simples, la indexación devuelve valores enteros, de forma similar a `bytes` y `bytearray`. La documentación también indica que los `memoryview` pueden indexarse y, desde Python 3.5, admiten tuplas de enteros para indexación multidimensional. ([Python documentation][2])

```python id="qh2j3f"
vista = memoryview(b"ABC")

print(vista[0])
print(vista[1])
print(vista[-1])
```

Salida:

```text id="m7v3wd"
65
66
67
```

## Slicing

Los `memoryview` unidimensionales admiten slicing. El resultado sigue siendo una vista, no una copia de bytes. La documentación oficial destaca que el slicing en vistas unidimensionales está soportado y que el objeto sigue trabajando sobre el mismo buffer subyacente. ([Python documentation][2])

```python id="r3b6ku"
vista = memoryview(b"Python")

subvista = vista[1:4]

print(subvista)
print(type(subvista))
print(subvista.tobytes())
```

Salida posible:

```text id="p1u6ne"
<memory at 0x...>
<class 'memoryview'>
b'yth'
```

## Conversión a bytes

`memoryview` puede convertirse a bytes mediante `tobytes()`, y la documentación oficial señala que esto equivale a construir `bytes` a partir de la vista. Esa operación sí materializa una copia de los datos. ([Python documentation][2])

```python id="yt6dz1"
vista = memoryview(b"abc")
print(vista.tobytes())
```

Salida:

```text id="p9m2wf"
b'abc'
```

## Conversión a lista

`memoryview` también puede convertirse a lista mediante `tolist()`, devolviendo los elementos interpretados según el formato de la vista. ([Python documentation][2])

```python id="ch4r6q"
vista = memoryview(b"abc")
print(vista.tolist())
```

Salida:

```text id="m6b4ru"
[97, 98, 99]
```

## Escritura cuando la vista es modificable

Si el objeto subyacente exporta un buffer escribible, la vista puede modificar esos datos. En el nivel Python, esto se observa claramente con `bytearray`. ([Python documentation][3])

```python id="m9n2pk"
datos = bytearray(b"abc")
vista = memoryview(datos)

vista[0] = 65

print(datos)
print(vista.tobytes())
```

Salida:

```text id="s2v7qm"
bytearray(b'Abc')
b'Abc'
```

## Solo lectura y escritura

La documentación oficial expone el atributo `readonly`, que indica si la vista es de solo lectura. También incluye el método `toreadonly()`, que produce una versión de solo lectura de una vista existente. ([Python documentation][2])

```python id="n7d5qw"
vista_1 = memoryview(b"abc")
vista_2 = memoryview(bytearray(b"abc"))

print(vista_1.readonly)
print(vista_2.readonly)
```

Salida típica:

```text id="j5m8rp"
True
False
```

## Tamaño de elemento

La documentación oficial expone el atributo `itemsize`, que indica el tamaño en bytes de cada elemento de la vista. En vistas simples sobre bytes, el tamaño por elemento suele ser `1`. ([Python documentation][2])

```python id="t4x7mu"
vista = memoryview(b"abc")
print(vista.itemsize)
```

Salida:

```text id="p4r1ks"
1
```

## Dimensión y forma

`memoryview` puede exponer información estructural como `ndim` y, cuando corresponde, forma lógica multidimensional. La documentación oficial muestra además que puede convertirse o reinterpretarse con `cast()` sin copiar el buffer, tanto para cambiar formato como para dar forma unidimensional o multidimensional compatible. ([Python documentation][2])

```python id="m3q9tz"
vista = memoryview(b"abc")

print(vista.ndim)
```

Salida típica:

```text id="r9n5pu"
1
```

## Liberación de la vista

La documentación oficial incluye el método `release()`, que libera el buffer expuesto por la vista. Después de llamar a `release()`, cualquier operación posterior sobre la vista produce `ValueError`. También se puede usar el protocolo de contexto con `with` para obtener un efecto similar. ([Python documentation][2])

```python id="v6r2kw"
vista = memoryview(b"abc")
vista.release()
```

## Uso con `with`

```python id="d8m1qp"
with memoryview(b"abc") as vista:
    print(vista[0])
```

Después del bloque, la vista queda liberada y ya no debería usarse. ([Python documentation][2])

## Relación con `bytes` y `bytearray`

La relación práctica entre estos tipos puede resumirse así:

* `bytes` representa datos binarios inmutables
* `bytearray` representa datos binarios mutables
* `memoryview` representa una vista sobre un buffer existente, sin copiarlo inicialmente ([Python documentation][4])

Esto hace que `memoryview` no reemplace a `bytes` o `bytearray`, sino que los complemente cuando interesa eficiencia o acceso compartido a memoria. ([Python documentation][2])

## Truthiness

Como otros objetos de secuencia, `memoryview` puede evaluarse en contexto booleano. Una vista vacía se evalúa como falsa y una no vacía como verdadera por la regla general de truthiness basada en longitud. ([Python documentation][4])

```python id="w1m6zs"
print(bool(memoryview(b"")))
print(bool(memoryview(b"A")))
```

Salida:

```text id="n3q8vu"
False
True
```

## Casos de uso frecuentes

## Evitar copias innecesarias

```python id="m5c2wr"
datos = bytearray(b"abcdef")
vista = memoryview(datos)

print(vista[1:4].tobytes())
```

Esto permite trabajar con una porción del contenido sin copiar el buffer original hasta que se llame a `tobytes()`. ([Python documentation][2])

## Modificar parte de un buffer mutable

```python id="x9q4kp"
datos = bytearray(b"abcdef")
vista = memoryview(datos)

vista[2:4] = b"XY"

print(datos)
```

Salida:

```text id="f3m8tq"
bytearray(b'abXYef')
```

## Inspeccionar datos binarios como enteros

```python id="p7m3vx"
vista = memoryview(b"ABC")
print(vista.tolist())
```

## Errores comunes

## Confundir `memoryview` con una copia de bytes

Problemático:

```python id="w8q1nr"
datos = bytearray(b"abc")
vista = memoryview(datos)
vista[0] = 65
print(datos)
```

Aquí sí cambia `datos`, porque la vista apunta al mismo buffer subyacente. ([Python documentation][3])

## Intentar escribir sobre una vista de solo lectura

Problemático:

```python id="j5m9ut"
vista = memoryview(b"abc")
vista[0] = 65
```

Esto genera `TypeError`, porque la vista es de solo lectura. ([Python documentation][2])

## Seguir usando la vista después de `release()`

Problemático:

```python id="v4r7me"
vista = memoryview(b"abc")
vista.release()
vista[0]
```

Esto genera `ValueError`. ([Python documentation][2])

## Suponer que es una lista o una cadena

`memoryview` no es ni `list` ni `str`. Si se necesita una representación materializada, normalmente corresponde usar `tobytes()` o `tolist()`. ([Python documentation][2])

## Buenas prácticas

## Usar `memoryview` cuando interese trabajar sobre buffers sin copiar

Esto es especialmente útil con datos binarios grandes o cuando se necesita acceder a segmentos de memoria de forma eficiente. ([Python documentation][2])

## Usar `bytes` o `bytearray` si se necesita una representación binaria “propietaria”

`memoryview` tiene más sentido como vista sobre otro objeto que como formato principal de almacenamiento. ([Python documentation][4])

## Liberar la vista cuando ya no se necesite

Si el exportador impone restricciones mientras la vista está activa, `release()` o `with` permiten cerrar antes ese acceso. ([Python documentation][2])

## Ejemplo integrado

```python id="q6m2tw"
def show_memoryview_summary(text):
    data = bytearray(text, encoding="utf-8")
    view = memoryview(data)

    print("Resumen de memoryview")
    print("." * 30)
    print("Vista:", view)
    print("Longitud:", len(view))
    print("Solo lectura:", view.readonly)
    print("Primer elemento:", view[0])
    print("Slice inicial:", view[:2].tobytes())

    view[0] = ord("H")

    print("Buffer modificado:", data)
    print("Reconstruido:", data.decode("utf-8"))


show_memoryview_summary("hola")
```

Salida aproximada:

```text id="e1m8rv"
Resumen de memoryview
..............................
Vista: <memory at 0x...>
Longitud: 4
Solo lectura: False
Primer elemento: 104
Slice inicial: b'ho'
Buffer modificado: bytearray(b'Hola')
Reconstruido: Hola
```

## Relación con otros elementos integrados

`memoryview` se relaciona especialmente con:

* `bytes`, como secuencia binaria inmutable
* `bytearray`, como secuencia binaria mutable
* `len()` para longitud
* `iter()` y `next()` por su comportamiento iterable
* `open()` en modo binario cuando se trabaja con buffers
* métodos específicos de vistas de memoria, como `tobytes()`, `tolist()`, `toreadonly()`, `release()` y `cast()` ([Python documentation][2])

## Orden didáctico interno

```text id="m4q7pw"
1. Propósito de memoryview
2. Naturaleza del tipo
3. Construcción con memoryview()
4. Longitud, indexación y slicing
5. Solo lectura y escritura
6. Conversión a bytes o lista
7. Relación con bytes y bytearray
8. Liberación con release() y with
9. Errores comunes
10. Buenas prácticas
```