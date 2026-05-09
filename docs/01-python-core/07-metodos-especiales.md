# Métodos especiales

## Propósito

Los métodos especiales permiten integrar objetos personalizados con el comportamiento interno del lenguaje. Gracias a ellos, una instancia puede imprimirse de forma legible, responder a operadores, comportarse como secuencia, ser iterable, compararse con otras instancias o actuar como una función invocable.

## Alcance

Este tema cubre:

- qué son los métodos especiales
- cómo se activan implícitamente
- diferencia entre invocación directa y uso idiomático del lenguaje
- representación textual
- longitud, indexación y pertenencia
- iteración
- invocación como función
- comparación
- conversión booleana
- operaciones aritméticas básicas
- buenas prácticas de implementación

## Ideas fundamentales

1. Los métodos especiales son métodos con nombres delimitados por doble guion bajo.
2. No se llaman normalmente de forma manual en el uso cotidiano.
3. Python los invoca de manera implícita cuando se usan operadores, funciones integradas o construcciones del lenguaje.
4. Permiten que un objeto personalizado se comporte como un tipo integrado.
5. Deben implementarse solo cuando tengan un significado claro para la clase.
6. Una implementación parcial o inconsistente puede producir comportamientos confusos.

## Qué son los métodos especiales

Los métodos especiales son métodos con nombres como:

```python
__init__
__str__
__repr__
__len__
__iter__
__next__
__getitem__
__setitem__
__contains__
__call__
````

Se conocen también como dunder methods, por la abreviatura de double underscore.

Estos métodos definen cómo reacciona un objeto ante operaciones del lenguaje.

Ejemplo:

```python id="q2yv2l"
class Box:
    def __len__(self):
        return 5

box = Box()
print(len(box))
```

Salida:

```text id="fj9bvb"
5
```

`len(box)` provoca la llamada implícita a `box.__len__()`.

## Llamada implícita

La forma idiomática de usar un método especial no suele ser llamarlo directamente.

Menos natural:

```python id="u3dq0b"
box.__len__()
```

Forma correcta de uso:

```python id="r17sgt"
len(box)
```

Lo importante no es memorizar únicamente el nombre del método especial, sino la construcción del lenguaje que lo activa.

## Representación textual

## `__str__`

Define la representación legible de un objeto para usuarios finales.

```python id="00jlwm"
class Person:
    def __init__(self, name):
        self.name = name

    def __str__(self):
        return f"Person(name={self.name})"

person = Person("Ana")
print(person)
```

Salida:

```text id="1koeqj"
Person(name=Ana)
```

`print(obj)` usa `__str__()` cuando está disponible.

## `__repr__`

Define una representación más técnica y orientada a depuración.

```python id="l1q7mk"
class Person:
    def __init__(self, name):
        self.name = name

    def __repr__(self):
        return f"Person(name={self.name!r})"

person = Person("Ana")
print(repr(person))
```

Salida:

```text id="wcom75"
Person(name='Ana')
```

## Diferencia entre `__str__` y `__repr__`

Regla práctica:

* `__str__` debe ser legible
* `__repr__` debe ser informativo y útil para depuración

Ejemplo combinado:

```python id="6xjlwm"
class Product:
    def __init__(self, name, price):
        self.name = name
        self.price = price

    def __str__(self):
        return f"{self.name} - S/ {self.price:.2f}"

    def __repr__(self):
        return f"Product(name={self.name!r}, price={self.price!r})"

product = Product("Teclado", 120)

print(product)
print(repr(product))
```

Salida:

```text id="srxrai"
Teclado - S/ 120.00
Product(name='Teclado', price=120)
```

## Comportamiento por defecto

Si no se implementan estos métodos, Python muestra una representación genérica.

```python id="4pxa42"
class Product:
    pass

product = Product()
print(product)
```

Salida posible:

```text id="s5589p"
<__main__.Product object at 0x...>
```

## Longitud y verdad lógica

## `__len__`

Permite que un objeto responda a `len()`.

```python id="ybhzj4"
class Team:
    def __init__(self, members):
        self.members = members

    def __len__(self):
        return len(self.members)

