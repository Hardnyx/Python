# Decoradores y context managers

## Propósito

Los decoradores y los context managers permiten abstraer patrones repetitivos de comportamiento. Los decoradores actúan sobre funciones o clases para extender o modificar su comportamiento sin alterar directamente su código interno. Los context managers permiten controlar la entrada y salida de un bloque de ejecución de forma estructurada, especialmente cuando intervienen recursos que deben abrirse, cerrarse, reservarse o liberarse correctamente.

## Alcance

Este tema cubre:

- funciones de orden superior
- decoradores
- sintaxis con `@`
- cierres aplicados a decoradores
- preservación de argumentos y resultados
- composición de decoradores
- `with`
- protocolo de contexto
- `__enter__`
- `__exit__`
- relación entre excepciones y context managers
- buenas prácticas de diseño

## Ideas fundamentales

1. Un decorador recibe una función o clase y devuelve otra función o clase.
2. La sintaxis `@decorador` es azúcar sintáctico.
3. Un decorador suele envolver una función original dentro de otra.
4. Para envolver correctamente una función, el envoltorio debe aceptar argumentos flexibles.
5. Un context manager controla lo que ocurre al entrar y salir de un bloque `with`.
6. `with` garantiza la ejecución del protocolo de salida incluso si ocurre una excepción.
7. Los context managers son adecuados para manejar recursos y estados temporales.
8. Ambos mecanismos existen para encapsular comportamiento transversal sin duplicación excesiva.

## Funciones de orden superior

Una función de orden superior es una función que hace al menos una de estas dos cosas:

1. recibe otra función como argumento
2. devuelve una función como resultado

Ejemplo de función que recibe otra función:

```python id="xpehfa"
def apply_operation(a, b, operation):
    return operation(a, b)

def add(a, b):
    return a + b

print(apply_operation(3, 4, add))
````

Salida:

```text id="c50naf"
7
```

Ejemplo de función que devuelve otra función:

```python id="i6p04q"
def create_multiplier(factor):
    def multiplier(value):
        return value * factor

    return multiplier

double = create_multiplier(2)
print(double(10))
```

Salida:

```text id="yhgqdi"
20
```

Los decoradores se apoyan directamente en esta propiedad del lenguaje.

## Qué es un decorador

Un decorador es una función que recibe otra función y devuelve una nueva función, normalmente con comportamiento adicional.

Forma conceptual:

```python id="85i0ul"
def decorador(func):
    def envoltura(*args, **kwargs):
        ...
        return func(*args, **kwargs)

    return envoltura
```

## Primer ejemplo de decorador

```python id="7vdwax"
def simple_decorator(func):
    def wrapper():
        print("Antes de ejecutar la función")
        func()
        print("Después de ejecutar la función")

    return wrapper


def greet():
    print("Hola")


greet = simple_decorator(greet)
greet()
```

Salida:

```text id="7bma2l"
Antes de ejecutar la función
Hola
Después de ejecutar la función
```

Aquí la función original `greet` fue reemplazada por la función devuelta por `simple_decorator`.

## Sintaxis con `@`

La sintaxis con `@` equivale a reasignar manualmente la función decorada.

Ejemplo anterior, forma decorada:

```python id="nd6nrz"
def simple_decorator(func):
    def wrapper():
        print("Antes de ejecutar la función")
        func()
        print("Después de ejecutar la función")

    return wrapper


@simple_decorator
def greet():
    print("Hola")


greet()
```

Salida:

```text id="ezb8op"
Antes de ejecutar la función
Hola
Después de ejecutar la función
```

Equivalencia conceptual:

```python id="wydswt"
@simple_decorator
def greet():
    print("Hola")
```

equivale a:

```python id="7h7a3m"
def greet():
    print("Hola")

greet = simple_decorator(greet)
```

## Decoradores y argumentos

Un decorador simple como el anterior falla si la función decorada recibe argumentos.

Problemático:

```python id="9bfjca"
def simple_decorator(func):
    def wrapper():
        print("Antes")
        result = func()
        print("Después")
        return result

    return wrapper


@simple_decorator
def add(a, b):
    return a + b


print(add(2, 3))
```

Esto genera error porque `wrapper()` no acepta argumentos.

## Uso de `*args` y `**kwargs`

Para envolver funciones de forma general, la función envoltorio debe aceptar cualquier combinación de argumentos posicionales y nombrados.

```python id="o9ky6v"
def log_decorator(func):
    def wrapper(*args, **kwargs):
        print("Llamada iniciada")
        result = func(*args, **kwargs)
        print("Llamada finalizada")
        return result

    return wrapper


