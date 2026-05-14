# `frozenset`

## Propósito

`frozenset` representa una colección inmutable de elementos únicos sin orden posicional. Se utiliza cuando se necesita la lógica de conjuntos, como pertenencia, unión, intersección y diferencia, pero sin permitir modificaciones posteriores sobre la estructura.

## Naturaleza del tipo

`frozenset` es un tipo de colección, inmutable y no indexado.

Esto significa que:

- almacena elementos sin duplicados
- no tiene acceso posicional por índice
- no preserva un orden posicional utilizable como una lista o tupla
- no puede modificarse después de crearse
- sus elementos deben ser hashables
- el propio `frozenset` puede ser hashable y usarse como clave de diccionario o como elemento de otro conjunto

## Forma general

Un objeto `frozenset` se construye con:

```python id="5lqdry"
frozenset()
frozenset(iterable)
````

A diferencia de `set`, no existe una sintaxis literal específica para `frozenset`.

## Construcción básica

## Vacío

```python id="9afdv4"
valores = frozenset()

print(valores)
print(type(valores))
```

Salida:

```text id="mio6ti"
frozenset()
<class 'frozenset'>
```

## Desde lista

```python id="gi1m0r"
valores = frozenset([1, 2, 2, 3, 3])

print(valores)
```

Salida posible:

```text id="jlwmbu"
frozenset({1, 2, 3})
```

## Desde tupla

```python id="xswpne"
valores = frozenset((1, 1, 2, 3))

print(valores)
```

Salida posible:

```text id="yn5k53"
frozenset({1, 2, 3})
```

## Desde cadena

```python id="j7pgio"
valores = frozenset("Hola")

print(valores)
```

Salida posible:

```text id="ojb0h4"
frozenset({'H', 'o', 'l', 'a'})
```

## Desde diccionario

Si se aplica sobre un diccionario, toma sus claves.

```python id="ep8vps"
datos = {"nombre": "Ana", "edad": 20}

valores = frozenset(datos)
print(valores)
```

Salida posible:

```text id="kwf3gi"
frozenset({'nombre', 'edad'})
```

## Valor de retorno de `type()`

```python id="ewkte6"
print(type(frozenset([1, 2, 3])))
```

Salida:

```text id="niepko"
<class 'frozenset'>
```

## Inmutabilidad

`frozenset` es inmutable.

```python id="z4q0xw"
valores = frozenset([1, 2, 3])
print(id(valores))

valores = frozenset([1, 2, 3, 4])
print(valores)
print(id(valores))
```

Salida:

```text id="7kj80n"
frozenset({1, 2, 3, 4})
```

La identidad cambia porque no se modifica el objeto original. Se crea otro nuevo.

No existen métodos mutables como `add()`, `remove()`, `discard()` o `clear()`.

## Unicidad de elementos

Un `frozenset` no permite duplicados.

```python id="uuzwln"
valores = frozenset([1, 2, 2, 3, 3, 3])

print(valores)
```

Salida posible:

```text id="294m4v"
frozenset({1, 2, 3})
```

## Elementos hashables

Los elementos de un `frozenset` deben ser hashables.

Ejemplos válidos:

```python id="tndigb"
valores = frozenset([1, "Ana", (1, 2), True])

print(valores)
```

Ejemplos inválidos:

```python id="oy3a7n"
valores = frozenset([[1, 2], [3, 4]])
```

Esto genera error porque las listas no son hashables.

## Longitud

La longitud se obtiene con `len()`.

```python id="84dtql"
print(len(frozenset([1, 2, 3])))
print(len(frozenset()))
```

Salida:

```text id="dtdi6q"
3
0
```

## Pertenencia

El operador `in` es una de las utilidades principales de `frozenset`.

```python id="dgp95g"
valores = frozenset([10, 20, 30])

print(20 in valores)
print(99 in valores)
```

Salida:

```text id="8lz11u"
True
False
```

## Iteración

Un `frozenset` es iterable.

```python id="t0j0ne"
nombres = frozenset(["Ana", "Luis", "Marta"])

for nombre in nombres:
    print(nombre)
