# `None`

## Propósito

`None` representa la ausencia de valor. Se utiliza para indicar que una variable no tiene un dato significativo asignado, que una función no devuelve un resultado útil, o que un valor todavía no existe, no aplica o no está disponible.

## Naturaleza del tipo

`None` es un valor único del lenguaje.

Su tipo es:

```python id="r8uwnv"
NoneType
````

Esto puede comprobarse con:

```python id="owm2cj"
print(type(None))
```

Salida:

```text id="ewii7t"
<class 'NoneType'>
```

En uso cotidiano, lo importante es el valor `None`, no el nombre técnico `NoneType`.

## Idea central

`None` no significa:

* `0`
* `False`
* `""`
* `[]`
* `{}`
* `set()`

Todos esos valores son distintos de `None`.

`None` expresa específicamente:

```text id="5fzf7r"
no hay valor
```

o

```text id="zow4a6"
ausencia de resultado significativo
```

## Forma general

`None` es una palabra reservada del lenguaje.

```python id="xevxbi"
valor = None
print(valor)
```

Salida:

```text id="kgtg9q"
None
```

## Valor de retorno de `type()`

```python id="sau2fe"
print(type(None))
```

Salida:

```text id="g9zkkv"
<class 'NoneType'>
```

## Unicidad

`None` es un singleton. Eso significa que existe una única instancia de ese valor en todo el programa.

Por eso, para compararlo, la forma idiomática es:

```python id="a9mgg3"
if valor is None:
    print("No hay valor")
```

y no:

```python id="u7y8ss"
if valor == None:
    print("No hay valor")
```

## Truthiness

En contexto booleano, `None` se evalúa como `False`.

```python id="jzvzhz"
print(bool(None))
```

Salida:

```text id="ke6dlq"
False
```

Ejemplo:

```python id="csi0po"
valor = None

if valor:
    print("Verdadero")
else:
    print("Falso")
```

Salida:

```text id="1j0zzu"
Falso
```

## Relación con funciones

Si una función no usa `return`, devuelve `None`.

```python id="mgjlwm"
def saludar():
    print("Hola")

resultado = saludar()

print(resultado)
print(type(resultado))
```

Salida:

```text id="voek3t"
Hola
None
<class 'NoneType'>
```

Esto es fundamental para entender por qué una función que solo imprime no devuelve un dato reutilizable.

## Uso como valor por defecto

`None` se usa con frecuencia como valor inicial o valor por defecto.

```python id="juxfww"
resultado = None

print(resultado)
```

También se usa en parámetros opcionales:

```python id="3aqvsr"
def agregar_elemento(valor, lista=None):
    if lista is None:
        lista = []

    lista.append(valor)
    return lista

print(agregar_elemento(1))
print(agregar_elemento(2))
```

Salida:

```text id="ldbpog"
[1]
[2]
```

En este patrón, `None` permite distinguir entre:

* no se proporcionó argumento
* sí se proporcionó un objeto concreto

## Uso en búsquedas o resultados ausentes

`None` puede indicar que una búsqueda no encontró resultado.

```python id="nvxue9"
def buscar_usuario(usuarios, nombre):
    for usuario in usuarios:
        if usuario == nombre:
            return usuario
    return None

resultado = buscar_usuario(["Ana", "Luis"], "Marta")
print(resultado)
```

Salida:

```text id="gd3zbf"
None
```

## Uso en estructuras de datos

Una variable, lista o diccionario puede contener `None`.

```python id="cfgoyl"
datos = {
    "nombre": "Ana",
    "telefono": None
}

print(datos)
```

Salida:

```text id="3n8jie"
{'nombre': 'Ana', 'telefono': None}
```

Esto puede significar que el campo existe, pero no tiene valor registrado.

## Comparación correcta con `None`

La forma correcta es usar `is` o `is not`.

## Correcto

```python id="ldjlwm"
valor = None

if valor is None:
    print("Sin valor")

if valor is not None:
    print("Con valor")
```

## Menos conveniente

```python id="7kbxkq"
if valor == None:
    print("Sin valor")