@log_decorator
def add(a, b):
    return a + b


print(add(2, 3))
```

Salida:

```text id="ivqyn8"
Llamada iniciada
Llamada finalizada
5
```

Este patrón es el más común en decoradores generales.

## Decoradores que modifican el resultado

Un decorador puede intervenir sobre el valor retornado.

```python id="adlc2r"
def uppercase_decorator(func):
    def wrapper(*args, **kwargs):
        result = func(*args, **kwargs)
        return result.upper()

    return wrapper


@uppercase_decorator
def greet():
    return "hola"


print(greet())
```

Salida:

```text id="e55u68"
HOLA
```

## Decoradores que validan entradas

```python id="dn5jdx"
def non_negative_decorator(func):
    def wrapper(value):
        if value < 0:
            raise ValueError("El valor no puede ser negativo")

        return func(value)

    return wrapper


@non_negative_decorator
def square_root_input(value):
    return value ** 0.5


print(square_root_input(9))
```

Salida:

```text id="dqs6p8"
3.0
```

## Cierres y decoradores

La mayoría de los decoradores usan cierres. La función interna conserva acceso a la función original incluso después de que el decorador terminó su ejecución.

```python id="6m5323"
def debug_decorator(func):
    def wrapper(*args, **kwargs):
        print("Función original:", func.__name__)
        return func(*args, **kwargs)

    return wrapper
```

Aquí `wrapper` conserva una referencia a `func`.

## Decoradores con parámetros

Un decorador también puede recibir argumentos propios. En ese caso, se necesita un nivel adicional de funciones.

Estructura general:

```python id="3oan2k"
def decorador_con_parametro(config):
    def decorador(func):
        def envoltura(*args, **kwargs):
            ...
            return func(*args, **kwargs)

        return envoltura

    return decorador
```

Ejemplo:

```python id="q3wjlwm"
def repeat(times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            result = None

            for _ in range(times):
                result = func(*args, **kwargs)

            return result

        return wrapper

    return decorator


@repeat(3)
def greet():
    print("Hola")


greet()
```

Salida:

```text id="jlwm3a"
Hola
Hola
Hola
```

## Orden conceptual de los decoradores con parámetros

```python id="mbrr2i"
@repeat(3)
def greet():
    print("Hola")
```

equivale a:

```python id="fjlwm1"
def greet():
    print("Hola")

greet = repeat(3)(greet)
```

Primero se evalúa `repeat(3)`, que devuelve un decorador. Luego ese decorador recibe a `greet`.

## Composición de decoradores

Una función puede tener más de un decorador.

```python id="jlwm34"
def decorator_a(func):
    def wrapper(*args, **kwargs):
        print("A antes")
        result = func(*args, **kwargs)
        print("A después")
        return result

    return wrapper


def decorator_b(func):
    def wrapper(*args, **kwargs):
        print("B antes")
        result = func(*args, **kwargs)
        print("B después")
        return result

    return wrapper


@decorator_a
@decorator_b
def greet():
    print("Hola")


greet()
```

Salida:

```text id="jlwm83"
A antes
B antes
Hola
B después
A después
```

El decorador más cercano a la función se aplica primero, pero el más externo envuelve el resultado final.

Equivalencia conceptual:

```python id="ctjlwm"
greet = decorator_a(decorator_b(greet))
```

## Decoradores y metadatos de funciones

Cuando una función se decora, la nueva función envoltorio reemplaza a la original. Por eso pueden perderse metadatos como:

* `__name__`
* `__doc__`

Ejemplo:

```python id="nftjlwm"
def my_decorator(func):
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)

    return wrapper


@my_decorator
def greet():
    """Saluda al usuario."""
    return "Hola"


