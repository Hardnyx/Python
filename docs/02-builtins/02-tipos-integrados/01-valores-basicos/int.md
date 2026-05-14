# `int`

## Propósito

`int` representa números enteros, es decir, valores numéricos sin parte decimal. Es uno de los tipos integrados más importantes de Python y se usa en conteos, índices, cantidades, operaciones aritméticas, control de flujo y manipulación de datos numéricos discretos.

## Naturaleza del tipo

`int` es un tipo numérico escalar e inmutable.

Esto significa que:

- representa un solo valor numérico
- no es una colección
- no puede modificarse internamente después de crearse
- cualquier cambio produce un nuevo objeto

## Forma general

Un entero puede aparecer como literal:

```python
10
-5
0
1000000
````

o construirse explícitamente con:

```python
int(valor)
```

## Literales enteros

Los enteros pueden escribirse directamente en el código.

```python id="f6j8td"
a = 10
b = -25
c = 0

print(a)
print(b)
print(c)
```

Salida:

```text id="lbm5xq"
10
-25
0
```

## Signo

Un entero puede ser:

* positivo
* negativo
* cero

```python id="j8g9ky"
print(15)
print(-15)
print(0)
```

## Separadores visuales

Python permite usar guiones bajos para mejorar legibilidad en números grandes.

```python id="v2y4rm"
poblacion = 1_250_000
print(poblacion)
```

Salida:

```text id="a3n7wc"
1250000
```

El guion bajo no afecta el valor numérico.

## Valor de retorno de `type()`

```python id="e9q2hj"
print(type(10))
```

Salida:

```text id="r5m8zk"
<class 'int'>
```

## Mutabilidad

`int` es inmutable.

Ejemplo:

```python id="x7w3nf"
numero = 10
print(id(numero))

numero = numero + 1
print(numero)
print(id(numero))
```

Salida:

```text id="k4p1ye"
11
```

La identidad del objeto cambia, porque no se modificó el entero original. Se creó otro entero y la variable pasó a referenciarlo.

## Operaciones aritméticas principales

## Suma

```python id="m1y8tr"
print(10 + 5)
```

Salida:

```text id="c7v3qa"
15
```

## Resta

```python id="q6r2ks"
print(10 - 5)
```

Salida:

```text id="u4p9jm"
5
```

## Multiplicación

```python id="d3w7ht"
print(10 * 5)
```

Salida:

```text id="v6m1qx"
50
```

## División real

La división con `/` devuelve `float`, incluso si la división es exacta.

```python id="n2p4yb"
print(10 / 2)
print(type(10 / 2))
```

Salida:

```text id="s8q5cn"
5.0
<class 'float'>
```

## División entera

La división con `//` devuelve la parte entera del cociente.

```python id="t5v9rd"
print(10 // 3)
print(10 // 2)
```

Salida:

```text id="m7k2wp"
3
5
```

## Módulo o residuo

```python id="h4q8jx"
print(10 % 3)
```

Salida:

```text id="y1n6fc"
1
```

## Potencia

```python id="p8w3kg"
print(2 ** 3)
```

Salida:

```text id="b2r7mt"
8
```

## Precedencia básica

```python id="g6t2xv"
print(2 + 3 * 4)
print((2 + 3) * 4)
```

Salida:

```text id="e5q9wn"
14
20
```

## Enteros y comparación

Los enteros pueden compararse con operadores relacionales.

```python id="u7w4pl"
print(10 > 5)
print(10 == 10)
print(10 != 3)
print(4 <= 4)
```

Salida:

```text id="n4v8ys"
True
True
True
True
```

Estas comparaciones producen valores de tipo `bool`.

## Enteros y truthiness

En contexto booleano:

* `0` se evalúa como `False`
* cualquier entero distinto de `0` se evalúa como `True`

```python id="z1m7kd"
print(bool(0))
print(bool(1))
print(bool(-8))
```

Salida:

```text id="q2w9jr"
False
True
True
```

Ejemplo:

```python id="k9p3xf"
numero = 0

if numero:
    print("Valor verdadero")
else:
    print("Valor falso")
```

Salida:

```text id="w5r1tn"
Valor falso
```

## Conversión con `int()`

La función `int()` permite construir enteros a partir de otros valores compatibles.

## Desde cadena numérica

```python id="m3y8qv"
print(int("10"))
print(int("-25"))
```

Salida:

```text id="s6v2pk"
10
-25
```

## Desde flotante

```python id="r7n4bj"
print(int(3.9))
print(int(-3.9))
```

Salida:

```text id="h1q8xm"
3
-3
```

`int()` no redondea. Trunca la parte decimal hacia cero.

## Desde booleanos

```python id="j5w2tp"
print(int(True))
print(int(False))
```

Salida:

```text id="v3k7nc"
1
0
```

## Error de conversión

No toda cadena puede convertirse a entero.

```python id="c8m1ry"
print(int("hola"))
```

Error típico:

```text id="x4q9jd"
ValueError
```

## Conversión con base

`int()` puede interpretar cadenas en distintas bases.

Forma general:

```python
int(texto, base)
```

Ejemplo en base 2:

```python id="b7p4vf"
print(int("1010", 2))
```

Salida:

```text id="n6w3ky"
10
```

Ejemplo en base 16:

```python id="u9r2mc"
print(int("FF", 16))
```

Salida:

```text id="e1q7hz"
255
```

## Representación en distintas bases

Un entero puede escribirse en varias bases numéricas.

## Decimal

```python id="m2w6rb"
numero = 10
print(numero)
```

## Binario

Prefijo `0b`.

```python id="k5v9jq"
numero = 0b1010
print(numero)
```

Salida:

```text id="p4n1xs"
10
```

## Octal

Prefijo `0o`.

```python id="t7q3wk"
numero = 0o12
print(numero)
```

Salida:

```text id="c9m6yv"
10
```

## Hexadecimal

Prefijo `0x`.

```python id="w3r8pn"
numero = 0xA
print(numero)
```

Salida:

```text id="j2v5kq"
10
```

## Representación con funciones integradas

```python id="v8q1tm"
numero = 26

print(bin(numero))
print(oct(numero))
print(hex(numero))
```

Salida:

```text id="x7p4rd"
0b11010
0o32
0x1a
```

## Enteros y funciones integradas relacionadas

## `abs()`

Devuelve el valor absoluto.

```python id="u5m9yn"
print(abs(-10))
```

Salida:

```text id="b1q7wf"
10
```

## `pow()`

Calcula potencias.

```python id="y2r4kc"
print(pow(2, 3))
```

Salida:

```text id="m9w1jq"
8
```

## `divmod()`

Devuelve cociente y residuo.

```python id="h6q8tv"
print(divmod(10, 3))
```

Salida:

```text id="r3n5px"
(3, 1)
```

## `round()`

Aunque normalmente se usa con `float`, también puede recibir enteros.

```python id="c4w7mq"
print(round(10))
```

Salida:

```text id="k8v2yr"
10
```

## Orden y comparación con otros numéricos

Los enteros pueden operar y compararse con flotantes.

```python id="q7m3wd"
print(10 + 2.5)
print(10 > 2.5)
```

Salida:

```text id="n5r1xj"
12.5
True
```

Cuando se mezcla `int` con `float`, el resultado suele convertirse a `float` si la operación lo requiere.

## Enteros grandes

Python permite enteros de tamaño arbitrario, limitados en la práctica por la memoria disponible.

```python id="m1t8qk"
numero = 10 ** 50
print(numero)
```

Esto es una diferencia importante frente a lenguajes con enteros de tamaño fijo.

## Casos de uso frecuentes

## Contadores

```python id="z3v6wp"
contador = 0

for _ in range(5):
    contador += 1

print(contador)
```

