# `set`

## Propósito

`set` representa una colección mutable de elementos únicos sin orden posicional. Se utiliza para eliminar duplicados, verificar pertenencia de forma eficiente y realizar operaciones de teoría de conjuntos como unión, intersección y diferencia.

## Naturaleza del tipo

`set` es un tipo de colección, mutable y no indexado.

Esto significa que:

- almacena elementos sin duplicados
- no tiene acceso posicional por índice
- no preserva un orden posicional utilizable como una lista o tupla
- puede modificarse después de crearse
- sus elementos deben ser hashables

## Forma general

Un conjunto puede escribirse como literal:

```python
{1, 2, 3}
{"Ana", "Luis"}
````

o construirse explícitamente con:

```python id="o2m7qw"
set(iterable)
```

## Literales de conjunto

```python id="v9m1pk"
valores = {1, 2, 3}
nombres = {"Ana", "Luis", "Marta"}

print(valores)
print(nombres)
```

Salida posible:

```text id="r4m8tv"
{1, 2, 3}
{'Luis', 'Marta', 'Ana'}
```

El orden visual puede variar.

## Conjunto vacío

Un error común es pensar que `{}` crea un conjunto vacío.

```python id="m7q3rw"
print(type({}))
```

Salida:

```text id="p1m6pk"
<class 'dict'>
```

Para crear un conjunto vacío debe usarse:

```python id="t4m9qw"
vacio = set()
print(vacio)
print(type(vacio))
```

Salida:

```text id="n8m2tv"
set()
<class 'set'>
```

## Valor de retorno de `type()`

```python id="p3m7pk"
print(type({1, 2, 3}))
```

Salida:

```text id="v6m4qw"
<class 'set'>
```

## Mutabilidad

`set` es mutable.

```python id="r9m1tv"
valores = {1, 2, 3}
print(id(valores))

valores.add(4)
print(valores)
print(id(valores))
```

Salida:

```text id="m4q8pk"
{1, 2, 3, 4}
```

La identidad del objeto no cambia. Se modifica el mismo conjunto.

## Unicidad de elementos

Un conjunto no permite duplicados.

```python id="t7m2qw"
valores = {1, 2, 2, 3, 3, 3}
print(valores)
```

Salida:

```text id="n1m5tv"
{1, 2, 3}
```

Los duplicados se eliminan automáticamente.

## Elementos hashables

Los elementos de un conjunto deben ser hashables.

Ejemplos válidos:

```python id="p4m8pk"
valores = {1, "Ana", (1, 2), True}
print(valores)
```

Ejemplos inválidos:

```python id="v7m3qw"
valores = {[1, 2], [3, 4]}
```

Esto genera error porque las listas no son hashables.

## Longitud

La longitud se obtiene con `len()`.

```python id="r2m6tv"
print(len({1, 2, 3}))
print(len(set()))
```

Salida:

```text id="m9q1pk"
3
0
```

## Pertenencia

El operador `in` es una de las utilidades principales de `set`.

```python id="t5m4qw"
valores = {10, 20, 30}

print(20 in valores)
print(99 in valores)
```

Salida:

```text id="n8m7tv"
True
False
```

## Iteración

Un conjunto es iterable.

```python id="p8m2pk"
nombres = {"Ana", "Luis", "Marta"}

for nombre in nombres:
    print(nombre)
```

Salida posible:

```text id="v1m5qw"
Luis
Ana
Marta
```

El orden no debe asumirse como fijo para propósitos posicionales.

## Conversión con `set()`

La función `set()` construye conjuntos a partir de iterables.

## Desde lista

```python id="r4m7tv"
print(set([1, 2, 2, 3, 3]))
```

Salida:

```text id="m7q4pk"
{1, 2, 3}
```

## Desde tupla

```python id="t1m8qw"
print(set((1, 1, 2, 3)))
```

Salida:

```text id="n4m2tv"
{1, 2, 3}
```

## Desde cadena

```python id="p6m1pk"
print(set("Hola"))
```

Salida posible:

```text id="v9m8qw"
{'H', 'o', 'a', 'l'}
```

Se eliminan caracteres repetidos y el orden no debe interpretarse como significativo.

## Desde diccionario

Si se aplica sobre un diccionario, produce el conjunto de claves.

```python id="r3m5tv"
datos = {"nombre": "Ana", "edad": 20}
print(set(datos))
```

Salida:

```text id="m6q9pk"
{'nombre', 'edad'}
```

## Operaciones de conjuntos

## Unión

La unión reúne todos los elementos de ambos conjuntos.

```python id="t8m3qw"
a = {1, 2, 3}
b = {3, 4, 5}

