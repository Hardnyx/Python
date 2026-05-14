# `type()`

## Propósito

`type()` permite consultar el tipo de un objeto. Es una función integrada fundamental para inspección, depuración, validación y comprensión del modelo de objetos de Python.

## Forma general

`type()` tiene dos formas principales de uso:

```python
type(obj)
type(nombre, bases, dict)
````

En esta documentación se prioriza la primera, que es la forma de uso habitual en programación general.

## `type(obj)`

Devuelve el tipo del objeto recibido.

```python id="8ft5jj"
print(type(10))
print(type("Python"))
print(type([1, 2, 3]))
```

Salida:

```text id="l5qjkl"
<class 'int'>
<class 'str'>
<class 'list'>
```

## Valor de retorno

El valor retornado por `type(obj)` es un objeto de tipo clase.

```python id="1vny11"
resultado = type(10)

print(resultado)
print(type(resultado))
```

Salida:

```text id="1tuu5g"
<class 'int'>
<class 'type'>
```

Esto refleja que en Python las clases también son objetos.

## Idea central

`type(obj)` responde a la pregunta:

```text id="cq5rhe"
¿De qué tipo es este objeto?
```

Es útil para:

* inspeccionar valores durante pruebas o depuración
* comprender estructuras de datos
* explorar resultados intermedios
* verificar tipos concretos en ciertos contextos

## Uso con tipos integrados

## Números

```python id="dr0gxq"
print(type(10))
print(type(3.14))
print(type(True))
print(type(2 + 3j))
```

Salida:

```text id="xpkfjg"
<class 'int'>
<class 'float'>
<class 'bool'>
<class 'complex'>
```

## Texto y binarios

```python id="fzwwvf"
print(type("Hola"))
print(type(b"Hola"))
print(type(bytearray(b"Hola")))
```

Salida:

```text id="xlo1qq"
<class 'str'>
<class 'bytes'>
<class 'bytearray'>
```

## Colecciones

```python id="xy6fso"
print(type([1, 2, 3]))
print(type((1, 2, 3)))
print(type({1, 2, 3}))
print(type({"a": 1}))
```

Salida:

```text id="vjlwm8"
<class 'list'>
<class 'tuple'>
<class 'set'>
<class 'dict'>
```

## Valores especiales

```python id="6oqj9x"
print(type(None))
print(type(range(5)))
```

Salida:

```text id="ei7y4m"
<class 'NoneType'>
<class 'range'>
```

## Uso con objetos personalizados

`type()` también funciona con instancias de clases definidas por el usuario.

```python id="3ouo71"
class Person:
    pass

person = Person()

print(type(person))
```

Salida:

```text id="ukjlwm"
<class '__main__.Person'>
```

## Uso con clases

Una clase también es un objeto. Por eso puede pasarse a `type()`.

```python id="jlwm7w"
class Person:
    pass

print(type(Person))
```

Salida:

```text id="jlwm6n"
<class 'type'>
```

Esto indica que `Person` es una clase y que las clases son instancias de `type`.

## Relación con el modelo de objetos

`type()` conecta directamente con el modelo de objetos de Python:

* todo valor es un objeto
* todo objeto tiene un tipo
* las clases también son objetos

Ejemplo:

```python id="9az6v4"
x = [1, 2, 3]

print(type(x))
print(type(type(x)))
```

Salida:

```text id="jlwmqd"
<class 'list'>
<class 'type'>
```

## Diferencia entre `type()` e `isinstance()`

Aunque ambas se usan para trabajar con tipos, no responden exactamente a la misma pregunta.

## `type(obj)`

Indica el tipo exacto del objeto.

```python id="v34ard"
print(type(True))
```

Salida:

```text id="jlwm30"
<class 'bool'>
```

## `isinstance(obj, cls)`

Indica si un objeto pertenece a una clase o a alguna de sus subclases.

```python id="3k2m4s"
print(isinstance(True, bool))
print(isinstance(True, int))
```

Salida:

```text id="jlwmj7"
True
True
```

Esto ocurre porque `bool` es subtipo de `int` en Python.

En cambio:

```python id="y13q4n"
print(type(True) == int)
print(type(True) == bool)
```

Salida:

```text id="2jlwm5"
False
True
```

## Cuándo usar `type()`

`type()` conviene cuando interesa conocer el tipo exacto del objeto.

Ejemplo:

```python id="jlwmdd"
valor = {"nombre": "Ana"}

print(type(valor))
```

También es útil en depuración:

```python id="1lc1jm"
resultado = input("Ingrese un valor: ")
print(type(resultado))
```

Salida esperada:

```text id="5hlobo"
<class 'str'>
```

## Cuándo no conviene usar `type()` como validación principal

Para validación de comportamiento o compatibilidad, muchas veces resulta más apropiado `isinstance()`.

Menos flexible:

```python id="9i77tv"
if type(valor) == list:
    print("Es una lista")
```

Más flexible:

```python id="8arx3b"
if isinstance(valor, list):
    print("Es una lista")
```

La segunda forma es más adecuada cuando se consideran jerarquías de herencia.

## Comparación exacta con `type()`

Cuando se necesita comparar con un tipo concreto, puede usarse:

```python id="jlwm9s"
valor = [1, 2, 3]

print(type(valor) == list)
```

Salida:

```text id="jlwmh6"
True
```

Esto verifica igualdad exacta de tipo.

## `type()` con funciones

Las funciones también son objetos.

```python id="5nnq1g"
def greet():
    pass

