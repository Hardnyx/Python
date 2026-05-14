# `list`

## Propósito

`list` representa una secuencia ordenada y mutable de elementos. Se usa para almacenar colecciones de valores, recorrer datos, acumular resultados, modelar registros simples y trabajar con estructuras que cambian a lo largo de la ejecución.

## Naturaleza del tipo

`list` es un tipo secuencial, ordenado y mutable.

Esto significa que:

- mantiene el orden de inserción
- permite acceso posicional
- admite elementos repetidos
- puede contener valores de distintos tipos
- puede modificarse después de crearse

## Forma general

Una lista puede escribirse como literal:

```python
[1, 2, 3]
["Ana", "Luis"]
[]
````

o construirse explícitamente con:

```python id="p2v8mk"
list(iterable)
```

## Literales de lista

```python id="s7q4wn"
numeros = [1, 2, 3]
nombres = ["Ana", "Luis", "Marta"]
vacia = []

print(numeros)
print(nombres)
print(vacia)
```

Salida:

```text id="m4r9px"
[1, 2, 3]
['Ana', 'Luis', 'Marta']
[]
```

## Listas heterogéneas

Una lista puede contener elementos de distintos tipos.

```python id="v1q6tw"
datos = ["Ana", 20, True, 3.14]

print(datos)
```

Salida:

```text id="q8m2rv"
['Ana', 20, True, 3.14]
```

## Valor de retorno de `type()`

```python id="x5p9mk"
print(type([1, 2, 3]))
```

Salida:

```text id="r3q7wn"
<class 'list'>
```

## Mutabilidad

`list` es mutable.

```python id="n6m1qx"
valores = [1, 2, 3]
print(id(valores))

valores.append(4)
print(valores)
print(id(valores))
```

Salida:

```text id="k9p4rv"
[1, 2, 3, 4]
```

La identidad del objeto no cambia. Se modifica la misma lista.

## Longitud

La longitud se obtiene con `len()`.

```python id="w2q8tm"
print(len([1, 2, 3]))
print(len([]))
```

Salida:

```text id="p7m3rx"
3
0
```

## Indexación

Una lista permite acceder a elementos por posición.

```python id="m8q1wv"
nombres = ["Ana", "Luis", "Marta"]

print(nombres[0])
print(nombres[1])
print(nombres[-1])
```

Salida:

```text id="r5p8mk"
Ana
Luis
Marta
```

## Índices negativos

Los índices negativos cuentan desde el final.

```python id="t3q7wn"
valores = [10, 20, 30, 40]

print(valores[-1])
print(valores[-2])
```

Salida:

```text id="n1m6qx"
40
30
```

## Error por índice fuera de rango

```python id="q4p9rv"
valores = [1, 2, 3]
print(valores[10])
```

Error típico:

```text id="m7q2tw"
IndexError: list index out of range
```

## Slicing

El slicing permite extraer sublistas.

Forma general:

```python id="v6m3pk"
lista[inicio:fin:paso]
```

## Segmento simple

```python id="p9q5wn"
valores = [10, 20, 30, 40, 50]

print(valores[1:4])
print(valores[:3])
print(valores[2:])
print(valores[:])
```

Salida:

```text id="r2m8qx"
[20, 30, 40]
[10, 20, 30]
[30, 40, 50]
[10, 20, 30, 40, 50]
```

## Paso en slicing

```python id="m1p7rv"
valores = [10, 20, 30, 40, 50, 60]

print(valores[::2])
print(valores[::-1])
```

Salida:

```text id="q6m4tw"
[10, 30, 50]
[60, 50, 40, 30, 20, 10]
```

## Concatenación

Las listas pueden unirse con `+`.

```python id="w8q2mk"
a = [1, 2]
b = [3, 4]

print(a + b)
```

Salida:

```text id="p4m9rx"
[1, 2, 3, 4]
```

## Repetición

Las listas pueden repetirse con `*`.

```python id="n3q6wv"
print([1, 2] * 3)
```

Salida:

```text id="r8m1pk"
[1, 2, 1, 2, 1, 2]
```

## Pertenencia

Se puede verificar si un valor está presente con `in`.

```python id="t5q9wn"
valores = [10, 20, 30]

print(20 in valores)
print(99 in valores)
```

Salida:

```text id="m2p7rv"
True
False
```

## Iteración

Una lista es iterable.

```python id="q1m8tw"
nombres = ["Ana", "Luis", "Marta"]