print(a | b)
```

Salida:

```text id="n1q6tv"
{1, 2, 3, 4, 5}
```

## Intersección

La intersección conserva solo los elementos comunes.

```python id="p5m9pk"
a = {1, 2, 3}
b = {3, 4, 5}

print(a & b)
```

Salida:

```text id="v2m4qw"
{3}
```

## Diferencia

La diferencia conserva los elementos del primer conjunto que no están en el segundo.

```python id="r7m1tv"
a = {1, 2, 3}
b = {3, 4, 5}

print(a - b)
print(b - a)
```

Salida:

```text id="m4q7pk"
{1, 2}
{4, 5}
```

## Diferencia simétrica

La diferencia simétrica conserva los elementos que están en uno u otro, pero no en ambos.

```python id="t2m6qw"
a = {1, 2, 3}
b = {3, 4, 5}

print(a ^ b)
```

Salida:

```text id="n8m1tv"
{1, 2, 4, 5}
```

## Comparación entre conjuntos

## Subconjunto

```python id="p9m4pk"
a = {1, 2}
b = {1, 2, 3}

print(a <= b)
print(a < b)
```

Salida:

```text id="v6m9qw"
True
True
```

## Superconjunto

```python id="r1m5tv"
a = {1, 2, 3}
b = {1, 2}

print(a >= b)
print(a > b)
```

Salida:

```text id="m7q2pk"
True
True
```

## Igualdad

```python id="t4m8qw"
print({1, 2, 3} == {3, 2, 1})
```

Salida:

```text id="n2m5tv"
True
```

El orden no importa en la comparación.

## Métodos principales

`set` tiene varios métodos importantes, documentados aparte en la sección de métodos de tipos.

Ejemplos frecuentes:

```python id="p7m1pk"
valores = {1, 2, 3}

valores.add(4)
print(valores)

valores.remove(2)
print(valores)

valores.discard(99)
print(valores)
```

Salida posible:

```text id="v3m6qw"
{1, 2, 3, 4}
{1, 3, 4}
{1, 3, 4}
```

## Diferencia entre `remove()` y `discard()`

`remove()` genera error si el elemento no existe.

```python id="r8m4tv"
valores = {1, 2, 3}
valores.remove(99)
```

Esto genera `KeyError`.

`discard()` no genera error si el elemento no existe.

```python id="m1q7pk"
valores = {1, 2, 3}
valores.discard(99)

print(valores)
```

## Copia de conjuntos

## Referencia compartida

```python id="t5m2qw"
a = {1, 2, 3}
b = a

b.add(4)

print(a)
print(b)
```

Salida:

```text id="n9m6tv"
{1, 2, 3, 4}
{1, 2, 3, 4}
```

No se creó copia. Ambas variables apuntan al mismo conjunto.

## Copia superficial

```python id="p2m8pk"
a = {1, 2, 3}
b = a.copy()

b.add(4)

print(a)
print(b)
```

Salida:

```text id="v7m1qw"
{1, 2, 3}
{1, 2, 3, 4}
```

## No indexación

Un conjunto no admite acceso por índice.

```python id="r4m9tv"
valores = {10, 20, 30}
print(valores[0])
```

Esto genera `TypeError`.

## No slicing

Tampoco admite slicing como lista o tupla.

## Comprensiones de conjunto

Las comprensiones permiten construir conjuntos de forma compacta.

```python id="m6q3pk"
pares = {x for x in range(10) if x % 2 == 0}
print(pares)
```

Salida:

```text id="t8m5qw"
{0, 2, 4, 6, 8}
```

Otro ejemplo:

```python id="n1m9tv"
residuos = {x % 3 for x in range(10)}
print(residuos)
```

Salida:

```text id="p4m2pk"
{0, 1, 2}
```

## Truthiness

En contexto booleano:

* un conjunto vacío se evalúa como `False`
* un conjunto no vacío se evalúa como `True`

```python id="v9m7qw"
print(bool(set()))
print(bool({1}))
```

Salida:

```text id="r2m4tv"
False
True
```

## Casos de uso frecuentes

## Eliminar duplicados

```python id="m7q1pk"
valores = [1, 2, 2, 3, 3, 3]
unicos = set(valores)

