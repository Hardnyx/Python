# `print()`

## Propósito

`print()` permite enviar una representación textual de uno o varios objetos a una salida de texto. Su uso más frecuente es mostrar resultados en consola, pero también puede escribir en otros flujos que acepten texto.

## Forma general

```python
print(*objects, sep=' ', end='\n', file=None, flush=False)
````

## Idea central

`print()` toma los objetos recibidos, obtiene una representación textual de cada uno, los une usando un separador y finalmente escribe el resultado en el flujo indicado.

En ausencia de configuración adicional:

* los objetos se separan con un espacio
* la salida termina con salto de línea
* el destino es la salida estándar

## Parámetros

## `*objects`

Representa cero o más objetos a imprimir.

```python id="uq3c39"
print("Hola")
print("Ana", 20, True)
print([1, 2, 3])
```

Salida:

```text id="yd8p0x"
Hola
Ana 20 True
[1, 2, 3]
```

Cada objeto se convierte a texto antes de imprimirse.

## `sep`

Define el separador entre los objetos.

Valor por defecto:

```python id="n1u5au"
' '
```

Ejemplo:

```python id="9jd85c"
print("2026", "05", "09", sep="-")
```

Salida:

```text id="no6fbl"
2026-05-09
```

También puede usarse una cadena vacía:

```python id="gqv7q6"
print("A", "B", "C", sep="")
```

Salida:

```text id="85loq8"
ABC
```

## `end`

Define cómo termina la salida.

Valor por defecto:

```python id="xgm8l1"
'\n'
```

Ejemplo:

```python id="1w8ih0"
print("Carga", end="...")
print("completa")
```

Salida:

```text id="q78c3g"
Carga...completa
```

Es útil cuando se desea evitar el salto de línea final o personalizar el terminador.

## `file`

Indica el flujo de salida.

Por defecto, `print()` escribe en la salida estándar. También puede escribirse en un archivo u otro objeto que admita texto.

Ejemplo:

```python id="b7grsp"
with open("salida.txt", "w", encoding="utf-8") as file:
    print("Hola", file=file)
    print("Línea 2", file=file)
```

Esto escribe el contenido en `salida.txt` en lugar de mostrarlo en consola.

## `flush`

Si vale `True`, fuerza el vaciado inmediato del búfer de salida.

Ejemplo:

```python id="smh2sz"
print("Procesando...", end="", flush=True)
```

Esto es útil en procesos largos, barras de progreso simples o salidas que deben aparecer inmediatamente.

## Comportamiento general

## Sin argumentos

Si se llama sin argumentos, imprime una línea en blanco.

```python id="qw9cyj"
print("Línea 1")
print()
print("Línea 3")
```

Salida:

```text id="u9ui0m"
Línea 1

Línea 3
```

## Con varios argumentos

Cuando se pasan varios argumentos, `print()` los separa usando `sep`.

```python id="8z9xeh"
print("Nombre:", "Ana", "Edad:", 20)
```

Salida:

```text id="t8e65f"
Nombre: Ana Edad: 20
```

## Conversión implícita a texto

`print()` convierte automáticamente a texto los objetos recibidos.

```python id="6rv8p2"
print(10)
print(3.14)
print(True)
print(None)
```

Salida:

```text id="iqjlwm"
10
3.14
True
None
```

No es necesario convertirlos manualmente con `str()` para poder imprimirlos.

## Diferencia entre `print()` y `return`

`print()` muestra información.

`return` devuelve un valor desde una función.

Ejemplo:

```python id="z0gmj1"
def show_sum(a, b):
    print(a + b)

def calculate_sum(a, b):
    return a + b

x = show_sum(2, 3)
y = calculate_sum(2, 3)

print("x =", x)
print("y =", y)
```

Salida:

```text id="rrvmhe"
5
x = None
y = 5
```

Cuando el resultado debe reutilizarse, corresponde usar `return`, no solo `print()`.

## Uso con expresiones

`print()` puede recibir expresiones directamente.

```python id="w8xv3x"
print(2 + 3)
print(len("Python"))
print(10 > 3)
```

Salida:

```text id="d24w4s"
5
6
True
```

## Uso con f-strings

Una forma habitual y clara de construir mensajes es usar f-strings.

```python id="0f53zy"
name = "Ana"
age = 20

