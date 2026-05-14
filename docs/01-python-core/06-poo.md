# Programación orientada a objetos

## Propósito

La programación orientada a objetos permite modelar entidades, comportamientos y relaciones mediante clases y objetos. En Python, este enfoque se integra con el modelo general del lenguaje: las clases son objetos, las instancias son objetos y los métodos son funciones asociadas a clases u objetos.

## Alcance

Este tema cubre:

- definición de clases
- creación de instancias
- atributos y métodos
- `self`
- constructor `__init__`
- atributos de instancia y de clase
- encapsulamiento por convención
- herencia
- sobrescritura
- uso de `super()`
- métodos de clase y métodos estáticos
- principios básicos de diseño orientado a objetos

## Ideas fundamentales

1. Una clase define una estructura y un conjunto de comportamientos.
2. Un objeto es una instancia concreta de una clase.
3. Los atributos representan estado.
4. Los métodos representan comportamiento.
5. `self` representa la instancia sobre la que se está operando.
6. `__init__` inicializa el estado de una instancia recién creada.
7. Los atributos de instancia pertenecen a cada objeto.
8. Los atributos de clase pertenecen a la clase y pueden compartirse.
9. La herencia permite especializar y reutilizar comportamiento.
10. Python no impone encapsulamiento estricto, pero sí convenciones claras para expresarlo.

## Clase y objeto

Una clase actúa como plantilla o definición general.

```python
class Person:
    pass
````

Un objeto es una instancia concreta creada a partir de esa clase.

```python
class Person:
    pass

person_1 = Person()
person_2 = Person()

print(type(person_1))
print(type(person_2))
```

Salida:

```text
<class '__main__.Person'>
<class '__main__.Person'>
```

Aunque ambas instancias pertenecen a la misma clase, siguen siendo objetos distintos.

## Definición básica de una clase

Forma general:

```python
class NombreDeClase:
    cuerpo
```

Ejemplo:

```python
class Product:
    def show_message(self):
        print("Producto disponible")
```

Para usar la clase, primero se crea una instancia y luego se llama al método.

```python
class Product:
    def show_message(self):
        print("Producto disponible")

product = Product()
product.show_message()
```

Salida:

```text
Producto disponible
```

## Convención de nombres

Las clases suelen nombrarse con `PascalCase`.

Ejemplos adecuados:

```python
class BankAccount:
    pass

class SalesReport:
    pass

class CustomerProfile:
    pass
```

Ejemplos menos adecuados:

```python
class bank_account:
    pass

class reporte:
    pass
```

## Instanciación

Instanciar consiste en crear un objeto a partir de una clase.

```python
class Car:
    pass

car = Car()
print(car)
```

El valor impreso será una representación del objeto en memoria.

## Atributos

Los atributos almacenan información asociada al objeto o a la clase.

## Atributos de instancia

Pertenecen a cada objeto en particular.

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Al crear una instancia:

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

person = Person("Ana", 20)

print(person.name)
print(person.age)
```

Salida:

```text
Ana
20
```

Cada instancia puede tener valores distintos.

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

person_1 = Person("Ana", 20)
person_2 = Person("Luis", 25)

print(person_1.name, person_1.age)
print(person_2.name, person_2.age)
```

## Atributos de clase

Pertenecen a la clase y se comparten entre todas las instancias, salvo que una instancia defina un atributo con el mismo nombre.

```python
class Person:
    species = "Human"

    def __init__(self, name):
        self.name = name
```

Uso:

```python
class Person:
    species = "Human"

    def __init__(self, name):
        self.name = name

person_1 = Person("Ana")
person_2 = Person("Luis")

print(Person.species)
print(person_1.species)
print(person_2.species)
```

Salida:

```text
Human
Human
Human
```

## Diferencia entre atributo de clase y atributo de instancia

```python
class Counter:
    shared_value = 0

    def __init__(self):
        self.local_value = 0
```

Aquí:

* `shared_value` pertenece a la clase
* `local_value` pertenece a cada instancia

Ejemplo:

```python
class Counter:
    shared_value = 0

    def __init__(self):
        self.local_value = 0

a = Counter()
b = Counter()

a.local_value = 10
b.local_value = 20

print(a.local_value)
print(b.local_value)
print(Counter.shared_value)
```

Salida:

```text
10
20
0
```

## Métodos

Un método es una función definida dentro de una clase.

```python
class Greeter:
    def greet(self):
        print("Hola")
```

Uso:

```python
class Greeter:
    def greet(self):
        print("Hola")

greeter = Greeter()
greeter.greet()
```

Salida:

```text
Hola
```

## `self`

`self` representa la instancia actual. Permite acceder a sus atributos y métodos.

```python
class Person:
    def __init__(self, name):
        self.name = name

    def introduce(self):
        print(f"Nombre: {self.name}")