team = Team(["Ana", "Luis", "Marta"])
print(len(team))
```

Salida:

```text id="kyjlwm"
3
```

## `__bool__`

Permite definir cómo se evalúa un objeto en contexto booleano.

```python id="wpjlwm"
class Wallet:
    def __init__(self, balance):
        self.balance = balance

    def __bool__(self):
        return self.balance > 0

wallet_1 = Wallet(100)
wallet_2 = Wallet(0)

print(bool(wallet_1))
print(bool(wallet_2))
```

Salida:

```text id="u9m4ov"
True
False
```

## Relación entre `__bool__` y `__len__`

Si una clase no define `__bool__`, Python puede usar `__len__` para decidir su valor lógico.

Regla general:

* longitud cero, falso
* longitud positiva, verdadero

Ejemplo:

```python id="0dhms6"
class Basket:
    def __init__(self, items):
        self.items = items

    def __len__(self):
        return len(self.items)

basket_1 = Basket([1, 2])
basket_2 = Basket([])

print(bool(basket_1))
print(bool(basket_2))
```

Salida:

```text id="sljlwm"
True
False
```

## Indexación, asignación y pertenencia

## `__getitem__(self, key)`

Permite acceder a elementos usando corchetes.

```python id="soxjlwm"
class Scores:
    def __init__(self, values):
        self.values = values

    def __getitem__(self, index):
        return self.values[index]

scores = Scores([10, 20, 30])
print(scores[1])
```

Salida:

```text id="9t2k0b"
20
```

El parámetro `key` puede representar:

* un índice entero
* un slice
* una clave
* cualquier objeto usado como selector

Ejemplo con slices:

```python id="112xko"
class Scores:
    def __init__(self, values):
        self.values = values

    def __getitem__(self, key):
        return self.values[key]

scores = Scores([10, 20, 30, 40])
print(scores[1:3])
```

Salida:

```text id="aitaly"
[20, 30]
```

## `__setitem__(self, key, value)`

Permite asignar valores mediante corchetes.

```python id="p6ie8g"
class Scores:
    def __init__(self, values):
        self.values = values

    def __getitem__(self, key):
        return self.values[key]

    def __setitem__(self, key, value):
        self.values[key] = value

scores = Scores([10, 20, 30])
scores[1] = 99

print(scores[1])
print(scores.values)
```

Salida:

```text id="jlwmyn"
99
[10, 99, 30]
```

## `__delitem__(self, key)`

Permite eliminar elementos con `del obj[key]`.

```python id="sqqscc"
class Scores:
    def __init__(self, values):
        self.values = values

    def __delitem__(self, key):
        del self.values[key]

scores = Scores([10, 20, 30])
del scores[1]

print(scores.values)
```

Salida:

```text id="zby2qw"
[10, 30]
```

## `__contains__(self, item)`

Permite controlar el uso del operador `in`.

```python id="5mjlwm"
class Team:
    def __init__(self, members):
        self.members = members

    def __contains__(self, member):
        return member in self.members

team = Team(["Ana", "Luis", "Marta"])

print("Ana" in team)
print("Carlos" in team)
```

Salida:

```text id="u5zxo4"
True
False
```

Si no se implementa `__contains__`, Python puede intentar resolver `in` mediante iteración.

## Iteración

## `__iter__`

Debe devolver un iterador.

```python id="d8jlwm"
class Team:
    def __init__(self, members):
        self.members = members

    def __iter__(self):
        return iter(self.members)

team = Team(["Ana", "Luis", "Marta"])

for member in team:
    print(member)
```

Salida:

```text id="1winby"
Ana
Luis
Marta
```

## `__next__`

Se implementa cuando la propia instancia actúa como iterador.

```python id="h6s8um"
class Counter:
    def __init__(self, limit):
        self.current = 0
        self.limit = limit

    def __iter__(self):
        return self

    def __next__(self):
        if self.current >= self.limit:
            raise StopIteration

        value = self.current
        self.current += 1
        return value

counter = Counter(3)

for value in counter:
    print(value)
