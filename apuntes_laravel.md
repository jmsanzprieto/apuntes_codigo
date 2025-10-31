# GUÍA DE COMANDOS FUNDAMENTALES DE LARAVEL

## 1. INSTALACIÓN Y CONFIGURACIÓN INICIAL

### Instalación de Laravel

```bash
# Instalar Laravel vía Composer (método recomendado)
composer create-project laravel/laravel nombre-proyecto

# O usando el instalador de Laravel
composer global require laravel/installer
laravel new nombre-proyecto
```

### Servidor de Desarrollo

```bash
# Iniciar servidor de desarrollo
php artisan serve

# Especificar puerto personalizado
php artisan serve --port=8080

# Hacer accesible desde la red
php artisan serve --host=0.0.0.0
```

### Configuración del Entorno

```bash
# Copiar archivo de configuración
cp .env.example .env

# Generar clave de aplicación
php artisan key:generate

# Limpiar caché de configuración
php artisan config:clear
php artisan config:cache
```

## 2. AUTENTICACIÓN Y GESTIÓN DE USUARIOS

### Laravel Breeze (Starter Kit Ligero)

```bash
# Instalar Breeze
composer require laravel/breeze --dev

# Instalar con stack Blade
php artisan breeze:install blade

# Instalar con stack React
php artisan breeze:install react

# Instalar con stack Vue
php artisan breeze:install vue

# Instalar API (sin frontend)
php artisan breeze:install api

# Ejecutar migraciones y compilar assets
php artisan migrate
npm install && npm run dev
```

### Laravel Jetstream (Starter Kit Completo)

```bash
# Instalar Jetstream
composer require laravel/jetstream

# Instalar con Livewire
php artisan jetstream:install livewire

# Instalar con Inertia + Vue
php artisan jetstream:install inertia

# Con opciones adicionales (equipos, API)
php artisan jetstream:install livewire --teams
php artisan jetstream:install livewire --api

# Ejecutar migraciones y compilar assets
php artisan migrate
npm install && npm run dev
```

### Laravel Sanctum (Autenticación API/SPA)

```bash
# Instalar Sanctum
composer require laravel/sanctum

# Publicar configuración
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"

# Ejecutar migraciones
php artisan migrate
```

### Laravel Passport (OAuth2)

```bash
# Instalar Passport
composer require laravel/passport

# Ejecutar migraciones
php artisan migrate

# Instalar Passport
php artisan passport:install

# Crear cliente personal de tokens
php artisan passport:client --personal

# Crear cliente de credenciales de contraseña
php artisan passport:client --password
```

### Autenticación UI (Legacy - Laravel 8 y anteriores)

```bash
# Instalar Laravel UI
composer require laravel/ui

# Generar scaffolding de autenticación con Bootstrap
php artisan ui bootstrap --auth

# Con Vue
php artisan ui vue --auth

# Con React
php artisan ui react --auth

# Compilar assets
npm install && npm run dev
```

### Comandos de Usuario Personalizados

```bash
# Crear política de autorización
php artisan make:policy UsuarioPolicy

# Crear política asociada a modelo
php artisan make:policy UsuarioPolicy --model=Usuario

# Crear notificación
php artisan make:notification BienvenidaUsuario

# Crear evento de usuario
php artisan make:event UsuarioRegistrado

# Crear listener
php artisan make:listener EnviarEmailBienvenida --event=UsuarioRegistrado
```

## 3. GESTIÓN DE BASE DE DATOS

### Migraciones

```bash
# Crear nueva migración
php artisan make:migration crear_tabla_usuarios
php artisan make:migration add_campo_to_tabla --table=usuarios

# Ejecutar migraciones
php artisan migrate

# Revertir última migración
php artisan migrate:rollback

# Revertir todas las migraciones
php artisan migrate:reset

# Revertir y volver a ejecutar todas las migraciones
php artisan migrate:refresh

# Eliminar todas las tablas y volver a migrar
php artisan migrate:fresh

# Ver estado de las migraciones
php artisan migrate:status
```

### Seeders

```bash
# Crear seeder
php artisan make:seeder UsuariosSeeder

# Ejecutar todos los seeders
php artisan db:seed

# Ejecutar seeder específico
php artisan db:seed --class=UsuariosSeeder

# Migrar y ejecutar seeders
php artisan migrate:fresh --seed
```

