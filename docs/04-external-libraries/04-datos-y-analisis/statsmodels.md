# `statsmodels`

## Propósito

`statsmodels` es una librería externa para modelamiento estadístico y econométrico en Python. Se utiliza para estimar modelos, interpretar coeficientes, realizar pruebas estadísticas, analizar residuos, trabajar con series de tiempo y obtener resultados detallados de inferencia.

A diferencia de librerías orientadas principalmente a predicción, como `scikit-learn`, `statsmodels` pone mayor énfasis en la interpretación estadística del modelo: coeficientes, errores estándar, valores p, intervalos de confianza, pruebas de hipótesis y diagnósticos.

## Naturaleza de la librería

`statsmodels` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install statsmodels
```

Importaciones frecuentes:

```python
import statsmodels.api as sm
import statsmodels.formula.api as smf
```

También puede combinarse con:

```python
import pandas as pd
import numpy as np
```

## Idea central

La idea principal de `statsmodels` es estimar modelos estadísticos sobre datos estructurados.

Ejemplo mínimo con fórmula:

```python
import pandas as pd
import statsmodels.formula.api as smf

df = pd.DataFrame({
    "sales": [10, 12, 15, 18, 20],
    "price": [5, 4, 3, 2, 1]
})

model = smf.ols(
    formula="sales ~ price",
    data=df
)

results = model.fit()

print(results.summary())
```

## Instalación

Instalación básica:

```bash
python -m pip install statsmodels
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
statsmodels==0.x.x
```

La versión exacta puede variar según el entorno.

## Importación

## API general

```python
import statsmodels.api as sm
```

Se usa con arreglos, matrices o estructuras ya preparadas.

## API de fórmulas

```python
import statsmodels.formula.api as smf
```

Permite definir modelos usando fórmulas de estilo estadístico. La documentación oficial describe `statsmodels.formula.api` como una interfaz conveniente para especificar modelos mediante cadenas de fórmula y `DataFrame`. :contentReference[oaicite:1]{index=1}

## Verificación

```python
import statsmodels

print(statsmodels.__version__)
```

## API general vs API de fórmulas

## API general

La API general suele requerir definir explícitamente la variable dependiente y la matriz de variables explicativas.

```python
import statsmodels.api as sm

y = df["sales"]
x = df[["price"]]

x = sm.add_constant(x)

model = sm.OLS(y, x)
results = model.fit()
```

## API de fórmulas

La API de fórmulas permite escribir una relación estadística de forma más compacta.

```python
import statsmodels.formula.api as smf

model = smf.ols(
    formula="sales ~ price",
    data=df
)

results = model.fit()
```

## Regla práctica

La API de fórmulas suele ser más cómoda para análisis estadístico y econométrico.

La API general suele ser útil cuando las matrices ya están preparadas o cuando se necesita más control sobre los arreglos de entrada.

## Fórmulas

`statsmodels` permite ajustar modelos usando fórmulas de estilo R. Internamente usa `patsy` para convertir fórmulas y datos en matrices de diseño. :contentReference[oaicite:2]{index=2}

## Forma básica

```text
variable_dependiente ~ variables_explicativas
```

Ejemplo:

```python
"sales ~ price"
```

## Varias variables explicativas

```python
"sales ~ price + advertising + income"
```

## Variable categórica

```python
"sales ~ price + C(region)"
```

## Interacción

```python
"sales ~ price * advertising"
```

Esto incluye:

```text
price
advertising
price:advertising
```

## Sin intercepto

```python
"sales ~ price - 1"
```

## Regresión lineal con OLS

## Propósito

OLS, por Ordinary Least Squares, estima una relación lineal entre una variable dependiente y una o más variables explicativas. La documentación oficial de regresión lineal de `statsmodels` incluye modelos como OLS, WLS, GLS y GLSAR. :contentReference[oaicite:3]{index=3}

## Ejemplo básico

```python
import pandas as pd
import statsmodels.formula.api as smf

df = pd.DataFrame({
    "y": [2, 4, 5, 4, 6],
    "x": [1, 2, 3, 4, 5]
})

model = smf.ols(
    formula="y ~ x",
    data=df
)

results = model.fit()

print(results.summary())
```

## Coeficientes

```python
print(results.params)
```

## Valores p

```python
print(results.pvalues)
```

## Intervalos de confianza

```python
print(results.conf_int())
```

## Residuos

```python
print(results.resid)
```

## Valores ajustados

```python
print(results.fittedvalues)
```

## Predicción

```python
new_data = pd.DataFrame({
    "x": [6, 7]
})

