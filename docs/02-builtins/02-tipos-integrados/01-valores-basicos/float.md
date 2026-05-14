El siguiente archivo debería ser:

```text
docs/02-builtins/02-tipos-integrados/float.md
```

Contenido propuesto:

````markdown
# `float`

## Propósito

`float` representa números reales en punto flotante. Se usa para expresar cantidades con parte decimal, magnitudes continuas, promedios, porcentajes, medidas, razones y resultados de operaciones que no permanecen en el dominio de los enteros.

## Naturaleza del tipo

`float` es un tipo numérico escalar e inmutable.

Esto significa que:

- representa un solo valor numérico
- no es una colección
- no puede modificarse internamente después de crearse
- cualquier operación que cambie su valor produce un nuevo objeto

## Forma general

Un valor de tipo `float` puede aparecer como literal:

```python
3.14
-0.5
0.0
2.5e3
````

o construirse explícitamente con:

```python
float(valor)
```

## Literales en punto flotante

Los valores decimales pueden escribirse directamente.

```python
a = 3.14
b = -2.5
c = 0.0

print(a)
print(b)
print(c)
```

Salida:

```text
3.14
-2.5
0.0
```

## Notación científica

`float` también admite notación científica.

```python
print(2.5e3)
print(1.2e-4)
```

Salida:

```text
2500.0
0.00012
```

Esto resulta útil cuando se trabaja con magnitudes muy grandes o muy pequeñas.

## Separadores visuales

Python permite usar guiones bajos para mejorar legibilidad.

```python
distancia = 1_234.567_89
print(distancia)
```

Salida:

```text
1234.56789
```

## Valor de retorno de `type()`

```python
print(type(3.14))
```

Salida:

```text
<class 'float'>
```

## Mutabilidad

`float` es inmutable.

```python
numero = 3.5
print(id(numero))

numero = numero + 1.0
print(numero)
print(id(numero))
```

Salida:

```text
4.5
```

La identidad del objeto cambia porque no se modifica el valor original. Se crea otro objeto y la variable pasa a referenciarlo.

## Operaciones aritméticas principales

## Suma

```python
print(2.5 + 1.2)
```

Salida:

```text
3.7
```

## Resta

```python
print(5.5 - 2.0)
```

Salida:

```text
3.5
```

## Multiplicación

```python
print(2.5 * 4)
```

Salida:

```text
10.0
```

## División real

```python
print(7.5 / 2.5)
```

Salida:

```text
3.0
```

## División entera

La división con `//` también puede aplicarse sobre flotantes.

```python
print(7.5 // 2.0)
```

Salida:

```text
3.0
```

El resultado representa la parte entera del cociente, pero el tipo puede seguir siendo `float`.

## Módulo o residuo

```python
print(7.5 % 2.0)
```

Salida:

```text
1.5
```

## Potencia

```python
print(2.5 ** 2)
```

Salida:

```text
6.25
```

## Operaciones mixtas con `int`

Cuando una operación combina `int` y `float`, el resultado suele ser `float`.

```python
print(10 + 2.5)
print(type(10 + 2.5))
```

Salida:

```text
12.5
<class 'float'>
```

## Comparación

Los valores `float` pueden compararse con operadores relacionales.

```python
print(3.5 > 2.0)
print(3.5 == 3.5)
print(1.2 != 1.3)
print(2.5 <= 2.5)
```

Salida:

```text
True
True
True
True
```

## Truthiness

En contexto booleano:

* `0.0` se evalúa como `False`
* cualquier valor distinto de `0.0` se evalúa como `True`

```python
print(bool(0.0))
print(bool(1.5))
print(bool(-0.25))
```

Salida:

```text
False
True
True
```

Ejemplo:

```python
valor = 0.0

if valor:
    print("Valor verdadero")
else:
    print("Valor falso")
```

Salida:

```text
Valor falso
```

## Conversión con `float()`

La función `float()` permite construir valores en punto flotante a partir de otros valores compatibles.

## Desde entero

```python
print(float(10))
print(float(-3))
```

Salida:

```text
10.0
-3.0
```

## Desde cadena numérica

```python
print(float("3.14"))
print(float("-2.5"))
print(float("10"))
```

Salida:

```text
3.14
-2.5
10.0
```

## Desde booleanos

```python
print(float(True))
print(float(False))
```

Salida:

```text
1.0
0.0
```

## Error de conversión

No toda cadena puede convertirse a `float`.

```python
print(float("hola"))
```

Error típico:

```text
ValueError
```

## Precisión en punto flotante

Los valores de tipo `float` no siempre representan exactamente todos los números decimales.

Ejemplo clásico:

```python
print(0.1 + 0.2)
```

Salida típica:

```text
0.30000000000000004
```

Esto no significa que Python esté sumando mal, sino que ciertos decimales no tienen representación binaria exacta en punto flotante.

## Comparación de flotantes

Por la naturaleza del punto flotante, no siempre conviene comparar resultados decimales con igualdad exacta.

Problemático:

```python
print(0.1 + 0.2 == 0.3)
```

Salida típica:

```text
False
```