## 4. MODELOS, CONTROLADORES Y RECURSOS

### Modelos

```bash
# Crear modelo
php artisan make:model Usuario

# Crear modelo con migración
php artisan make:model Usuario -m

# Crear modelo con migración, factory y seeder
php artisan make:model Usuario -mfs

# Crear modelo con todos los recursos
php artisan make:model Usuario --all
```

### Controladores

```bash
# Crear controlador básico
php artisan make:controller UsuarioController

# Crear controlador de recursos (CRUD completo)
php artisan make:controller UsuarioController --resource

# Crear controlador API
php artisan make:controller UsuarioController --api

# Crear controlador con modelo
php artisan make:controller UsuarioController --model=Usuario
```

### Middleware

```bash
# Crear middleware
php artisan make:middleware CheckEdad
```

### Requests (Validación)

```bash
# Crear request de validación
php artisan make:request StoreUsuarioRequest
```

## 5. RUTAS Y VISTAS

### Listar Rutas

```bash
# Ver todas las rutas
php artisan route:list

# Ver rutas de forma compacta
php artisan route:list --compact

# Filtrar por nombre
php artisan route:list --name=usuarios

# Filtrar por método
php artisan route:list --method=GET
```

### Limpiar Caché de Rutas

```bash
php artisan route:clear
php artisan route:cache
```

### Vistas

```bash
# Limpiar caché de vistas compiladas
php artisan view:clear
php artisan view:cache
```

## 6. GESTIÓN DE DEPENDENCIAS

### Composer

```bash
# Instalar dependencias del proyecto
composer install

# Actualizar dependencias
composer update

# Instalar paquete específico
composer require nombre/paquete

# Instalar paquete de desarrollo
composer require --dev nombre/paquete

# Eliminar paquete
composer remove nombre/paquete

# Actualizar autoload
composer dump-autoload
```

### NPM/Node

```bash
# Instalar dependencias de Node
npm install

# Compilar assets para desarrollo
npm run dev

# Compilar assets para producción
npm run build

# Modo watch (recompilación automática)
npm run watch
```

## 7. ARTISAN - COMANDOS ÚTILES

### Caché

```bash
# Limpiar toda la caché
php artisan cache:clear

# Limpiar caché de aplicación
php artisan optimize:clear

# Optimizar aplicación (cachear config, rutas, vistas)
php artisan optimize
```

### Queue (Colas)

```bash
# Crear job
php artisan make:job ProcesarPedido

# Ejecutar trabajos de la cola
php artisan queue:work

# Procesar un solo trabajo
php artisan queue:work --once

# Ver trabajos fallidos
php artisan queue:failed

# Reintentar trabajo fallido
php artisan queue:retry {id}
```

### Storage

```bash
# Crear enlace simbólico de storage
php artisan storage:link
```

### Mantenimiento

```bash
# Poner aplicación en modo mantenimiento
php artisan down

# Salir del modo mantenimiento
php artisan up

# Modo mantenimiento con secreto de acceso
php artisan down --secret="token-secreto"
```

## 8. TESTING

```bash
# Crear test
php artisan make:test UsuarioTest

# Crear test unitario
php artisan make:test UsuarioTest --unit

# Ejecutar tests
php artisan test

# Ejecutar PHPUnit directamente
./vendor/bin/phpunit
```

## 9. DESPLIEGUE EN PRODUCCIÓN

### Optimización

```bash
# Cachear configuración
php artisan config:cache

# Cachear rutas
php artisan route:cache

# Cachear vistas
php artisan view:cache

# Optimizar autoload de Composer
composer install --optimize-autoloader --no-dev

# Compilar assets
npm run build
```

### Permisos (Linux)

```bash
# Dar permisos a directorios de Laravel
chmod -R 775 storage bootstrap/cache
chown -R www-data:www-data storage bootstrap/cache
```

## 10. COMANDOS PERSONALIZADOS

```bash
# Crear comando Artisan personalizado
php artisan make:command NombreComando

# Ejecutar comando personalizado
php artisan comando:nombre
```

## 11. DEPURACIÓN Y LOG

```bash
# Ver logs en tiempo real
tail -f storage/logs/laravel.log

# Limpiar logs (manualmente)
> storage/logs/laravel.log
```

---

**Documento generado como guía de referencia rápida para Laravel**