```

Salida posible:

```text id="eigxvs"
Luis
Ana
Marta
```

El orden de iteración no debe interpretarse como orden posicional fijo.

## No indexación

Un `frozenset` no admite acceso por índice.

```python id="jlwm8s"
valores = frozenset([10, 20, 30])
print(valores[0])
```

Esto genera `TypeError`.

## No slicing

Tampoco admite slicing como una secuencia posicional.

## Operaciones de conjuntos

`frozenset` soporta las operaciones típicas de conjuntos.

## Unión

```python id="eu15fe"
a = frozenset([1, 2, 3])
b = frozenset([3, 4, 5])

print(a | b)
```

Salida posible:

```text id="n0vbxt"
frozenset({1, 2, 3, 4, 5})
```

## Intersección

```python id="jlwm0s"
a = frozenset([1, 2, 3])
b = frozenset([3, 4, 5])

print(a & b)
```

Salida:

```text id="jlwm9t"
frozenset({3})
```

## Diferencia

```python id="jlwm1s"
a = frozenset([1, 2, 3])
b = frozenset([3, 4, 5])

print(a - b)
print(b - a)
```

Salida posible:

```text id="jlwm7s"
frozenset({1, 2})
frozenset({4, 5})
```

## Diferencia simétrica

```python id="jlwm3s"
a = frozenset([1, 2, 3])
b = frozenset([3, 4, 5])

print(a ^ b)
```

Salida posible:

```text id="jlwm4s"
frozenset({1, 2, 4, 5})
```

## Comparación entre conjuntos

## Subconjunto

```python id="jlwm5s"
a = frozenset([1, 2])
b = frozenset([1, 2, 3])

print(a <= b)
print(a < b)
```

Salida:

```text id="jlwm6s"
True
True
```

## Superconjunto

```python id="jlwm2t"
a = frozenset([1, 2, 3])
b = frozenset([1, 2])

print(a >= b)
print(a > b)
```

Salida:

```text id="jlwm3t"
True
True
```

## Igualdad

```python id="jlwm4t"
print(frozenset([1, 2, 3]) == frozenset([3, 2, 1]))
```

Salida:

```text id="jlwm5t"
True
```

El orden no importa en la comparación.

## Relación con `set`

`set` y `frozenset` comparten la lógica de conjuntos, pero difieren en mutabilidad.

## `set`

* mutable
* permite agregar y eliminar elementos

## `frozenset`

* inmutable
* no permite modificaciones después de la creación

Ejemplo comparativo:

```python id="jlwm6t"
a = set([1, 2, 3])
b = frozenset([1, 2, 3])

print(type(a))
print(type(b))
```

Salida:

```text id="jlwm7t"
<class 'set'>
<class 'frozenset'>
```

## Hashabilidad

Una diferencia muy importante es que `frozenset` puede ser hashable si sus elementos también lo son.

Esto permite usarlo como clave de diccionario:

```python id="jlwm8t"
datos = {
    frozenset([1, 2]): "par pequeño",
    frozenset([3, 4]): "otro par"
}

print(datos[frozenset([1, 2])])
```

Salida:

```text id="jlwm9u"
par pequeño
```

También permite usarlo como elemento dentro de un `set`:

```python id="jlwm0u"
valores = {frozenset([1, 2]), frozenset([3, 4])}

print(valores)
```

Salida posible:

```text id="jlwm1u"
{frozenset({3, 4}), frozenset({1, 2})}
```

Esto no sería posible con `set`, porque `set` no es hashable.

## Métodos principales

`frozenset` comparte varios métodos de consulta y operaciones con `set`, pero no los mutadores.

Ejemplos frecuentes:

```python id="jlwm2u"
a = frozenset([1, 2, 3])
b = frozenset([3, 4, 5])

