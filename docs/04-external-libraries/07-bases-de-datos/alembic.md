El siguiente archivo debería ser:

```text
docs/04-external-libraries/08-bases-de-datos/alembic.md
```

Base factual principal: `Alembic` es una herramienta ligera de migraciones de base de datos diseñada para usarse con SQLAlchemy. Permite crear, gestionar y ejecutar scripts de cambios de esquema sobre bases relacionales. Su documentación oficial cubre comandos como `alembic init`, `alembic revision`, `alembic upgrade`, `alembic downgrade`, migraciones autogeneradas con `--autogenerate`, configuración mediante `alembic.ini` y `env.py`, además de generación de scripts SQL en modo offline. ([Alembic][1])

Contenido propuesto:

````markdown
# `alembic`

## Propósito

`alembic` es una librería externa para gestionar migraciones de base de datos en proyectos que usan SQLAlchemy.

Se utiliza para registrar, versionar y aplicar cambios en el esquema de una base de datos, como creación de tablas, modificación de columnas, eliminación de campos, creación de índices, restricciones, relaciones y otros cambios estructurales.

Su objetivo principal es evitar que el esquema de la base de datos se modifique manualmente sin control histórico.

## Naturaleza de la librería

`alembic` no forma parte de la biblioteca estándar de Python.

Debe instalarse antes de usarse:

```bash
python -m pip install alembic
```

La herramienta se usa principalmente desde línea de comandos:

```bash
alembic init migrations
alembic revision -m "create products table"
alembic upgrade head
```

También se integra con código Python mediante archivos de configuración y scripts de migración.

## Relación con SQLAlchemy

`alembic` está diseñado para trabajar junto con SQLAlchemy.

Relación conceptual:

```text
SQLAlchemy -> define modelos y metadatos
Alembic    -> versiona y aplica cambios del esquema
Base de datos -> guarda las tablas reales
```

Ejemplo:

```text
models.py cambia
alembic revision --autogenerate crea una migración
alembic upgrade head aplica el cambio en la base de datos
```

## Problema que resuelve

Cuando un proyecto crece, el esquema de la base de datos cambia muchas veces.

Ejemplos:

```text
crear una tabla nueva
agregar una columna
renombrar un campo
crear un índice
agregar una restricción
cambiar el tipo de una columna
eliminar una tabla
```

Sin migraciones, estos cambios suelen hacerse manualmente y pueden generar diferencias entre entornos.

Con Alembic, cada cambio queda registrado como un archivo versionado.

## Idea central

La idea principal de Alembic es mantener una secuencia de revisiones.

Cada revisión representa un cambio en el esquema.

```text
revision_1 -> revision_2 -> revision_3 -> head
```

Cada archivo de migración suele tener dos funciones principales:

```python
def upgrade():
    ...


def downgrade():
    ...
```

## `upgrade()`

Define cómo aplicar el cambio.

Ejemplo:

```python
def upgrade():
    op.add_column(
        "products",
        sa.Column("stock", sa.Integer(), nullable=False)
    )
```

## `downgrade()`

Define cómo revertir el cambio.

Ejemplo:

```python
def downgrade():
    op.drop_column("products", "stock")
```

## Cuándo usar Alembic

Conviene usar Alembic cuando se necesita:

```text
versionar cambios de base de datos
trabajar con SQLAlchemy
mantener entornos sincronizados
aplicar cambios de esquema en desarrollo, pruebas y producción
revertir migraciones
generar scripts SQL
automatizar cambios estructurales
controlar el historial de cambios de tablas
trabajar en equipo sobre una misma base de datos
```

## Cuándo no usar Alembic

No suele ser necesario cuando:

```text
solo se usa un script temporal
la base de datos se crea y destruye en cada ejecución
no se usa SQLAlchemy
se trabaja únicamente con archivos CSV o Excel
se usa Django ORM con sus propias migraciones
```

En proyectos Django, normalmente se usan las migraciones propias de Django.

En scripts pequeños, puede bastar con `Base.metadata.create_all(engine)`, aunque esto no reemplaza un sistema real de migraciones.

## Instalación

Instalación básica:

```bash
python -m pip install alembic
```

Verificación:

```bash
alembic --version
```