```

La recomendación general en Python es usar identidad, no igualdad, para `None`.

## Diferencia entre `None` y valores vacíos

Estos valores no son `None`:

```python id="fkxpgm"
print(None == 0)
print(None == False)
print(None == "")
print(None == [])
print(None == {})
```

Salida:

```text id="n6vryb"
False
False
False
False
False
```

También sus tipos son distintos:

```python id="svd55i"
print(type(None))
print(type(False))
print(type(0))
print(type(""))
print(type([]))
```

Salida:

```text id="s3ku9s"
<class 'NoneType'>
<class 'bool'>
<class 'int'>
<class 'str'>
<class 'list'>
```

## Uso con `isinstance()`

Puede verificarse así:

```python id="1cxdk8"
print(isinstance(None, type(None)))
```

Salida:

```text id="b44q2n"
True
```

Aun así, en la práctica suele ser más claro usar:

```python id="ulvpzk"
valor is None
```

## Uso en APIs y diseño de funciones

`None` suele aparecer cuando:

* un dato es opcional
* un valor no fue calculado todavía
* una operación no encontró resultado
* una función no devuelve nada útil
* se necesita distinguir entre “vacío” y “ausente”

Ejemplo:

```python id="bjguub"
def obtener_descuento(cliente):
    if cliente == "vip":
        return 0.2
    return None

print(obtener_descuento("normal"))
```

Salida:

```text id="4j1lr7"
None
```

Aquí `None` puede significar que no aplica descuento, no simplemente que el descuento sea cero.

## Casos de uso frecuentes

## Inicialización de variables

```python id="ynmzsk"
resultado = None
print(resultado)
```

## Retorno explícito de ausencia

```python id="t0h7ri"
def buscar_id(ids, objetivo):
    for valor in ids:
        if valor == objetivo:
            return valor
    return None
```

## Parámetros opcionales

```python id="pi1sk9"
def procesar(datos=None):
    if datos is None:
        datos = []
    return datos
```

## Marcador de dato faltante

```python id="3l7w2w"
usuario = {
    "nombre": "Ana",
    "correo": None
}
```

## Errores comunes

## Confundir `None` con `False`

Problemático:

```python id="6sfx11"
valor = None

if valor == False:
    print("Falso")
```

Salida:

```text id="pfm8ve"
False
```

`None` no es igual a `False`.

## Confundir `None` con cero o vacío

Problemático:

```python id="3wimso"
print(None == 0)
print(None == "")
print(None == [])
```

Todos esos resultados son `False`.

## Comparar con `== None`

Menos idiomático:

```python id="p5zlku"
if valor == None:
    print("Sin valor")
```

Más correcto:

```python id="71p7z8"
if valor is None:
    print("Sin valor")
```

## Usar el resultado de una función que solo imprime

Problemático:

```python id="90v01r"
def mostrar():
    print("Hola")

x = mostrar()
print(x + 1)
```

Aquí `x` vale `None`, por lo que la operación fallará.

## Buenas prácticas

## Usar `None` para ausencia real de valor

```python id="1cav22"
resultado = None
```

## Comparar con `is None` y `is not None`

```python id="t88v8g"
if valor is None:
    print("Sin valor")
```

## Usar `None` como valor por defecto en parámetros mutables

```python id="pzk0iy"
def agregar(valor, lista=None):
    if lista is None:
        lista = []
    lista.append(valor)
    return lista
```

## Distinguir entre vacío y ausente

No es lo mismo:

* una lista vacía `[]`
* una cadena vacía `""`
* el valor ausente `None`

## Ejemplo integrado

```python id="8dj2v6"
def find_student(students, name):
    for student in students:
        if student == name:
            return student
    return None


def show_search_result(students, name):
    result = find_student(students, name)

    print("Resultado de búsqueda")
    print("-" * 30)
    print("Nombre buscado:", name)
    print("Resultado bruto:", result)
    print("Tipo:", type(result))

    if result is None:
        print("No se encontró estudiante")
    else:
        print("Estudiante encontrado:", result)


show_search_result(["Ana", "Luis", "Marta"], "Carlos")
```

Salida aproximada:

```text id="uh8nwb"
Resultado de búsqueda
------------------------------
Nombre buscado: Carlos
Resultado bruto: None
Tipo: <class 'NoneType'>
No se encontró estudiante
```

## Relación con otros elementos integrados

`None` se relaciona especialmente con:

* `bool()`, porque se evalúa como falso
* `type()`, porque su tipo es `NoneType`
* `is` e `is not`, por la comparación idiomática correcta
* funciones sin `return`, que devuelven `None`
* diseño de parámetros opcionales y resultados ausentes

## Orden didáctico interno

```text id="muw6ui"
1. Propósito de None
2. Naturaleza del valor
3. Relación con NoneType
4. Truthiness
5. Uso en funciones
6. Uso como valor por defecto
7. Comparación correcta con is None
8. Diferencia frente a valores vacíos
9. Errores comunes
10. Buenas prácticas
```