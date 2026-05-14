# `dict`

## Propósito

`dict` representa una estructura de mapeo entre claves y valores. Se utiliza para almacenar datos asociados mediante identificadores, etiquetas o nombres de campo, y es una de las estructuras más importantes de Python para modelar registros, configuraciones, parámetros y resultados organizados por clave.

## Naturaleza del tipo

`dict` es un tipo de mapeo, mutable y basado en pares clave-valor.

Esto significa que:

- cada elemento tiene una clave y un valor asociado
- el acceso principal se realiza por clave, no por posición
- las claves no pueden repetirse
- los valores sí pueden repetirse
- puede modificarse después de crearse

## Forma general

Un diccionario puede escribirse como literal:

```python
{"nombre": "Ana", "edad": 20}
{}
````

o construirse explícitamente con:

```python id="n8x3tp"
dict(...)
```

## Literales de diccionario

```python id="v1m7qd"
persona = {"nombre": "Ana", "edad": 20}
vacio = {}

print(persona)
print(vacio)
```

Salida:

```text id="r4p9wk"
{'nombre': 'Ana', 'edad': 20}
{}
```

## Claves y valores

Cada entrada de un diccionario tiene la forma:

```text id="m2q6rv"
clave: valor
```

Ejemplo:

```python id="q5m8tw"
datos = {
    "nombre": "Ana",
    "edad": 20,
    "ciudad": "Lima"
}

print(datos)
```

## Valor de retorno de `type()`

```python id="x7p3mk"
print(type({"a": 1}))
```

Salida:

```text id="t9m1qx"
<class 'dict'>
```

## Mutabilidad

`dict` es mutable.

```python id="p6q4rv"
datos = {"nombre": "Ana"}
print(id(datos))

datos["edad"] = 20
print(datos)
print(id(datos))
```

Salida:

```text id="n3m8tw"
{'nombre': 'Ana', 'edad': 20}
```

La identidad del objeto no cambia. Se modifica el mismo diccionario.

## Longitud

La longitud se obtiene con `len()` y corresponde a la cantidad de claves.

```python id="r1q7pk"
datos = {"nombre": "Ana", "edad": 20, "ciudad": "Lima"}

print(len(datos))
print(len({}))
```

Salida:

```text id="v5m2rw"
3
0
```

## Acceso por clave

El acceso principal se hace mediante corchetes y la clave correspondiente.

```python id="t8q4mv"
persona = {"nombre": "Ana", "edad": 20}

print(persona["nombre"])
print(persona["edad"])
```

Salida:

```text id="m4p7qx"
Ana
20
```

## Error por clave inexistente

```python id="p9m1tw"
persona = {"nombre": "Ana"}
print(persona["edad"])
```

Error típico:

```text id="q2r8vk"
KeyError: 'edad'
```

## Asignación por clave

Un diccionario permite agregar o actualizar valores usando una clave.

## Agregar una nueva clave

```python id="v3q6rm"
datos = {"nombre": "Ana"}
datos["edad"] = 20

print(datos)
```

Salida:

```text id="t7m4px"
{'nombre': 'Ana', 'edad': 20}
```

## Actualizar una clave existente

```python id="n1q9tw"
datos = {"nombre": "Ana", "edad": 20}
datos["edad"] = 21

print(datos)
```

Salida:

```text id="r6m3qv"
{'nombre': 'Ana', 'edad': 21}
```

## Claves únicas

Las claves no pueden repetirse. Si una clave aparece más de una vez, prevalece la última asignación.

```python id="m8q2pk"
datos = {"a": 1, "a": 2, "a": 3}
print(datos)
```

Salida:

```text id="p4m7rw"
{'a': 3}
```

## Qué tipos pueden ser claves

Las claves deben ser hashables.

Ejemplos válidos:

```python id="q7m1tv"
datos = {
    "nombre": "Ana",
    10: "entero",
    (1, 2): "tupla"
}

print(datos)
```

Ejemplos inválidos:

```python id="r2q8mk"
datos = {
    [1, 2]: "lista"
}
```

Esto genera error porque las listas no son hashables.

## Valores heterogéneos

Los valores de un diccionario pueden ser de distintos tipos.

```python id="v9m4qx"
registro = {
    "nombre": "Ana",
    "edad": 20,
    "activo": True,
    "notas": [15, 18, 14]
}

print(registro)
```

## Iteración

Al iterar directamente sobre un diccionario, se recorren sus claves.

```python id="p3q7rw"
datos = {"nombre": "Ana", "edad": 20, "ciudad": "Lima"}

for clave in datos:
    print(clave)
```

Salida:

```text id="n6m1qv"
nombre
edad
ciudad
```

## Iteración explícita sobre claves

```python id="t5q9pk"
datos = {"nombre": "Ana", "edad": 20}