for nombre in nombres:
    print(nombre)
```

Salida:

```text id="v7q3mk"
Ana
Luis
Marta
```

## Desempaquetado

```python id="p6m4rx"
a, b, c = [10, 20, 30]

print(a)
print(b)
print(c)
```

Salida:

```text id="n9q1wv"
10
20
30
```

La cantidad de variables debe coincidir con la cantidad de elementos, salvo que se use desempaquetado extendido.

## Desempaquetado extendido

```python id="r4m7pk"
primero, *medio, ultimo = [10, 20, 30, 40, 50]

print(primero)
print(medio)
print(ultimo)
```

Salida:

```text id="q8p2tw"
10
[20, 30, 40]
50
```

## Conversión con `list()`

La función `list()` construye listas a partir de iterables.

## Desde cadena

```python id="m7q5rv"
print(list("Hola"))
```

Salida:

```text id="p1m9wx"
['H', 'o', 'l', 'a']
```

## Desde tupla

```python id="v3q8pk"
print(list((1, 2, 3)))
```

Salida:

```text id="r6m2tw"
[1, 2, 3]
```

## Desde rango

```python id="n4p7wv"
print(list(range(5)))
```

Salida:

```text id="q9m3rk"
[0, 1, 2, 3, 4]
```

## Desde diccionario

Si se aplica sobre un diccionario, produce la lista de claves.

```python id="t2q6mx"
datos = {"nombre": "Ana", "edad": 20}
print(list(datos))
```

Salida:

```text id="m8p4rw"
['nombre', 'edad']
```

## Modificación por índice

Una lista permite reasignar elementos por posición.

```python id="p5q1tw"
valores = [10, 20, 30]
valores[1] = 99

print(valores)
```

Salida:

```text id="r1m7qk"
[10, 99, 30]
```

## Modificación por slicing

También puede reemplazarse un segmento.

```python id="n8q3rv"
valores = [10, 20, 30, 40]
valores[1:3] = [200, 300]

print(valores)
```

Salida:

```text id="q4m9wx"
[10, 200, 300, 40]
```

## Métodos principales

`list` tiene muchos métodos propios, documentados aparte en la sección de métodos de tipos.

Ejemplos frecuentes:

```python id="v2q7pk"
valores = [1, 2, 3]

valores.append(4)
print(valores)

valores.insert(0, 0)
print(valores)

valores.remove(2)
print(valores)
```

Salida:

```text id="m5p1tw"
[1, 2, 3, 4]
[0, 1, 2, 3, 4]
[0, 1, 3, 4]
```

## Copia de listas

## Referencia compartida

```python id="q7m4rv"
a = [1, 2, 3]
b = a

b.append(4)

print(a)
print(b)
```

Salida:

```text id="p3q8wx"
[1, 2, 3, 4]
[1, 2, 3, 4]
```

No se creó copia. Ambas variables apuntan a la misma lista.

## Copia superficial

```python id="t1m6pk"
a = [1, 2, 3]
b = a.copy()

b.append(4)

print(a)
print(b)
```

Salida:

```text id="r8q2tv"
[1, 2, 3]
[1, 2, 3, 4]
```

## Ordenamiento

Las listas pueden ordenarse con `sorted()` o con su método `sort()`.

```python id="n6p9rw"
valores = [3, 1, 2]

print(sorted(valores))
print(valores)

valores.sort()
print(valores)
```

Salida:

```text id="q2m5pk"
[1, 2, 3]
[3, 1, 2]
[1, 2, 3]
```

## Truthiness

En contexto booleano:

* una lista vacía se evalúa como `False`
* una lista no vacía se evalúa como `True`

```python id="p9q4tv"
print(bool([]))
print(bool([0]))
print(bool([1, 2, 3]))
```

Salida:

```text id="m4p8rw"
False
True
True
```

## Listas anidadas

Una lista puede contener otras listas.

```python id="v1q7pk"
matriz = [
    [1, 2, 3],
    [4, 5, 6]
]

