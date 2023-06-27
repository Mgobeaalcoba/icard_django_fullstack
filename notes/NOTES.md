# Proyecto: Carta eléctronica para un restaurante

1. Dependencias: Node, Npm(Viene con node), Yarn(Se instala luego de NPM), Python3, Django, Django Rest Framework

---

## Paso a paso para comenzar el backend de mi proyecto con Django

1. Dependencias: Django + Venv propio
2. Con Django instalado armo la estructura de un proyecto fullstack:

   - Carpeta padre del proyecto:
     - Carpeta hija para el backend con Django
     - Carpeta hija para el frontend con React

3. Me ubico en la carpeta para el frontend y comenzamos a trabajar
4. Comandos en consola:

```bash
mkdir icard_django
cd icard_django
python3 -m venv venv
source venv/bin/activate
pip3 install django
pip freeze > requirements.txt
django-admin startproject icard
touch LICENSE
touch README.md
touch .gitignore
cd icard
python3 manage.py runserver
```

5. Con eso dejamos el servidor encendido escuchando cambios.
   Luego abrimos otra consola y migramos el proyecto para poder tener acceso al Django Admin también.

```bash
python3 manage.py migrate
```

6. Ahora vamos a instalar Django Rest Framework que nos permite
   construir una API-REST con django, sin necesidad de retornar templates. Es decir, solo retornando datos. Y nos va a permitir también construir documentación interactica al estilo FastApi.

```bash
pip3 install djangorestframework
pip3 freeze > requirements.txt
```

7. Una vez instalado el framework vamos a agregarlo a nuestras INSTALLED_APPS en el file settings.py del proyecto "icard"

```py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework'
]
```

8. Luego volvemos a migrar el proyecto y reencendemos el server:

```bash
python3 manage.py migrate
python3 manage.py runserver
```

---

## Configurando la documentación de la API

1. Con django_rest_framework instalado ahora debemos instalar una librería para que nos genere la documentación de forma automatica. El paquete a instalar se llama **"drf-yasg"**

```bash
pip3 install drf-yasg
pip3 freeze > requirements.txt
```

2. En INSTALLED_APPS del file settings.py del proyecto django que creamos tenemos que agregar entonces esta nueva dependencia:

```py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'drf_yasg'
]
```

3. Ahora debemos hacer algunas ediciones en nuestro archivo urls.py del proyecto "icard":

```py
from django.contrib import admin
from django.urls import path

# Dependendencias para documentación interactiva
from drf_yasg.views import get_schema_view
from drf_yasg import openapi

schema_view = get_schema_view(
   openapi.Info(
      title="ICard ApiDoc",
      default_version='v1',
      description="Documentación de la REST-API de Icard",
      terms_of_service="https://www.linkedin.com/in/mariano-gobea-alcoba/",
      contact=openapi.Contact(email="gobeamariano@gmail.com"),
      license=openapi.License(name="BSD License"),
   ),
   public=True,
)

urlpatterns = [
    path('admin/', admin.site.urls),
    path('docs/', schema_view.with_ui('swagger', cache_timeout=0), name="schema-swagger-ui"),
    path('redoc/', schema_view.with_ui('redoc', cache_timeout=0), name='schema-redoc-ui')
]
```

¿Que hicimos en ese archivo? Agregamos dos import mas, ambos de drf_yasg, luego creamos la variable schema_view donde configuramos los atributos de nuestra documentación interactiva. Y finalmente agregamos dos URL´s para poder visualizar la documentación interactiva en entorno swagger "docs/" y en entorno redoc "redoc/"

4. Repetimos el paso de migrar y recargar el server por si acaso:

```bash
python3 manage.py makemigrations
python3 manage.py migrate
python3 manage.py runserver
```

Felicidades!!! Ya tenemos el proyecto base en Django construido para que podamos comenzar a iterarlo.

---