predictions = results.predict(new_data)

print(predictions)
```

## Interpretación básica de resultados

El resumen de un modelo OLS suele incluir:

```text
coef
std err
t
P>|t|
R-squared
Adj. R-squared
F-statistic
AIC
BIC
```

## `coef`

Estimación del efecto de cada variable explicativa sobre la variable dependiente, manteniendo constantes las demás variables del modelo.

## `std err`

Error estándar del coeficiente estimado.

## `P>|t|`

Valor p asociado a la prueba de hipótesis sobre el coeficiente.

## `R-squared`

Proporción de variabilidad explicada por el modelo lineal.

## `AIC` y `BIC`

Criterios de información usados para comparar modelos, especialmente cuando se evalúan especificaciones alternativas.

## Agregar constante en API general

Cuando se usa `statsmodels.api`, normalmente debe agregarse una constante de forma explícita.

```python
import statsmodels.api as sm

x = df[["x"]]
x = sm.add_constant(x)

y = df["y"]

model = sm.OLS(y, x)
results = model.fit()

print(results.summary())
```

En cambio, con fórmulas, el intercepto se incluye por defecto salvo que se indique lo contrario.

## Variables categóricas

Con la API de fórmulas puede usarse `C()` para indicar que una variable debe tratarse como categórica.

```python
model = smf.ols(
    formula="sales ~ price + C(region)",
    data=df
)

results = model.fit()
```

Esto permite generar variables indicadoras para categorías.

## Modelos lineales generalizados

## Propósito

Los modelos lineales generalizados permiten modelar variables dependientes que no siguen necesariamente una distribución normal. La documentación oficial indica que `statsmodels` soporta GLM usando familias exponenciales de un parámetro. :contentReference[oaicite:4]{index=4}

## Ejemplo con familia binomial

```python
import pandas as pd
import statsmodels.api as sm
import statsmodels.formula.api as smf

df = pd.DataFrame({
    "default": [0, 0, 1, 0, 1, 1],
    "income": [30, 40, 20, 50, 25, 15]
})

model = smf.glm(
    formula="default ~ income",
    data=df,
    family=sm.families.Binomial()
)

results = model.fit()

print(results.summary())
```

## Familias frecuentes

```python
sm.families.Gaussian()
sm.families.Binomial()
sm.families.Poisson()
sm.families.Gamma()
```

## Regresión logística

Para una variable dependiente binaria, puede usarse una familia binomial dentro de GLM.

```python
model = smf.glm(
    formula="approved ~ income + age",
    data=df,
    family=sm.families.Binomial()
)
```

También existen modelos discretos específicos, como `logit`.

```python
model = smf.logit(
    formula="approved ~ income + age",
    data=df
)

results = model.fit()
```

## Regresión Poisson

Para conteos, puede usarse una familia Poisson.

```python
model = smf.glm(
    formula="claims ~ exposure + age",
    data=df,
    family=sm.families.Poisson()
)

results = model.fit()
```

## Modelos con variable dependiente discreta

`statsmodels` incluye modelos para variables dependientes discretas, como:

```text
Logit
Probit
Poisson
NegativeBinomial
MNLogit
```

Ejemplo con `logit`:

```python
model = smf.logit(
    formula="approved ~ income + debt_ratio",
    data=df
)

results = model.fit()

print(results.summary())
```

## Pruebas estadísticas

`statsmodels` incluye herramientas para pruebas estadísticas y diagnósticos.

Importaciones frecuentes:

```python
from statsmodels.stats.diagnostic import het_breuschpagan
from statsmodels.stats.stattools import durbin_watson
from statsmodels.stats.outliers_influence import variance_inflation_factor
```

## Durbin-Watson

Prueba relacionada con autocorrelación de residuos.

```python
from statsmodels.stats.stattools import durbin_watson

dw = durbin_watson(results.resid)

print(dw)
```

## Breusch-Pagan

Prueba relacionada con heterocedasticidad.

```python
from statsmodels.stats.diagnostic import het_breuschpagan

test = het_breuschpagan(
    results.resid,
    results.model.exog
)

print(test)
```

## VIF

El factor de inflación de varianza se usa para revisar multicolinealidad.

```python
from statsmodels.stats.outliers_influence import variance_inflation_factor

x = results.model.exog

vif_values = [
    variance_inflation_factor(x, i)
    for i in range(x.shape[1])
]