for clave in datos.keys():
    print(clave)
```

## Iteración sobre valores

```python id="m1q4tw"
datos = {"nombre": "Ana", "edad": 20}

for valor in datos.values():
    print(valor)
```

Salida:

```text id="r8m2px"
Ana
20
```

## Iteración sobre pares clave-valor

```python id="v6q7rk"
datos = {"nombre": "Ana", "edad": 20}

for clave, valor in datos.items():
    print(clave, valor)
```

Salida:

```text id="p2m9qw"
nombre Ana
edad 20
```

## Pertenencia

El operador `in` verifica pertenencia de claves, no de valores.

```python id="q4m1tv"
datos = {"nombre": "Ana", "edad": 20}

print("nombre" in datos)
print("Ana" in datos)
```

Salida:

```text id="n7m5pk"
True
False
```

Si se quiere verificar un valor, debe hacerse explícitamente:

```python id="r9q3wx"
datos = {"nombre": "Ana", "edad": 20}

print("Ana" in datos.values())
```

Salida:

```text id="m2q8rv"
True
```

## Conversión con `dict()`

La función `dict()` permite construir diccionarios de varias formas.

## Desde pares clave-valor

```python id="t1m6qw"
datos = dict([("nombre", "Ana"), ("edad", 20)])
print(datos)
```

Salida:

```text id="r5m9pk"
{'nombre': 'Ana', 'edad': 20}
```

## Desde argumentos nombrados

```python id="v8q2rw"
datos = dict(nombre="Ana", edad=20)
print(datos)
```

Salida:

```text id="p3m7tv"
{'nombre': 'Ana', 'edad': 20}
```

## Desde `zip()`

```python id="m4q1pk"
claves = ["nombre", "edad", "ciudad"]
valores = ["Ana", 20, "Lima"]

datos = dict(zip(claves, valores))
print(datos)
```

Salida:

```text id="n9m6qw"
{'nombre': 'Ana', 'edad': 20, 'ciudad': 'Lima'}
```

## Métodos principales

`dict` tiene muchos métodos propios, documentados aparte en la sección de métodos de tipos.

Ejemplos frecuentes:

```python id="r2q7pk"
datos = {"nombre": "Ana", "edad": 20}

print(datos.get("nombre"))
print(datos.get("ciudad"))
print(datos.keys())
print(datos.values())
print(datos.items())
```

Salida:

```text id="v7m3rw"
Ana
None
dict_keys(['nombre', 'edad'])
dict_values(['Ana', 20])
dict_items([('nombre', 'Ana'), ('edad', 20)])
```

## Diferencia entre acceso directo y `get()`

Acceso directo:

```python id="p1q8tv"
datos = {"nombre": "Ana"}
print(datos["nombre"])
```

Esto exige que la clave exista.

Con `get()`:

```python id="m6q2pk"
datos = {"nombre": "Ana"}

print(datos.get("nombre"))
print(datos.get("edad"))
print(datos.get("edad", "No disponible"))
```

Salida:

```text id="r4m9qw"
Ana
None
No disponible
```

`get()` es útil cuando la clave puede no existir.

## Eliminación de claves

Puede eliminarse una entrada con `del`.

```python id="v3q7rw"
datos = {"nombre": "Ana", "edad": 20}
del datos["edad"]

print(datos)
```

Salida:

```text id="p8m1tv"
{'nombre': 'Ana'}
```

Si la clave no existe, se genera `KeyError`.

## Copia de diccionarios

## Referencia compartida

```python id="m2q6pk"
a = {"nombre": "Ana"}
b = a

b["edad"] = 20

print(a)
print(b)
```

Salida:

```text id="r7m4qw"
{'nombre': 'Ana', 'edad': 20}
{'nombre': 'Ana', 'edad': 20}
```

No se creó copia. Ambas variables apuntan al mismo diccionario.

## Copia superficial

```python id="t9q3rw"
a = {"nombre": "Ana"}
b = a.copy()

b["edad"] = 20

print(a)
print(b)
```

Salida:

```text id="n1m8tv"
{'nombre': 'Ana'}
{'nombre': 'Ana', 'edad': 20}
```

## Orden de inserción

Los diccionarios preservan el orden de inserción de sus claves.

```python id="p5q7pk"
datos = {}
datos["b"] = 2
datos["a"] = 1
datos["c"] = 3

print(datos)
```

Salida:

```text id="v2m4qw"
{'b': 2, 'a': 1, 'c': 3}
```

Esto no significa que el acceso sea posicional, sino que el recorrido conserva el orden de inserción.

## Diccionarios anidados

Un diccionario puede contener otros diccionarios.

```python id="m8q1tv"
persona = {
    "nombre": "Ana",
    "direccion": {
        "ciudad": "Lima",
        "pais": "Perú"
    }
}