print(matriz)
print(matriz[0])
print(matriz[0][1])
```

Salida:

```text id="r5m2tw"
[[1, 2, 3], [4, 5, 6]]
[1, 2, 3]
2
```

## Comprensiones de lista

Las comprensiones permiten construir listas de forma compacta.

```python id="n3q9rv"
cuadrados = [x ** 2 for x in range(5)]
print(cuadrados)
```

Salida:

```text id="q7m1pk"
[0, 1, 4, 9, 16]
```

Con condición:

```python id="t8p4wx"
pares = [x for x in range(10) if x % 2 == 0]
print(pares)
```

Salida:

```text id="m1q6tv"
[0, 2, 4, 6, 8]
```

## Casos de uso frecuentes

## Acumular datos

```python id="p4m7rw"
valores = []

for x in range(5):
    valores.append(x)

print(valores)
```

## Representar registros simples

```python id="v9q2pk"
estudiantes = ["Ana", "Luis", "Marta"]
print(estudiantes)
```

## Trabajar con resultados intermedios

```python id="r2m8tw"
numeros = [10, 20, 30]
dobles = [n * 2 for n in numeros]

print(dobles)
```

## Errores comunes

## Suponer que la asignación crea copia

Problemático:

```python id="n5q1rv"
a = [1, 2, 3]
b = a
```

Aquí no hay copia. Ambas variables apuntan a la misma lista.

## Usar índices fuera de rango

Problemático:

```python id="t2m7pk"
valores = [1, 2, 3]
print(valores[5])
```

Esto genera `IndexError`.

## Modificar la lista mientras se recorre sin cuidado

Problemático:

```python id="p8q4wx"
valores = [1, 2, 3, 4]

for valor in valores:
    if valor % 2 == 0:
        valores.remove(valor)

print(valores)
```

Esto puede producir resultados inesperados porque la estructura cambia durante la iteración.

## Crear listas anidadas repetidas con `*` de forma incorrecta

Problemático:

```python id="m4q9tv"
matriz = [[0] * 3] * 2
matriz[0][0] = 1

print(matriz)
```

Salida:

```text id="r1m5pk"
[[1, 0, 0], [1, 0, 0]]
```

Las sublistas internas comparten referencia.

Forma más segura:

```python id="n7q2rw"
matriz = [[0] * 3 for _ in range(2)]
matriz[0][0] = 1

print(matriz)
```

Salida:

```text id="q3m8tv"
[[1, 0, 0], [0, 0, 0]]
```

## Buenas prácticas

## Usar listas cuando se necesite mutabilidad y orden

```python id="v6q1pk"
tareas = ["Estudiar", "Practicar", "Repasar"]
```

## Usar comprensiones cuando la transformación sea clara

```python id="r9m4wx"
dobles = [x * 2 for x in valores]
```

## Copiar explícitamente cuando no se quiera compartir referencia

```python id="p1q7tv"
copia = valores.copy()
```

## Preferir `append()` para acumulación progresiva

```python id="n4m2pk"
resultado = []
resultado.append(10)
```

## Usar `sorted()` si se quiere conservar la lista original

```python id="t7q5rw"
ordenados = sorted(valores)
```

## Ejemplo integrado

```python id="m8q3tv"
def show_student_summary(names):
    names_copy = names.copy()
    names_copy.sort()

    print("Resumen de estudiantes")
    print("-" * 30)
    print("Original:", names)
    print("Ordenados:", names_copy)
    print("Cantidad:", len(names))
    print("Primer estudiante:", names[0] if names else "Sin datos")
    print("Existe 'Ana':", "Ana" in names)


show_student_summary(["Luis", "Ana", "Marta"])
```

Salida aproximada:

```text id="q2m6pk"
Resumen de estudiantes
------------------------------
Original: ['Luis', 'Ana', 'Marta']
Ordenados: ['Ana', 'Luis', 'Marta']
Cantidad: 3
Primer estudiante: Luis
Existe 'Ana': True
```

## Relación con otros elementos integrados

`list` se relaciona especialmente con:

* `tuple` como otra secuencia ordenada, pero inmutable
* `set` y `dict` como colecciones con otras reglas estructurales
* `len()`, `sorted()`, `sum()`, `min()` y `max()` al operar sobre colecciones
* `range()`, `enumerate()` y `zip()` en iteración
* métodos de lista, documentados en la sección correspondiente

## Orden didáctico interno

```text id="t5q9rw"
1. Propósito de list
2. Naturaleza del tipo
3. Literales y construcción con list()
4. Longitud, indexación y slicing
5. Concatenación, repetición y pertenencia
6. Iteración y desempaquetado
7. Mutabilidad y modificación
8. Copia y referencias
9. Comprensiones de lista
10. Errores comunes
11. Buenas prácticas
```