```

Salida:

```text id="3g4rmd"
0
1
2
```

## Diferencia entre iterable e iterador en una clase

Una clase puede:

* ser iterable, devolviendo un iterador distinto en `__iter__`
* ser a la vez iterable e iterador, devolviéndose a sí misma en `__iter__`

La primera opción suele ser más segura para recorridos repetidos. La segunda requiere más cuidado, porque el estado del iterador se consume.

## Ejemplo de iterable que devuelve un iterador nuevo

```python id="jlwm57"
class Team:
    def __init__(self, members):
        self.members = members

    def __iter__(self):
        return iter(self.members)

team = Team(["Ana", "Luis"])

print(list(team))
print(list(team))
```

Salida:

```text id="ufkaht"
['Ana', 'Luis']
['Ana', 'Luis']
```

## Ejemplo de objeto que es su propio iterador

```python id="jlwmws"
class Counter:
    def __init__(self, limit):
        self.current = 0
        self.limit = limit

    def __iter__(self):
        return self

    def __next__(self):
        if self.current >= self.limit:
            raise StopIteration

        value = self.current
        self.current += 1
        return value

counter = Counter(3)

print(list(counter))
print(list(counter))
```

Salida:

```text id="0qvzf0"
[0, 1, 2]
[]
```

El segundo recorrido está vacío porque el iterador ya fue consumido.

## Invocación como función

## `__call__`

Permite que una instancia pueda llamarse como si fuera una función.

```python id="nozucb"
class Multiplier:
    def __init__(self, factor):
        self.factor = factor

    def __call__(self, value):
        return value * self.factor

double = Multiplier(2)

print(double(10))
```

Salida:

```text id="u3hcy3"
20
```

Esto resulta útil cuando un objeto debe combinar estado interno con comportamiento invocable.

## Comparación

## `__eq__`

Define la igualdad con `==`.

```python id="jlwmqo"
class Product:
    def __init__(self, code):
        self.code = code

    def __eq__(self, other):
        if not isinstance(other, Product):
            return NotImplemented

        return self.code == other.code

product_1 = Product("A1")
product_2 = Product("A1")
product_3 = Product("B2")

print(product_1 == product_2)
print(product_1 == product_3)
```

Salida:

```text id="rjlwmv"
True
False
```

## `__ne__`

Define la desigualdad con `!=`.

En muchas versiones modernas, si se implementa `__eq__`, Python puede derivar el comportamiento de `!=`, por lo que no siempre es necesario implementarlo por separado.

## `__lt__`, `__le__`, `__gt__`, `__ge__`

Permiten definir comparaciones de orden:

* `<`
* `<=`
* `>`
* `>=`

Ejemplo:

```python id="rjlwmg"
class Score:
    def __init__(self, value):
        self.value = value

    def __lt__(self, other):
        if not isinstance(other, Score):
            return NotImplemented

        return self.value < other.value

score_1 = Score(10)
score_2 = Score(20)

print(score_1 < score_2)
```

Salida:

```text id="jlwm0v"
True
```

## Uso de `NotImplemented`

Cuando una comparación no tiene sentido con el otro objeto, conviene devolver `NotImplemented`.

```python id="jlwmj4"
class Score:
    def __init__(self, value):
        self.value = value

    def __eq__(self, other):
        if not isinstance(other, Score):
            return NotImplemented

        return self.value == other.value
```

Esto permite a Python intentar la operación reflejada o producir el comportamiento adecuado.

## Operaciones aritméticas

## `__add__`

Define el comportamiento del operador `+`.

```python id="jlwm2r"
class Vector2D:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        if not isinstance(other, Vector2D):
            return NotImplemented

        return Vector2D(self.x + other.x, self.y + other.y)

    def __repr__(self):
        return f"Vector2D(x={self.x}, y={self.y})"

v1 = Vector2D(1, 2)
v2 = Vector2D(3, 4)