Registro en `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Ejemplo de línea:

```text
alembic==1.x.x
```

La versión exacta puede variar según el entorno.

## Inicializar Alembic

Para crear la estructura inicial:

```bash
alembic init migrations
```

Esto suele crear una estructura similar a:

```text
alembic.ini
migrations/
├─ env.py
├─ README
├─ script.py.mako
└─ versions/
```

## Archivos principales

## `alembic.ini`

Archivo de configuración principal.

Contiene opciones como:

```text
script_location
sqlalchemy.url
loggers
formatters
handlers
```

Ejemplo:

```ini
[alembic]
script_location = migrations
sqlalchemy.url = sqlite:///app.db
```

## `migrations/env.py`

Archivo que configura cómo Alembic se conecta a la base de datos y cómo obtiene los metadatos de SQLAlchemy.

Es un archivo clave para autogenerar migraciones.

## `migrations/versions/`

Carpeta donde se guardan los archivos de migración.

Ejemplo:

```text
migrations/versions/
├─ 4f9a12b8c123_create_products_table.py
├─ 7c2b91a9d456_add_stock_column.py
└─ b1e8c4d2a789_create_index_on_name.py
```

## `script.py.mako`

Plantilla usada para generar nuevos archivos de migración.

Normalmente no se modifica al inicio.

## Configurar la URL de base de datos

En `alembic.ini` puede configurarse directamente:

```ini
sqlalchemy.url = sqlite:///app.db
```

Para PostgreSQL:

```ini
sqlalchemy.url = postgresql+psycopg://user:password@localhost:5432/app
```

Para MySQL:

```ini
sqlalchemy.url = mysql+pymysql://user:password@localhost:3306/app
```

Para SQL Server:

```ini
sqlalchemy.url = mssql+pyodbc://user:password@server/database?driver=ODBC+Driver+18+for+SQL+Server
```

## URL desde variables de entorno

En proyectos reales, no conviene guardar credenciales en `alembic.ini`.

Puede leerse la URL desde una variable de entorno dentro de `env.py`.

Ejemplo conceptual:

```python
import os

from alembic import context
from sqlalchemy import engine_from_config
from sqlalchemy import pool

config = context.config

database_url = os.getenv("DATABASE_URL")

if database_url:
    config.set_main_option("sqlalchemy.url", database_url)
```

## Conectar Alembic con modelos SQLAlchemy

Para que `--autogenerate` funcione, Alembic necesita acceder al `metadata` de los modelos.

Ejemplo de modelos:

```python
from sqlalchemy.orm import DeclarativeBase
from sqlalchemy.orm import Mapped
from sqlalchemy.orm import mapped_column


class Base(DeclarativeBase):
    pass


class Product(Base):
    __tablename__ = "products"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str]
    price: Mapped[float]
```

En `env.py` debe importarse la base:

```python
from app.models import Base

target_metadata = Base.metadata
```

Sin `target_metadata`, Alembic no puede comparar correctamente modelos contra la base de datos.

## Crear una migración manual

```bash
alembic revision -m "create products table"
```

Esto crea un archivo en:

```text
migrations/versions/
```

Ejemplo de archivo generado:

```python
"""create products table

Revision ID: 4f9a12b8c123
Revises:
Create Date: 2026-01-01 10:00:00.000000
"""

from typing import Sequence
from typing import Union

from alembic import op
import sqlalchemy as sa


revision: str = "4f9a12b8c123"
down_revision: Union[str, None] = None
branch_labels: Union[str, Sequence[str], None] = None
depends_on: Union[str, Sequence[str], None] = None


def upgrade() -> None:
    pass


def downgrade() -> None:
    pass
```

## Identificadores de revisión

Cada migración tiene variables como:

```python
revision = "4f9a12b8c123"
down_revision = None
```

## `revision`

Identificador único de la migración.

## `down_revision`

Indica cuál es la migración anterior.

Esto permite formar una cadena de versiones.

```text
revision A -> revision B -> revision C
```

## Crear tabla en una migración

```python
from alembic import op
import sqlalchemy as sa


def upgrade() -> None:
    op.create_table(
        "products",
        sa.Column("id", sa.Integer(), primary_key=True),
        sa.Column("name", sa.String(length=100), nullable=False),
        sa.Column("price", sa.Numeric(10, 2), nullable=False),
        sa.Column("stock", sa.Integer(), nullable=False, server_default="0"),
    )


