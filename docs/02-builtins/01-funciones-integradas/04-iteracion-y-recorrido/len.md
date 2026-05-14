# `len()`

## Propósito

`len()` devuelve la cantidad de elementos de un objeto que tiene longitud definida. Es una de las funciones integradas más usadas en Python y resulta fundamental para trabajar con secuencias, colecciones, diccionarios, rangos y objetos personalizados que implementen longitud.

## Forma general

```python
len(obj)
````

## Idea central

`len()` no cuenta manualmente los elementos uno por uno en el momento de la llamada. En términos del modelo de objetos de Python, solicita la longitud del objeto mediante su protocolo correspondiente.

Esto permite usar la misma función sobre muchos tipos distintos, siempre que el objeto soporte longitud.

## Argumentos

## `obj`

Representa el objeto cuya longitud se desea obtener.

Debe ser un objeto con longitud definida.

Ejemplos típicos:

* cadenas
* listas
* tuplas
* diccionarios
* conjuntos
* rangos
* bytes
* bytearray

Ejemplo:

```python
print(len("Python"))
print(len([1, 2, 3]))
print(len((10, 20)))
print(len({"a": 1, "b": 2}))
```

Salida:

```text
6
3
2
2
```

## Valor de retorno

`len()` devuelve un entero no negativo.

```python
texto = "Hola"
resultado = len(texto)

print(resultado)
print(type(resultado))
```

Salida:

```text
4
<class 'int'>
```

## Comportamiento general

## Longitud de cadenas

En una cadena, `len()` devuelve la cantidad de caracteres.

```python
print(len("Python"))
print(len(""))
print(len("Hola mundo"))
```

Salida:

```text
6
0
10
```

## Longitud de listas

En una lista, devuelve la cantidad de elementos.

```python
print(len([1, 2, 3]))
print(len([]))
print(len(["Ana", "Luis", "Marta"]))
```

Salida:

```text
3
0
3
```

## Longitud de tuplas

```python
print(len((10, 20, 30)))
print(len(()))
```

Salida:

```text
3
0
```

## Longitud de diccionarios

En un diccionario, `len()` devuelve la cantidad de claves.

```python
datos = {"nombre": "Ana", "edad": 20, "ciudad": "Lima"}
print(len(datos))
```

Salida:

```text
3
```

## Longitud de conjuntos

```python
print(len({1, 2, 3}))
print(len(set()))
```

Salida:

```text
3
0
```

## Longitud de `range`

```python
print(len(range(10)))
print(len(range(1, 10)))
print(len(range(2, 11, 2)))
```

Salida:

```text
10
9
5
```

## Longitud de `bytes` y `bytearray`

```python
print(len(b"abc"))
print(len(bytearray(b"hola")))
```

Salida:

```text
3
4
```

## Uso con objetos personalizados

Una clase puede definir su propia longitud implementando `__len__()`.

```python
class Team:
    def __init__(self, members):
        self.members = members

    def __len__(self):
        return len(self.members)

team = Team(["Ana", "Luis", "Marta"])
print(len(team))
```

Salida:

```text
3
```

## Relación con `__len__()`

`len(obj)` activa el método especial `obj.__len__()` cuando el objeto lo implementa.

Ejemplo ilustrativo:

```python
class Box:
    def __len__(self):
        return 5

box = Box()
print(len(box))
```

Salida:

```text
5
```

Esto conecta `len()` con el protocolo de objetos del lenguaje.

## Longitud y verdad lógica

Muchos objetos usan su longitud para determinar su valor lógico en contextos booleanos cuando no definen `__bool__()`.

Ejemplo:

```python
valores = []

if valores:
    print("Tiene elementos")
else:
    print("Está vacío")
```

Salida:

```text
Está vacío
```

La lista vacía se evalúa como falsa porque su longitud es cero.

## Uso frecuente en condiciones

`len()` se usa a menudo para verificar cantidad de elementos.

```python
nombres = ["Ana", "Luis"]

if len(nombres) > 0:
    print("Hay nombres registrados")
```

Sin embargo, cuando solo interesa saber si una colección está vacía o no, suele ser más idiomático usar la colección directamente.

Menos idiomático:

```python
if len(nombres) > 0:
    print("Hay nombres registrados")
```

Más idiomático:

```python
if nombres:
    print("Hay nombres registrados")
```

## Uso con iteración

`len()` puede combinarse con `range()` para iterar por índices, aunque no siempre es la opción más clara.

```python
nombres = ["Ana", "Luis", "Marta"]

for i in range(len(nombres)):
    print(i, nombres[i])
```

Salida:

```text
0 Ana
1 Luis
2 Marta
```

Esta forma es válida, pero si no se necesita el índice, conviene iterar directamente. Si se necesita índice y valor, suele ser preferible `enumerate()`.

Forma más clara:

```python
nombres = ["Ana", "Luis", "Marta"]

