# Funciones

## Propósito

Las funciones permiten encapsular lógica reutilizable, definir interfaces claras entre partes del programa y separar el qué se hace del cómo se organiza la ejecución. Constituyen uno de los mecanismos centrales para estructurar código en Python, reducir duplicación y mejorar mantenibilidad.

## Alcance

Este tema se centra en la definición, llamada y organización de funciones. El tratamiento incluye parámetros, argumentos, retorno de valores, alcance de nombres, cierres, funciones anónimas y convenciones de diseño. No se limita a la sintaxis mínima de `def`, sino que aborda el comportamiento semántico de las funciones dentro del lenguaje.

## Ideas fundamentales

1. Una función es un objeto invocable.
2. Definir una función no equivale a ejecutarla.
3. Los parámetros pertenecen a la definición; los argumentos pertenecen a la llamada.
4. Una función puede recibir cero o más argumentos y devolver cero o más resultados.
5. El valor retornado por defecto es `None` si no se usa `return`.
6. El alcance de los nombres afecta qué variables puede leer o modificar una función.
7. Las funciones pueden almacenarse, pasarse como argumentos y devolverse como resultado.
8. Las funciones pueden cerrar sobre variables de un entorno externo.

## Definición de funciones

La forma general de una función es:

```python
def nombre(parámetros):
    cuerpo
````

Ejemplo:

```python id="i3dgkm"
def saludar():
    print("Hola")
```

La función queda definida, pero no se ejecuta hasta que se la llama.

```python id="4w0nmg"
def saludar():
    print("Hola")

saludar()
```

Salida:

```text id="0cm0en"
Hola
```

## Nombre de la función

El nombre de una función debe describir su propósito.

Ejemplos adecuados:

```python id="vfr57e"
def calculate_total():
    pass

def load_data():
    pass

def validate_email():
    pass
```

Ejemplos poco claros:

```python id="qq0j7r"
def x():
    pass

def proceso1():
    pass

def hacer():
    pass
```

Las funciones suelen nombrarse con `snake_case`.

## Parámetros y argumentos

## Parámetros

Los parámetros son los nombres definidos en la firma de la función.

```python id="c2tlb4"
def sumar(a, b):
    return a + b
```

En este caso, `a` y `b` son parámetros.

## Argumentos

Los argumentos son los valores concretos enviados al llamar a la función.

```python id="0yxpnn"
resultado = sumar(10, 5)
```

Aquí, `10` y `5` son argumentos.

## Diferencia conceptual

```text id="9ni5tq"
Parámetro: nombre definido en la función
Argumento: valor enviado en la llamada
```

## Llamada de funciones

La llamada se realiza escribiendo el nombre de la función seguido de paréntesis.

```python id="9by1ou"
def mostrar_mensaje():
    print("Mensaje")

mostrar_mensaje()
```

Si la función requiere argumentos, deben enviarse en el orden adecuado, salvo que se usen nombres explícitos.

```python id="1clrr5"
def restar(a, b):
    return a - b

print(restar(10, 3))
```

Salida:

```text id="iasicg"
7
```

## Retorno de valores

## `return`

La instrucción `return` finaliza la ejecución de la función y devuelve un valor.

```python id="rw054m"
def sumar(a, b):
    return a + b

resultado = sumar(4, 6)
print(resultado)
```

Salida:

```text id="hkap8h"
10
```

## Retorno implícito de `None`

Si una función no usa `return`, devuelve `None`.

```python id="aszz4m"
def saludar():
    print("Hola")

resultado = saludar()
print(resultado)
```

Salida:

```text id="g3g4jq"
Hola
None
```

## Diferencia entre imprimir y retornar

Imprimir no equivale a retornar.

```python id="6n7262"
def incorrecta(a, b):
    print(a + b)

def correcta(a, b):
    return a + b

x = incorrecta(2, 3)
y = correcta(2, 3)