print(a.union(b))
print(a.intersection(b))
print(a.difference(b))
print(a.symmetric_difference(b))
print(a.isdisjoint(frozenset([7, 8])))
```

Salida posible:

```text id="jlwm3u"
frozenset({1, 2, 3, 4, 5})
frozenset({3})
frozenset({1, 2})
frozenset({1, 2, 4, 5})
True
```

Los métodos se documentan aparte en la sección correspondiente de métodos de tipos.

## Truthiness

En contexto booleano:

* un `frozenset` vacío se evalúa como `False`
* un `frozenset` no vacío se evalúa como `True`

```python id="jlwm4u"
print(bool(frozenset()))
print(bool(frozenset([1])))
```

Salida:

```text id="jlwm5u"
False
True
```

## Casos de uso frecuentes

## Representar colecciones fijas de elementos únicos

```python id="jlwm6u"
roles = frozenset(["admin", "editor", "viewer"])

print(roles)
```

## Usar conjuntos como claves

```python id="jlwm7u"
configuraciones = {
    frozenset(["python", "sql"]): "perfil analítico",
    frozenset(["excel", "power bi"]): "perfil reporting"
}

print(configuraciones[frozenset(["python", "sql"])])
```

## Trabajar con teoría de conjuntos sin riesgo de modificación

```python id="jlwm8u"
a = frozenset([1, 2, 3])
b = frozenset([3, 4, 5])

print(a & b)
print(a | b)
```

## Errores comunes

## Suponer que `frozenset` se crea con llaves

Problemático:

```python id="jlwm9v"
valores = {1, 2, 3}
print(type(valores))
```

Salida:

```text id="jlwm0v"
<class 'set'>
```

Las llaves crean `set`, no `frozenset`.

## Intentar modificar un `frozenset`

Problemático:

```python id="jlwm1v"
valores = frozenset([1, 2, 3])
valores.add(4)
```

Esto genera `AttributeError`, porque `frozenset` no tiene métodos mutadores como `add()`.

## Usar elementos no hashables

Problemático:

```python id="jlwm2v"
frozenset([[1, 2], [3, 4]])
```

Esto genera `TypeError`.

## Confundir `frozenset` con secuencia indexable

Problemático:

```python id="jlwm3v"
valores = frozenset([10, 20, 30])
print(valores[0])
```

Esto genera `TypeError`.

## Buenas prácticas

## Usar `frozenset` cuando la colección deba permanecer fija

```python id="jlwm4v"
permisos = frozenset(["leer", "escribir"])
```

## Usarlo cuando se necesite hashabilidad de un conjunto

```python id="jlwm5v"
clave = frozenset(["python", "sql"])
```

## Preferir `set` si se requiere agregar o eliminar elementos

Cuando se necesite mutabilidad, suele corresponder `set`, no `frozenset`.

## No usarlo si se necesita orden o acceso posicional

En esos casos conviene considerar `list` o `tuple`.

## Ejemplo integrado

```python id="jlwm6v"
def show_permission_summary(user_permissions):
    permisos = frozenset(user_permissions)
    requeridos = frozenset(["leer", "escribir"])

    print("Resumen de permisos")
    print("-" * 30)
    print("Permisos del usuario:", permisos)
    print("Cantidad:", len(permisos))
    print("Tiene 'leer':", "leer" in permisos)
    print("Cumple mínimos:", requeridos <= permisos)
    print("Clave hashable:", hash(permisos))


show_permission_summary(["leer", "escribir", "leer"])
```

Salida aproximada:

```text id="jlwm7v"
Resumen de permisos
------------------------------
Permisos del usuario: frozenset({'leer', 'escribir'})
Cantidad: 2
Tiene 'leer': True
Cumple mínimos: True
Clave hashable: ...
```

## Relación con otros elementos integrados

`frozenset` se relaciona especialmente con:

* `set`, como variante mutable de colección sin duplicados
* `dict`, por la importancia de la hashabilidad
* `len()` para contar elementos únicos
* `in` para pertenencia eficiente
* métodos de conjuntos, documentados en la sección correspondiente

## Orden didáctico interno

```text id="jlwm8v"
1. Propósito de frozenset
2. Naturaleza del tipo
3. Construcción con frozenset()
4. Unicidad de elementos
5. Inmutabilidad
6. Operaciones de conjuntos
7. Relación con set
8. Hashabilidad
9. Errores comunes
10. Buenas prácticas
```