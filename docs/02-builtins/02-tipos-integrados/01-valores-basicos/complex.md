# `complex`

## Propósito

`complex` representa números complejos. Se utiliza cuando un valor tiene una parte real y una parte imaginaria, especialmente en contextos matemáticos, físicos, de ingeniería y procesamiento de señales.

## Naturaleza del tipo

`complex` es un tipo numérico escalar e inmutable.

Esto significa que:

- representa un solo valor numérico
- no es una colección
- no puede modificarse internamente después de crearse
- cualquier operación produce un nuevo objeto

Un número complejo tiene la forma general:

```text
a + bj
````

donde:

* `a` es la parte real
* `b` es la parte imaginaria
* `j` representa la unidad imaginaria en Python

## Forma general

Un valor de tipo `complex` puede escribirse como literal:

```python
2 + 3j
-1 + 0.5j
4j
```

o construirse explícitamente con:

```python
complex(real, imag)
```

## Literales complejos

```python
z1 = 2 + 3j
z2 = -1 + 0.5j
z3 = 4j

print(z1)
print(z2)
print(z3)
```

Salida:

```text
(2+3j)
(-1+0.5j)
4j
```

## Valor de retorno de `type()`

```python
print(type(2 + 3j))
```

Salida:

```text
<class 'complex'>
```

## Construcción con `complex()`

La función `complex()` permite crear números complejos a partir de parte real e imaginaria.

```python
z = complex(2, 3)

print(z)
print(type(z))
```

Salida:

```text
(2+3j)
<class 'complex'>
```

También puede construirse solo con la parte real:

```python
print(complex(5))
```

Salida:

```text
(5+0j)
```

## Parte real y parte imaginaria

Un número complejo expone sus componentes mediante los atributos `.real` e `.imag`.

```python
z = 2 + 3j

print(z.real)
print(z.imag)
```

Salida:

```text
2.0
3.0
```

Ambos se devuelven como `float`.

## Mutabilidad

`complex` es inmutable.

```python
z = 2 + 3j
print(id(z))

z = z + 1
print(z)
print(id(z))
```

Salida:

```text
(3+3j)
```

La identidad cambia porque se crea un nuevo objeto.

## Operaciones aritméticas principales

## Suma

```python
z1 = 2 + 3j
z2 = 1 + 2j

print(z1 + z2)
```

Salida:

```text
(3+5j)
```

## Resta

```python
z1 = 2 + 3j
z2 = 1 + 2j

print(z1 - z2)
```

Salida:

```text
(1+1j)
```

## Multiplicación

```python
z1 = 2 + 3j
z2 = 1 + 2j

print(z1 * z2)
```

Salida:

```text
(-4+7j)
```

## División

```python
z1 = 2 + 3j
z2 = 1 + 2j

print(z1 / z2)
```

Salida aproximada:

```text
(1.6-0.2j)
```

## Potencia

```python
z = 1 + 1j

print(z ** 2)
```

Salida:

```text
2j
```

## Operaciones mixtas con `int` y `float`

Los números complejos pueden operar con enteros y flotantes.

```python
z = 2 + 3j

print(z + 5)
print(z + 2.5)
print(z * 2)
```

Salida:

```text
(7+3j)
(4.5+3j)
(4+6j)
```

## Conjugado

Todo número complejo tiene conjugado. Python lo expone con el método `conjugate()`.

```python
z = 2 + 3j

print(z.conjugate())
```

Salida:

```text
(2-3j)
```

## Comparación

Los números complejos admiten igualdad y desigualdad.

```python
z1 = 2 + 3j
z2 = 2 + 3j
z3 = 1 + 3j