```

Uso:

```python
class Person:
    def __init__(self, name):
        self.name = name

    def introduce(self):
        print(f"Nombre: {self.name}")

person = Person("Ana")
person.introduce()
```

Salida:

```text
Nombre: Ana
```

El nombre `self` es una convención muy fuerte. En la práctica, debe respetarse.

## `__init__`

`__init__` es el método inicializador. Se ejecuta automáticamente al crear una instancia.

Forma general:

```python
def __init__(self, ...):
    ...
```

Ejemplo:

```python
class Product:
    def __init__(self, name, price):
        self.name = name
        self.price = price
```

Uso:

```python
class Product:
    def __init__(self, name, price):
        self.name = name
        self.price = price

product = Product("Teclado", 120.0)

print(product.name)
print(product.price)
```

## Métodos de instancia

Son los métodos más comunes. Operan sobre una instancia concreta y reciben `self` como primer parámetro.

```python
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount
```

Uso:

```python
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount

account = BankAccount("Ana", 100)
account.deposit(50)

print(account.balance)
```

Salida:

```text
150
```

## Métodos que retornan valores

No todos los métodos deben imprimir. Muchas veces conviene que retornen un resultado.

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height
```

Uso:

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

rectangle = Rectangle(4, 6)
print(rectangle.area())
```

Salida:

```text
24
```

## Modificación del estado interno

Un método puede modificar atributos de la instancia.

```python
class Counter:
    def __init__(self):
        self.value = 0

    def increment(self):
        self.value += 1
```

Uso:

```python
class Counter:
    def __init__(self):
        self.value = 0

    def increment(self):
        self.value += 1

counter = Counter()

counter.increment()
counter.increment()

print(counter.value)
```

Salida:

```text
2
```

## Representación de objetos

Si no se define ninguna representación, Python muestra una salida genérica del objeto.

```python
class Person:
    def __init__(self, name):
        self.name = name

person = Person("Ana")
print(person)
```

Salida posible:

```text
<__main__.Person object at 0x...>
```

Para controlar esta salida se usan métodos especiales como `__str__` y `__repr__`, que corresponden al archivo de métodos especiales. Sin embargo, resulta útil anticipar su efecto.

```python
class Person:
    def __init__(self, name):
        self.name = name

    def __str__(self):
        return f"Person(name={self.name})"

person = Person("Ana")
print(person)
```

Salida:

```text
Person(name=Ana)
```

## Encapsulamiento

El encapsulamiento consiste en organizar el acceso al estado interno del objeto.

En Python no existe encapsulamiento estricto como regla del lenguaje, pero sí convenciones.

## Atributo público

Se accede libremente.

```python
class Product:
    def __init__(self, name):
        self.name = name
```

## Atributo no público por convención

Un nombre con guion bajo inicial indica que no debería usarse directamente fuera de la clase, salvo necesidad justificada.

```python
class Product:
    def __init__(self, name, stock):
        self.name = name
        self._stock = stock
```

## Name mangling con doble guion bajo

Un atributo con doble guion bajo inicial activa name mangling.

```python
class Product:
    def __init__(self, name, price):
        self.name = name
        self.__price = price
```

Acceso directo problemático:

```python
product = Product("Mouse", 80)
print(product.__price)
```

Esto produce `AttributeError`.

Internamente, Python renombra el atributo. Aun así, no debe interpretarse como un mecanismo de seguridad fuerte, sino como una forma de evitar colisiones accidentales.

## Métodos para controlar acceso

En lugar de exponer directamente ciertos atributos, puede definirse un método para leer o modificar el valor de forma controlada.

```python
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner
        self._balance = balance

    def get_balance(self):
        return self._balance

    def deposit(self, amount):
        if amount > 0:
            self._balance += amount
```

Uso:

```python
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner
        self._balance = balance

    def get_balance(self):
        return self._balance

    def deposit(self, amount):
        if amount > 0:
            self._balance += amount

account = BankAccount("Ana", 100)
account.deposit(50)

print(account.get_balance())
```

Salida:

```text
150
```

## Herencia

La herencia permite definir una clase nueva a partir de otra existente.

Forma general:

```python
class Hija(Padre):
    ...
```

Ejemplo:

```python
class Animal:
    def speak(self):
        print("Sonido genérico")

class Dog(Animal):
    pass
```

Uso:

```python
class Animal:
    def speak(self):
        print("Sonido genérico")

class Dog(Animal):
    pass

dog = Dog()
dog.speak()
```

Salida:

```text
Sonido genérico
```

`Dog` hereda el método `speak()` de `Animal`.

## Sobrescritura de métodos

Una subclase puede redefinir un método heredado.

```python
class Animal:
    def speak(self):
        print("Sonido genérico")