def downgrade() -> None:
    op.drop_table("products")
```

## Aplicar migraciones

Para aplicar todas las migraciones pendientes:

```bash
alembic upgrade head
```

`head` representa la última revisión disponible.

## Aplicar hasta una revisión específica

```bash
alembic upgrade 4f9a12b8c123
```

También puede usarse un identificador parcial si no hay ambigüedad.

## Revertir migraciones

Para volver una revisión atrás:

```bash
alembic downgrade -1
```

Para volver a una revisión específica:

```bash
alembic downgrade 4f9a12b8c123
```

Para volver al inicio:

```bash
alembic downgrade base
```

## Ver revisión actual

```bash
alembic current
```

Este comando muestra en qué revisión se encuentra la base de datos.

## Ver historial

```bash
alembic history
```

Con más detalle:

```bash
alembic history --verbose
```

## Ver heads

```bash
alembic heads
```

Muestra las revisiones finales disponibles.

En un flujo normal debería haber una sola `head`.

## Crear migración autogenerada

```bash
alembic revision --autogenerate -m "add stock column"
```

Alembic compara:

```text
metadata de SQLAlchemy
esquema actual de la base de datos
```

y genera una migración candidata.

## Importante sobre `--autogenerate`

La migración autogenerada debe revisarse manualmente.

Alembic puede detectar muchos cambios comunes, pero no debe asumirse que siempre genera la migración perfecta.

Conviene revisar:

```text
tablas creadas
columnas agregadas
columnas eliminadas
tipos de datos
índices
restricciones
renombres
valores por defecto
nulabilidad
```

## Flujo común de trabajo

```bash
alembic revision --autogenerate -m "add stock column"
alembic upgrade head
```

Flujo completo:

```text
1. Modificar modelos SQLAlchemy
2. Crear migración con autogenerate
3. Revisar archivo generado
4. Ajustar upgrade() y downgrade()
5. Aplicar con alembic upgrade head
6. Confirmar cambios en Git
```

## Agregar columna

```python
def upgrade() -> None:
    op.add_column(
        "products",
        sa.Column("stock", sa.Integer(), nullable=False, server_default="0")
    )


def downgrade() -> None:
    op.drop_column("products", "stock")
```

## Eliminar columna

```python
def upgrade() -> None:
    op.drop_column("products", "description")


def downgrade() -> None:
    op.add_column(
        "products",
        sa.Column("description", sa.Text(), nullable=True)
    )
```

## Alterar columna

```python
def upgrade() -> None:
    op.alter_column(
        "products",
        "name",
        existing_type=sa.String(length=100),
        type_=sa.String(length=150),
        existing_nullable=False,
    )


def downgrade() -> None:
    op.alter_column(
        "products",
        "name",
        existing_type=sa.String(length=150),
        type_=sa.String(length=100),
        existing_nullable=False,
    )
```

## Crear índice

```python
def upgrade() -> None:
    op.create_index(
        "ix_products_name",
        "products",
        ["name"]
    )


def downgrade() -> None:
    op.drop_index(
        "ix_products_name",
        table_name="products"
    )
```

## Crear restricción única

```python
def upgrade() -> None:
    op.create_unique_constraint(
        "uq_products_name",
        "products",
        ["name"]
    )


def downgrade() -> None:
    op.drop_constraint(
        "uq_products_name",
        "products",
        type_="unique"
    )
```

## Crear foreign key

```python
def upgrade() -> None:
    op.create_foreign_key(
        "fk_products_category_id_categories",
        source_table="products",
        referent_table="categories",
        local_cols=["category_id"],
        remote_cols=["id"]
    )


def downgrade() -> None:
    op.drop_constraint(
        "fk_products_category_id_categories",
        "products",
        type_="foreignkey"
    )
```

## Crear datos iniciales

Las migraciones también pueden insertar datos estructurales.

Ejemplo:

```python
def upgrade() -> None:
    op.execute("""
        INSERT INTO categories (name)
        VALUES ('Tecnología'), ('Oficina')
    """)


def downgrade() -> None:
    op.execute("""
        DELETE FROM categories
        WHERE name IN ('Tecnología', 'Oficina')
    """)
