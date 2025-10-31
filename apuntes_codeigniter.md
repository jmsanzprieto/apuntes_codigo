# GUÍA FUNDAMENTAL DE CODEIGNITER 4

## 1. INSTALACIÓN Y CONFIGURACIÓN INICIAL

### Instalación de CodeIgniter

```bash
# Instalar CodeIgniter vía Composer (método recomendado)
composer create-project codeigniter4/appstarter nombre-proyecto

# O clonar el repositorio
git clone https://github.com/codeigniter4/appstarter.git nombre-proyecto
cd nombre-proyecto
composer install
```

### Servidor de Desarrollo

```bash
# Iniciar servidor de desarrollo
php spark serve

# Especificar puerto personalizado
php spark serve --port=8080

# Especificar host
php spark serve --host=localhost --port=8080
```

### Configuración del Entorno

```bash
# Copiar archivo de configuración de entorno
cp env .env

# Editar .env y configurar:
# - CI_ENVIRONMENT (development, production, testing)
# - Base URL de la aplicación
# - Configuración de base de datos
# - Claves de encriptación
```

### Generar Clave de Encriptación

```bash
# Generar clave de encriptación
php spark key:generate
```

## 2. ESTRUCTURA Y CONFIGURACIÓN

### Directorios Principales

```
app/
├── Config/         # Archivos de configuración
├── Controllers/    # Controladores
├── Models/         # Modelos
├── Views/          # Vistas
├── Database/       # Migraciones y seeds
├── Filters/        # Filtros (middleware)
├── Helpers/        # Helpers personalizados
├── Libraries/      # Librerías personalizadas
└── Language/       # Archivos de idioma

public/             # Punto de entrada (index.php)
writable/           # Logs, caché, uploads
```

### Modos de Entorno

```bash
# En .env configurar:
CI_ENVIRONMENT = development  # Muestra errores detallados
CI_ENVIRONMENT = production   # Oculta errores, modo optimizado
CI_ENVIRONMENT = testing      # Para pruebas
```

## 3. SPARK CLI - COMANDOS PRINCIPALES

### Información del Sistema

```bash
# Ver versión de CodeIgniter
php spark --version

# Listar todos los comandos disponibles
php spark list

# Ver ayuda de un comando específico
php spark help nombre_comando
```

### Controladores

```bash
# Crear controlador
php spark make:controller Usuarios

# Crear controlador con opciones
php spark make:controller Admin/Usuarios

# Crear controlador RESTful
php spark make:controller Api/Usuarios --restful

# Crear controlador con sufijo
php spark make:controller Usuarios --suffix
```

### Modelos

```bash
# Crear modelo
php spark make:model UsuarioModel

# Crear modelo con entidad
php spark make:model UsuarioModel --entity

# Crear modelo en subdirectorio
php spark make:model Admin/UsuarioModel
```

### Entidades

```bash
# Crear entidad
php spark make:entity Usuario

# Crear entidad en subdirectorio
php spark make:entity Admin/Usuario
```

### Filtros (Middleware)

```bash
# Crear filtro
php spark make:filter AuthFilter

# Crear filtro en subdirectorio
php spark make:filter Admin/AuthFilter
```

### Comandos Personalizados

```bash
# Crear comando personalizado
php spark make:command NombreComando

# Ejecutar comando personalizado
php spark nombre:comando
```

## 4. BASE DE DATOS

### Migraciones

```bash
# Crear migración
php spark make:migration NombreMigracion

# Crear migración para tabla específica
php spark make:migration crear_tabla_usuarios

# Ejecutar migraciones pendientes
php spark migrate

# Revertir última migración
php spark migrate:rollback

# Revertir todas las migraciones
php spark migrate:rollback --all

# Refrescar (revertir y ejecutar todas)
php spark migrate:refresh

# Ver estado de migraciones
php spark migrate:status

# Migrar a versión específica
php spark migrate --version=20231015120000
```

### Seeders

```bash
# Crear seeder
php spark make:seeder UsuariosSeeder

# Ejecutar todos los seeders
php spark db:seed

# Ejecutar seeder específico
php spark db:seed UsuariosSeeder

# Ejecutar seeder de grupo específico
php spark db:seed --group=usuarios
```

### Comandos de Base de Datos