for i, nombre in enumerate(nombres):
    print(i, nombre)
```

## Longitud de estructuras anidadas

`len()` solo cuenta los elementos del nivel superior inmediato.

```python
matriz = [[1, 2], [3, 4], [5, 6]]
print(len(matriz))
```

Salida:

```text
3
```

No devuelve la cantidad total de elementos internos, sino la cantidad de sublistas.

## Diferencia entre longitud y tamaño conceptual

`len()` devuelve cantidad de elementos, no tamaño en memoria ni magnitud matemática.

Ejemplo:

```python
print(len("1000"))
print(len([1000]))
```

Salida:

```text
4
1
```

En el primer caso se cuentan caracteres. En el segundo, elementos de la lista.

## Casos de uso frecuentes

## Verificar si una colección está vacía

```python
clientes = []

if len(clientes) == 0:
    print("No hay clientes registrados")
```

## Obtener cantidad de registros

```python
ventas = [120, 150, 90, 200]
print("Cantidad de ventas:", len(ventas))
```

## Trabajar con índices válidos

```python
nombres = ["Ana", "Luis", "Marta"]

ultimo_indice = len(nombres) - 1
print(ultimo_indice)
print(nombres[ultimo_indice])
```

Salida:

```text
2
Marta
```

## Comparar tamaños

```python
equipo_a = ["Ana", "Luis"]
equipo_b = ["Carlos", "Marta", "Sofía"]

print(len(equipo_a) < len(equipo_b))
```

Salida:

```text
True
```

## Errores comunes

## Usar `len()` sobre un entero

Problemático:

```python
print(len(10))
```

Error típico:

```text
TypeError: object of type 'int' has no len()
```

Los enteros no tienen longitud.

## Usar `len()` sobre un valor `None`

Problemático:

```python
valor = None
print(len(valor))
```

Error típico:

```text
TypeError
```

## Suponer que `len()` mide tamaño total profundo

Problemático:

```python
matriz = [[1, 2], [3, 4], [5, 6]]
print(len(matriz))
```

Salida:

```text
3
```

No devuelve `6`, porque no cuenta recursivamente todos los elementos internos.

## Usar `len()` para saber si algo existe cuando puede bastar una evaluación booleana

Menos idiomático:

```python
if len(texto) > 0:
    print("Hay contenido")
```

Más idiomático:

```python
if texto:
    print("Hay contenido")
```

## Suponer que todo iterable tiene longitud

No todo iterable tiene una longitud definida.

Ejemplo problemático:

```python
gen = (x for x in range(5))
print(len(gen))
```

Error típico:

```text
TypeError
```

Un generador es iterable, pero no tiene longitud incorporada.

## Buenas prácticas

## Usar `len()` cuando interese la cantidad exacta de elementos

```python
nombres = ["Ana", "Luis", "Marta"]
print(len(nombres))
```

## Usar evaluación booleana cuando solo interese saber si está vacío o no

```python
if nombres:
    print("Hay elementos")
```

## Preferir `enumerate()` frente a `range(len(...))` cuando se necesiten índice y valor

Menos claro:

```python
for i in range(len(nombres)):
    print(i, nombres[i])
```

Más claro:

```python
for i, nombre in enumerate(nombres):
    print(i, nombre)
```

## Implementar `__len__()` en clases solo cuando la noción de longitud tenga sentido

```python
class Team:
    def __init__(self, members):
        self.members = members

    def __len__(self):
        return len(self.members)
```

No toda clase necesita longitud.

## Ejemplo integrado

```python
class Inventory:
    def __init__(self, items):
        self.items = items

    def __len__(self):
        return len(self.items)

    def is_empty(self):
        return len(self) == 0


inventory = Inventory(["teclado", "mouse", "monitor"])

print("Cantidad de elementos:", len(inventory))
print("Inventario vacío:", inventory.is_empty())

if inventory:
    print("Hay elementos disponibles")
```

Salida aproximada:

```text
Cantidad de elementos: 3
Inventario vacío: False
Hay elementos disponibles
```

## Relación con otros elementos integrados

`len()` se relaciona especialmente con:

* `range()` al iterar por índices
* `enumerate()` cuando se quiere índice y valor
* `type()` e `isinstance()` al inspeccionar tipos que soportan longitud
* métodos especiales, particularmente `__len__()`
* estructuras integradas como `str`, `list`, `tuple`, `dict` y `set`

## Orden didáctico interno

```text
1. Propósito de len()
2. Forma general
3. Argumento y valor de retorno
4. Uso sobre cadenas, listas, tuplas, diccionarios y conjuntos
5. Relación con __len__()
6. Longitud y verdad lógica
7. Uso en condiciones e iteración
8. Errores comunes
9. Buenas prácticas
```