```

Este patrón debe usarse con cuidado.

Las migraciones de datos deben ser claras, reversibles cuando sea posible y seguras para producción.

## `op.execute()`

`op.execute()` permite ejecutar SQL directo dentro de una migración.

```python
op.execute("UPDATE products SET stock = 0 WHERE stock IS NULL")
```

Es útil para cambios puntuales que no se expresan cómodamente con operaciones estructurales.

## Migraciones de datos

Una migración de datos modifica registros existentes.

Ejemplo:

```python
def upgrade() -> None:
    op.execute("""
        UPDATE products
        SET is_active = 1
        WHERE is_active IS NULL
    """)


def downgrade() -> None:
    pass
```

No todas las migraciones de datos son reversibles.

Si no se puede revertir con seguridad, debe quedar explícito.

## Offline mode

Alembic puede generar scripts SQL sin aplicarlos directamente.

```bash
alembic upgrade head --sql
```

Esto imprime SQL en lugar de ejecutar cambios contra la base.

También puede guardarse:

```bash
alembic upgrade head --sql > migration.sql
```

Este modo es útil cuando un DBA debe revisar o ejecutar los cambios manualmente.

## `stamp`

`stamp` permite marcar la base de datos como si estuviera en una revisión, sin ejecutar migraciones.

```bash
alembic stamp head
```

Uso típico:

```text
la base ya tiene el esquema correcto
se desea empezar a controlar versiones desde Alembic
no se quiere ejecutar una migración inicial
```

Debe usarse con mucho cuidado.

## SQLite y batch mode

SQLite tiene limitaciones para ciertas operaciones de alteración de tablas.

Alembic ofrece batch operations para algunos cambios.

Ejemplo:

```python
def upgrade() -> None:
    with op.batch_alter_table("products") as batch_op:
        batch_op.add_column(
            sa.Column("stock", sa.Integer(), nullable=False, server_default="0")
        )


def downgrade() -> None:
    with op.batch_alter_table("products") as batch_op:
        batch_op.drop_column("stock")
```

Este patrón puede ser útil cuando se trabaja con SQLite.

## Naming convention

Es recomendable definir convenciones de nombres para constraints e índices en SQLAlchemy.

Ejemplo:

```python
from sqlalchemy import MetaData
from sqlalchemy.orm import DeclarativeBase


convention = {
    "ix": "ix_%(column_0_label)s",
    "uq": "uq_%(table_name)s_%(column_0_name)s",
    "ck": "ck_%(table_name)s_%(constraint_name)s",
    "fk": "fk_%(table_name)s_%(column_0_name)s_%(referred_table_name)s",
    "pk": "pk_%(table_name)s",
}


class Base(DeclarativeBase):
    metadata = MetaData(naming_convention=convention)
```

Esto ayuda a que Alembic pueda generar y revertir constraints de forma más consistente.

## Ramas de migración

En equipos, pueden aparecer dos migraciones creadas desde la misma revisión anterior.

Ejemplo conceptual:

```text
A -> B
A -> C
```

Esto genera múltiples heads.

Para revisar:

```bash
alembic heads
```

Para unir ramas:

```bash
alembic merge -m "merge heads" head1 head2
```

El resultado es una nueva migración que une ambas ramas.

## Estructura recomendada con SQLAlchemy

```text
app/
├─ database.py
├─ models.py
├─ main.py
├─ repositories.py
└─ services.py
alembic.ini
migrations/
├─ env.py
└─ versions/
```

## `app/database.py`

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import Session

engine = create_engine("sqlite:///app.db")


def get_session():
    with Session(engine) as session:
        yield session
```

## `app/models.py`

```python
from sqlalchemy.orm import DeclarativeBase
from sqlalchemy.orm import Mapped
from sqlalchemy.orm import mapped_column


class Base(DeclarativeBase):
    pass


class Product(Base):
    __tablename__ = "products"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str]
    price: Mapped[float]
    stock: Mapped[int] = mapped_column(default=0)
```

## `migrations/env.py`

Debe importar `Base`:

```python
from app.models import Base

target_metadata = Base.metadata
```

## Diferencia entre `create_all()` y Alembic

## `Base.metadata.create_all(engine)`

Crea tablas que no existen.

Es útil para:

```text
aprendizaje
prototipos
tests simples
scripts temporales
```

No registra cambios históricos.

No altera automáticamente tablas existentes de forma controlada.

## Alembic

Registra cambios como revisiones.

