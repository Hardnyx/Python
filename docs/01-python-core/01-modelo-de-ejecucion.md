# Modelo de ejecución

## Propósito

El modelo de ejecución de Python describe cómo se interpreta, organiza y ejecuta el código fuente. Comprender este bloque permite explicar qué ocurre cuando se ejecuta un archivo `.py`, cómo se cargan módulos, qué orden sigue el intérprete y por qué ciertas construcciones, como `if __name__ == "__main__":`, resultan necesarias para separar código reutilizable de código ejecutable.

## Alcance

Este tema se concentra en la forma en que Python ejecuta programas y organiza archivos. No se centra en la sintaxis básica de variables, condicionales o bucles, sino en el comportamiento general del lenguaje al cargar y ejecutar código.

## Ideas fundamentales

El modelo de ejecución puede resumirse en los siguientes principios:

1. Python ejecuta instrucciones de arriba hacia abajo, salvo que alguna estructura altere el flujo normal.

2. Un archivo `.py` puede usarse como programa principal o como módulo reutilizable.

3. Al importar un módulo, Python ejecuta su código de nivel superior una sola vez por proceso y luego conserva el módulo cargado.

4. Cada archivo tiene un espacio de nombres propio.

5. La variable especial `__name__` permite distinguir si un archivo se está ejecutando directamente o si se está importando.

## Script, módulo y paquete

## Script

Un script es un archivo de Python que se ejecuta directamente.

Ejemplo:

```python id="bcbqm6"
print("Inicio del programa")
print("Fin del programa")
````

Si ese archivo se llama `main.py` y se ejecuta con:

```bash id="1c1yeh"
python main.py
```

Python leerá el archivo y ejecutará las instrucciones en orden.

## Módulo

Un módulo es un archivo `.py` cuyo contenido puede reutilizarse desde otros archivos mediante `import`.

Ejemplo:

```python id="q20x1w"
def sumar(a, b):
    return a + b
```

Si ese archivo se llama `operaciones.py`, puede importarse desde otro archivo:

```python id="h5kckh"
import operaciones

resultado = operaciones.sumar(10, 5)
print(resultado)
```

## Paquete

Un paquete es una carpeta que agrupa módulos relacionados.

Ejemplo conceptual:

```text id="v3jh0f"
mi_paquete/
    __init__.py
    operaciones.py
    validaciones.py
```

Esto permite organizar código en bloques más grandes y estructurados.

## Ejecución secuencial

En condiciones normales, Python ejecuta el código de arriba hacia abajo.

```python id="n8vi01"
print("Paso 1")
print("Paso 2")
print("Paso 3")
```

Salida:

```text id="vqbwj5"
Paso 1
Paso 2
Paso 3
```

Ese orden puede alterarse mediante:

* llamadas a funciones
* condicionales
* bucles
* excepciones
* importaciones
* retornos anticipados
* interrupciones del programa

Sin embargo, la regla base sigue siendo la ejecución secuencial.

## Código de nivel superior

Se llama código de nivel superior al que está escrito directamente en el archivo, fuera de funciones o clases.

Ejemplo:

```python id="nlhd0w"
print("Este mensaje pertenece al nivel superior")

def saludar():
    print("Hola")
```

En este caso, el `print()` se ejecuta al cargar el archivo. La función `saludar()` solo se define. No se ejecuta hasta que se la llama explícitamente.

## Diferencia entre definir y ejecutar

Definir una función o una clase no equivale a ejecutarla.

```python id="kfxqqa"
def mostrar_mensaje():
    print("Mensaje interno")
```

Ese bloque crea la función `mostrar_mensaje`, pero no imprime nada por sí solo.

Para ejecutarla:

```python id="qf6qju"
def mostrar_mensaje():
    print("Mensaje interno")

mostrar_mensaje()
```

Salida:

```text id="qws5cd"
Mensaje interno
```

Esta diferencia es clave para entender por qué un módulo puede contener muchas funciones sin ejecutarlas automáticamente al importarse.

## Ejecución de un archivo como programa principal

Cuando un archivo se ejecuta directamente, Python lo trata como el programa principal del proceso.

Ejemplo:

```bash id="xjlwmj"
python main.py
```

Durante esa ejecución, Python asigna a la variable especial `__name__` el valor:

```python id="ei5vju"
"__main__"
```

Ejemplo:

```python id="n2l1y2"
print(__name__)
```

Si ese archivo se ejecuta directamente, la salida será:

```text id="6lrizk"
__main__
```

## Ejecución de un archivo al importarlo

Cuando un archivo se importa desde otro, Python no le asigna `__name__ = "__main__"`. En su lugar, le asigna el nombre del módulo.

Supóngase este archivo `utilidades.py`:

```python id="j4rqk0"
print(__name__)
```

Y este otro archivo `main.py`:

```python id="d8kc11"
import utilidades
```

Si se ejecuta `main.py`, la salida será:

```text id="u5h4qk"
utilidades
```

Esto ocurre porque `utilidades.py` no es el programa principal, sino un módulo importado.

## Uso de `if __name__ == "__main__":`

Esta construcción permite separar el código reutilizable del código que solo debe ejecutarse cuando el archivo actúa como programa principal.

Estructura general:

```python id="68pbz5"
if __name__ == "__main__":
    ...
