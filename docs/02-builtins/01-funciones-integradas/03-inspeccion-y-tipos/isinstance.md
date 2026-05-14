# `isinstance()`

## Propósito

`isinstance()` permite verificar si un objeto pertenece a una clase determinada o a alguna de sus subclases. Es una función integrada fundamental para validación de tipos, control de flujo y programación orientada a objetos.

## Forma general

```python
isinstance(obj, classinfo)
````

## Idea central

`isinstance()` responde a la pregunta:

```text
¿Este objeto es instancia de esta clase o de alguna clase compatible dentro de su jerarquía?
```

A diferencia de `type()`, no se limita al tipo exacto del objeto. También considera herencia.

## Argumentos

## `obj`

Es el objeto que se desea verificar.

## `classinfo`

Puede ser:

* una clase
* una tupla de clases

Ejemplos válidos:

```python id="7m5c3n"
isinstance(10, int)
isinstance("Python", str)
isinstance([1, 2, 3], (list, tuple))
```

## Valor de retorno

Devuelve un valor booleano:

* `True` si el objeto pertenece a la clase indicada o a una de sus subclases
* `False` en caso contrario

Ejemplo:

```python id="3olsf9"
print(isinstance(10, int))
print(isinstance("Python", int))
```

Salida:

```text id="zq5nq5"
True
False
```

## Uso con tipos integrados

## Números

```python id="3o37m2"
print(isinstance(10, int))
print(isinstance(3.14, float))
print(isinstance(True, bool))
print(isinstance(2 + 3j, complex))
```

Salida:

```text id="h6k4xm"
True
True
True
True
```

## Texto y binarios

```python id="e9nwsj"
print(isinstance("Hola", str))
print(isinstance(b"Hola", bytes))
print(isinstance(bytearray(b"Hola"), bytearray))
```

Salida:

```text id="sa6ax2"
True
True
True
```

## Colecciones

```python id="lodv54"
print(isinstance([1, 2, 3], list))
print(isinstance((1, 2, 3), tuple))
print(isinstance({1, 2, 3}, set))
print(isinstance({"a": 1}, dict))
```

Salida:

```text id="a6hyad"
True
True
True
True
```

## Verificación contra varias clases

`classinfo` puede ser una tupla de clases.

```python id="k9vfrl"
valor_1 = [1, 2, 3]
valor_2 = (1, 2, 3)
valor_3 = "Python"

print(isinstance(valor_1, (list, tuple)))
print(isinstance(valor_2, (list, tuple)))
print(isinstance(valor_3, (list, tuple)))
```

Salida:

```text id="dcvrkq"
True
True
False
```

Esto es útil cuando distintas clases deben tratarse de la misma manera.

## Diferencia entre `isinstance()` y `type()`

## `type(obj)`

Comprueba el tipo exacto del objeto.

```python id="qzcu21"
print(type(True))
print(type(True) == bool)
print(type(True) == int)
```

Salida:

```text id="nppiy4"
<class 'bool'>
True
False
```

## `isinstance(obj, cls)`

Comprueba si el objeto pertenece a la clase o a una subclase.

```python id="kqqwyd"
print(isinstance(True, bool))
print(isinstance(True, int))
```

Salida:

```text id="8eh3k4"
True
True
```

Esto ocurre porque en Python `bool` es subtipo de `int`.

## Regla práctica

* `type()` sirve para consultar el tipo exacto
* `isinstance()` sirve para validar pertenencia a una clase o jerarquía

## Uso con herencia

La diferencia se aprecia con más claridad en clases personalizadas.

```python id="eh41ud"
class Animal:
    pass

class Dog(Animal):
    pass

dog = Dog()

print(type(dog) == Dog)
print(type(dog) == Animal)
print(isinstance(dog, Dog))
print(isinstance(dog, Animal))
```

Salida:

```text id="hcgrv6"
True
False
True
True
```

`dog` es exactamente de tipo `Dog`, pero también es instancia válida de `Animal` por herencia.

## Uso típico en programación orientada a objetos

`isinstance()` es frecuente cuando se desea aceptar una interfaz o una categoría de objetos, no solo un tipo exacto.

```python id="x8mswu"
class Employee:
    pass

class Manager(Employee):
    pass

def process_employee(employee):
    if not isinstance(employee, Employee):
        raise TypeError("Se esperaba un objeto Employee")

    print("Procesando empleado")

process_employee(Manager())
```

Aquí tiene sentido aceptar instancias de `Manager`, porque también pertenecen a la jerarquía de `Employee`.

## Uso con tipos numéricos

Un punto importante es que algunas relaciones de tipos en Python pueden sorprender.

```python id="jy6b4h"
print(isinstance(True, int))
print(isinstance(False, int))
```

Salida:

```text id="i7lurx"
True
True
```

Esto no significa que `True` y `1` sean el mismo objeto, sino que `bool` hereda de `int`.

## Uso con `None`

```python id="ptfjlwm"
valor = None

print(isinstance(valor, type(None)))
```

Salida:

```text id="namjlwm"
True
```

Aun así, para comparar con `None`, la forma idiomática habitual sigue siendo:

```python id="wtu654"
if valor is None:
    print("No hay valor")