Es útil para:

```text
proyectos reales
equipos de desarrollo
bases persistentes
entornos de producción
control de versiones del esquema
```

## Uso con FastAPI

Alembic puede usarse con FastAPI cuando la aplicación usa SQLAlchemy.

Flujo típico:

```text
FastAPI -> SQLAlchemy models -> Alembic migrations -> database
```

Estructura frecuente:

```text
app/
├─ main.py
├─ database.py
├─ models.py
├─ routers/
└─ schemas/
migrations/
alembic.ini
```

Comandos:

```bash
alembic revision --autogenerate -m "create products table"
alembic upgrade head
```

## Uso con Flask

En Flask puede usarse Alembic directamente con SQLAlchemy.

Si se usa Flask-SQLAlchemy, también existen extensiones como Flask-Migrate, que internamente se apoyan en Alembic.

Flujo conceptual:

```text
Flask -> SQLAlchemy / Flask-SQLAlchemy -> Alembic -> database
```

## Uso con pandas

Alembic no se usa para transformar datos con pandas.

Sin embargo, puede participar en proyectos donde pandas lee o escribe sobre una base cuyo esquema se gestiona con Alembic.

Ejemplo conceptual:

```text
Alembic -> crea tabla
pandas -> carga datos en esa tabla
SQLAlchemy -> consulta y administra conexión
```

## Comandos frecuentes

## Inicializar

```bash
alembic init migrations
```

## Crear migración manual

```bash
alembic revision -m "message"
```

## Crear migración autogenerada

```bash
alembic revision --autogenerate -m "message"
```

## Aplicar hasta última revisión

```bash
alembic upgrade head
```

## Revertir una revisión

```bash
alembic downgrade -1
```

## Ver revisión actual

```bash
alembic current
```

## Ver historial

```bash
alembic history
```

## Ver heads

```bash
alembic heads
```

## Marcar revisión sin ejecutar

```bash
alembic stamp head
```

## Generar SQL sin ejecutar

```bash
alembic upgrade head --sql
```

## Errores comunes

## No configurar `target_metadata`

Problemático:

```python
target_metadata = None
```

cuando se quiere usar:

```bash
alembic revision --autogenerate
```

Correcto:

```python
from app.models import Base

target_metadata = Base.metadata
```

## No importar todos los modelos

Si un modelo no se importa, puede que no aparezca en `Base.metadata`.

Conviene asegurar que todos los modelos estén cargados antes de autogenerar.

## Creer que `--autogenerate` siempre es perfecto

La migración generada debe revisarse manualmente.

Especialmente en casos como:

```text
renombres de columnas
renombres de tablas
cambios complejos de tipos
migraciones de datos
restricciones avanzadas
índices especiales
```

## Crear una migración y no aplicarla

Si se crea una migración:

```bash
alembic revision --autogenerate -m "add column"
```

también debe aplicarse:

```bash
alembic upgrade head
```

## Base de datos no actualizada

Si la base no está en la última revisión, Alembic puede impedir crear nuevas migraciones autogeneradas.

Solución típica:

```bash
alembic upgrade head
```

antes de crear una nueva migración.

## Usar `create_all()` junto con Alembic sin criterio

Si se usa Alembic, no conviene que la aplicación cree o modifique tablas automáticamente en producción mediante `create_all()`.

Alembic debería ser la fuente de control del esquema.

## Guardar credenciales en `alembic.ini`

Problemático:

```ini
sqlalchemy.url = postgresql+psycopg://user:password@host/db
```

Mejor leer desde variable de entorno en `env.py`.

## No escribir `downgrade()`

Un `downgrade()` vacío puede dificultar revertir cambios.

Si no es reversible, conviene dejarlo explícito y documentado.

## No versionar migraciones en Git

Los archivos en `migrations/versions/` deben versionarse.

No son archivos temporales.

## Editar migraciones ya aplicadas en entornos compartidos

Modificar una migración ya aplicada en otros entornos puede generar inconsistencias.

Lo normal es crear una nueva migración correctiva.

## No usar nombres claros

Menos claro:

```bash
alembic revision -m "changes"
```

Más claro:

```bash
alembic revision -m "add stock column to products"
```

## Buenas prácticas

## Usar mensajes descriptivos

```bash
alembic revision -m "create products table"
```