```bash
# Crear base de datos
php spark db:create nombre_base_datos

# Ver información de tabla
php spark db:table nombre_tabla

# Ejecutar query directa
php spark db:query "SELECT * FROM usuarios"
```

## 5. RUTAS

### Archivo de Rutas
**Ubicación:** `app/Config/Routes.php`

```php
<?php
// Ruta básica
$routes->get('/', 'Home::index');

// Rutas con parámetros
$routes->get('usuarios/(:num)', 'Usuarios::ver/$1');
$routes->get('perfil/(:any)', 'Usuarios::perfil/$1');

// Rutas con diferentes métodos HTTP
$routes->get('usuarios', 'Usuarios::index');
$routes->post('usuarios', 'Usuarios::crear');
$routes->put('usuarios/(:num)', 'Usuarios::actualizar/$1');
$routes->delete('usuarios/(:num)', 'Usuarios::eliminar/$1');

// Grupos de rutas
$routes->group('admin', ['filter' => 'auth'], function($routes) {
    $routes->get('dashboard', 'Admin\Dashboard::index');
    $routes->get('usuarios', 'Admin\Usuarios::index');
});

// Rutas de recursos (CRUD automático)
$routes->resource('productos');
$routes->presenter('articulos');

// Ruta CLI (solo accesible desde consola)
$routes->cli('tarea/ejecutar', 'Tareas::ejecutar');
```

### Listar Rutas

```bash
# Ver todas las rutas definidas
php spark routes

# Ver rutas ordenadas por método
php spark routes --method

# Buscar ruta específica
php spark routes usuarios
```

## 6. CACHÉ

### Comandos de Caché

```bash
# Limpiar toda la caché
php spark cache:clear

# Limpiar caché específica por clave
php spark cache:clear mi_clave

# Ver información de caché
php spark cache:info
```

## 7. SESIONES

```bash
# La gestión de sesiones se configura en:
# app/Config/App.php

# Tipos de almacenamiento de sesión:
# - FileHandler (archivos en writable/session)
# - DatabaseHandler (base de datos)
# - MemcachedHandler
# - RedisHandler
```

## 8. TESTING

```bash
# Crear test
php spark make:test UsuarioTest

# Ejecutar todos los tests
php spark test

# Ejecutar test específico
php spark test App\\Tests\\UsuarioTest

# Ejecutar tests con cobertura
php spark test --coverage
```

## 9. LIBRERÍAS Y HELPERS

### Crear Componentes Personalizados

```bash
# Crear helper
php spark make:helper MiHelper

# Crear librería (se crea manualmente en app/Libraries/)
# No hay comando spark para librerías

# Crear configuración
php spark make:config MiConfig

# Crear validación personalizada
php spark make:validation MiValidacion
```

### Helpers Comunes de CodeIgniter

```php
<?php
// Cargar helpers en controlador o modelo
helper(['url', 'form', 'text', 'date']);

// Algunos helpers útiles:
// - url_helper: base_url(), site_url(), current_url()
// - form_helper: form_open(), form_close(), form_input()
// - text_helper: word_limiter(), character_limiter()
// - date_helper: now(), timezone_select()
// - filesystem_helper: directory_map(), get_filenames()
```

## 10. NAMESPACES Y AUTOLOAD

### Configuración de Autoload
**Ubicación:** `app/Config/Autoload.php`

```php
<?php
public $psr4 = [
    APP_NAMESPACE => APPPATH,
    'MiNamespace' => APPPATH . 'MiDirectorio',
];

// Helpers que se cargan automáticamente
public $helpers = ['url', 'form'];
```

## 11. PUBLICACIÓN DE ASSETS

```bash
# Publicar assets de un paquete
php spark publish

# Listar items publicables
php spark publish --list
```

## 12. DEPURACIÓN Y LOGS

### Debug Toolbar

```php
<?php
// Activar en .env
CI_DEBUG = true

// O en app/Config/Boot/development.php
```

### Comandos de Log

```bash
# Los logs se guardan en: writable/logs/

# Ver logs en tiempo real (Linux/Mac)
tail -f writable/logs/log-*.log

# Limpiar logs antiguos manualmente
# No hay comando específico, se debe hacer manual
```

### Niveles de Log