print(unicos)
```

## Verificar pertenencia

```python id="t1m6qw"
permitidos = {"admin", "editor", "viewer"}

print("admin" in permitidos)
```

## Operaciones entre colecciones

```python id="n4m8tv"
a = {"python", "sql", "excel"}
b = {"python", "power bi", "excel"}

print(a & b)
print(a | b)
```

## Comparar elementos únicos

```python id="p9m3pk"
usuarios_a = {"Ana", "Luis", "Marta"}
usuarios_b = {"Luis", "Carlos"}

print(usuarios_a - usuarios_b)
```

## Errores comunes

## Usar `{}` pensando que crea un conjunto vacío

Problemático:

```python id="v6m1qw"
vacio = {}
print(type(vacio))
```

Salida:

```text id="r8m5tv"
<class 'dict'>
```

Para conjunto vacío corresponde `set()`.

## Intentar indexar un conjunto

Problemático:

```python id="m2q9pk"
valores = {1, 2, 3}
print(valores[0])
```

Esto genera `TypeError`.

## Suponer que preserva orden posicional como lista

Problemático:

```python id="t7m4qw"
valores = {"a", "b", "c"}
for valor in valores:
    print(valor)
```

El recorrido puede existir, pero no debe interpretarse como orden posicional estable para indexación o lógica secuencial.

## Usar elementos mutables dentro del conjunto

Problemático:

```python id="n8m2tv"
valores = {[1, 2], [3, 4]}
```

Esto genera `TypeError` porque las listas no son hashables.

## Confundir eliminación de duplicados con preservación de orden original

```python id="p3m7pk"
valores = [3, 1, 3, 2, 1]
print(set(valores))
```

Salida posible:

```text id="v5m9qw"
{1, 2, 3}
```

Los duplicados desaparecen, pero el orden original no se conserva como lista ordenada por aparición.

## Buenas prácticas

## Usar `set` cuando la unicidad sea importante

```python id="r1m4tv"
usuarios = {"Ana", "Luis", "Marta"}
```

## Usar `set()` para eliminar duplicados cuando el orden no sea la prioridad

```python id="m6q8pk"
unicos = set(valores)
```

## Usar operaciones de conjuntos cuando expresen mejor la lógica del problema

```python id="t9m1qw"
comunes = habilidades_a & habilidades_b
```

## No usar conjuntos si se necesita indexación o preservación estructural del orden como lista

En esos casos suele corresponder `list` o `tuple`.

## Tener cuidado con el tipo de los elementos

Todos los elementos deben ser hashables.

## Ejemplo integrado

```python id="n2m5tv"
def show_skill_summary(skills_a, skills_b):
    set_a = set(skills_a)
    set_b = set(skills_b)

    print("Resumen de habilidades")
    print("-" * 30)
    print("A:", set_a)
    print("B:", set_b)
    print("Comunes:", set_a & set_b)
    print("Totales:", set_a | set_b)
    print("Solo en A:", set_a - set_b)
    print("Solo en B:", set_b - set_a)


show_skill_summary(
    ["Python", "SQL", "Excel", "Python"],
    ["Excel", "Power BI", "SQL"]
)
```

Salida aproximada:

```text id="p8m7pk"
Resumen de habilidades
------------------------------
A: {'Excel', 'SQL', 'Python'}
B: {'Excel', 'Power BI', 'SQL'}
Comunes: {'Excel', 'SQL'}
Totales: {'Excel', 'SQL', 'Python', 'Power BI'}
Solo en A: {'Python'}
Solo en B: {'Power BI'}
```

## Relación con otros elementos integrados

`set` se relaciona especialmente con:

* `list` y `tuple` como colecciones iterables con otras propiedades
* `dict`, por compartir la necesidad de hashabilidad en claves o elementos
* `len()` para contar elementos únicos
* `in` para pertenencia eficiente
* métodos de conjunto, documentados en la sección correspondiente

## Orden didáctico interno

```text id="v4m1qw"
1. Propósito de set
2. Naturaleza del tipo
3. Literales y construcción con set()
4. Unicidad de elementos
5. Mutabilidad
6. Pertenencia e iteración
7. Operaciones de conjuntos
8. Métodos principales
9. Comprensiones de conjunto
10. Errores comunes
11. Buenas prácticas
```