## Índices

```python id="g2m9rb"
nombres = ["Ana", "Luis", "Marta"]

for i in range(len(nombres)):
    print(i, nombres[i])
```

## Cantidades discretas

```python id="p6q1yn"
cantidad_productos = 15
print(cantidad_productos)
```

## Potencias y combinaciones simples

```python id="r4w8km"
print(2 ** 10)
print(15 % 4)
print(15 // 4)
```

## Errores comunes

## Confundir `int()` con redondeo

Problemático:

```python id="v1m4tx"
print(int(3.9))
```

Salida:

```text id="x8q2wc"
3
```

`int()` no redondea a `4`. Elimina la parte decimal hacia cero.

## Suponer que `/` devuelve entero

Problemático:

```python id="n7r3vp"
print(10 / 2)
print(type(10 / 2))
```

Salida:

```text id="m5q9jd"
5.0
<class 'float'>
```

Si se necesita cociente entero, corresponde `//`.

## Intentar convertir una cadena no numérica

Problemático:

```python id="t2w6yn"
int("abc")
```

Esto genera `ValueError`.

## Confundir valor y tipo booleano

```python id="g9q4mb"
print(type(True))
print(isinstance(True, int))
```

Salida:

```text id="c1r8xp"
<class 'bool'>
True
```

`bool` es un tipo distinto, aunque esté relacionado con `int`.

## Usar enteros donde corresponde texto

Problemático:

```python id="u4w1kq"
edad = 20
print("Edad: " + edad)
```

Esto genera `TypeError`.

Forma correcta:

```python id="m8p2rv"
edad = 20
print("Edad:", edad)
```

o:

```python id="w6q9tn"
edad = 20
print(f"Edad: {edad}")
```

## Buenas prácticas

## Usar `int` para cantidades discretas

```python id="e3r7vk"
numero_estudiantes = 25
```

## Usar `//` y `%` cuando interese cociente entero y residuo

```python id="j1w8pm"
cociente = 17 // 5
residuo = 17 % 5
```

## No usar `int()` como sustituto de redondeo

Si se necesita redondear, corresponde `round()` u otra estrategia explícita.

## Usar separadores visuales en enteros grandes cuando mejore la legibilidad

```python id="r9m2xq"
presupuesto = 2_500_000
```

## Validar antes de convertir cadenas cuando el origen sea incierto

Si la entrada puede no ser numérica, conviene usar validación o manejo de excepciones.

## Ejemplo integrado

```python id="y4q8nk"
def show_purchase_summary(unit_price, quantity):
    subtotal = unit_price * quantity
    packages = quantity // 6
    remaining = quantity % 6

    print("Resumen de compra")
    print("-" * 30)
    print("Precio unitario:", unit_price)
    print("Cantidad:", quantity)
    print("Subtotal:", subtotal)
    print("Paquetes de 6:", packages)
    print("Sobrantes:", remaining)


show_purchase_summary(25, 17)
```

Salida aproximada:

```text id="n2w6rm"
Resumen de compra
------------------------------
Precio unitario: 25
Cantidad: 17
Subtotal: 425
Paquetes de 6: 2
Sobrantes: 5
```

## Relación con otros elementos integrados

`int` se relaciona especialmente con:

* `float` y `bool` dentro de los tipos numéricos y lógicos
* `abs()`, `round()`, `pow()` y `divmod()` en cálculo básico
* `bin()`, `oct()` y `hex()` para representación en otras bases
* `range()` en recorridos enteros
* `sum()`, `min()` y `max()` en agregación numérica

## Orden didáctico interno

```text id="z8m4vq"
1. Propósito de int
2. Naturaleza del tipo
3. Literales enteros
4. Mutabilidad
5. Operaciones aritméticas
6. Comparación y truthiness
7. Conversión con int()
8. Bases numéricas
9. Funciones relacionadas
10. Errores comunes
11. Buenas prácticas
```