```php
<?php
// En código PHP:
log_message('emergency', 'Mensaje de emergencia');
log_message('alert', 'Mensaje de alerta');
log_message('critical', 'Mensaje crítico');
log_message('error', 'Mensaje de error');
log_message('warning', 'Mensaje de advertencia');
log_message('notice', 'Mensaje de notificación');
log_message('info', 'Mensaje informativo');
log_message('debug', 'Mensaje de debug');
```

## 13. OPTIMIZACIÓN Y PRODUCCIÓN

### Configuración para Producción

```bash
# En .env cambiar a:
CI_ENVIRONMENT = production

# Desactivar debug
CI_DEBUG = false

# Configurar caché
# En app/Config/Cache.php
```

### Optimización

```bash
# No hay comandos específicos de optimización en CI4
# Las optimizaciones se hacen mediante:

# 1. Configuración de caché en Config/Cache.php
# 2. Uso de caché de base de datos
# 3. Caché de vistas
# 4. Compresión de salida en Config/App.php
```

### Permisos (Linux)

```bash
# Dar permisos a directorios de escritura
chmod -R 775 writable
chown -R www-data:www-data writable

# Proteger directorios sensibles
chmod 644 .env
```

## 14. SEGURIDAD

### CSRF Protection

```php
<?php
// Configurar en app/Config/Security.php
public $csrfProtection = 'session'; // o 'cookie'

// En formularios automáticamente con form_open()
<?= form_open('usuarios/crear') ?>
```

### Encriptación

```bash
# Generar clave de encriptación
php spark key:generate

# La clave se guarda en .env como:
# encryption.key = hex2bin:tu_clave_aqui
```

### Filtros de Seguridad

```php
<?php
// Configurar en app/Config/Filters.php
public $globals = [
    'before' => [
        'csrf',
        'throttle',
    ],
];
```

## 15. INTERNACIONALIZACIÓN

```bash
# Crear archivos de idioma en:
# app/Language/es/MiArchivo.php
# app/Language/en/MiArchivo.php

# Configurar idioma por defecto en app/Config/App.php
public $defaultLocale = 'es';
```

## 16. EMAIL

### Configuración de Email
**Ubicación:** `app/Config/Email.php`

```php
<?php
// Tipos de protocolo soportados:
// - mail (PHP mail)
// - sendmail
// - smtp

// Configuración SMTP típica
public $SMTPHost = 'smtp.example.com';
public $SMTPUser = 'usuario@example.com';
public $SMTPPass = 'password';
public $SMTPPort = 587;
public $SMTPCrypto = 'tls';
```

## 17. TAREAS PROGRAMADAS (CRON)

```bash
# CodeIgniter 4 no tiene scheduler integrado
# Se deben crear comandos spark y programarlos con cron

# Ejemplo de cron:
# */5 * * * * cd /ruta/proyecto && php spark tarea:ejecutar
```

## 18. API Y REST

```bash
# Crear controlador API
php spark make:controller Api/Usuarios --restful

# Usar ResponseTrait en el controlador
use CodeIgniter\API\ResponseTrait;
```

### Configuración de CORS

```php
<?php
// Crear filtro CORS personalizado
// app/Filters/Cors.php
```

## 19. COMANDOS ÚTILES ADICIONALES

```bash
# Generar archivo .htaccess
php spark publish codeigniter4/framework

# Limpiar archivos de sesión antiguos
php spark sessions:gc

# Verificar configuración del entorno
php spark env

# Mostrar rutas en formato JSON
php spark routes --json
```

## 20. COMPOSER - GESTIÓN DE DEPENDENCIAS

```bash
# Instalar dependencias
composer install

# Actualizar dependencias
composer update

# Agregar paquete
composer require vendor/package

# Remover paquete
composer remove vendor/package

# Actualizar autoload
composer dump-autoload
```

---

## DIFERENCIAS CLAVE CON LARAVEL

- CodeIgniter usa `php spark` en lugar de `php artisan`
- Estructura más simple y flexible
- No tiene ORM tipo Eloquent (usa Query Builder)
- Configuración mediante archivos PHP en lugar de .env completo
- Más ligero y con menor curva de aprendizaje
- Sin sistema de autenticación integrado (Breeze/Jetstream)
- Mayor control manual sobre componentes

---

**Documento generado como guía de referencia rápida para CodeIgniter 4**