```

Ejemplo:

```python id="ajpb5n"
def saludar():
    print("Hola")

if __name__ == "__main__":
    saludar()
```

Comportamiento:

* Si el archivo se ejecuta directamente, `saludar()` se ejecuta.
* Si el archivo se importa desde otro módulo, `saludar()` no se ejecuta automáticamente.

## Función principal

Una práctica habitual consiste en encapsular la lógica principal en una función llamada `main()` y ejecutarla dentro del bloque `if __name__ == "__main__":`.

Ejemplo:

```python id="p4qdwf"
def main():
    print("Inicio del programa")
    print("Procesamiento principal")

if __name__ == "__main__":
    main()
```

Ventajas de esta estructura:

* separa definiciones de ejecución
* mejora la legibilidad
* facilita pruebas
* permite reutilizar funciones sin efectos colaterales al importar

## Importación de módulos

## `import modulo`

Importa el módulo completo.

```python id="3wmotq"
import math

print(math.sqrt(25))
```

## `from modulo import nombre`

Importa un elemento concreto del módulo.

```python id="3jq38t"
from math import sqrt

print(sqrt(25))
```

## `import modulo as alias`

Importa un módulo usando un alias.

```python id="4ep6rf"
import math as m

print(m.sqrt(25))
```

En el contexto del modelo de ejecución, lo importante no es solo la sintaxis de `import`, sino el hecho de que importar un módulo provoca la carga y ejecución de su código de nivel superior.

## Qué ocurre al importar un módulo

Cuando Python encuentra una instrucción `import`, en términos generales ocurre lo siguiente:

1. Busca el módulo solicitado.
2. Si lo encuentra, carga su contenido.
3. Ejecuta su código de nivel superior.
4. Crea un objeto módulo.
5. Guarda ese módulo en memoria para reutilizarlo si vuelve a importarse.

Ejemplo:

Archivo `utilidades.py`:

```python id="xwwjhy"
print("Módulo cargado")

def duplicar(x):
    return x * 2
```

Archivo `main.py`:

```python id="mw33tz"
import utilidades

print("Programa principal")
```

Salida:

```text id="rmyy9a"
Módulo cargado
Programa principal
```

La línea `print("Módulo cargado")` pertenece al nivel superior de `utilidades.py`, por eso se ejecuta al importar el módulo.

## Importación repetida en un mismo proceso

Un módulo no se vuelve a ejecutar completamente cada vez que se importa en el mismo proceso. Python lo carga una vez y luego reutiliza la versión ya cargada.

Esto evita trabajo repetido y previene múltiples ejecuciones del mismo código de inicialización.

## Espacios de nombres

Cada módulo mantiene su propio espacio de nombres.

Archivo `utilidades.py`:

```python id="zdcv0a"
valor = 100
```

Archivo `main.py`:

```python id="r9vw45"
import utilidades

print(utilidades.valor)
```

Salida:

```text id="878mbu"
100
```

El nombre `valor` pertenece al módulo `utilidades`. No aparece automáticamente como variable local de `main.py`.

Si se quisiera acceder sin prefijo, habría que importar explícitamente el nombre:

```python id="2xpa2v"
from utilidades import valor

print(valor)
```

## Orden de carga y dependencia entre archivos

Si un archivo depende de otro, ese otro archivo debe poder cargarse correctamente.

Ejemplo:

Archivo `operaciones.py`:

```python id="z6yae1"
def sumar(a, b):
    return a + b
```

Archivo `main.py`:

```python id="22yotv"
from operaciones import sumar

print(sumar(3, 4))
```

Aquí `main.py` depende de que `operaciones.py` exista y sea importable.

## Importaciones circulares

Una importación circular ocurre cuando dos módulos dependen entre sí de forma directa o indirecta durante su carga.

Ejemplo problemático:

Archivo `a.py`:

```python id="cgbx3b"
import b

def funcion_a():
    return "A"
```

Archivo `b.py`:

```python id="6w4g9t"
import a

def funcion_b():
    return "B"
```

Este tipo de estructura puede generar errores o comportamientos incompletos durante la inicialización.

La regla general es evitar dependencias circulares entre módulos.

## Ejecución condicionada por importación

Ejemplo correcto de separación entre lógica reusable y ejecución directa:

Archivo `calculadora.py`:

```python id="af0qft"
def sumar(a, b):
    return a + b

def restar(a, b):
    return a - b

def main():
    print(sumar(10, 5))
    print(restar(10, 5))

if __name__ == "__main__":
    main()
```

Con esta estructura:

* `sumar()` y `restar()` pueden reutilizarse desde otros módulos
* `main()` solo se ejecuta cuando el archivo se corre directamente

## Organización mínima de un proyecto

Estructura simple:

```text id="h64px1"
proyecto/
    main.py
    operaciones.py