print("x =", x)
print("y =", y)
```

Salida:

```text id="gl4qeu"
5
x = None
y = 5
```

Una función debe retornar un valor cuando ese resultado vaya a reutilizarse.

## Retorno múltiple

Python permite devolver varios valores en una sola instrucción `return`.

```python id="ig2hl0"
def dividir(a, b):
    cociente = a // b
    residuo = a % b
    return cociente, residuo

resultado = dividir(17, 5)
print(resultado)
```

Salida:

```text id="swt5cu"
(3, 2)
```

También puede desempaquetarse el resultado:

```python id="3svmod"
def dividir(a, b):
    return a // b, a % b

cociente, residuo = dividir(17, 5)

print(cociente)
print(residuo)
```

Salida:

```text id="mmptzi"
3
2
```

## Parámetros posicionales

Son los parámetros que reciben argumentos según su posición.

```python id="4vlov8"
def presentar(nombre, edad):
    print(nombre, edad)

presentar("Ana", 20)
```

Aquí, el primer argumento se asigna a `nombre` y el segundo a `edad`.

## Argumentos nombrados

Permiten especificar explícitamente qué valor corresponde a cada parámetro.

```python id="z8xom9"
def presentar(nombre, edad):
    print(nombre, edad)

presentar(edad=20, nombre="Ana")
```

Salida:

```text id="0641c8"
Ana 20
```

## Combinación de posicionales y nombrados

Los argumentos posicionales deben ir antes que los nombrados.

Correcto:

```python id="ue3rc2"
def presentar(nombre, edad):
    print(nombre, edad)

presentar("Ana", edad=20)
```

Incorrecto:

```python id="r8yioh"
def presentar(nombre, edad):
    print(nombre, edad)

presentar(nombre="Ana", 20)
```

Esto genera error de sintaxis.

## Parámetros con valor por defecto

Permiten omitir ciertos argumentos en la llamada.

```python id="tvdali"
def saludar(nombre, mensaje="Hola"):
    print(mensaje, nombre)

saludar("Ana")
saludar("Luis", "Bienvenido")
```

Salida:

```text id="q8bx1p"
Hola Ana
Bienvenido Luis
```

## Regla de orden

Los parámetros con valor por defecto deben ir después de los parámetros obligatorios.

Correcto:

```python id="96lq0f"
def crear_usuario(nombre, activo=True):
    pass
```

Incorrecto:

```python id="8y0ec3"
def crear_usuario(activo=True, nombre):
    pass
```

## Error común con valores mutables por defecto

No conviene usar listas, diccionarios o conjuntos mutables como valores por defecto.

Problemático:

```python id="j28ghk"
def agregar_elemento(valor, lista=[]):
    lista.append(valor)
    return lista

print(agregar_elemento(1))
print(agregar_elemento(2))
```

Salida:

```text id="7nx0wg"
[1]
[1, 2]
```

La lista por defecto se comparte entre llamadas.

Forma recomendada:

```python id="vl42l0"
def agregar_elemento(valor, lista=None):
    if lista is None:
        lista = []

    lista.append(valor)
    return lista

print(agregar_elemento(1))
print(agregar_elemento(2))
```

Salida:

```text id="nfjlwm"
[1]
[2]
```

## Parámetros arbitrarios posicionales: `*args`

Permiten recibir una cantidad variable de argumentos posicionales.

```python id="uzg3sp"
def sumar_todos(*args):
    return sum(args)

print(sumar_todos(1, 2, 3))
print(sumar_todos(10, 20, 30, 40))
```

Salida:

```text id="dl0mnw"
6
100
```

Dentro de la función, `args` es una tupla.

```python id="hl7mk4"
def mostrar_argumentos(*args):
    print(type(args))
    print(args)

