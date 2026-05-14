# `bool`

## Propósito

`bool` representa valores lógicos. Se utiliza para expresar verdad o falsedad en condiciones, comparaciones, validaciones y control de flujo.

## Naturaleza del tipo

`bool` es un tipo numérico lógico e inmutable.

Esto significa que:

- representa uno de dos valores posibles
- no es una colección
- no puede modificarse internamente después de crearse
- cualquier cambio produce un nuevo objeto

Los únicos valores booleanos directos son:

```python
True
False
````

## Forma general

Un valor booleano puede aparecer como literal:

```python
True
False
```

o surgir como resultado de:

* comparaciones
* operaciones lógicas
* conversiones con `bool()`
* evaluación de objetos en contexto condicional

## Literales booleanos

```python
activo = True
disponible = False

print(activo)
print(disponible)
```

Salida:

```text
True
False
```

## Valor de retorno de `type()`

```python
print(type(True))
print(type(False))
```

Salida:

```text
<class 'bool'>
<class 'bool'>
```

## Mutabilidad

`bool` es inmutable.

```python
valor = True
print(id(valor))

valor = False
print(valor)
print(id(valor))
```

Salida:

```text
False
```

La variable deja de referenciar el valor anterior y pasa a referenciar otro.

## Origen de los valores booleanos

## Comparaciones

Las comparaciones producen valores booleanos.

```python
print(10 > 5)
print(10 == 10)
print(10 != 3)
print(4 <= 2)
```

Salida:

```text
True
True
True
False
```

## Operadores lógicos

Los operadores lógicos también producen resultados booleanos.

```python
print(True and True)
print(True and False)
print(True or False)
print(not True)
```

Salida:

```text
True
False
True
False
```

## Conversión con `bool()`

La función `bool()` convierte un valor a su interpretación lógica.

```python
print(bool(1))
print(bool(0))
print(bool("Hola"))
print(bool(""))
```

Salida:

```text
True
False
True
False
```

## `bool()` y truthiness

Muchos objetos en Python pueden evaluarse como verdaderos o falsos.

Regla general:

* valores vacíos o nulos suelen evaluarse como `False`
* valores no vacíos o distintos de cero suelen evaluarse como `True`

## Valores comunes que se evalúan como `False`

```python
print(bool(False))
print(bool(None))
print(bool(0))
print(bool(0.0))
print(bool(""))
print(bool([]))
print(bool(()))
print(bool({}))
print(bool(set()))
```

Salida:

```text
False
False
False
False
False
False
False
False
False
```

## Valores comunes que se evalúan como `True`

```python
print(bool(True))
print(bool(1))
print(bool(-3))
print(bool("Python"))
print(bool([1, 2]))
print(bool({"a": 1}))
```

Salida:

```text
True
True
True
True
True
True
```

## Uso en condiciones

`bool` está en la base de estructuras como `if`, `while` y expresiones condicionales.

```python
edad = 20

if edad >= 18:
    print("Mayor de edad")
else:
    print("Menor de edad")
```

Salida:

```text
Mayor de edad
```

La condición `edad >= 18` produce un valor booleano.

## Operadores de comparación

Los operadores de comparación más comunes son:

| Operador | Significado       |
| -------- | ----------------- |
| `==`     | igual a           |
| `!=`     | distinto de       |
| `>`      | mayor que         |
| `<`      | menor que         |
| `>=`     | mayor o igual que |
| `<=`     | menor o igual que |

Ejemplo:

```python
a = 10
b = 5

print(a == b)
print(a != b)
print(a > b)
print(a < b)
print(a >= b)
print(a <= b)
```

Salida:

```text
False
True
True
False
True
False
```

## Operadores lógicos

## `and`

Devuelve verdadero solo si ambas condiciones son verdaderas.

```python
print(True and True)
print(True and False)
```

Salida:

```text
True
False
```

## `or`

Devuelve verdadero si al menos una condición es verdadera.

```python
print(True or False)
print(False or False)
```

Salida:

```text
True
False
```

## `not`

Invierte el valor lógico.

```python
print(not True)
print(not False)
```

Salida:

```text
False
True
```

## Precedencia básica

```python
print(True or False and False)
print((True or False) and False)
```

Salida:

```text
True
False
```

`and` tiene mayor precedencia que `or`.

## `bool` como subtipo de `int`

En Python, `bool` es un subtipo de `int`.

```python
print(isinstance(True, bool))
print(isinstance(True, int))
```

Salida:

```text
True
True
```

También puede observarse en operaciones aritméticas:

```python
print(True + True)
print(True + False)
print(False + False)
```

Salida:

```text
2
1
0
```

Esto ocurre porque internamente:

```text
True  -> 1
False -> 0
```

## Conversión con `int()` y `float()`

```python
print(int(True))
print(int(False))
print(float(True))
print(float(False))
```

Salida:

```text
1
0
1.0
0.0
```

## Igualdad y relación con enteros

```python
print(True == 1)
print(False == 0)
print(True == 2)
```

Salida:

```text
True
True
False
```

Esto no implica que `bool` e `int` sean exactamente el mismo tipo.

```python
print(type(True))
print(type(1))
print(type(True) == type(1))
```

Salida:

```text
<class 'bool'>
<class 'int'>
False
```

## `bool()` sobre colecciones y secuencias

```python
print(bool([1, 2, 3]))
print(bool([]))
print(bool((1, 2)))
print(bool(()))
print(bool({"a": 1}))
print(bool({}))
```

Salida:

```text
True
False
True
False
True
False
```

## `bool()` sobre texto

```python
print(bool("Hola"))
print(bool(" "))
print(bool(""))
```

Salida:

```text
True
True
False
```

Una cadena con espacio sigue siendo no vacía, por lo tanto se evalúa como verdadera.

## `bool()` sobre números

```python
print(bool(100))
print(bool(-5))
print(bool(0))
print(bool(0.0))
```

Salida:

```text
True
True
False
False
```

## Uso en conteo de condiciones

Como `True` equivale a `1` y `False` a `0`, se pueden contar condiciones verdaderas con `sum()`.

```python
valores = [10, 15, 8, 20, 3]
cantidad = sum(valor > 10 for valor in valores)