print(vif_values)
```

## Errores estándar robustos

Puede obtenerse una versión de resultados con errores estándar robustos.

```python
robust_results = results.get_robustcov_results(
    cov_type="HC1"
)

print(robust_results.summary())
```

Esto es útil cuando se sospecha heterocedasticidad.

## Series de tiempo

`statsmodels` tiene un bloque amplio para análisis de series de tiempo. Su documentación incluye modelos autoregresivos, ARIMA, SARIMAX, ARDL y herramientas relacionadas. :contentReference[oaicite:5]{index=5}

## ARIMA

La clase `ARIMA` es una interfaz para modelos tipo ARIMA, incluyendo casos autoregresivos, medias móviles, ARMA, ARIMA y componentes estacionales según la documentación oficial. :contentReference[oaicite:6]{index=6}

```python
import pandas as pd
from statsmodels.tsa.arima.model import ARIMA

series = pd.Series(
    [100, 105, 107, 110, 115, 120, 125]
)

model = ARIMA(series, order=(1, 1, 1))
results = model.fit()

print(results.summary())
```

## Pronóstico con ARIMA

```python
forecast = results.forecast(steps=3)

print(forecast)
```

## SARIMAX

`SARIMAX` permite trabajar con modelos con componentes estacionales y variables exógenas, además de estructuras tipo ARMA en representación de espacio de estados. :contentReference[oaicite:7]{index=7}

```python
from statsmodels.tsa.statespace.sarimax import SARIMAX

model = SARIMAX(
    series,
    order=(1, 1, 1),
    seasonal_order=(1, 0, 1, 12)
)

results = model.fit()

print(results.summary())
```

## Descomposición de series

```python
from statsmodels.tsa.seasonal import seasonal_decompose

decomposition = seasonal_decompose(
    series,
    model="additive",
    period=12
)

print(decomposition.trend)
print(decomposition.seasonal)
print(decomposition.resid)
```

## Autocorrelación

```python
from statsmodels.tsa.stattools import acf, pacf

acf_values = acf(series)
pacf_values = pacf(series)

print(acf_values)
print(pacf_values)
```

## Relación con pandas

`statsmodels` se integra frecuentemente con `pandas`, especialmente mediante la API de fórmulas.

```python
import pandas as pd
import statsmodels.formula.api as smf

df = pd.DataFrame({
    "y": [1, 2, 3, 4],
    "x": [2, 4, 6, 8]
})

results = smf.ols("y ~ x", data=df).fit()
```

## Relación con NumPy

La API general acepta arreglos y matrices.

```python
import numpy as np
import statsmodels.api as sm

y = np.array([1, 2, 3, 4])
x = np.array([2, 4, 6, 8])

x = sm.add_constant(x)

results = sm.OLS(y, x).fit()
```

## Relación con matplotlib

Los resultados pueden visualizarse con `matplotlib`.

```python
import matplotlib.pyplot as plt