mostrar_argumentos("a", "b", "c")
```

Salida:

```text id="vwgmgv"
<class 'tuple'>
('a', 'b', 'c')
```

## Parámetros arbitrarios nombrados: `**kwargs`

Permiten recibir una cantidad variable de argumentos nombrados.

```python id="3puww4"
def mostrar_datos(**kwargs):
    print(kwargs)

mostrar_datos(nombre="Ana", edad=20, ciudad="Lima")
```

Salida:

```text id="zabctu"
{'nombre': 'Ana', 'edad': 20, 'ciudad': 'Lima'}
```

Dentro de la función, `kwargs` es un diccionario.

## Uso conjunto de parámetros

La firma puede combinar varios tipos de parámetros.

```python id="jsgzzb"
def ejemplo(a, b=10, *args, **kwargs):
    print("a =", a)
    print("b =", b)
    print("args =", args)
    print("kwargs =", kwargs)

ejemplo(1, 2, 3, 4, nombre="Ana", edad=20)
```

Salida:

```text id="gyitvq"
a = 1
b = 2
args = (3, 4)
kwargs = {'nombre': 'Ana', 'edad': 20}
```

## Desempaquetado en llamadas

Una tupla o lista puede desempaquetarse con `*`, y un diccionario con `**`.

```python id="2x0a0z"
def presentar(nombre, edad):
    print(nombre, edad)

datos = ("Ana", 20)
presentar(*datos)
```

```python id="w44zz2"
def presentar(nombre, edad):
    print(nombre, edad)

datos = {"nombre": "Luis", "edad": 25}
presentar(**datos)
```

## Parámetros solo posicionales y solo nombrados

Python permite restringir cómo se pasan ciertos argumentos.

## Solo posicionales con `/`

Los parámetros ubicados antes de `/` solo pueden recibirse por posición.

```python id="t2l7r7"
def dividir(a, b, /):
    return a / b

print(dividir(10, 2))
```

Llamada inválida:

```python id="tkcthz"
dividir(a=10, b=2)
```

## Solo nombrados con `*`

Los parámetros ubicados después de `*` deben pasarse por nombre.

```python id="31wt7t"
def crear_usuario(nombre, *, activo=True, admin=False):
    print(nombre, activo, admin)

crear_usuario("Ana", activo=False, admin=True)
```

Esto mejora claridad cuando hay parámetros opcionales importantes.

## Documentación básica de funciones

Una función debería dejar claro qué recibe y qué devuelve.

```python id="ir0d13"
def calculate_area(radius):
    """Calcula el área de un círculo a partir de su radio."""
    return 3.14159 * radius ** 2
```

La cadena entre comillas al inicio del cuerpo se llama docstring.

Puede consultarse con:

```python id="ervvnu"
print(calculate_area.__doc__)
```

## Funciones como objetos

Las funciones son objetos de primera clase.

Esto significa que pueden:

1. asignarse a variables
2. almacenarse en estructuras
3. pasarse como argumentos
4. devolverse desde otras funciones

## Asignación a variables

```python id="2rath5"
def saludar():
    return "Hola"

f = saludar

print(f())
```

Salida:

```text id="txb0uw"
Hola
```

## Paso como argumento

```python id="v0hzgv"
def apply_operation(a, b, operation):
    return operation(a, b)

def sumar(a, b):
    return a + b

print(apply_operation(3, 4, sumar))
```

Salida:

```text id="2d5ikm"
7
```

## Retorno de funciones

```python id="89w96g"
def create_multiplier(factor):
    def multiplier(value):
        return value * factor

    return multiplier

duplicar = create_multiplier(2)
print(duplicar(10))
```

Salida:

```text id="5m8vml"
20
```

## Alcance de variables

Python resuelve nombres siguiendo la regla LEGB:

```text id="iuwth0"
L: Local
E: Enclosing
G: Global
B: Built-in
```

## Alcance local

Una variable definida dentro de una función pertenece a su ámbito local.

```python id="jzjwbw"
def mostrar():
    mensaje = "Hola"
    print(mensaje)