print(cantidad)
```

Salida:

```text
2
```

## Uso en expresiones condicionales

```python
edad = 17
mensaje = "Mayor" if edad >= 18 else "Menor"

print(mensaje)
```

Salida:

```text
Menor
```

La condición central de la expresión es booleana.

## Casos de uso frecuentes

## Validaciones

```python
correo = "usuario@dominio.com"
tiene_arroba = "@" in correo

print(tiene_arroba)
```

## Control de flujo

```python
saldo = 100

if saldo > 0:
    print("Saldo disponible")
```

## Banderas de estado

```python
proceso_completado = False

if not proceso_completado:
    print("Proceso pendiente")
```

## Conteos lógicos

```python
notas = [8, 12, 15, 9, 18]
aprobados = sum(nota >= 11 for nota in notas)

print(aprobados)
```

## Errores comunes

## Confundir `=` con `==`

Problemático:

```python
if x = 10:
    print("Valor")
```

Esto genera error de sintaxis. Para comparar se usa `==`.

Correcto:

```python
if x == 10:
    print("Valor")
```

## Suponer que `bool("False")` da `False`

```python
print(bool("False"))
```

Salida:

```text
True
```

La cadena `"False"` no está vacía, por lo que se evalúa como verdadera.

## Suponer que `bool([0])` da `False`

```python
print(bool([0]))
```

Salida:

```text
True
```

La lista contiene un elemento, así que no está vacía.

## Confundir valor booleano con tipo exacto entero

```python
print(True == 1)
print(type(True) == int)
```

Salida:

```text
True
False
```

## Usar comparaciones innecesarias con `True` o `False`

Menos idiomático:

```python
if activo == True:
    print("Activo")
```

Más idiomático:

```python
if activo:
    print("Activo")
```

Y para el caso negativo:

```python
if not activo:
    print("Inactivo")
```

## Buenas prácticas

## Usar condiciones directas cuando el valor ya es booleano

```python
if disponible:
    print("Disponible")
```

## Usar `not` para expresar negación de forma clara

```python
if not errores:
    print("Sin errores")
```

## Evitar comparaciones explícitas con `True` y `False` salvo que sea realmente necesario

```python
if resultado:
    ...
```

suele ser mejor que:

```python
if resultado == True:
    ...
```

## Recordar que muchos objetos se evalúan por truthiness

Esto permite escribir código más idiomático:

```python
if nombres:
    print("Hay nombres")
```

en lugar de:

```python
if len(nombres) > 0:
    print("Hay nombres")
```

## Ejemplo integrado

```python
def show_access_status(age, has_id, balance):
    is_adult = age >= 18
    can_enter = is_adult and has_id
    has_balance = bool(balance)

    print("Estado de acceso")
    print("-" * 30)
    print("Mayor de edad:", is_adult)
    print("Tiene documento:", has_id)
    print("Puede ingresar:", can_enter)
    print("Tiene saldo:", has_balance)


show_access_status(20, True, 150)
```

Salida aproximada:

```text
Estado de acceso
------------------------------
Mayor de edad: True
Tiene documento: True
Puede ingresar: True
Tiene saldo: True
```

## Relación con otros elementos integrados

`bool` se relaciona especialmente con:

* `int` y `float` dentro de los tipos numéricos
* comparaciones y operadores lógicos
* `all()` y `any()` en evaluación lógica de iterables
* `sum()` cuando se cuentan condiciones verdaderas
* estructuras de control como `if` y `while`

## Orden didáctico interno

```text
1. Propósito de bool
2. Naturaleza del tipo
3. Literales booleanos
4. Comparaciones y operadores lógicos
5. Conversión con bool()
6. Truthiness
7. Relación con int
8. Uso en condiciones y validaciones
9. Errores comunes
10. Buenas prácticas
```