```

## Validación de entradas

`isinstance()` es útil para validar argumentos antes de operar con ellos.

```python id="n9f4gq"
def double(value):
    if not isinstance(value, (int, float)):
        raise TypeError("Se esperaba un número")

    return value * 2

print(double(10))
print(double(3.5))
```

Salida:

```text id="tcrs3f"
20
7.0
```

## Relación con duck typing

Aunque `isinstance()` es útil, Python no siempre exige validación explícita de tipo. En muchos casos importa más el comportamiento que la clase exacta.

Ejemplo con validación explícita:

```python id="pq0e09"
def process_numbers(values):
    if not isinstance(values, list):
        raise TypeError("Se esperaba una lista")

    return sum(values)
```

Ejemplo más flexible:

```python id="ktvjlwm"
def process_numbers(values):
    return sum(values)
```

La segunda versión acepta cualquier iterable compatible con `sum()`, no solo listas.

Por eso, `isinstance()` debe usarse cuando la validación de tipo realmente aporta claridad o seguridad, no como sustituto automático del diseño basado en comportamiento.

## Casos de uso frecuentes

## Validar un tipo único

```python id="u6sscm"
nombre = "Ana"

if isinstance(nombre, str):
    print("Nombre válido")
```

## Validar varios tipos aceptados

```python id="gk2puk"
valor = 10.5

if isinstance(valor, (int, float)):
    print("Es un número real simple")
```

## Validar jerarquías de clases

```python id="s4rjlwm"
class Vehicle:
    pass

class Car(Vehicle):
    pass

car = Car()

print(isinstance(car, Vehicle))
```

## Filtrar elementos por tipo

```python id="m17qka"
valores = [1, "Ana", 3.14, [1, 2], True]

numericos = [v for v in valores if isinstance(v, (int, float))]

print(numericos)
```

Salida:

```text id="ynirjlwm"
[1, 3.14, True]
```

## Errores comunes

## Usar `type()` cuando se necesita considerar herencia

Problemático:

```python id="0o5it8"
class Animal:
    pass

class Dog(Animal):
    pass

dog = Dog()

print(type(dog) == Animal)
```

Salida:

```text id="jlwmfz"
False
```

Si se quiere aceptar también subclases, corresponde usar `isinstance()`.

## Pasar una lista en lugar de una tupla en `classinfo`

Problemático:

```python id="jny2y0"
print(isinstance(10, [int, float]))
```

Esto genera error porque `classinfo` debe ser una clase o una tupla de clases.

Correcto:

```python id="6jlwm2"
print(isinstance(10, (int, float)))
```

## Usar `isinstance()` como sustituto de comprensión del comportamiento

No siempre es necesario validar tipo antes de actuar. En muchos casos resulta mejor escribir código que opere sobre cualquier objeto compatible.

## Suponer que `isinstance(True, int)` debería ser falso

```python id="jlwm4z"
print(isinstance(True, int))
```

Salida:

```text id="fhwjlwm"
True
```

Esto responde al diseño de la jerarquía de tipos en Python.

## Usar `isinstance()` con valores en lugar de clases

Problemático:

```python id="jlwmnw"
print(isinstance(10, 5))
```

Esto genera `TypeError`, porque el segundo argumento debe representar clases, no valores.

## Buenas prácticas

## Usar `isinstance()` cuando se quiera aceptar una jerarquía de tipos

```python id="jlwmc7"
if isinstance(obj, BaseClass):
    ...
```

## Usar una tupla cuando existan varios tipos válidos

```python id="jlwm9q"
if isinstance(valor, (int, float)):
    ...
```

## Preferir `isinstance()` sobre `type(...) == ...` en validaciones orientadas a objetos

```python id="owy131"
if isinstance(animal, Animal):
    ...
```

## No abusar de validaciones rígidas de tipo cuando baste con compatibilidad de comportamiento

En Python, muchas veces conviene permitir objetos compatibles en lugar de restringir innecesariamente el diseño.

## Ejemplo integrado

```python id="jlwm67"
class Employee:
    def __init__(self, name):
        self.name = name


class Manager(Employee):
    def approve(self):
        return "Aprobado"


def register_employee(employee):
    if not isinstance(employee, Employee):
        raise TypeError("Se esperaba una instancia de Employee")

    print(f"Empleado registrado: {employee.name}")


def describe_value(value):
    print("Valor:", value)
    print("Tipo exacto:", type(value))
    print("Es texto:", isinstance(value, str))
    print("Es numérico simple:", isinstance(value, (int, float)))
    print("-" * 30)


register_employee(Manager("Ana"))

describe_value("Python")
describe_value(10)
describe_value([1, 2, 3])
```

## Relación con otros elementos integrados

`isinstance()` se relaciona especialmente con:

* `type()` para inspección de tipo exacto
* `issubclass()` en el contexto de clases y herencia
* `dir()` y `help()` para introspección
* el modelo de objetos y la programación orientada a objetos
* tipos integrados y clases personalizadas

## Orden didáctico interno

```text id="9jlwm4"
1. Propósito de isinstance()
2. Forma general
3. Argumentos y valor de retorno
4. Uso con tipos integrados
5. Verificación contra múltiples clases
6. Diferencia entre isinstance() y type()
7. Uso con herencia
8. Casos de validación
9. Relación con duck typing
10. Errores comunes
11. Buenas prácticas
```