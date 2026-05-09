# Excepciones

## Propósito

Las excepciones permiten representar y manejar errores de forma estructurada. En lugar de interrumpir abruptamente la ejecución o depender de verificaciones dispersas, Python ofrece un mecanismo formal para detectar, propagar, capturar y comunicar situaciones anómalas.

## Alcance

Este tema cubre:

- qué es una excepción
- diferencia entre error de sintaxis y excepción en ejecución
- flujo de propagación
- uso de `try`, `except`, `else` y `finally`
- instrucción `raise`
- excepciones integradas frecuentes
- jerarquía básica de excepciones
- creación de excepciones personalizadas
- buenas prácticas de manejo

## Ideas fundamentales

1. Una excepción es un objeto que representa una condición anómala.
2. Las excepciones aparecen durante la ejecución del programa.
3. Si una excepción no se captura, el programa se detiene y muestra un traceback.
4. `try` delimita el bloque que puede fallar.
5. `except` captura una o varias excepciones específicas.
6. `else` se ejecuta solo si no hubo excepción.
7. `finally` se ejecuta siempre, haya o no error.
8. `raise` permite generar excepciones explícitamente.
9. Capturar excepciones demasiado generales puede ocultar errores importantes.
10. Las excepciones personalizadas permiten modelar errores del dominio del problema.

## Qué es una excepción

Una excepción es un objeto que indica que ocurrió un error o una situación especial durante la ejecución.

Ejemplo:

```python id="gwg9nq"
print(10 / 0)
````

Esto produce una excepción:

```text id="royk2s"
ZeroDivisionError
```

Otro ejemplo:

```python id="7lazyy"
valores = [1, 2, 3]
print(valores[10])
```

Esto produce:

```text id="v7flda"
IndexError
```

## Error de sintaxis y excepción en tiempo de ejecución

## Error de sintaxis

Impide que el programa se interprete correctamente.

```python id="aiwyja"
if True
    print("Hola")
```

Esto produce un `SyntaxError`.

## Excepción en tiempo de ejecución

El código es sintácticamente válido, pero falla al ejecutarse.

```python id="32i6x0"
numero = int("hola")
```

Esto produce:

```text id="7xv20m"
ValueError
```

## Flujo de una excepción

Cuando ocurre una excepción:

1. Python interrumpe la ejecución normal del bloque actual.
2. Busca un manejador compatible en el contexto más cercano.
3. Si no lo encuentra, continúa propagando la excepción hacia niveles superiores.
4. Si no se captura en ningún nivel, el programa termina y se muestra el traceback.

## Traceback

El traceback muestra la cadena de llamadas y el punto donde ocurrió la excepción.

Ejemplo:

```python id="p5j8d7"
def dividir(a, b):
    return a / b

def procesar():
    return dividir(10, 0)

procesar()
```

Python mostrará un traceback que indica:

* qué archivo se ejecutó
* en qué línea ocurrió la falla
* qué funciones estaban involucradas
* qué tipo de excepción se produjo

## `try` y `except`

La estructura básica para manejar excepciones es:

```python id="qsc3i0"
try:
    ...
except TipoDeExcepcion:
    ...
```

Ejemplo:

```python id="s6xn0o"
try:
    resultado = 10 / 0
except ZeroDivisionError:
    print("No se puede dividir entre cero")
```

Salida:

```text id="dw67ni"
No se puede dividir entre cero
```

## Captura de excepciones específicas

Se debe capturar la excepción más específica posible.

```python id="agk7q0"
try:
    numero = int("hola")
except ValueError:
    print("El texto no puede convertirse a entero")
```

Salida:

```text id="uww7hb"
El texto no puede convertirse a entero
```

## Múltiples bloques `except`

Puede haber más de un bloque `except`.

```python id="wzkyg4"
try:
    valor = int(input("Ingrese un número: "))
    resultado = 10 / valor
except ValueError:
    print("Entrada inválida")
except ZeroDivisionError:
    print("No se puede dividir entre cero")
```

Cada bloque captura una clase distinta de error.

## Capturar varias excepciones en un solo bloque

Forma general:

```python id="p1yy9f"
except (Tipo1, Tipo2, ...):
    ...
```

Ejemplo:

```python id="cbwjcx"
try:
    valor = int("hola")
except (ValueError, TypeError):
    print("Conversión inválida")
```

## Captura del objeto excepción con `as`

Permite acceder al objeto excepción y consultar su mensaje.

Forma general:

```python id="zucrxv"
except TipoDeExcepcion as error:
    ...
```

Ejemplo:

```python id="5r4c3r"
try:
    numero = int("hola")
except ValueError as error:
    print("Ocurrió un error:", error)
```

Salida posible:

```text id="ywll90"
Ocurrió un error: invalid literal for int() with base 10: 'hola'
```

## `else`

El bloque `else` se ejecuta solo si el bloque `try` termina sin excepciones.

Estructura:

```python id="yu6v4n"
try:
    ...
except ...:
    ...
else:
    ...
```

Ejemplo:

```python id="ftt03f"
try:
    numero = int("25")
