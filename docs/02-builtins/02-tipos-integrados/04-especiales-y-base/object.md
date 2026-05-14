# `object`

## Propósito

`object` representa la clase base más general del modelo de objetos de Python. Su importancia principal no está en el uso cotidiano de instancias “vacías”, sino en que sirve como raíz de la jerarquía de clases y permite entender que en Python todo valor es un objeto.

## Naturaleza del tipo

`object` es el tipo base fundamental del sistema de objetos.

Esto significa que:

- todas las clases nuevas heredan directa o indirectamente de `object`
- cualquier valor en Python es, en sentido amplio, un objeto
- `object()` permite crear una instancia mínima, sin comportamiento de dominio específico
- su utilidad es más conceptual y estructural que práctica en código diario

## Forma general

Una instancia básica puede construirse con:

```python id="u2m7pk"
object()
````

## Construcción básica

```python id="r8m4tv"
valor = object()

print(valor)
print(type(valor))
```

Salida posible:

```text id="n1q6rw"
<object object at 0x...>
<class 'object'>
```

La representación exacta en memoria puede variar.

## Valor de retorno de `type()`

```python id="p4m9qw"
print(type(object()))
```

Salida:

```text id="v7m2pk"
<class 'object'>
```

## Relación con `type`

La clase `object` es un objeto de tipo `type`.

```python id="r2m5tv"
print(type(object))
```

Salida:

```text id="m8q1rw"
<class 'type'>
```

Esto ayuda a entender dos ideas del modelo de objetos de Python:

* `object` es una clase
* las clases también son objetos

## `object` como raíz de la jerarquía

En Python 3, las clases definidas por el usuario heredan de `object`, directa o indirectamente.

```python id="t5m8pk"
class Persona:
    pass

print(isinstance(Persona(), object))
print(issubclass(Persona, object))
```

Salida:

```text id="n4m2qw"
True
True
```

Esto también ocurre con tipos integrados:

```python id="p9m3tv"
print(isinstance(10, object))
print(isinstance("Hola", object))
print(isinstance([1, 2, 3], object))
print(isinstance({"a": 1}, object))
```

Salida:

```text id="v6m1pk"
True
True
True
True
```

## Idea central

La afirmación “todo en Python es un objeto” puede entenderse mejor observando que distintos valores pertenecen a clases, y todas esas clases se insertan en una jerarquía que desemboca en `object`.

```python id="r8m5qw"
print(isinstance(10, object))
print(isinstance(3.14, object))
print(isinstance(True, object))
print(isinstance(None, object))
```

Salida:

```text id="m2q9tv"
True
True
True
True
```

## Instancias mínimas

Una instancia de `object()` no contiene datos de dominio ni métodos de trabajo habituales como una lista, un diccionario o una cadena.

```python id="t5m2pk"
valor = object()

print(valor)
print(type(valor))
```

Ese objeto existe principalmente como instancia genérica mínima.

## Identidad

Cada llamada a `object()` crea una instancia distinta.

```python id="n9m6qw"
a = object()
b = object()

print(a is b)
print(a == b)
```

Salida:

```text id="p2m8pk"
False
False
```

Dos instancias distintas de `object()` no son el mismo objeto.

## Comparación

Las instancias básicas de `object` no se usan normalmente para comparación de contenido. En la práctica, dos instancias distintas se consideran distintas.

```python id="v7m1qw"
a = object()
b = object()
c = a

print(a == b)
print(a == c)
print(a is c)
```

Salida:

```text id="r4m9tv"
False
True
True
```

## Truthiness

Una instancia de `object()` se evalúa como verdadera en contexto booleano.

```python id="m6q3pk"
valor = object()

print(bool(valor))
```

Salida:

```text id="t8m5qw"
True
```

## Representación textual

La representación por defecto de un objeto base incluye el nombre del tipo y una referencia de memoria.

```python id="n1m9tv"
valor = object()
print(valor)
print(repr(valor))
```

Salida posible:

```text id="p4m2pk"
<object object at 0x...>
<object object at 0x...>
```

Esa representación suele cambiar cuando una clase personalizada redefine `__str__` o `__repr__`.

## Atributos en una instancia básica de `object`

Una instancia simple de `object()` no sirve como contenedor libre de atributos definidos arbitrariamente.

Problemático:

```python id="v9m7qw"
valor = object()
valor.nombre = "Ana"
```

Esto genera `AttributeError`.

Para almacenar atributos propios, normalmente corresponde definir una clase.

```python id="r2m4tv"
class Persona:
    pass