print(type(greet))
```

Salida:

```text id="ii7j0x"
<class 'function'>
```

## `type()` con módulos

Si se inspecciona un módulo importado:

```python id="zl6n7q"
import math

print(type(math))
```

Salida:

```text id="jlwm3w"
<class 'module'>
```

## `type()` y objetos iterables

No todos los iterables tienen el mismo tipo.

```python id="n54kmu"
print(type([1, 2, 3]))
print(type((1, 2, 3)))
print(type(range(5)))
print(type(x for x in range(5)))
```

Salida:

```text id="gnmqvx"
<class 'list'>
<class 'tuple'>
<class 'range'>
<class 'generator'>
```

Esto ayuda a distinguir objetos que pueden parecer similares desde fuera pero tienen comportamiento distinto.

## `type()` y resultados intermedios

Es frecuente usar `type()` para explorar el resultado de una operación.

```python id="rxawum"
datos = {"a": 1, "b": 2}

print(type(datos.keys()))
print(type(datos.values()))
print(type(datos.items()))
```

Salida posible:

```text id="jlwm2v"
<class 'dict_keys'>
<class 'dict_values'>
<class 'dict_items'>
```

## `type(nombre, bases, dict)`

`type()` también puede usarse para crear clases dinámicamente.

Forma general:

```python id="jlwm7z"
type(nombre_de_clase, clases_base, atributos_y_metodos)
```

Ejemplo:

```python id="jlwmqa"
Person = type("Person", (), {"species": "Human"})

person = Person()

print(type(person))
print(Person.species)
```

Salida:

```text id="9jlwmx"
<class '__main__.Person'>
Human
```

Esta forma existe, pero no es la vía normal para definir clases en programación cotidiana. La forma habitual sigue siendo `class`.

## Errores comunes

## Confundir `type()` con `isinstance()`

Problemático:

```python id="jlwmj0"
if type(valor) == int:
    ...
```

Esto solo acepta el tipo exacto `int`. No considera subtipos.

Si lo que se quiere es verificar pertenencia a una clase o jerarquía, suele corresponder `isinstance()`.

## Usar `type()` como reemplazo de comprensión del objeto

Saber el tipo exacto de un objeto no siempre basta para comprender su comportamiento. En muchos casos también interesa:

* qué métodos tiene
* si es iterable
* si es invocable
* si soporta determinada operación

Para eso puede ser útil combinar con `dir()` o `help()`.

## Suponer que `type()` devuelve una cadena

Problemático:

```python id="jlwmq4"
resultado = type(10)
print(resultado == "int")
```

Salida:

```text id="35jlwm"
False
```

`type(10)` devuelve la clase `int`, no la cadena `"int"`.

Correcto:

```python id="jlwmi1"
print(type(10) == int)
```

## Usar `type()` para imprimir información sin contexto

Menos útil:

```python id="qjlwm6"
print(type(valor))
```

Más informativo:

```python id="8wln3o"
print("Tipo de valor:", type(valor))
```

## Buenas prácticas

## Usar `type()` para inspección y depuración

```python id="8upvxd"
resultado = [1, 2, 3]
print(type(resultado))
```

## Usar `isinstance()` cuando se quiera compatibilidad por jerarquía

```python id="m2nmxu"
if isinstance(valor, dict):
    print("Es un mapeo tipo dict")
```

## No abusar de comparaciones explícitas de tipo cuando baste con el comportamiento

En Python muchas veces importa más lo que el objeto puede hacer que su tipo exacto.

## Combinar `type()` con otras herramientas de introspección cuando sea necesario

```python id="jlwm48"
valor = {"a": 1}
print(type(valor))
print(dir(valor))
```

## Ejemplo integrado

```python id="x90yv1"
def describe_value(value):
    print("Valor:", value)
    print("Tipo exacto:", type(value))

    if isinstance(value, (list, tuple, set, dict, str, bytes, bytearray, range)):
        print("Longitud:", len(value))

    print("-" * 30)


describe_value("Python")
describe_value([1, 2, 3])
describe_value({"nombre": "Ana"})
describe_value(3.14)
```

Salida aproximada:

```text id="48q3db"
Valor: Python
Tipo exacto: <class 'str'>
Longitud: 6
------------------------------
Valor: [1, 2, 3]
Tipo exacto: <class 'list'>
Longitud: 3
------------------------------
Valor: {'nombre': 'Ana'}
Tipo exacto: <class 'dict'>
Longitud: 1
------------------------------
Valor: 3.14
Tipo exacto: <class 'float'>
------------------------------
```

## Relación con otros elementos integrados

`type()` se relaciona especialmente con:

* `isinstance()` para validación de tipos
* `dir()` para explorar atributos del objeto
* `help()` para consultar documentación
* tipos integrados como `int`, `str`, `list`, `dict` y `set`
* el modelo de objetos del lenguaje

## Orden didáctico interno

```text id="2krjlwm"
1. Propósito de type()
2. Forma general
3. type(obj) como inspección del tipo exacto
4. Uso con tipos integrados
5. Uso con objetos personalizados
6. Relación con el modelo de objetos
7. Diferencia entre type() e isinstance()
8. Uso con funciones, módulos e iterables
9. type() como constructor dinámico de clases
10. Errores comunes
11. Buenas prácticas
```