print(f"Nombre: {name}, Edad: {age}")
```

Salida:

```text id="2ku650"
Nombre: Ana, Edad: 20
```

Esto suele resultar más legible que mezclar muchos argumentos o concatenaciones.

## Casos de uso frecuentes

## Mostrar resultados simples

```python id="w1gk5s"
total = 150
print("Total:", total)
```

## Mostrar mensajes de estado

```python id="sz3ya8"
print("Archivo cargado correctamente")
```

## Depuración básica

```python id="2rmr3e"
value = 42
print("Valor actual:", value)
```

## Escritura en archivo

```python id="3qjlwm"
with open("log.txt", "w", encoding="utf-8") as file:
    print("Proceso iniciado", file=file)
```

## Separadores personalizados

```python id="asjlwm"
print("10", "20", "30", sep=" | ")
```

Salida:

```text id="l3jlwm"
10 | 20 | 30
```

## Control del terminador

```python id="9jlwmn"
for i in range(3):
    print(i, end=" ")
```

Salida:

```text id="vjlwmz"
0 1 2 
```

## Errores comunes

## Confundir `print()` con `return`

Problemático:

```python id="otjlwm"
def calculate_total(a, b):
    print(a + b)
```

Si se necesita reutilizar el valor, esta función no es adecuada tal como está.

## Concatenar cadenas con tipos no textuales

Problemático:

```python id="jlwm0f"
age = 20
print("Edad: " + age)
```

Esto genera `TypeError`.

Correcto:

```python id="jlwm8u"
age = 20
print("Edad:", age)
```

o:

```python id="jjlwm6"
age = 20
print(f"Edad: {age}")
```

## Usar `sep` con un solo argumento esperando efecto visible

```python id="jlwm2l"
print("Hola", sep="-")
```

Aquí `sep` no se aprecia porque solo hay un objeto.

## Olvidar que `end` reemplaza el salto de línea

```python id="hjlwm9"
print("A", end="")
print("B")
```

Salida:

```text id="0jlwm5"
AB
```

Si se esperaba una línea por separado, no corresponde usar `end=""`.

## Intentar usar `print()` para formato complejo repetitivo

Cuando el formato se vuelve repetitivo o estructurado, suele ser mejor preparar el texto antes de imprimir o usar herramientas más adecuadas de presentación.

## Buenas prácticas

## Usar `print()` para salida simple y depuración ligera

```python id="jlwm4y"
print("Proceso completado")
```

## Usar f-strings para mensajes compuestos

```python id="54zqro"
name = "Ana"
score = 18.5

print(f"Estudiante: {name}, Puntaje: {score}")
```

## Usar varios argumentos cuando no se requiera formato complejo

```python id="jlwmj8"
print("Total:", 150, "Estado:", "aprobado")
```

## Usar `file=` cuando la salida deba escribirse en un archivo

```python id="56uvcb"
with open("salida.txt", "w", encoding="utf-8") as file:
    print("Resultado final", file=file)
```

## Usar `flush=True` solo cuando sea necesario

```python id="wmx7jh"
print("Iniciando...", end="", flush=True)
```

No siempre es necesario forzar el vaciado del búfer.

## Preferir claridad sobre trucos de impresión

Cuando el mensaje es importante, conviene que la salida sea explícita y legible.

## Ejemplo integrado

```python id="jlwm31"
def show_report(name, scores):
    average = sum(scores) / len(scores)

    print("Reporte del estudiante")
    print("-" * 30)
    print(f"Nombre: {name}")
    print("Notas:", scores, sep=" ")
    print(f"Promedio: {average:.2f}")
    print("Estado:", "Aprobado" if average >= 11 else "Desaprobado")


show_report("Ana", [15, 18, 14, 17])
```

Salida aproximada:

```text id="jlwmgo"
Reporte del estudiante
------------------------------
Nombre: Ana
Notas: [15, 18, 14, 17]
Promedio: 16.00
Estado: Aprobado
```

## Relación con otros elementos integrados

`print()` suele aparecer junto con:

* `input()` para interacción básica
* `len()` para mostrar tamaños
* `type()` para inspección simple
* `open()` cuando se escribe en archivos
* f-strings y métodos de `str` para construir mensajes

## Orden didáctico interno

```text id="6o3t6c"
1. Propósito de print()
2. Forma general y parámetros
3. Uso con uno o varios objetos
4. sep y end
5. file y flush
6. Diferencia entre print() y return
7. Uso con expresiones y f-strings
8. Errores comunes
9. Buenas prácticas
```