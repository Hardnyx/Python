# `tuple`

## Propósito

`tuple` representa una secuencia ordenada e inmutable de elementos. Se utiliza cuando se necesita agrupar valores preservando su orden, pero sin permitir modificaciones posteriores sobre la estructura.

## Naturaleza del tipo

`tuple` es un tipo secuencial, ordenado e inmutable.

Esto significa que:

- mantiene el orden de inserción
- permite acceso posicional
- admite elementos repetidos
- puede contener valores de distintos tipos
- no permite modificar, agregar ni eliminar elementos después de su creación

## Forma general

Una tupla puede escribirse como literal:

```python
(1, 2, 3)
("Ana", "Luis")
()
````

o construirse explícitamente con:

```python
tuple(iterable)
```

## Literales de tupla

```python
numeros = (1, 2, 3)
nombres = ("Ana", "Luis", "Marta")
vacia = ()

print(numeros)
print(nombres)
print(vacia)
```

Salida:

```text
(1, 2, 3)
('Ana', 'Luis', 'Marta')
()
```

## Tupla de un solo elemento

Una tupla de un solo elemento requiere una coma final.

```python
valor = (10,)
print(valor)
print(type(valor))
```

Salida:

```text
(10,)
<class 'tuple'>
```

Sin la coma, no se crea una tupla:

```python
valor = (10)
print(valor)
print(type(valor))
```

Salida:

```text
10
<class 'int'>
```

## Tuplas heterogéneas

Una tupla puede contener elementos de distintos tipos.

```python
datos = ("Ana", 20, True, 3.14)

print(datos)
```

Salida:

```text
('Ana', 20, True, 3.14)
```

## Valor de retorno de `type()`

```python
print(type((1, 2, 3)))
```

Salida:

```text
<class 'tuple'>
```

## Inmutabilidad

`tuple` es inmutable.

```python
datos = (10, 20, 30)
print(id(datos))

datos = datos + (40,)
print(datos)
print(id(datos))
```

Salida:

```text
(10, 20, 30, 40)
```

La identidad cambia porque no se modifica la tupla original. Se crea una nueva.

## Longitud

La longitud se obtiene con `len()`.

```python
print(len((1, 2, 3)))
print(len(()))
```

Salida:

```text
3
0
```

## Indexación

Una tupla permite acceder a elementos por posición.

```python
nombres = ("Ana", "Luis", "Marta")

print(nombres[0])
print(nombres[1])
print(nombres[-1])
```

Salida:

```text
Ana
Luis
Marta
```

## Índices negativos

Los índices negativos cuentan desde el final.

```python
valores = (10, 20, 30, 40)

print(valores[-1])
print(valores[-2])
```

Salida:

```text
40
30
```

## Error por índice fuera de rango

```python
valores = (1, 2, 3)
print(valores[10])
```

Error típico:

```text
IndexError: tuple index out of range
```

## Slicing

El slicing permite extraer subtuplas.

Forma general:

```python
tupla[inicio:fin:paso]
```

## Segmento simple

```python
valores = (10, 20, 30, 40, 50)

print(valores[1:4])
print(valores[:3])
print(valores[2:])
print(valores[:])
```

Salida:

```text
(20, 30, 40)
(10, 20, 30)
(30, 40, 50)
(10, 20, 30, 40, 50)
```

## Paso en slicing

```python
valores = (10, 20, 30, 40, 50, 60)

print(valores[::2])
print(valores[::-1])
```

Salida:

```text
(10, 30, 50)
(60, 50, 40, 30, 20, 10)
```

## Concatenación

Las tuplas pueden unirse con `+`.

```python
a = (1, 2)
b = (3, 4)

print(a + b)
```

Salida:

```text
(1, 2, 3, 4)
```

## Repetición

Las tuplas pueden repetirse con `*`.

```python
print((1, 2) * 3)
```

Salida:

```text
(1, 2, 1, 2, 1, 2)
```

## Pertenencia

Se puede verificar si un valor está presente con `in`.

```python
valores = (10, 20, 30)

print(20 in valores)
print(99 in valores)
```

Salida:

```text
True
False
```

## Iteración

Una tupla es iterable.

```python
nombres = ("Ana", "Luis", "Marta")

for nombre in nombres:
    print(nombre)
```

Salida:

```text
Ana
Luis
Marta
```

## Desempaquetado

```python
a, b, c = (10, 20, 30)

print(a)
print(b)
print(c)
```

Salida:

```text
10
20
30
```

## Desempaquetado extendido

```python
primero, *medio, ultimo = (10, 20, 30, 40, 50)

print(primero)
print(medio)
print(ultimo)
```

Salida:

```text
10
[20, 30, 40]
50
```

Aunque el origen sea una tupla, la parte capturada por `*` se devuelve como lista.

## Conversión con `tuple()`

La función `tuple()` construye tuplas a partir de iterables.

## Desde cadena

```python
print(tuple("Hola"))
```

Salida:

```text
('H', 'o', 'l', 'a')
```

## Desde lista

```python
print(tuple([1, 2, 3]))
```

Salida:

```text
(1, 2, 3)
```

## Desde rango

```python
print(tuple(range(5)))
```

Salida:

```text
(0, 1, 2, 3, 4)
```

## Desde diccionario

Si se aplica sobre un diccionario, produce la tupla de claves.

```python
datos = {"nombre": "Ana", "edad": 20}
print(tuple(datos))
```

Salida:

```text
('nombre', 'edad')
```

## Relación con inmutabilidad

No se pueden reasignar elementos por índice.

```python
valores = (10, 20, 30)
valores[1] = 99
```

Error típico:

```text
TypeError: 'tuple' object does not support item assignment
```

Tampoco existen métodos como `append()` o `remove()` para modificar la estructura.

## Tuplas con objetos mutables dentro

Una tupla es inmutable en su estructura, pero puede contener objetos mutables.

```python
datos = ([1, 2], [3, 4])