class Dog(Animal):
    def speak(self):
        print("Guau")
```

Uso:

```python
class Animal:
    def speak(self):
        print("Sonido genérico")

class Dog(Animal):
    def speak(self):
        print("Guau")

animal = Animal()
dog = Dog()

animal.speak()
dog.speak()
```

Salida:

```text
Sonido genérico
Guau
```

## `super()`

`super()` permite acceder a la implementación de la superclase.

Ejemplo en el constructor:

```python
class Employee:
    def __init__(self, name):
        self.name = name

class Manager(Employee):
    def __init__(self, name, department):
        super().__init__(name)
        self.department = department
```

Uso:

```python
class Employee:
    def __init__(self, name):
        self.name = name

class Manager(Employee):
    def __init__(self, name, department):
        super().__init__(name)
        self.department = department

manager = Manager("Ana", "Ventas")

print(manager.name)
print(manager.department)
```

Salida:

```text
Ana
Ventas
```

## Reutilización de lógica heredada

`super()` también puede usarse en métodos ordinarios.

```python
class Animal:
    def speak(self):
        return "Sonido"

class Dog(Animal):
    def speak(self):
        base_sound = super().speak()
        return f"{base_sound} -> Guau"
```

Uso:

```python
class Animal:
    def speak(self):
        return "Sonido"

class Dog(Animal):
    def speak(self):
        base_sound = super().speak()
        return f"{base_sound} -> Guau"

dog = Dog()
print(dog.speak())
```

Salida:

```text
Sonido -> Guau
```

## `isinstance()` e `issubclass()`

Estas funciones son útiles en el contexto orientado a objetos.

## `isinstance(obj, cls)`

Verifica si un objeto es instancia de una clase o de alguna de sus subclases.

```python
class Animal:
    pass

class Dog(Animal):
    pass

dog = Dog()

print(isinstance(dog, Dog))
print(isinstance(dog, Animal))
```

Salida:

```text
True
True
```

## `issubclass(sub, cls)`

Verifica si una clase hereda de otra.

```python
class Animal:
    pass

class Dog(Animal):
    pass

print(issubclass(Dog, Animal))
```

Salida:

```text
True
```

## Polimorfismo

El polimorfismo permite tratar objetos distintos mediante una interfaz común.

```python
class Dog:
    def speak(self):
        return "Guau"

class Cat:
    def speak(self):
        return "Miau"

def make_it_speak(animal):
    print(animal.speak())

make_it_speak(Dog())
make_it_speak(Cat())
```

Salida:

```text
Guau
Miau
```

No hace falta que ambos objetos compartan formalmente una misma superclase para que esto funcione. Lo importante es que ofrezcan el comportamiento esperado.

## Duck typing

Python se apoya fuertemente en duck typing: si un objeto se comporta como se necesita, puede usarse.

```python
class FileLogger:
    def write(self, message):
        print(f"Archivo: {message}")

class ConsoleLogger:
    def write(self, message):
        print(f"Consola: {message}")

def send_log(logger, message):
    logger.write(message)

send_log(FileLogger(), "Proceso iniciado")
send_log(ConsoleLogger(), "Proceso iniciado")
```

## Métodos de clase

Un método de clase recibe la clase como primer parámetro y se define con `@classmethod`.

```python
class Product:
    tax_rate = 0.18

    @classmethod
    def show_tax_rate(cls):
        return cls.tax_rate
```

Uso:

```python
class Product:
    tax_rate = 0.18

    @classmethod
    def show_tax_rate(cls):
        return cls.tax_rate

print(Product.show_tax_rate())
```

Salida:

```text
0.18
```

También puede llamarse desde una instancia, aunque conceptualmente pertenece a la clase.

## Caso típico de `@classmethod`

Se usa frecuentemente para métodos alternativos de construcción.

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @classmethod
    def from_text(cls, text):
        name, age = text.split(",")
        return cls(name, int(age))
```

Uso:

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @classmethod
    def from_text(cls, text):
        name, age = text.split(",")
        return cls(name, int(age))

person = Person.from_text("Ana,20")

print(person.name)
print(person.age)
```

## Métodos estáticos

Un método estático se define con `@staticmethod` y no recibe ni instancia ni clase automáticamente.

```python
class Calculator:
    @staticmethod
    def add(a, b):
        return a + b
```

Uso:

```python
class Calculator:
    @staticmethod
    def add(a, b):
        return a + b

