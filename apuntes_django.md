# GUÍA FUNDAMENTAL DE DJANGO

## 1. INSTALACIÓN Y CONFIGURACIÓN INICIAL

### Instalación de Django

```bash
# Crear entorno virtual (recomendado)
python -m venv venv

# Activar entorno virtual
# En Windows:
venv\Scripts\activate
# En Linux/Mac:
source venv/bin/activate

# Instalar Django
pip install django

# Instalar versión específica
pip install django==4.2

# Verificar instalación
python -m django --version
```

### Crear Proyecto

```bash
# Crear nuevo proyecto Django
django-admin startproject nombre_proyecto

# Crear proyecto en directorio actual
django-admin startproject nombre_proyecto .

# Estructura básica generada:
# nombre_proyecto/
# ├── manage.py
# └── nombre_proyecto/
#     ├── __init__.py
#     ├── settings.py
#     ├── urls.py
#     ├── asgi.py
#     └── wsgi.py
```

### Servidor de Desarrollo

```bash
# Iniciar servidor de desarrollo
python manage.py runserver

# Especificar puerto personalizado
python manage.py runserver 8080

# Especificar host y puerto
python manage.py runserver 0.0.0.0:8000

# Ejecutar sin recarga automática
python manage.py runserver --noreload
```

## 2. APLICACIONES (APPS)

### Crear y Gestionar Apps

```bash
# Crear nueva aplicación
python manage.py startapp nombre_app

# Estructura de una app:
# nombre_app/
# ├── __init__.py
# ├── admin.py          # Configuración del panel admin
# ├── apps.py           # Configuración de la app
# ├── models.py         # Modelos de base de datos
# ├── tests.py          # Tests
# ├── views.py          # Vistas/Controladores
# └── migrations/       # Migraciones de BD
#     └── __init__.py

# Registrar app en settings.py:
INSTALLED_APPS = [
    # ...
    'nombre_app',
]
```

## 3. BASE DE DATOS Y MODELOS

### Configuración de Base de Datos

```python
# En settings.py

# SQLite (por defecto)
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

# PostgreSQL
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'nombre_bd',
        'USER': 'usuario',
        'PASSWORD': 'contraseña',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}

# MySQL
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'nombre_bd',
        'USER': 'usuario',
        'PASSWORD': 'contraseña',
        'HOST': 'localhost',
        'PORT': '3306',
    }
}
```

### Migraciones

```bash
# Crear migraciones basadas en cambios en models.py
python manage.py makemigrations

# Crear migración para app específica
python manage.py makemigrations nombre_app

# Ver SQL de una migración
python manage.py sqlmigrate nombre_app 0001

# Aplicar migraciones
python manage.py migrate

# Aplicar migraciones de app específica
python manage.py migrate nombre_app

# Revertir a migración específica
python manage.py migrate nombre_app 0001

# Ver estado de migraciones
python manage.py showmigrations

# Crear migración vacía (para datos o SQL personalizado)
python manage.py makemigrations --empty nombre_app

# Verificar problemas en modelos
python manage.py check
```

### Comandos de Base de Datos

```bash
# Abrir shell de base de datos
python manage.py dbshell

# Volcar datos a JSON
python manage.py dumpdata > datos.json

# Volcar datos de app específica
python manage.py dumpdata nombre_app > datos.json

# Volcar datos con formato legible
python manage.py dumpdata --indent 4 > datos.json

# Cargar datos desde archivo
python manage.py loaddata datos.json

# Limpiar y recrear base de datos (SQLite)
python manage.py flush
```

## 4. SHELL INTERACTIVO

```bash
# Abrir shell de Python con Django cargado
python manage.py shell

# Usar shell mejorado (IPython)
pip install ipython
python manage.py shell

# Usar shell con Django Extensions
pip install django-extensions
python manage.py shell_plus

# Ejecutar script Python con contexto Django
python manage.py shell < script.py
```

## 5. USUARIOS Y AUTENTICACIÓN

### Crear Superusuario

```bash
# Crear superusuario para admin
python manage.py createsuperuser

# Cambiar contraseña de usuario
python manage.py changepassword nombre_usuario
```

### Gestión de Usuarios

```bash
# No hay comandos específicos, pero se puede usar shell:
python manage.py shell

# Crear usuario desde shell:
from django.contrib.auth.models import User
user = User.objects.create_user('username', 'email@example.com', 'password')

# Crear superusuario desde shell:
user = User.objects.create_superuser('admin', 'admin@example.com', 'password')
```

## 6. ARCHIVOS ESTÁTICOS Y MEDIA

### Configuración de Archivos Estáticos

```python
# En settings.py

# URL para servir archivos estáticos
STATIC_URL = '/static/'

# Directorio donde se recopilarán los archivos estáticos
STATIC_ROOT = BASE_DIR / 'staticfiles'

# Directorios adicionales de archivos estáticos
STATICFILES_DIRS = [
    BASE_DIR / 'static',
]

# Archivos de media (uploads de usuarios)
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'
```