datos[0].append(99)
print(datos)
```

Salida:

```text
([1, 2, 99], [3, 4])
```

No se modificó la tupla en sí, pero sí uno de los objetos mutables contenidos dentro de ella.

## Métodos principales

`tuple` tiene pocos métodos públicos porque es inmutable.

Los más importantes son:

```python
valores = (10, 20, 10, 30)

print(valores.count(10))
print(valores.index(20))
```

Salida:

```text
2
1
```

## Hashabilidad

Las tuplas pueden ser hashables si todos sus elementos también lo son.

Eso permite usarlas como claves de diccionario o como elementos de un conjunto.

Ejemplo válido:

```python
datos = {
    (1, 2): "par",
    (3, 4): "otro par"
}

print(datos[(1, 2)])
```

Salida:

```text
par
```

Ejemplo inválido:

```python
datos = {
    ([1, 2], [3, 4]): "valor"
}
```

Esto genera error porque las listas no son hashables.

## Ordenamiento

Las tuplas pueden ordenarse con `sorted()`.

```python
valores = (3, 1, 2)

print(sorted(valores))
```

Salida:

```text
[1, 2, 3]
```

El resultado es una lista, no una tupla.

Si se necesita una tupla ordenada:

```python
valores = (3, 1, 2)

ordenados = tuple(sorted(valores))
print(ordenados)
```

Salida:

```text
(1, 2, 3)
```

## Comparación

Las tuplas pueden compararse lexicográficamente.

```python
print((1, 2) == (1, 2))
print((1, 2) < (1, 3))
print((2, 0) > (1, 9))
```

Salida:

```text
True
True
True
```

La comparación se realiza elemento por elemento de izquierda a derecha.

## Truthiness

En contexto booleano:

* una tupla vacía se evalúa como `False`
* una tupla no vacía se evalúa como `True`

```python
print(bool(()))
print(bool((0,)))
print(bool((1, 2, 3)))
```

Salida:

```text
False
True
True
```

## Casos de uso frecuentes

## Agrupar valores relacionados

```python
persona = ("Ana", 20, "Lima")
print(persona)
```

## Retornar varios valores desde una función

```python
def dividir(a, b):
    return a // b, a % b

resultado = dividir(17, 5)
print(resultado)
```

Salida:

```text
(3, 2)
```

## Usar coordenadas o pares fijos

```python
punto = (10, 20)
print(punto)
```

## Usar claves compuestas en diccionarios

```python
ventas = {
    ("2026", "01"): 1200,
    ("2026", "02"): 1500
}

print(ventas[("2026", "01")])
```

## Errores comunes

## Olvidar la coma en tuplas de un solo elemento

Problemático:

```python
valor = (10)
print(type(valor))
```

Salida:

```text
<class 'int'>
```

Correcto:

```python
valor = (10,)
print(type(valor))
```

## Intentar modificar una tupla

Problemático:

```python
valores = (1, 2, 3)
valores[0] = 99
```

Esto genera `TypeError`.

## Suponer que toda tupla es hashable

Problemático:

```python
clave = ([1, 2], [3, 4])
datos = {clave: "valor"}
```

Esto falla porque contiene listas, que no son hashables.

## Confundir inmutabilidad de la tupla con inmutabilidad de sus contenidos

```python
datos = ([1, 2], [3, 4])
datos[0].append(99)
```

La tupla sigue siendo la misma, pero uno de sus elementos mutables sí puede cambiar.

## Buenas prácticas

## Usar tuplas cuando la estructura deba permanecer fija

```python
coordenada = (10, 20)
```

## Usar tuplas para agrupar pocos valores relacionados

```python
registro = ("Ana", 20, "Lima")
```

## Usar tuplas como claves compuestas cuando todos sus elementos sean hashables

```python
ventas[("2026", "03")] = 1800
```

## No usar tuplas cuando se requiera agregar, quitar o reemplazar elementos con frecuencia

En esos casos suele corresponder `list`.

## Tener cuidado con tuplas que contienen objetos mutables

La inmutabilidad de la estructura no implica inmutabilidad profunda del contenido.

## Ejemplo integrado

```python
def show_product_summary(product):
    code, name, price = product

    print("Resumen de producto")
    print("-" * 30)
    print("Código:", code)
    print("Nombre:", name)
    print("Precio:", price)
    print("Registro completo:", product)
    print("Cantidad de campos:", len(product))


show_product_summary(("P001", "Teclado", 120.0))
```

Salida aproximada:

```text
Resumen de producto
------------------------------
Código: P001
Nombre: Teclado
Precio: 120.0
Registro completo: ('P001', 'Teclado', 120.0)
Cantidad de campos: 3
```

## Relación con otros elementos integrados

`tuple` se relaciona especialmente con:

* `list` como otra secuencia ordenada, pero mutable
* `set` y `dict` como colecciones con reglas distintas
* `len()`, `sorted()`, `min()` y `max()` al operar sobre colecciones
* desempaquetado en asignaciones y retorno múltiple de funciones
* métodos de tupla, documentados en la sección correspondiente

## Orden didáctico interno

```text
1. Propósito de tuple
2. Naturaleza del tipo
3. Literales y construcción con tuple()
4. Longitud, indexación y slicing
5. Concatenación, repetición y pertenencia
6. Iteración y desempaquetado
7. Inmutabilidad
8. Hashabilidad
9. Comparación y truthiness
10. Errores comunes
11. Buenas prácticas
```