En estos casos, suele ser mejor comparar con tolerancia. Ese criterio se verá con más detalle en la biblioteca estándar, especialmente con `math.isclose()`.

## Redondeo con `round()`

`round()` permite redondear flotantes.

```python
print(round(3.14159))
print(round(3.14159, 2))
```

Salida:

```text
3
3.14
```

## Valor absoluto con `abs()`

```python
print(abs(-3.5))
```

Salida:

```text
3.5
```

## División por cero

La división entre cero con flotantes también genera excepción.

```python
print(3.5 / 0.0)
```

Error típico:

```text
ZeroDivisionError
```

## Valores especiales

`float` puede representar ciertos valores especiales.

## Infinito

```python
valor = float("inf")
print(valor)
print(type(valor))
```

Salida:

```text
inf
<class 'float'>
```

## Menos infinito

```python
valor = float("-inf")
print(valor)
```

Salida:

```text
-inf
```

## NaN

`NaN` significa “not a number”.

```python
valor = float("nan")
print(valor)
```

Salida:

```text
nan
```

Estos valores especiales aparecen en contextos numéricos avanzados y en algunas operaciones con datos.

## Representación textual

Un `float` puede convertirse a texto con `str()` o formatearse con mayor control.

```python
valor = 3.14159265

print(str(valor))
print(f"{valor:.2f}")
print(f"{valor:.4f}")
```

Salida:

```text
3.14159265
3.14
3.1416
```

## Casos de uso frecuentes

## Precios y montos decimales

```python
precio = 19.99
cantidad = 3
total = precio * cantidad

print(total)
```

## Promedios

```python
notas = [15, 18, 14]
promedio = sum(notas) / len(notas)

print(promedio)
```

## Porcentajes

```python
monto = 200
tasa = 0.18
impuesto = monto * tasa

print(impuesto)
```

## Medidas continuas

```python
temperatura = 23.5
distancia = 12.75

print(temperatura)
print(distancia)
```

## Errores comunes

## Suponer que todos los decimales se representan exactamente

Problemático:

```python
print(0.1 + 0.2)
print(0.1 + 0.2 == 0.3)
```

Esto puede dar resultados inesperados si se espera exactitud decimal absoluta.

## Usar `float` para comparación exacta sin tolerancia

Problemático:

```python
a = 0.1 + 0.2
b = 0.3

print(a == b)
```

La igualdad exacta puede fallar por representación interna.

## Confundir `float()` con validación completa de formato

Problemático:

```python
float("3,14")
```

Esto genera `ValueError` porque el separador decimal válido en Python es el punto, no la coma.

## Esperar que `//` con flotantes devuelva siempre `int`

```python
print(7.5 // 2.0)
print(type(7.5 // 2.0))
```

Salida:

```text
3.0
<class 'float'>
```

## Usar `float` donde se necesita precisión decimal estricta

Para ciertos contextos sensibles, como cálculos monetarios muy exactos, `float` puede no ser la mejor herramienta. Ese caso pertenece más a módulos de biblioteca estándar como `decimal`.

## Buenas prácticas

## Usar `float` para magnitudes continuas

```python
temperatura = 23.5
velocidad = 88.7
```

## Tener presente la diferencia entre exactitud decimal y representación binaria

Los resultados decimales pueden requerir redondeo o comparación con tolerancia.

## Usar `round()` cuando se necesite presentación numérica más limpia

```python
promedio = round(16.666666, 2)
print(promedio)
```

## Usar formateo para mostrar resultados al usuario

```python
monto = 123.4567
print(f"{monto:.2f}")
```

## No usar `float()` como validación única cuando la entrada sea incierta

Si la entrada puede venir mal formada, conviene validarla o manejar excepciones.

## Ejemplo integrado

```python
def show_grade_summary(scores):
    average = sum(scores) / len(scores) if scores else 0.0
    maximum = max(scores) if scores else 0.0
    minimum = min(scores) if scores else 0.0

    print("Resumen de notas")
    print("-" * 30)
    print("Notas:", scores)
    print("Promedio:", round(average, 2))
    print("Máxima:", maximum)
    print("Mínima:", minimum)


show_grade_summary([15.5, 18.0, 14.75, 16.25])
```

Salida aproximada:

```text
Resumen de notas
------------------------------
Notas: [15.5, 18.0, 14.75, 16.25]
Promedio: 16.12
Máxima: 18.0
Mínima: 14.75
```

## Relación con otros elementos integrados

`float` se relaciona especialmente con:

* `int` y `bool` dentro de los tipos numéricos y lógicos
* `round()`, `abs()`, `pow()` y `divmod()` en cálculo básico
* `sum()`, `min()` y `max()` en agregación numérica
* formateo textual mediante `str()` y f-strings
* comparación numérica y truthiness

## Orden didáctico interno

```text
1. Propósito de float
2. Naturaleza del tipo
3. Literales y notación científica
4. Mutabilidad
5. Operaciones aritméticas
6. Comparación y truthiness
7. Conversión con float()
8. Precisión en punto flotante
9. Valores especiales
10. Errores comunes
11. Buenas prácticas
```