except ValueError:
    print("Conversión inválida")
else:
    print("Conversión exitosa:", numero)
```

Salida:

```text id="wyt4ag"
Conversión exitosa: 25
```

## Cuándo usar `else`

Conviene usar `else` para separar claramente:

* el código que puede fallar
* el código que debe ejecutarse solo si no falló nada

Esto mejora la claridad del flujo.

## `finally`

El bloque `finally` se ejecuta siempre, haya o no excepción.

Estructura:

```python id="jjlwmk"
try:
    ...
except ...:
    ...
finally:
    ...
```

Ejemplo:

```python id="3796w0"
try:
    archivo = open("datos.txt", "r", encoding="utf-8")
    contenido = archivo.read()
except FileNotFoundError:
    print("Archivo no encontrado")
finally:
    print("Fin del intento de lectura")
```

Aunque ocurra un error, `finally` se ejecuta.

## Uso típico de `finally`

`finally` se usa para:

* cerrar archivos
* liberar recursos
* cerrar conexiones
* registrar finalización de procesos
* asegurar limpieza de estado

## Estructura completa

Python permite combinar `try`, `except`, `else` y `finally`.

```python id="cizqki"
try:
    valor = int("10")
except ValueError:
    print("Error de conversión")
else:
    print("Conversión correcta")
finally:
    print("Bloque final")
```

Salida:

```text id="m4nhmy"
Conversión correcta
Bloque final
```

## `raise`

`raise` permite lanzar una excepción explícitamente.

Ejemplo:

```python id="0i0jje"
def dividir(a, b):
    if b == 0:
        raise ZeroDivisionError("El divisor no puede ser cero")

    return a / b
```

Si `b` es cero, la función lanza la excepción manualmente.

## `raise` con excepciones integradas

Ejemplo:

```python id="82obxn"
edad = -5

if edad < 0:
    raise ValueError("La edad no puede ser negativa")
```

## `raise` sin argumentos dentro de `except`

Dentro de un `except`, `raise` sin argumentos vuelve a lanzar la excepción actual.

```python id="ik795x"
try:
    numero = int("hola")
except ValueError:
    print("Se detectó el error")
    raise
```

Esto permite registrar o inspeccionar el problema sin suprimirlo.

## Excepciones integradas frecuentes

## `ValueError`

Se usa cuando el tipo es correcto, pero el valor no es válido.

```python id="5cuiqc"
int("hola")
```

## `TypeError`

Se usa cuando una operación recibe un tipo incompatible.

```python id="3ir397"
"2" + 3
```

## `IndexError`

Se usa cuando se intenta acceder a una posición inexistente en una secuencia.

```python id="9j0kms"
[1, 2, 3][10]
```

## `KeyError`

Se usa cuando se accede a una clave inexistente en un diccionario.

```python id="r02zau"
datos = {"nombre": "Ana"}
print(datos["edad"])
```

## `ZeroDivisionError`

Se usa cuando se divide entre cero.

```python id="27as22"
10 / 0
```

## `FileNotFoundError`

Se usa cuando se intenta abrir un archivo que no existe.

```python id="ez6py4"
open("archivo_inexistente.txt", "r", encoding="utf-8")
```

## `AttributeError`

Se usa cuando se intenta acceder a un atributo o método inexistente.

```python id="md9c4m"
numero = 10
numero.append(5)
```

## `ImportError` y `ModuleNotFoundError`

Se usan cuando falla una importación.

```python id="8ouwnd"
import modulo_que_no_existe
```

## `StopIteration`

Se produce cuando un iterador no tiene más elementos.

```python id="nfij2p"
it = iter([1])
print(next(it))
print(next(it))
```

## Jerarquía básica de excepciones

Las excepciones en Python forman una jerarquía.

De forma simplificada:

```text id="ko7oia"
BaseException
├─ Exception
│  ├─ ValueError
│  ├─ TypeError
│  ├─ IndexError
│  ├─ KeyError
│  ├─ FileNotFoundError
│  ├─ ZeroDivisionError
│  ├─ AttributeError
│  └─ ...
├─ KeyboardInterrupt
├─ SystemExit
└─ GeneratorExit
```

En general, el código de aplicación debería capturar clases derivadas de `Exception`, no de `BaseException`.

## Capturar `Exception`

Es válido en algunos casos generales:

```python id="ez4lo9"
try:
    ejecutar_proceso()
except Exception as error:
    print("Ocurrió un error:", error)
```

Pero debe usarse con cuidado.

## No capturar `BaseException`

No es recomendable:

```python id="zose6v"
try:
    ejecutar_proceso()
except BaseException:
    print("Algo ocurrió")
```

Esto también atraparía interrupciones del sistema como `KeyboardInterrupt`, lo cual suele ser indeseable.

## Excepciones personalizadas

Cuando un problema pertenece al dominio específico de la aplicación, conviene definir excepciones propias.

Forma general:

```python id="89nd6z"
class MiError(Exception):
    pass
```

Ejemplo:

```python id="l8cwjb"
class InvalidAgeError(Exception):
    pass