### Comandos de Archivos Estáticos

```bash
# Recopilar todos los archivos estáticos en STATIC_ROOT
python manage.py collectstatic

# Recopilar sin confirmación
python manage.py collectstatic --noinput

# Limpiar archivos estáticos antiguos
python manage.py collectstatic --clear

# Buscar archivos estáticos
python manage.py findstatic nombre_archivo.css
```

## 7. TESTING

```bash
# Ejecutar todos los tests
python manage.py test

# Ejecutar tests de app específica
python manage.py test nombre_app

# Ejecutar test específico
python manage.py test nombre_app.tests.MiTestCase

# Ejecutar con verbosidad
python manage.py test --verbosity=2

# Mantener base de datos de test
python manage.py test --keepdb

# Ejecutar tests en paralelo
python manage.py test --parallel

# Ejecutar con cobertura (requiere coverage.py)
pip install coverage
coverage run --source='.' manage.py test
coverage report
coverage html
```

## 8. ADMINISTRACIÓN

### Panel de Administración

```bash
# El panel admin está en: http://localhost:8000/admin/

# Configurar en admin.py de cada app:
from django.contrib import admin
from .models import MiModelo

admin.site.register(MiModelo)

# O con personalización:
@admin.register(MiModelo)
class MiModeloAdmin(admin.ModelAdmin):
    list_display = ['campo1', 'campo2']
    search_fields = ['campo1']
    list_filter = ['fecha']
```

## 9. PLANTILLAS (TEMPLATES)

### Configuración de Plantillas

```python
# En settings.py
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

## 10. MENSAJES Y CACHÉ

### Comandos de Caché

```bash
# Limpiar toda la caché
python manage.py clear_cache  # Requiere django-extensions

# Crear tablas de caché (si se usa cache de BD)
python manage.py createcachetable
```

### Configuración de Caché

```python
# En settings.py

# Caché en memoria (desarrollo)
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.locmem.LocMemCache',
    }
}

# Caché en base de datos
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.db.DatabaseCache',
        'LOCATION': 'cache_table',
    }
}

# Redis (requiere django-redis)
CACHES = {
    'default': {
        'BACKEND': 'django_redis.cache.RedisCache',
        'LOCATION': 'redis://127.0.0.1:6379/1',
    }
}
```

## 11. INTERNACIONALIZACIÓN (i18n)

```bash
# Crear archivos de traducción
python manage.py makemessages -l es
python manage.py makemessages -l en

# Compilar traducciones
python manage.py compilemessages

# Crear traducciones para JavaScript
python manage.py makemessages -d djangojs -l es
```

### Configuración i18n

```python
# En settings.py
LANGUAGE_CODE = 'es'
TIME_ZONE = 'Europe/Madrid'
USE_I18N = True
USE_TZ = True

LANGUAGES = [
    ('es', 'Español'),
    ('en', 'English'),
]

LOCALE_PATHS = [
    BASE_DIR / 'locale',
]
```

## 12. COMANDOS PERSONALIZADOS

### Crear Comando Personalizado

```bash
# Estructura:
# nombre_app/
# └── management/
#     └── commands/
#         └── mi_comando.py

# Ejecutar comando personalizado
python manage.py mi_comando

# Comando con argumentos
python manage.py mi_comando --argumento valor
```

### Ejemplo de Comando Personalizado

```python
# nombre_app/management/commands/mi_comando.py
from django.core.management.base import BaseCommand

class Command(BaseCommand):
    help = 'Descripción del comando'

    def add_arguments(self, parser):
        parser.add_argument('--argumento', type=str)

    def handle(self, *args, **options):
        self.stdout.write('Ejecutando comando...')
```

## 13. DJANGO REST FRAMEWORK (DRF)

```bash
# Instalar Django REST Framework
pip install djangorestframework

# Agregar a INSTALLED_APPS:
INSTALLED_APPS = [
    # ...
    'rest_framework',
]

# Comandos específicos de DRF
# (DRF no tiene comandos propios, se usan las vistas y serializadores)
```

## 14. SEGURIDAD

```bash
# Generar nueva SECRET_KEY
python -c 'from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())'

# Verificar configuración de seguridad
python manage.py check --deploy

# Verificar problemas de seguridad
python manage.py check --tag security
```

### Configuración de Seguridad

```python
# En settings.py (producción)

DEBUG = False
ALLOWED_HOSTS = ['midominio.com', 'www.midominio.com']

# Seguridad HTTPS
SECURE_SSL_REDIRECT = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
SECURE_BROWSER_XSS_FILTER = True
SECURE_CONTENT_TYPE_NOSNIFF = True
X_FRAME_OPTIONS = 'DENY'

# HSTS
SECURE_HSTS_SECONDS = 31536000
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
SECURE_HSTS_PRELOAD = True
```

## 15. DESPLIEGUE Y PRODUCCIÓN

### Configuración para Producción

```bash
# Instalar gunicorn (servidor WSGI)
pip install gunicorn