print(v1 + v2)
```

Salida:

```text id="wxrjlwm"
Vector2D(x=4, y=6)
```

## `__sub__`, `__mul__`, `__truediv__`

La misma lógica se aplica a otros operadores:

* `__sub__` para `-`
* `__mul__` para `*`
* `__truediv__` para `/`

Ejemplo:

```python id="jlwmny"
class Vector2D:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __mul__(self, scalar):
        if not isinstance(scalar, (int, float)):
            return NotImplemented

        return Vector2D(self.x * scalar, self.y * scalar)

    def __repr__(self):
        return f"Vector2D(x={self.x}, y={self.y})"

vector = Vector2D(2, 3)
print(vector * 10)
```

Salida:

```text id="u8r2mh"
Vector2D(x=20, y=30)
```

## Operaciones reflejadas

Cuando una operación puede aparecer con el objeto en el lado derecho, pueden implementarse versiones reflejadas.

Ejemplo:

* `__radd__`
* `__rmul__`

```python id="jlwmpr"
class Vector2D:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __mul__(self, scalar):
        if not isinstance(scalar, (int, float)):
            return NotImplemented

        return Vector2D(self.x * scalar, self.y * scalar)

    __rmul__ = __mul__

    def __repr__(self):
        return f"Vector2D(x={self.x}, y={self.y})"

vector = Vector2D(2, 3)

print(vector * 5)
print(5 * vector)
```

Salida:

```text id="jlwm5i"
Vector2D(x=10, y=15)
Vector2D(x=10, y=15)
```

## Contexto de atributos

## `__getattr__`

Se ejecuta cuando se intenta acceder a un atributo que no existe de manera normal.

```python id="5jlwmn"
class Config:
    def __init__(self, values):
        self.values = values

    def __getattr__(self, name):
        if name in self.values:
            return self.values[name]

        raise AttributeError(f"No existe el atributo {name!r}")

config = Config({"host": "localhost", "port": 8000})

print(config.host)
print(config.port)
```

Salida:

```text id="zjlwmv"
localhost
8000
```

## `__setattr__`

Permite interceptar asignaciones de atributos. Debe usarse con mucho cuidado para evitar recursión infinita.

```python id="4jlwm8"
class User:
    def __setattr__(self, name, value):
        if name == "age" and value < 0:
            raise ValueError("La edad no puede ser negativa")

        super().__setattr__(name, value)

user = User()
user.age = 20
print(user.age)
```

## `__delattr__`

Interviene cuando se elimina un atributo con `del obj.attr`.

## Gestión de contexto

Aunque el tema completo corresponde al archivo de decoradores y context managers, conviene registrar los métodos implicados.

## `__enter__` y `__exit__`

Permiten que un objeto se use con `with`.

```python id="jlwmrf"
class DemoContext:
    def __enter__(self):
        print("Entrando")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print("Saliendo")

with DemoContext():
    print("Dentro del bloque")
```

Salida:

```text id="kjlwmq"
Entrando
Dentro del bloque
Saliendo
```

## Resumen de métodos especiales más importantes

| Método                  | Activa                        | Uso principal                 |
| ----------------------- | ----------------------------- | ----------------------------- |
| `__init__`              | creación de instancia         | inicialización                |
| `__str__`               | `str(obj)`, `print(obj)`      | representación legible        |
| `__repr__`              | `repr(obj)`                   | representación técnica        |
| `__len__`               | `len(obj)`                    | longitud                      |
| `__bool__`              | `bool(obj)`, `if obj:`        | verdad lógica                 |
| `__getitem__`           | `obj[key]`                    | acceso por índice o clave     |
| `__setitem__`           | `obj[key] = value`            | asignación por índice o clave |
| `__delitem__`           | `del obj[key]`                | eliminación                   |
| `__contains__`          | `item in obj`                 | pertenencia                   |
| `__iter__`              | `iter(obj)`, `for ... in obj` | iteración                     |
| `__next__`              | `next(it)`                    | siguiente elemento            |
| `__call__`              | `obj(...)`                    | invocación                    |
| `__eq__`                | `==`                          | igualdad                      |
| `__lt__`                | `<`                           | comparación                   |
| `__add__`               | `+`                           | suma                          |
| `__mul__`               | `*`                           | multiplicación                |
| `__getattr__`           | acceso a atributo faltante    | resolución dinámica           |
| `__setattr__`           | asignación de atributos       | control de asignación         |
| `__enter__`, `__exit__` | `with`                        | gestión de contexto           |

## Errores comunes

## Implementar `__iter__` sin devolver un iterador

Problemático:

```python id="jlwm23"
class Team:
    def __iter__(self):
        return [1, 2, 3]