def validar_edad(edad):
    if edad < 0:
        raise InvalidAgeError("La edad no puede ser negativa")
```

## Ventajas de excepciones personalizadas

Permiten:

* expresar errores del dominio con claridad
* distinguir mejor entre tipos de falla
* capturar errores propios sin mezclar con excepciones genéricas
* mejorar legibilidad del código

## Excepciones con herencia

También puede construirse una jerarquía propia.

```python id="br7do9"
class ValidationError(Exception):
    pass

class InvalidAgeError(ValidationError):
    pass

class InvalidEmailError(ValidationError):
    pass
```

Esto permite capturar tanto errores específicos como categorías generales.

## Manejo correcto de recursos

En muchos casos, más que usar `try/finally` manualmente, conviene usar context managers con `with`.

Ejemplo preferible:

```python id="pjlwmj"
with open("datos.txt", "r", encoding="utf-8") as archivo:
    contenido = archivo.read()
```

Esto asegura el cierre del archivo incluso si ocurre una excepción.

## Excepciones y flujo de control

Las excepciones no deben usarse como reemplazo habitual de condicionales cuando la situación es completamente predecible y barata de verificar.

Menos adecuado:

```python id="7n1y9q"
try:
    if lista[0]:
        print("Hay valor")
except IndexError:
    print("Lista vacía")
```

Más claro en este caso:

```python id="by3l8m"
if lista:
    print("Hay valor")
else:
    print("Lista vacía")
```

Sin embargo, sí es razonable usar excepciones cuando la operación realmente puede fallar por múltiples causas difíciles de prevalidar, como abrir archivos, convertir datos o acceder a recursos externos.

## Errores comunes

## Capturar excepciones demasiado generales

Problemático:

```python id="6gngg6"
try:
    ejecutar()
except Exception:
    print("Error")
```

Esto puede ocultar la causa real del problema si se usa indiscriminadamente.

## Silenciar excepciones sin justificación

Problemático:

```python id="e27ukh"
try:
    ejecutar()
except ValueError:
    pass
```

Esto descarta el error sin registrar nada y dificulta depuración.

## Envolver demasiado código dentro de `try`

Problemático:

```python id="jlwmv8"
try:
    valor = int(texto)
    procesar(valor)
    guardar_resultado(valor)
    imprimir_resumen(valor)
except ValueError:
    print("Error de conversión")
```

Aquí no queda claro qué línea realmente puede generar el `ValueError`.

Más conveniente:

```python id="o2x8fx"
try:
    valor = int(texto)
except ValueError:
    print("Error de conversión")
else:
    procesar(valor)
    guardar_resultado(valor)
    imprimir_resumen(valor)
```

## Usar excepciones para ocultar errores de diseño

No conviene envolver bloques enteros solo para que “no falle” el programa. El manejo debe ser intencional y específico.

## Confundir validación con manejo de excepciones

Una excepción no reemplaza toda validación previa. Ambas herramientas se complementan.

## Buenas prácticas

## Capturar excepciones específicas

```python id="bjhbey"
try:
    numero = int(texto)
except ValueError:
    print("Entrada inválida")
```

## Mantener pequeño el bloque `try`

```python id="mjlwmu"
try:
    numero = int(texto)
except ValueError:
    print("Entrada inválida")
else:
    procesar(numero)
```

## Usar `else` para lógica que solo debe ejecutarse si no hubo error

Esto mejora la claridad y evita capturar excepciones desde código que no debía estar protegido por ese `try`.

## Usar `finally` o `with` para liberar recursos

```python id="ejm7w0"
with open("datos.txt", "r", encoding="utf-8") as archivo:
    contenido = archivo.read()
```

## Definir excepciones personalizadas cuando el dominio lo requiera

```python id="6b3ns6"
class ValidationError(Exception):
    pass
```

## No ocultar errores silenciosamente

Si una excepción se captura, debe haber una razón clara para hacerlo y una respuesta coherente: registrar, transformar, reintentar, limpiar recursos o comunicar el problema.

## Ejemplo integrado

```python id="5nerqn"
class InvalidAgeError(Exception):
    pass


def parse_age(text):
    try:
        age = int(text)
    except ValueError as error:
        raise InvalidAgeError("La edad debe ser un entero válido") from error

    if age < 0:
        raise InvalidAgeError("La edad no puede ser negativa")

    return age


def main():
    values = ["20", "-3", "hola"]

    for value in values:
        try:
            age = parse_age(value)
        except InvalidAgeError as error:
            print(f"Entrada '{value}': {error}")
        else:
            print(f"Entrada '{value}': edad válida -> {age}")


if __name__ == "__main__":
    main()
```

## Orden didáctico interno

```text id="q3oq2z"
1. Qué es una excepción
2. Error de sintaxis vs excepción en ejecución
3. Flujo de propagación y traceback
4. try y except
5. except específicos y múltiples
6. Captura con as
7. else
8. finally
9. raise
10. Excepciones integradas frecuentes
11. Jerarquía básica
12. Excepciones personalizadas
13. Buenas prácticas de manejo
```