mostrar()
```

Fuera de la función, `mensaje` no existe.

```python id="pzodvc"
def mostrar():
    mensaje = "Hola"

mostrar()
print(mensaje)
```

Esto genera `NameError`.

## Lectura de variables globales

Una función puede leer variables globales si no existe una local con el mismo nombre.

```python id="fvhy0p"
pais = "Perú"

def mostrar_pais():
    print(pais)

mostrar_pais()
```

## Modificación de variables globales con `global`

Si se quiere reasignar una variable global dentro de una función, debe declararse con `global`.

```python id="y1zcdm"
contador = 0

def incrementar():
    global contador
    contador += 1

incrementar()
print(contador)
```

Salida:

```text id="t9xa4w"
1
```

Sin `global`, Python trataría `contador` como local y produciría error.

## Alcance envolvente y `nonlocal`

Cuando una función está definida dentro de otra, puede acceder al entorno envolvente.

```python id="xvhc1w"
def externa():
    mensaje = "Hola"

    def interna():
        print(mensaje)

    interna()

externa()
```

Si se quiere reasignar una variable del entorno envolvente, debe usarse `nonlocal`.

```python id="dl2drf"
def contador():
    valor = 0

    def incrementar():
        nonlocal valor
        valor += 1
        return valor

    return incrementar

f = contador()

print(f())
print(f())
print(f())
```

Salida:

```text id="oq73bo"
1
2
3
```

## Closures

Un closure aparece cuando una función interna conserva acceso a variables de una función externa incluso después de que la función externa terminó.

```python id="8uk6a1"
def create_power(exponent):
    def power(base):
        return base ** exponent

    return power

cuadrado = create_power(2)
cubo = create_power(3)

print(cuadrado(5))
print(cubo(5))
```

Salida:

```text id="vbpknq"
25
125
```

Aquí, cada función retornada conserva su propio valor de `exponent`.

## Funciones anónimas con `lambda`

`lambda` permite definir funciones pequeñas en una sola expresión.

Forma general:

```python
lambda parámetros: expresión
```

Ejemplo:

```python id="c10qsh"
cuadrado = lambda x: x ** 2

print(cuadrado(4))
```

Salida:

```text id="p9z32u"
16
```

También puede usarse directamente:

```python id="c8cx2r"
print((lambda a, b: a + b)(3, 4))
```

Salida:

```text id="u6w8td"
7
```

## Cuándo usar `lambda`

Adecuado para funciones cortas y simples, especialmente en contextos como:

* `sorted()`
* `map()`
* `filter()`
* claves de ordenamiento
* transformaciones breves

Ejemplo:

```python id="qjlwmn"
datos = [("Ana", 20), ("Luis", 18), ("Carlos", 25)]

ordenados = sorted(datos, key=lambda x: x[1])

print(ordenados)
```

Salida:

```text id="74skue"
[('Luis', 18), ('Ana', 20), ('Carlos', 25)]
```

No conviene usar `lambda` cuando la lógica es extensa o compleja. En esos casos resulta más claro definir una función con `def`.

## Anotaciones de tipo

Las funciones pueden incluir anotaciones para indicar el tipo esperado de parámetros y retornos.

```python id="2ft1s2"
def sumar(a: int, b: int) -> int:
    return a + b
```

Las anotaciones no imponen tipos en tiempo de ejecución por sí solas, pero mejoran legibilidad, documentación y herramientas de análisis estático.

## Funciones puras y efectos colaterales

## Función pura

Una función pura depende solo de sus entradas y no modifica estado externo.

```python id="xgx7ad"
def sumar(a, b):
    return a + b
```

## Función con efecto colateral

Una función con efecto colateral modifica algo fuera de sí misma o interactúa con el entorno.

```python id="kfl8es"
def registrar(mensaje):
    print(mensaje)