```

Debe devolverse un iterador, no una lista.

Correcto:

```python id="jlwmha"
class Team:
    def __iter__(self):
        return iter([1, 2, 3])
```

## Implementar `__next__` sin lanzar `StopIteration`

Si nunca se lanza `StopIteration`, el iterador no terminará correctamente.

## Usar métodos especiales sin significado claro

No conviene definir `__add__`, `__len__` o `__call__` si la operación no tiene una interpretación natural para la clase.

## Hacer que `__str__` o `__repr__` devuelvan algo que no sea `str`

Incorrecto:

```python id="jlwmzh"
class Product:
    def __str__(self):
        return 100
```

Esto produce error, porque el valor retornado debe ser una cadena.

## Provocar recursión infinita en `__setattr__`

Problemático:

```python id="h2dyyz"
class User:
    def __setattr__(self, name, value):
        self.name = value
```

Aquí `self.name = value` vuelve a llamar `__setattr__`.

Correcto:

```python id="l5gi0w"
class User:
    def __setattr__(self, name, value):
        super().__setattr__(name, value)
```

## Buenas prácticas

## Implementar solo los métodos especiales que tengan sentido semántico

Una clase debe comportarse como secuencia, contenedor, iterador o entidad comparable solo si eso representa realmente su naturaleza.

## Mantener coherencia entre operaciones relacionadas

Si se define `__eq__`, la noción de igualdad debe ser estable y consistente con el estado relevante del objeto.

## Hacer que `__repr__` sea informativo

```python id="jlwm0i"
class Product:
    def __init__(self, name, price):
        self.name = name
        self.price = price

    def __repr__(self):
        return f"Product(name={self.name!r}, price={self.price!r})"
```

## Preferir `iter(contenedor_interno)` cuando sea posible

```python id="qjlwm7"
class Team:
    def __init__(self, members):
        self.members = members

    def __iter__(self):
        return iter(self.members)
```

Esto simplifica la implementación y reduce errores.

## Devolver `NotImplemented` cuando la operación no corresponda

```python id="lwjlm9"
def __eq__(self, other):
    if not isinstance(other, Product):
        return NotImplemented

    return self.code == other.code
```

## Ejemplo integrado

```python id="jlwm6m"
class Inventory:
    def __init__(self, items=None):
        self.items = list(items) if items is not None else []

    def __repr__(self):
        return f"Inventory(items={self.items!r})"

    def __str__(self):
        return f"Inventario con {len(self.items)} elementos"

    def __len__(self):
        return len(self.items)

    def __getitem__(self, index):
        return self.items[index]

    def __setitem__(self, index, value):
        self.items[index] = value

    def __contains__(self, item):
        return item in self.items

    def __iter__(self):
        return iter(self.items)

    def __call__(self, item):
        self.items.append(item)

    def __eq__(self, other):
        if not isinstance(other, Inventory):
            return NotImplemented

        return self.items == other.items


inventory = Inventory(["teclado", "mouse"])

print(inventory)
print(repr(inventory))
print(len(inventory))
print(inventory[0])
print("mouse" in inventory)

inventory("monitor")
print(list(inventory))
```

## Orden didáctico interno

```text id="n1x4g0"
1. Qué son los métodos especiales
2. Llamada implícita desde el lenguaje
3. Representación: __str__ y __repr__
4. Longitud y verdad lógica: __len__, __bool__
5. Acceso y asignación: __getitem__, __setitem__, __delitem__
6. Pertenencia: __contains__
7. Iteración: __iter__, __next__
8. Invocación: __call__
9. Comparación: __eq__, __lt__ y relacionados
10. Operaciones aritméticas: __add__, __mul__ y reflejadas
11. Atributos dinámicos: __getattr__, __setattr__
12. Contexto: __enter__, __exit__
13. Errores comunes y buenas prácticas
```