# Ejecutar con gunicorn
gunicorn nombre_proyecto.wsgi:application

# Especificar workers y bind
gunicorn nombre_proyecto.wsgi:application --workers 4 --bind 0.0.0.0:8000

# Instalar whitenoise (servir archivos estáticos)
pip install whitenoise

# Agregar a MIDDLEWARE en settings.py:
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',  # Después de SecurityMiddleware
    # ...
]
```

### Requerimientos

```bash
# Generar archivo de requerimientos
pip freeze > requirements.txt

# Instalar desde requirements
pip install -r requirements.txt
```

## 16. SEÑALES (SIGNALS)

```python
# Las señales no tienen comandos, se configuran en código
# Ubicación común: nombre_app/signals.py

from django.db.models.signals import post_save
from django.dispatch import receiver
from .models import MiModelo

@receiver(post_save, sender=MiModelo)
def mi_handler(sender, instance, created, **kwargs):
    if created:
        # Hacer algo cuando se crea un objeto
        pass
```

## 17. MIDDLEWARE PERSONALIZADO

```python
# Crear en nombre_app/middleware.py
class MiMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        # Código antes de la vista
        response = self.get_response(request)
        # Código después de la vista
        return response

# Agregar a MIDDLEWARE en settings.py:
MIDDLEWARE = [
    # ...
    'nombre_app.middleware.MiMiddleware',
]
```

## 18. CELERY (TAREAS ASÍNCRONAS)

```bash
# Instalar Celery
pip install celery redis

# Iniciar worker de Celery
celery -A nombre_proyecto worker -l info

# Iniciar beat (tareas periódicas)
celery -A nombre_proyecto beat -l info

# Iniciar worker y beat juntos
celery -A nombre_proyecto worker --beat -l info

# Ver tareas activas
celery -A nombre_proyecto inspect active

# Purgar todas las tareas
celery -A nombre_proyecto purge
```

## 19. DJANGO EXTENSIONS

```bash
# Instalar django-extensions
pip install django-extensions

# Agregar a INSTALLED_APPS
INSTALLED_APPS = [
    # ...
    'django_extensions',
]

# Comandos adicionales disponibles:

# Shell mejorado con autoload de modelos
python manage.py shell_plus

# Generar diagrama de modelos (requiere pygraphviz o pydot)
python manage.py graph_models -a -o modelo.png

# Ver URLs del proyecto
python manage.py show_urls

# Limpiar archivos pyc
python manage.py clean_pyc

# Resetear migraciones
python manage.py reset_db

# Crear superusuario sin interacción
python manage.py create_superuser --username admin --email admin@example.com --noinput
```

## 20. UTILIDADES Y COMANDOS DIVERSOS

```bash
# Inspeccionar base de datos existente y generar modelos
python manage.py inspectdb > models.py

# Iniciar servidor HTTPS (desarrollo)
python manage.py runserver_plus --cert-file cert.pem

# Mostrar configuración actual
python manage.py diffsettings

# Enviar email de prueba
python manage.py sendtestemail usuario@example.com

# Limpiar sesiones expiradas
python manage.py clearsessions

# Crear archivo de mensajes para revisión
python manage.py makemessages --all
```

## 21. GESTIÓN DE DEPENDENCIAS

```bash
# Usando pip
pip list                          # Listar paquetes instalados
pip show nombre_paquete          # Ver info de paquete
pip install --upgrade nombre_paquete  # Actualizar paquete
pip uninstall nombre_paquete     # Desinstalar paquete

# Usando pip-tools (recomendado para producción)
pip install pip-tools
pip-compile requirements.in      # Generar requirements.txt
pip-sync requirements.txt        # Sincronizar entorno
```

## 22. LOGGING Y DEBUG

```python
# Configuración de logging en settings.py
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'file': {
            'level': 'DEBUG',
            'class': 'logging.FileHandler',
            'filename': BASE_DIR / 'debug.log',
        },
    },
    'loggers': {
        'django': {
            'handlers': ['file'],
            'level': 'DEBUG',
            'propagate': True,
        },
    },
}
```

```bash
# Ver logs en tiempo real
tail -f debug.log

# Instalar django-debug-toolbar
pip install django-debug-toolbar

# Configurar en INSTALLED_APPS y MIDDLEWARE
```

---

## COMANDOS MÁS USADOS (RESUMEN RÁPIDO)

```bash
# Proyecto y servidor
django-admin startproject nombre
python manage.py runserver
python manage.py startapp nombre_app

# Base de datos
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser

# Archivos estáticos
python manage.py collectstatic

# Shell y tests
python manage.py shell
python manage.py test

# Utilidades
python manage.py check
python manage.py showmigrations
python manage.py dumpdata > datos.json
python manage.py loaddata datos.json
```

---

**Documento generado como guía de referencia rápida para Django**