persona = Persona()
persona.nombre = "Ana"

print(persona.nombre)
```

## Métodos heredados desde `object`

Muchas clases reciben comportamiento base desde `object`, especialmente a través de métodos especiales como:

* representación por defecto
* comparación básica
* identidad como objeto
* verdad lógica por defecto

No se suelen invocar directamente sobre `object`, pero forman parte de la base del modelo orientado a objetos.

## Uso en herencia explícita

En Python 3 no es obligatorio escribir `object` al definir clases simples, pero puede aparecer en explicaciones o código con fines didácticos.

```python id="m7q1pk"
class Persona(object):
    pass
```

y

```python id="t1m6qw"
class Persona:
    pass
```

son equivalentes en lo esencial dentro de Python 3.

## `object()` sin argumentos

`object()` no acepta argumentos.

```python id="n4m8tv"
valor = object("hola")
```

Esto genera `TypeError`.

La instancia básica de `object` es deliberadamente mínima.

## Casos de uso frecuentes

En código cotidiano, `object` no suele usarse para crear instancias funcionales. Sus apariciones más comunes son estas:

## Entender jerarquía de clases

```python id="p9m3pk"
class Producto:
    pass

print(issubclass(Producto, object))
```

## Verificar que un valor es un objeto

```python id="v6m1qw"
print(isinstance("Hola", object))
```

## Base conceptual para POO

```python id="r8m5tv"
class CuentaBancaria:
    pass
```

Esta clase hereda finalmente de `object`, aunque no se escriba explícitamente.

## Errores comunes

## Pensar que `object()` sirve como estructura flexible de datos

Problemático:

```python id="m2q9pk"
valor = object()
valor.nombre = "Ana"
```

Una instancia básica de `object` no está pensada para almacenar atributos arbitrarios.

## Confundir `object` con `type`

Problemático:

```python id="t5m2qw"
print(type(object()))
print(type(object))
```

Salida:

```text id="n9m6tv"
<class 'object'>
<class 'type'>
```

* `object()` crea una instancia de `object`
* `object` es una clase
* la clase `object` es de tipo `type`

## Pensar que `object` es un tipo de datos de uso práctico cotidiano

Aunque es fundamental para el lenguaje, no cumple el mismo papel operativo diario que `list`, `dict`, `str` o `int`.

## Buenas prácticas

## Usar `object` principalmente como concepto estructural

Su valor principal está en comprender el sistema de tipos y herencia.

## No usar `object()` como sustituto de clases propias

Si se necesita una estructura con atributos o comportamiento, conviene definir una clase.

## Entender su relación con `isinstance()` e `issubclass()`

```python id="p2m8pk"
print(isinstance([], object))
print(issubclass(list, object))
```

## Usarlo para comprender que todo valor en Python participa del modelo de objetos

Eso ayuda a entender mejor clases, instancias, métodos especiales y herencia.

## Ejemplo integrado

```python id="v7m1qw"
class Persona:
    pass


def show_object_summary(value):
    print("Resumen de objeto")
    print("-" * 30)
    print("Valor:", value)
    print("Tipo exacto:", type(value))
    print("Es objeto:", isinstance(value, object))
    print("Clase hereda de object:", issubclass(type(value), object))


show_object_summary(Persona())
show_object_summary(10)
show_object_summary("Hola")
```

Salida aproximada:

```text id="r4m9tv"
Resumen de objeto
------------------------------
Valor: <__main__.Persona object at 0x...>
Tipo exacto: <class '__main__.Persona'>
Es objeto: True
Clase hereda de object: True
Resumen de objeto
------------------------------
Valor: 10
Tipo exacto: <class 'int'>
Es objeto: True
Clase hereda de object: True
Resumen de objeto
------------------------------
Valor: Hola
Tipo exacto: <class 'str'>
Es objeto: True
Clase hereda de object: True
```

## Relación con otros elementos integrados

`object` se relaciona especialmente con:

* `type`, porque `object` es una clase y su tipo es `type`
* `isinstance()` e `issubclass()` para verificar pertenencia e herencia
* clases definidas por el usuario, que heredan de `object`
* métodos especiales, porque muchos comportamientos base parten de esta clase raíz

## Orden didáctico interno

```text id="m7q2pk"
1. Propósito de object
2. Naturaleza del tipo
3. Construcción con object()
4. Relación con type
5. object como raíz de la jerarquía
6. Identidad y comparación
7. Truthiness y representación
8. Limitaciones prácticas de object()
9. Errores comunes
10. Buenas prácticas
```