## Revisar migraciones autogeneradas

Antes de aplicar:

```bash
alembic revision --autogenerate -m "add stock column"
```

revisar el archivo generado en:

```text
migrations/versions/
```

## Aplicar migraciones en orden

```bash
alembic upgrade head
```

## Versionar migraciones

Los archivos de migración deben estar en Git.

## No guardar credenciales

Usar variables de entorno.

```python
os.getenv("DATABASE_URL")
```

## Definir naming convention

Especialmente importante para constraints e índices.

## Separar migraciones de esquema y datos cuando convenga

Migraciones complejas pueden ser más claras si se dividen.

## Probar downgrade cuando sea relevante

```bash
alembic downgrade -1
alembic upgrade head
```

## Usar Alembic como fuente de verdad del esquema

En proyectos reales, los cambios de estructura deben pasar por migraciones.

## Ejemplo integrado

Estructura:

```text
project/
├─ app/
│  ├─ __init__.py
│  ├─ database.py
│  └─ models.py
├─ alembic.ini
└─ migrations/
   ├─ env.py
   └─ versions/
```

Archivo:

```text
app/database.py
```

Código:

```python
import os

from sqlalchemy import create_engine
from sqlalchemy.orm import Session


database_url = os.getenv("DATABASE_URL", "sqlite:///app.db")

engine = create_engine(database_url)


def get_session():
    with Session(engine) as session:
        yield session
```

Archivo:

```text
app/models.py
```

Código:

```python
from sqlalchemy import MetaData
from sqlalchemy.orm import DeclarativeBase
from sqlalchemy.orm import Mapped
from sqlalchemy.orm import mapped_column


convention = {
    "ix": "ix_%(column_0_label)s",
    "uq": "uq_%(table_name)s_%(column_0_name)s",
    "ck": "ck_%(table_name)s_%(constraint_name)s",
    "fk": "fk_%(table_name)s_%(column_0_name)s_%(referred_table_name)s",
    "pk": "pk_%(table_name)s",
}


class Base(DeclarativeBase):
    metadata = MetaData(naming_convention=convention)


class Product(Base):
    __tablename__ = "products"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(unique=True, index=True)
    price: Mapped[float]
    stock: Mapped[int] = mapped_column(default=0)
    is_active: Mapped[bool] = mapped_column(default=True)
```

Inicialización:

```bash
alembic init migrations
```

Ajuste en `migrations/env.py`:

```python
import os

from app.models import Base

target_metadata = Base.metadata

database_url = os.getenv("DATABASE_URL")

if database_url:
    config.set_main_option("sqlalchemy.url", database_url)
```

Crear migración:

```bash
alembic revision --autogenerate -m "create products table"
```

Revisar archivo generado.

Aplicar migración:

```bash
alembic upgrade head
```

## Relación con otras librerías

`alembic` se relaciona especialmente con:

```text
sqlalchemy
psycopg
pymysql
pyodbc
python-dotenv
fastapi
flask
pytest
```

## Relación con SQLAlchemy

SQLAlchemy define modelos, tablas, metadatos y conexión.

Alembic gestiona los cambios del esquema a lo largo del tiempo.

## Relación con drivers

Alembic puede trabajar con distintas bases mediante SQLAlchemy y sus drivers:

```text
PostgreSQL -> psycopg
MySQL      -> pymysql
SQL Server -> pyodbc
SQLite     -> sqlite3
```

## Relación con FastAPI y Flask

Alembic suele usarse en backends donde SQLAlchemy controla la capa de datos.

```text
FastAPI / Flask -> SQLAlchemy -> Alembic -> base de datos
```

## Orden didáctico interno

```text
1. Propósito de alembic
2. Relación con SQLAlchemy
3. Instalación
4. Inicialización con alembic init
5. alembic.ini
6. env.py
7. target_metadata
8. Crear migraciones manuales
9. Crear migraciones con autogenerate
10. upgrade() y downgrade()
11. Aplicar migraciones con upgrade head
12. Revertir con downgrade
13. current, history, heads y stamp
14. Operaciones frecuentes con op
15. Migraciones de datos
16. Offline mode
17. SQLite y batch mode
18. Naming convention
19. Ramas y merge
20. Uso con FastAPI, Flask y SQLAlchemy
21. Errores comunes
22. Buenas prácticas
```