```

Archivo `operaciones.py`:

```python id="bfi4g5"
def sumar(a, b):
    return a + b
```

Archivo `main.py`:

```python id="kgonv6"
from operaciones import sumar

def main():
    resultado = sumar(8, 4)
    print(resultado)

if __name__ == "__main__":
    main()
```

Esto muestra la forma mínima de dividir lógica en módulos sin perder claridad de ejecución.

## Diferencia entre ejecutar e importar

| Acción                        | `__name__`        | Se ejecuta el bloque principal | Se ejecuta el código de nivel superior |
| ----------------------------- | ----------------- | -----------------------------: | -------------------------------------: |
| Ejecutar archivo directamente | `"__main__"`      |                             Sí |                                     Sí |
| Importar archivo como módulo  | Nombre del módulo |                             No |                                     Sí |

Esto explica por qué el bloque `if __name__ == "__main__":` es una herramienta central en la organización de scripts reutilizables.

## Archivos `.pyc` y caché de importación

Cuando se importan módulos, Python puede generar archivos compilados en bytecode dentro de carpetas como:

```text id="cyjlwm"
__pycache__/
```

Ejemplo:

```text id="vv73uj"
__pycache__/operaciones.cpython-312.pyc
```

Estos archivos sirven para acelerar cargas futuras del módulo. No son el código fuente principal y normalmente no se editan manualmente.

## Flujo típico de ejecución en un proyecto simple

1. Se ejecuta `python main.py`.
2. Python carga `main.py`.
3. Ejecuta su código de nivel superior.
4. Si encuentra importaciones, carga los módulos correspondientes.
5. Cada módulo importado ejecuta su código de nivel superior.
6. Python vuelve al archivo principal.
7. Si existe un bloque `if __name__ == "__main__":`, lo evalúa.
8. Si la condición es verdadera, ejecuta la lógica principal.

## Errores comunes

## Colocar lógica ejecutable fuera del bloque principal

Problemático:

```python id="buv4ot"
def procesar():
    print("Procesando")

procesar()
```

Si ese archivo se importa, `procesar()` se ejecutará inmediatamente.

Más conveniente:

```python id="uyt3do"
def procesar():
    print("Procesando")

if __name__ == "__main__":
    procesar()
```

## Confundir definición con ejecución

Problemático:

```python id="j9gr90"
def saludar():
    print("Hola")
```

Aquí no se imprime nada hasta llamar a la función.

## Usar importaciones circulares

Dos módulos que se importan mutuamente durante su inicialización pueden generar errores difíciles de rastrear.

## Depender de nombres sin calificación

Si se importa un módulo completo:

```python id="nmftjw"
import operaciones
```

el acceso correcto es:

```python id="d72qz7"
operaciones.sumar(2, 3)
```

No:

```python id="q4zxfe"
sumar(2, 3)
```

salvo que ese nombre se haya importado explícitamente.

## Buenas prácticas

## Encapsular la lógica principal en `main()`

```python id="hfr7ib"
def main():
    print("Programa principal")

if __name__ == "__main__":
    main()
```

## Mantener el código de nivel superior al mínimo

El nivel superior debería contener principalmente:

* importaciones
* definiciones
* constantes simples
* bloque principal

## Evitar efectos colaterales al importar módulos

Un módulo debería poder importarse sin ejecutar procesos pesados, lecturas innecesarias o impresiones irrelevantes.

## Separar lógica reutilizable de lógica de ejecución

Las funciones y clases deberían declararse fuera del bloque principal. La ejecución concreta del programa debería quedar dentro de `main()` o dentro del bloque `if __name__ == "__main__":`.

## Organizar cada archivo con una responsabilidad clara

Un archivo debería representar una unidad lógica razonable: utilidades, validaciones, procesamiento, interfaz principal, configuración, etc.

## Ejemplo integrado

Archivo `operaciones.py`:

```python id="r6cfu2"
def sumar(a, b):
    return a + b

def multiplicar(a, b):
    return a * b
```

Archivo `main.py`:

```python id="r4n7fw"
from operaciones import sumar, multiplicar

def main():
    print("Resultado de la suma:", sumar(3, 4))
    print("Resultado de la multiplicación:", multiplicar(3, 4))

if __name__ == "__main__":
    main()
```

Resultado al ejecutar `python main.py`:

```text id="bgted2"
Resultado de la suma: 7
Resultado de la multiplicación: 12
```

## Orden didáctico interno

```text id="mk3hat"
1. Script, módulo y paquete
2. Ejecución secuencial
3. Código de nivel superior
4. Diferencia entre definir y ejecutar
5. Ejecución directa de un archivo
6. Variable __name__
7. Bloque if __name__ == "__main__"
8. Función principal main()
9. Importación de módulos
10. Espacios de nombres
11. Dependencias entre archivos
12. Importaciones circulares
13. Buenas prácticas de organización
```