print(Calculator.add(3, 4))
```

Salida:

```text
7
```

## Cuándo usar cada tipo de método

| Tipo de método      | Primer parámetro implícito | Uso principal                                |
| ------------------- | -------------------------- | -------------------------------------------- |
| Método de instancia | `self`                     | Operar sobre un objeto concreto              |
| Método de clase     | `cls`                      | Operar sobre la clase o construir instancias |
| Método estático     | ninguno                    | Función relacionada lógicamente con la clase |

## Composición

No todo en orientación a objetos debe resolverse con herencia. Muchas veces conviene usar composición: un objeto contiene a otro.

```python
class Engine:
    def start(self):
        return "Motor encendido"

class Car:
    def __init__(self):
        self.engine = Engine()

    def start(self):
        return self.engine.start()
```

La composición suele ser preferible cuando la relación es de “tiene un” en lugar de “es un”.

## Diseño básico orientado a objetos

Una clase bien diseñada suele tener:

* una responsabilidad clara
* atributos coherentes con su propósito
* métodos que operan sobre su propio estado
* nombres descriptivos
* poca dependencia de estado global
* relaciones simples y expresivas con otras clases

## Errores comunes

## Olvidar `self` en métodos de instancia

Problemático:

```python
class Person:
    def show_name():
        print("Ana")
```

Uso:

```python
person = Person()
person.show_name()
```

Esto genera error porque Python intenta pasar la instancia automáticamente.

Correcto:

```python
class Person:
    def show_name(self):
        print("Ana")
```

## Confundir atributo de clase con atributo de instancia

Problemático:

```python
class Counter:
    value = 0
```

Si se pretendía que cada instancia tuviera su propio `value`, entonces corresponde definirlo en `__init__`.

```python
class Counter:
    def __init__(self):
        self.value = 0
```

## Usar atributos mutables de clase sin intención

Problemático:

```python
class Team:
    members = []
```

Si se modifica `members`, el cambio afectará a todas las instancias que usen ese atributo compartido.

Correcto:

```python
class Team:
    def __init__(self):
        self.members = []
```

## Usar herencia cuando corresponde composición

No toda reutilización justifica una relación de herencia. Si una clase solo necesita usar otra internamente, la composición suele ser más adecuada.

## Diseñar clases con demasiadas responsabilidades

Una clase que valida, calcula, imprime, guarda archivos y maneja interfaz suele ser difícil de mantener. Conviene separar responsabilidades.

## Buenas prácticas

## Nombrar clases con `PascalCase`

```python
class CustomerAccount:
    pass
```

## Usar `self` de forma consistente

```python
class Product:
    def __init__(self, name):
        self.name = name
```

## Inicializar explícitamente el estado en `__init__`

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height
```

## Diferenciar claramente atributos de instancia y de clase

Los atributos compartidos deben declararse en la clase solo cuando realmente deban ser comunes.

## Usar herencia para especialización real

```python
class Employee:
    pass

class Manager(Employee):
    pass
```

## Usar composición cuando la relación sea de dependencia o colaboración

```python
class Report:
    def __init__(self, formatter):
        self.formatter = formatter
```

## Mantener el comportamiento cerca del estado que lo necesita

Un método debería pertenecer a la clase si su lógica depende directamente de los atributos del objeto.

## Ejemplo integrado

```python
class BankAccount:
    bank_name = "Banco Central"

    def __init__(self, owner, balance=0.0):
        self.owner = owner
        self._balance = balance

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("El monto debe ser positivo")

        self._balance += amount

    def withdraw(self, amount):
        if amount <= 0:
            raise ValueError("El monto debe ser positivo")

        if amount > self._balance:
            raise ValueError("Saldo insuficiente")

        self._balance -= amount

    def get_balance(self):
        return self._balance

    @classmethod
    def create_with_welcome_bonus(cls, owner):
        return cls(owner, balance=50.0)

    @staticmethod
    def validate_owner_name(name):
        return isinstance(name, str) and len(name.strip()) > 0


class SavingsAccount(BankAccount):
    def __init__(self, owner, balance=0.0, interest_rate=0.05):
        super().__init__(owner, balance)
        self.interest_rate = interest_rate

    def apply_interest(self):
        self._balance += self._balance * self.interest_rate


account = SavingsAccount.create_with_welcome_bonus("Ana")
account.deposit(100)
account.apply_interest()

print(account.owner)
print(account.get_balance())
print(account.bank_name)
print(BankAccount.validate_owner_name("Luis"))
```

## Orden didáctico interno

```text
1. Clase y objeto
2. Instanciación
3. Atributos de instancia
4. Atributos de clase
5. Métodos y self
6. Constructor __init__
7. Encapsulamiento por convención
8. Herencia
9. Sobrescritura
10. super()
11. isinstance() e issubclass()
12. Polimorfismo y duck typing
13. classmethod y staticmethod
14. Composición
15. Diseño, errores comunes y buenas prácticas
```