plt.scatter(df["x"], df["y"])
plt.plot(df["x"], results.fittedvalues)
plt.show()
```

## Relación con scipy

`statsmodels` complementa a `scipy` en estadística. `scipy.stats` ofrece distribuciones, pruebas y herramientas estadísticas generales. `statsmodels` se enfoca más en estimación, inferencia y resultados detallados para modelos estadísticos.

## Relación con scikit-learn

`scikit-learn` se orienta más a machine learning, predicción, pipelines y validación fuera de muestra.

`statsmodels` se orienta más a inferencia estadística, interpretación de coeficientes, pruebas y diagnósticos.

## Casos de uso frecuentes

## Regresión econométrica

```python
results = smf.ols(
    formula="wage ~ education + experience + C(region)",
    data=df
).fit()
```

## Modelo con errores robustos

```python
results = smf.ols(
    formula="y ~ x1 + x2",
    data=df
).fit(cov_type="HC1")
```

## Modelo logístico

```python
results = smf.logit(
    formula="approved ~ income + debt_ratio",
    data=df
).fit()
```

## Forecasting con ARIMA

```python
model = ARIMA(series, order=(1, 1, 1))
results = model.fit()
forecast = results.forecast(steps=6)
```

## Diagnóstico de residuos

```python
residuals = results.resid
print(residuals.describe())
```

## Errores comunes

## No agregar constante en API general

Problemático:

```python
model = sm.OLS(y, x)
```

Si se requiere intercepto, debe agregarse:

```python
x = sm.add_constant(x)
model = sm.OLS(y, x)
```

En fórmulas, el intercepto se incluye por defecto.

## Confundir correlación con causalidad

Un coeficiente significativo no demuestra por sí solo causalidad. La interpretación depende del diseño del estudio, supuestos, variables omitidas, endogeneidad, temporalidad y contexto.

## Ignorar supuestos del modelo

En regresión lineal, conviene revisar aspectos como:

```text
linealidad
heterocedasticidad
autocorrelación
multicolinealidad
valores atípicos
especificación del modelo
```

## Interpretar valores p mecánicamente

Un valor p debe interpretarse junto con magnitud del efecto, intervalo de confianza, tamaño de muestra, calidad de datos y supuestos del modelo.

## No revisar residuos

Un modelo puede tener buenos indicadores agregados y aun así mostrar problemas en residuos.

## Usar ARIMA sin revisar la serie

Antes de estimar modelos de series de tiempo conviene analizar:

```text
frecuencia
tendencia
estacionalidad
estacionariedad
valores faltantes
outliers
rupturas estructurales
```

## No controlar categorías correctamente

Si una variable debe tratarse como categórica, en fórmulas puede usarse:

```python
C(variable)
```

## Usar nombres de columnas problemáticos en fórmulas

Los nombres con espacios, símbolos o caracteres especiales pueden dificultar el uso de fórmulas. Conviene usar nombres limpios:

```text
snake_case
sin espacios
sin símbolos innecesarios
```

## Buenas prácticas

## Usar la API de fórmulas para modelos interpretables

```python
results = smf.ols("y ~ x1 + x2", data=df).fit()
```

## Revisar el resumen del modelo

```python
print(results.summary())
```

## Extraer resultados de forma programática

```python
params = results.params
pvalues = results.pvalues
conf_int = results.conf_int()
```

## Guardar diagnósticos relevantes

```python
residuals = results.resid
fitted = results.fittedvalues
```

## Separar preparación de datos y estimación

```python
clean_df = prepare_data(df)
results = estimate_model(clean_df)
```

## Documentar especificación del modelo

Debe quedar claro:

```text
variable dependiente
variables explicativas
muestra usada
filtros aplicados
tratamiento de faltantes
tipo de errores estándar
supuestos relevantes
```

## Usar errores robustos cuando corresponda

```python
results = smf.ols("y ~ x1 + x2", data=df).fit(cov_type="HC1")
```

## Validar interpretación con contexto estadístico

La salida numérica no reemplaza el criterio estadístico ni el conocimiento del problema.

## Ejemplo integrado

```python
from pathlib import Path

import pandas as pd
import statsmodels.formula.api as smf


def load_data(input_path):
    return pd.read_excel(input_path)


def prepare_data(df):
    result = df.copy()

    result["sales"] = pd.to_numeric(
        result["sales"],
        errors="coerce"
    )

    result["price"] = pd.to_numeric(
        result["price"],
        errors="coerce"
    )

    result["advertising"] = pd.to_numeric(
        result["advertising"],
        errors="coerce"
    )

    result = result.dropna(
        subset=["sales", "price", "advertising"]
    )

    return result


def estimate_model(df):
    model = smf.ols(
        formula="sales ~ price + advertising",
        data=df
    )

    return model.fit(cov_type="HC1")


def export_results(results, output_path):
    table = pd.DataFrame({
        "coef": results.params,
        "pvalue": results.pvalues
    })

    table.to_excel(output_path)


input_path = Path("sales_data.xlsx")
output_path = Path("model_results.xlsx")

data = load_data(input_path)
clean_data = prepare_data(data)
results = estimate_model(clean_data)

print(results.summary())

export_results(results, output_path)

print("Resultados exportados correctamente")
```

## Relación con otras librerías

`statsmodels` se relaciona especialmente con:

- `pandas`, para datos tabulares y API de fórmulas
- `numpy`, para arreglos y matrices
- `scipy`, para herramientas estadísticas y numéricas complementarias
- `matplotlib`, para gráficos de residuos, ajustes y series
- `seaborn`, para visualización estadística
- `scikit-learn`, como contraste entre inferencia estadística y machine learning predictivo
- `openpyxl`, para exportar resultados a Excel mediante pandas

## Orden didáctico interno

```text
1. Propósito de statsmodels
2. Instalación e importación
3. API general y API de fórmulas
4. OLS
5. Interpretación de resultados
6. Variables categóricas e interacciones
7. GLM y modelos discretos
8. Pruebas estadísticas y diagnósticos
9. Series de tiempo
10. Relación con pandas, numpy y scipy
11. Errores comunes
12. Buenas prácticas
```