print(greet.__name__)
print(greet.__doc__)
```

Salida típica:

```text id="jlwm27"
wrapper
None
```

En la práctica profesional suele corregirse esto con herramientas como `functools.wraps`, que pertenece a la biblioteca estándar y se documentará más adelante en su módulo correspondiente. Conceptualmente, el problema debe conocerse desde aquí: un decorador puede alterar la identidad visible de la función si no se preservan sus metadatos.

## Cuándo conviene usar decoradores

Los decoradores son adecuados cuando existe una lógica transversal que debe aplicarse a varias funciones, por ejemplo:

* registro de llamadas
* validación común
* medición de tiempo
* control de acceso
* repetición de comportamiento
* transformación de resultados

No conviene usarlos cuando la lógica es demasiado específica, difícil de leer o introduce demasiada indirección innecesaria.

## `with` y context managers

La instrucción `with` permite gestionar recursos y estados de forma segura y estructurada.

Ejemplo típico:

```python id="jlwmf3"
with open("datos.txt", "r", encoding="utf-8") as archivo:
    contenido = archivo.read()
```

Esto garantiza que el archivo se cierre correctamente al terminar el bloque, incluso si ocurre una excepción.

## Qué es un context manager

Un context manager es un objeto que implementa el protocolo de contexto mediante dos métodos especiales:

* `__enter__`
* `__exit__`

Forma general:

```python id="qfjlwm"
class MiContexto:
    def __enter__(self):
        ...

    def __exit__(self, exc_type, exc_value, traceback):
        ...
```

## `__enter__`

Se ejecuta al entrar al bloque `with`.

Puede devolver el objeto que se asignará tras `as`.

```python id="2qj6x9"
class DemoContext:
    def __enter__(self):
        print("Entrando al contexto")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print("Saliendo del contexto")


with DemoContext() as context:
    print("Dentro del bloque")
```

Salida:

```text id="o6njt7"
Entrando al contexto
Dentro del bloque
Saliendo del contexto
```

## `__exit__(self, exc_type, exc_value, traceback)`

Se ejecuta al salir del bloque `with`, tanto si hubo excepción como si no.

Los parámetros indican información sobre una posible excepción:

* `exc_type`: tipo de excepción
* `exc_value`: instancia de la excepción
* `traceback`: traceback asociado

Si no hubo excepción, los tres valores serán `None`.

## Ejemplo con excepción

```python id="jlwm6o"
class DemoContext:
    def __enter__(self):
        print("Entrando")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print("Saliendo")
        print("Tipo de excepción:", exc_type)
        print("Valor de excepción:", exc_value)


with DemoContext():
    print("Antes del error")
    1 / 0
```

Salida aproximada:

```text id="oacjlwm"
Entrando
Antes del error
Saliendo
Tipo de excepción: <class 'ZeroDivisionError'>
Valor de excepción: division by zero
```

Después de esto, la excepción seguirá propagándose si `__exit__` no la suprime.

## Supresión de excepciones

Si `__exit__` devuelve `True`, la excepción se considera manejada y no continúa propagándose.

```python id="jlwm5a"
class SafeContext:
    def __enter__(self):
        print("Entrando")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print("Saliendo")
        return True


with SafeContext():
    1 / 0

print("El programa continúa")
```

Salida:

```text id="jlwmti"
Entrando
Saliendo
El programa continúa
```

Esta posibilidad debe usarse con mucho cuidado. Suprimir excepciones sin una razón clara puede ocultar errores reales.

## Equivalencia conceptual de `with`

La instrucción:

```python id="6jlwmk"
with contexto as valor:
    bloque
```

puede entenderse conceptualmente de forma similar a:

```python id="jlwmn6"
valor = contexto.__enter__()

try:
    bloque
finally:
    contexto.__exit__(...)
```

No es una traducción exacta del funcionamiento interno, pero ayuda a comprender la lógica: entrar, ejecutar, salir.

## Context manager para manejo de recursos

Ejemplo simple de recurso con apertura y cierre:

```python id="jlwmhe"
class ManagedResource:
    def __enter__(self):
        print("Recurso adquirido")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print("Recurso liberado")


with ManagedResource():
    print("Usando el recurso")
```

Salida:

```text id="jlwm4x"
Recurso adquirido
Usando el recurso
Recurso liberado
```

## Uso de `as`

La parte `as nombre` recibe el valor retornado por `__enter__`.

```python id="jlwmis"
class NumberContext:
    def __enter__(self):
        return 100

    def __exit__(self, exc_type, exc_value, traceback):
        pass


with NumberContext() as value:
    print(value)