print(persona["direccion"]["ciudad"])
```

Salida:

```text id="r3m6pk"
Lima
```

## Comprensiones de diccionario

Las comprensiones permiten construir diccionarios de forma compacta.

```python id="t4q9rw"
cuadrados = {x: x ** 2 for x in range(5)}
print(cuadrados)
```

Salida:

```text id="n7m2tv"
{0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

Con condición:

```python id="p1m5pk"
pares = {x: x * 10 for x in range(10) if x % 2 == 0}
print(pares)
```

Salida:

```text id="v6m8qw"
{0: 0, 2: 20, 4: 40, 6: 60, 8: 80}
```

## Truthiness

En contexto booleano:

* un diccionario vacío se evalúa como `False`
* un diccionario no vacío se evalúa como `True`

```python id="r9m3tv"
print(bool({}))
print(bool({"a": 1}))
```

Salida:

```text id="m4q7pk"
False
True
```

## Casos de uso frecuentes

## Registros simples

```python id="t2m8qw"
persona = {"nombre": "Ana", "edad": 20, "ciudad": "Lima"}
print(persona)
```

## Configuración

```python id="n5m1tv"
config = {"host": "localhost", "port": 8000, "debug": True}
print(config)
```

## Conteos

```python id="p8m4pk"
frecuencias = {"a": 3, "b": 1, "c": 2}
print(frecuencias)
```

## Asociación de claves y valores

```python id="v1m7qw"
codigos = {"P001": "Teclado", "P002": "Mouse"}
print(codigos["P001"])
```

## Errores comunes

## Suponer que `in` busca valores

Problemático:

```python id="r4m2tv"
datos = {"nombre": "Ana"}
print("Ana" in datos)
```

Salida:

```text id="m9q6pk"
False
```

`in` busca claves, no valores.

## Acceder a una clave inexistente sin control

Problemático:

```python id="t7m3qw"
datos = {"nombre": "Ana"}
print(datos["edad"])
```

Esto genera `KeyError`.

## Usar claves mutables

Problemático:

```python id="n1m8pk"
datos = {[1, 2]: "valor"}
```

Esto genera `TypeError` porque la clave no es hashable.

## Suponer que la asignación crea copia

Problemático:

```python id="p4m9tv"
a = {"nombre": "Ana"}
b = a
```

Aquí no hay copia. Ambas variables apuntan al mismo diccionario.

## Confundir acceso por clave con acceso por posición

Problemático:

```python id="v7m2qw"
datos = {"a": 1, "b": 2}
print(datos[0])
```

Esto genera `KeyError` porque el acceso se hace por clave, no por índice posicional.

## Buenas prácticas

## Usar diccionarios cuando los datos tengan nombres de campo

```python id="r2m5pk"
persona = {"nombre": "Ana", "edad": 20}
```

## Usar `get()` cuando una clave pueda no existir

```python id="t5m8qw"
ciudad = datos.get("ciudad", "No registrada")
```

## Usar `items()` cuando se necesiten clave y valor al iterar

```python id="n8m1tv"
for clave, valor in datos.items():
    print(clave, valor)
```

## Copiar explícitamente cuando no se quiera compartir referencia

```python id="p3m6pk"
copia = datos.copy()
```

## Elegir claves estables y claras

Las claves suelen funcionar mejor cuando son simples, explícitas y hashables.

## Ejemplo integrado

```python id="v6m9qw"
def show_student_record(student):
    print("Ficha de estudiante")
    print("-" * 30)
    print("Nombre:", student.get("nombre", "No disponible"))
    print("Edad:", student.get("edad", "No disponible"))
    print("Carrera:", student.get("carrera", "No disponible"))
    print("Tiene correo:", "correo" in student)
    print("Campos registrados:", len(student))


student = {
    "nombre": "Ana",
    "edad": 20,
    "carrera": "Ingeniería Económica"
}

show_student_record(student)
```

Salida aproximada:

```text id="r1m4tv"
Ficha de estudiante
------------------------------
Nombre: Ana
Edad: 20
Carrera: Ingeniería Económica
Tiene correo: False
Campos registrados: 3
```

## Relación con otros elementos integrados

`dict` se relaciona especialmente con:

* `list` y `tuple` como otras estructuras para agrupar datos
* `set`, que comparte la noción de hashabilidad en sus elementos
* `zip()` y `dict()` para construcción de diccionarios
* `len()` para contar claves
* métodos de diccionario, documentados en la sección correspondiente

## Orden didáctico interno

```text id="m7q2pk"
1. Propósito de dict
2. Naturaleza del tipo
3. Literales y construcción con dict()
4. Acceso por clave
5. Mutabilidad y asignación
6. Iteración sobre claves, valores e items
7. Copia y referencias
8. Comprensiones de diccionario
9. Truthiness
10. Errores comunes
11. Buenas prácticas
```