print(z1 == z2)
print(z1 != z3)
```

Salida:

```text
True
True
```

No admiten comparaciones de orden como `<`, `>`, `<=` o `>=`.

```python
print((2 + 3j) > (1 + 2j))
```

Error típico:

```text
TypeError
```

## Truthiness

En contexto booleano:

* `0j` se evalúa como `False`
* cualquier número complejo distinto de cero se evalúa como `True`

```python
print(bool(0j))
print(bool(2 + 0j))
print(bool(0 + 3j))
```

Salida:

```text
False
True
True
```

## Conversión con `complex()`

La función `complex()` permite convertir algunos valores compatibles.

## Desde entero

```python
print(complex(10))
```

Salida:

```text
(10+0j)
```

## Desde flotante

```python
print(complex(3.5))
```

Salida:

```text
(3.5+0j)
```

## Desde cadena

```python
print(complex("2+3j"))
print(complex("5j"))
print(complex("4"))
```

Salida:

```text
(2+3j)
5j
(4+0j)
```

## Error de conversión

No toda cadena puede convertirse correctamente.

```python
print(complex("hola"))
```

Error típico:

```text
ValueError
```

## Relación con otros tipos numéricos

`complex` pertenece a la familia numérica junto con `int` y `float`.

Puede combinarse con ellos en operaciones, pero el resultado pasa al dominio complejo cuando corresponde.

```python
print(type(2 + 3j))
print(type((2 + 3j) + 5))
print(type((2 + 3j) + 1.5))
```

Salida:

```text
<class 'complex'>
<class 'complex'>
<class 'complex'>
```

## Valor absoluto

La función `abs()` sobre un número complejo devuelve su módulo.

```python
z = 3 + 4j

print(abs(z))
```

Salida:

```text
5.0
```

Esto corresponde a:

```text
sqrt(a² + b²)
```

para un número complejo `a + bj`.

## No uso de `round()` como operación natural

`round()` no opera directamente sobre `complex`.

```python
z = 2 + 3j
print(round(z))
```

Error típico:

```text
TypeError
```

Si se requiere redondear, debe hacerse sobre `z.real` y `z.imag` por separado.

## No uso de `//` ni `%`

Las operaciones de división entera y módulo no están definidas para complejos.

```python
z = 2 + 3j
print(z // 2)
```

Error típico:

```text
TypeError
```

## Casos de uso frecuentes

## Representar una magnitud con componente real e imaginaria

```python
z = 2 + 3j
print(z)
```

## Separar componentes

```python
z = -1 + 4j

print(z.real)
print(z.imag)
```

## Calcular módulo

```python
z = 6 + 8j
print(abs(z))
```

## Obtener conjugado

```python
z = 5 - 2j
print(z.conjugate())
```

## Errores comunes

## Usar `i` en lugar de `j`

Problemático:

```python
z = 2 + 3i
```

Eso no es válido en Python. Debe usarse `j`.

Correcto:

```python
z = 2 + 3j
```

## Intentar ordenar números complejos

Problemático:

```python
print((2 + 3j) > (1 + 1j))
```

Los números complejos no tienen orden natural en Python.

## Suponer que `.real` y `.imag` devuelven enteros

```python
z = 2 + 3j

print(type(z.real))
print(type(z.imag))
```

Salida:

```text
<class 'float'>
<class 'float'>
```

## Confundir `abs(z)` con la parte real

`abs(z)` devuelve el módulo, no `z.real`.

```python
z = 3 + 4j

print(abs(z))
print(z.real)
```

Salida:

```text
5.0
3.0
```

## Buenas prácticas

## Usar `complex` solo cuando el problema realmente pertenezca al dominio complejo

Para cantidades ordinarias sin componente imaginaria, suele corresponder `int` o `float`.

## Usar `.real`, `.imag` y `conjugate()` cuando se necesite trabajar por componentes

```python
z = 2 + 3j
parte_real = z.real
parte_imaginaria = z.imag
conjugado = z.conjugate()
```

## Usar `abs()` para obtener el módulo

```python
modulo = abs(3 + 4j)
```

## No intentar aplicar comparaciones de orden

Los complejos solo admiten comparación de igualdad y desigualdad.

## Ejemplo integrado

```python
def show_complex_summary(z):
    print("Resumen de número complejo")
    print("Parte real:", z.real)
    print("Parte imaginaria:", z.imag)
    print("Conjugado:", z.conjugate())
    print("Módulo:", abs(z))
    print("Es no nulo:", bool(z))


show_complex_summary(3 + 4j)
```

Salida aproximada:

```text
Resumen de número complejo
Parte real: 3.0
Parte imaginaria: 4.0
Conjugado: (3-4j)
Módulo: 5.0
Es no nulo: True
```

## Relación con otros elementos integrados

`complex` se relaciona especialmente con:

* `int` y `float` dentro de la familia numérica
* `abs()` para cálculo del módulo
* `type()` e `isinstance()` para inspección
* `bool()` para truthiness
* operaciones aritméticas numéricas

## Orden didáctico interno

```text
1. Propósito de complex
2. Naturaleza del tipo
3. Literales y construcción con complex()
4. Parte real e imaginaria
5. Operaciones aritméticas
6. Conjugado y módulo
7. Comparación y truthiness
8. Errores comunes
9. Buenas prácticas
```