```

Salida:

```text id="jlwm1h"
100
```

## Cuándo conviene usar context managers

Los context managers son especialmente adecuados para:

* archivos
* conexiones
* bloqueos
* sesiones
* recursos temporales
* cambios temporales de configuración
* adquisición y liberación de estado

## Diferencia conceptual entre decoradores y context managers

| Mecanismo       | Actúa sobre      | Propósito principal                         |
| --------------- | ---------------- | ------------------------------------------- |
| Decorador       | función o clase  | modificar o ampliar comportamiento reusable |
| Context manager | bloque de código | controlar entrada y salida de un contexto   |

Un decorador afecta la definición de una función o clase.

Un context manager afecta la ejecución de un bloque.

## Casos donde ambos pueden parecer cercanos

Ambos sirven para evitar repetición de lógica transversal. Sin embargo, no son intercambiables.

Un decorador es apropiado para lógica asociada a llamadas.

Un context manager es apropiado para lógica asociada a una región delimitada del código.

## Errores comunes con decoradores

## Olvidar retornar la función envoltorio

Problemático:

```python id="zyjlwm"
def decorador(func):
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
```

Aquí falta:

```python id="xjlwmh"
return wrapper
```

## Olvidar retornar el resultado de la función original

Problemático:

```python id="g9wmng"
def decorador(func):
    def wrapper(*args, **kwargs):
        func(*args, **kwargs)

    return wrapper
```

Si la función original devolvía un valor, se perderá.

## No aceptar argumentos flexibles en el envoltorio

Problemático:

```python id="axblcg"
def decorador(func):
    def wrapper():
        return func()

    return wrapper
```

Esto falla si la función decorada necesita argumentos.

## Crear decoradores difíciles de leer

Una pila grande de decoradores o decoradores demasiado complejos puede volver difícil seguir el flujo del programa.

## Errores comunes con context managers

## No devolver nada útil en `__enter__` cuando se necesita `as`

Si se espera usar:

```python id="jlwm6d"
with contexto as valor:
    ...
```

pero `__enter__` no retorna nada, entonces `valor` será `None`.

## Suprimir excepciones sin intención

Devolver `True` en `__exit__` suprime la excepción. Esto puede ocultar errores.

## Usar `with` sin necesidad de contexto real

No todo bloque necesita un context manager. Solo tiene sentido cuando hay una entrada y salida claramente definidas o un recurso que gestionar.

## Buenas prácticas

## En decoradores, usar `*args` y `**kwargs` si el objetivo es general

```python id="0jlwmq"
def decorator(func):
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)

    return wrapper
```

## Mantener los decoradores enfocados en una responsabilidad concreta

Un decorador debería hacer una sola cosa clara: registrar, validar, transformar o controlar.

## Evitar decoradores con demasiada lógica oculta

Si el comportamiento decorado deja de ser evidente, la abstracción puede volverse contraproducente.

## En context managers, reservar `with` para recursos o contextos reales

```python id="jlwm0w"
with open("archivo.txt", "r", encoding="utf-8") as archivo:
    contenido = archivo.read()
```

## No suprimir excepciones salvo que exista una razón clara

La opción más segura es dejar que las excepciones se propaguen, salvo que el context manager realmente deba absorberlas.

## Hacer explícita la relación entre entrada y salida del contexto

La adquisición y liberación del recurso deben ser claras y simétricas.

## Ejemplo integrado

```python id="jlwmk2"
def log_calls(func):
    def wrapper(*args, **kwargs):
        print(f"Iniciando {func.__name__}")
        result = func(*args, **kwargs)
        print(f"Finalizando {func.__name__}")
        return result

    return wrapper


class TemporaryMessage:
    def __init__(self, message):
        self.message = message

    def __enter__(self):
        print(f"Entrando: {self.message}")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print(f"Saliendo: {self.message}")


@log_calls
def add(a, b):
    return a + b


with TemporaryMessage("bloque principal"):
    total = add(3, 4)
    print("Resultado:", total)
```

Salida aproximada:

```text id="3yjlwm"
Entrando: bloque principal
Iniciando add
Finalizando add
Resultado: 7
Saliendo: bloque principal
```

## Orden didáctico interno

```text id="5g9ztp"
1. Funciones de orden superior
2. Qué es un decorador
3. Sintaxis con @
4. Decoradores con argumentos flexibles
5. Decoradores con parámetros
6. Composición de decoradores
7. Metadatos y efecto del envoltorio
8. Qué es un context manager
9. with
10. __enter__ y __exit__
11. Excepciones dentro de un contexto
12. Supresión de excepciones
13. Diferencia entre decoradores y context managers
14. Errores comunes y buenas prácticas
```