```

No todas las funciones deben ser puras, pero conviene distinguir ambos casos.

## Diseño de funciones

Una buena función suele cumplir estas propiedades:

1. tiene una responsabilidad clara
2. tiene nombre descriptivo
3. recibe solo los argumentos necesarios
4. devuelve resultados útiles
5. evita depender innecesariamente de estado global
6. mantiene un tamaño razonable
7. resulta comprobable de forma aislada

## Errores comunes

## Confundir `print` con `return`

Problemático:

```python id="07d2am"
def sumar(a, b):
    print(a + b)
```

Si el resultado debe reutilizarse, corresponde retornar:

```python id="aqxlzt"
def sumar(a, b):
    return a + b
```

## Usar argumentos mutables por defecto

Problemático:

```python id="ibcmmt"
def agregar(valor, lista=[]):
    lista.append(valor)
    return lista
```

Forma correcta:

```python id="67pq67"
def agregar(valor, lista=None):
    if lista is None:
        lista = []

    lista.append(valor)
    return lista
```

## Crear funciones demasiado largas

Una función extensa con demasiadas tareas suele ser difícil de entender y mantener.

Es preferible dividirla en funciones más pequeñas con una responsabilidad bien definida.

## Depender excesivamente de variables globales

Problemático:

```python id="8wedj8"
impuesto = 0.18

def calcular_total(base):
    return base * (1 + impuesto)
```

Más claro:

```python id="jlwmqn"
def calcular_total(base, impuesto):
    return base * (1 + impuesto)
```

## Usar nombres poco expresivos

Problemático:

```python id="n7w3cb"
def f(x, y):
    return x + y
```

Más claro:

```python id="satlaf"
def calculate_sum(first_value, second_value):
    return first_value + second_value
```

## Buenas prácticas

## Encapsular una sola responsabilidad por función

```python id="cye1pb"
def validate_email(email):
    return "@" in email
```

## Elegir nombres descriptivos

```python id="c6nm8t"
def load_transactions():
    pass

def calculate_average():
    pass

def export_report():
    pass
```

## Retornar resultados en lugar de imprimirlos cuando deban reutilizarse

```python id="je8rr7"
def calculate_area(width, height):
    return width * height
```

## Evitar dependencias innecesarias de variables globales

```python id="9s6faw"
def calculate_total(amount, tax_rate):
    return amount * (1 + tax_rate)
```

## Usar `lambda` solo en casos breves y claros

```python id="8mkg29"
datos = ["bbb", "a", "cc"]
ordenados = sorted(datos, key=lambda texto: len(texto))
```

## Documentar cuando la función no sea trivial

```python id="gw51m5"
def convert_temperature(celsius: float) -> float:
    """Convierte grados Celsius a Fahrenheit."""
    return (celsius * 9 / 5) + 32
```

## Ejemplo integrado

```python id="x2ki01"
def calculate_discounted_total(amount, discount_rate=0.0, tax_rate=0.18):
    subtotal = amount * (1 - discount_rate)
    total = subtotal * (1 + tax_rate)
    return total


def format_currency(value):
    return f"S/ {value:.2f}"


def main():
    amount = 1000
    total = calculate_discounted_total(amount, discount_rate=0.10)

    print("Monto original:", format_currency(amount))
    print("Monto final:", format_currency(total))


if __name__ == "__main__":
    main()
```

## Orden didáctico interno

```text id="qlvtqo"
1. Definición de funciones con def
2. Parámetros y argumentos
3. Llamadas y retorno de valores
4. Parámetros posicionales y nombrados
5. Valores por defecto
6. *args y **kwargs
7. Desempaquetado en llamadas
8. Parámetros solo posicionales y solo nombrados
9. Funciones como objetos
10. Alcance de variables
11. global y nonlocal
12. Closures
13. lambda
14. Anotaciones de tipo
15. Diseño, errores comunes y buenas prácticas
```