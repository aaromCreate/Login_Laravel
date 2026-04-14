<h1 align="center"> Laravel Login Lab</h1>

<p align="center">
  <a href="https://laravel.com/docs/13.x"> <img src="https://img.shields.io/badge/Laravel-13.x-FF2D20?style=for-the-badge&logo=laravel&logoColor=white" /> </a>
  <a href="https://www.php.net/">  <img src="https://img.shields.io/badge/PHP-8.0+-777BB4?style=for-the-badge&logo=php&logoColor=white" /> </a>
  <a href="https://www.mysql.com/">  <img src="https://img.shields.io/badge/MySQL-Database-4479A1?style=for-the-badge&logo=mysql&logoColor=white" /> </a>
  <a href="https://getcomposer.org/">  <img src="https://img.shields.io/badge/Composer-Dependency_Manager-885630?style=for-the-badge&logo=composer&logoColor=white" /> </a>
  <a href="https://getbootstrap.com/">  <img src="https://img.shields.io/badge/Bootstrap-5-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white" /> </a>
  <a href="https://nodejs.org/en">  <img src="https://img.shields.io/badge/Node.js-NPM-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" /> </a>
  <a href="https://code.visualstudio.com/">  <img src="https://img.shields.io/badge/VS_Code-Editor-007ACC?style=for-the-badge&logo=visualstudiocode&logoColor=white" /> </a>
  <a>  <img src="https://img.shields.io/badge/Windows_10%2F11-OS-0078D6?style=for-the-badge&logo=windows&logoColor=white" /> </a>
  <a>  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" /> </a>
</p>

<p align="center">
  Sistema de autenticación completo construido con Laravel bajo el patrón <strong>MVC</strong> — login, registro, recuperación de contraseña y verificación de email, utilizando <strong>Laravel/UI</strong>.
</p>

---

## Objetivo del Laboratorio

Implementar un módulo de autenticación (login y registro) en Laravel, explorando la estructura del framework bajo el patrón **Modelo–Vista–Controlador (MVC)**, y documentar todo el proceso de configuración, comandos utilizados y resultados obtenidos.

---

## Arquitectura MVC en Laravel

Laravel organiza el código bajo el patrón **Modelo–Vista–Controlador**, separando responsabilidades de la siguiente manera:

| Capa | Carpeta | Función |
|---|---|---|
| **Modelo** | `app/Models/` | Es la capa que gestiona los datos y la lógica de negocio. No sabe nada de cómo se ve la aplicación; su único trabajo es interactuar con la base de datos y aplicar reglas. |
| **Vista** | `resources/views/` | Es la interfaz de usuario. Es lo que el cliente ve y con lo que interactúa (botones, formularios, textos). |
| **Controlador** | `app/Http/Controllers/` | Es el intermediario o el cerebro de la operación. Escucha las peticiones del usuario y decide qué hacer. |

---
## Requisitos Previos

| Herramienta | Versión / Condición |
|---|---|
| **PHP** | 8.0 o superior |
| **Composer** | Última versión estable |
| **Laravel Installer** | Instalado vía `composer global require laravel/installer` |
| **Servidor local** | WAMP / XAMPP / LAMP / Laragon |
| **Servidor web** | Apache o Nginx activo |
| **Base de datos** | MySQL / MariaDB corriendo |
| **NPM** | Requerido para compilar assets con Vite |
| **Editor** | Visual Studio Code (recomendado) |
| **Sistema Operativo** | Windows 10/11, macOS o Linux |

---

## Dependencias de Laravel para Autenticación

| Paquete | Descripción |
|---|---|
| `laravel/ui` | Scaffolding oficial de auth con Bootstrap. Genera vistas de login, registro y recuperación de contraseña. |
| `bootstrap` | Framework CSS usado por Laravel/UI para estilizar las vistas generadas. |
| `axios` | Cliente HTTP incluido por defecto para peticiones AJAX desde el frontend. |
| `vite` | Bundler moderno que reemplaza Laravel Mix desde Laravel 9+. Compila JS y CSS. |
| `sass` | Preprocesador CSS; Laravel/UI genera archivos `.scss` que Vite compila. |

---

## Flujo de Comandos del Laboratorio

### Paso 1 — Crear el proyecto

```bash
# Instalar el Laravel Installer globalmente (si no está instalado)
composer global require laravel/installer

# Crear el nuevo proyecto
laravel new Login_Laravel
cd Login_Laravel
```

### Paso 2 — Configurar el archivo `.env`

```bash
# Generar la APP_KEY (clave criptográfica de 32 caracteres en base64)
# Sin esto Laravel lanza: "The application encryption key has not been specified"
php artisan key:generate
```

Editar `.env` con las credenciales de la base de datos:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=login_laravel
DB_USERNAME=root
DB_PASSWORD=
```

### Paso 3 — Instalar dependencias PHP y Node

```bash
# Instala las versiones exactas definidas en composer.lock
# (NO usar composer update en producción — ese actualiza versiones)
composer install

# Instala dependencias JS definidas en package.json
# Genera node_modules/ y package-lock.json
npm install
```

### Paso 4 — Instalar el paquete de autenticación (Laravel/UI)

```bash
# Agrega laravel/ui a composer.json y descarga el paquete
composer require laravel/ui

# Genera el scaffolding de Bootstrap
# Copia archivos Bootstrap a resources/ y configura la integración
php artisan ui bootstrap

# Genera las vistas de auth (login, register, forgot-password, etc.)
# --auth es la flag que indica que también genere los controladores y vistas de autenticación
php artisan ui bootstrap --auth

# Compila los assets (Bootstrap CSS/JS) con Vite en modo desarrollo
npm install
npm run dev
```

### Paso 5 — Migraciones y base de datos

```bash
# Limpiar caché de configuración para que Laravel lea el .env actualizado
php artisan config:clear
php artisan config:cache

# Ejecutar migraciones: crea las tablas users, cache y jobs
# Laravel busca en database/migrations/, revisa la tabla migrations
# y ejecuta solo las pendientes
php artisan migrate
```

### Paso 6 — Levantar el servidor

```bash
php artisan serve
# Servidor disponible en http://127.0.0.1:8000
```

---

## Detalles de Comandos Clave

### `php artisan key:generate`
Genera una clave segura de 32 caracteres codificada en base64 y la escribe automáticamente en `APP_KEY` del `.env`. Laravel la usa para encriptar cookies, sesiones y datos con `Crypt::encrypt()`. **No cambiarla en producción** — invalidaría todas las sesiones activas.

### `php artisan migrate`
Ejecuta todas las migraciones pendientes en `database/migrations/`. Cada migración tiene dos métodos: `up()` que crea la tabla al migrar, y `down()` que la revierte con `migrate:rollback`. Laravel registra en la tabla `migrations` cuáles ya se aplicaron para no repetirlas.

### `composer install` vs `composer update`

| Comando | Comportamiento |
|---|---|
| `composer install` | Instala versiones **exactas** del `composer.lock`. Para entornos consistentes. |
| `composer update` | Actualiza a versiones más recientes permitidas y regenera `composer.lock`. |

### `php artisan config:clear` y `config:cache`
Laravel optimiza el rendimiento cacheando toda la configuración de `config/` en un solo archivo (`bootstrap/cache/config.php`). Después de editar `.env` es necesario limpiar ese caché para que Laravel tome los nuevos valores.

### `npm run dev` / `composer run dev`
Ambos invocan Vite para compilar los assets front-end en modo desarrollo con hot-reloading. `composer run dev` llama internamente a Vite a través del script `dev` definido en `composer.json`. Si el script no existe en `composer.json`, usar directamente `npm run dev`.

---

### Migraciones generadas

| Archivo | Tabla creada | Descripción |
|---|---|---|
| `0001_01_01_000000_create_users_table.php` | `users` | Almacena usuarios: name, email, password, timestamps |
| `0001_01_01_000001_create_cache_table.php` | `cache` | Cache de base de datos de Laravel |
| `0001_01_01_000002_create_jobs_table.php` | `jobs` | Cola de trabajos asincrónicos |

### Comandos de migración utilizados

```bash
php artisan migrate              # Ejecuta migraciones pendientes
php artisan migrate:status       # Ver estado de cada migración
php artisan migrate:rollback     # Revierte la última migración
php artisan migrate:fresh        # Elimina todas las tablas y vuelve a migrar
```

---

## Resultado del Laboratorio

<img width="1320" height="610" alt="Captura de pantalla 2026-04-14 173019" src="https://github.com/user-attachments/assets/e1d578bc-25ba-4e62-a337-e9934c6c48a7" />
<img width="1320" height="610" alt="Captura de pantalla 2026-04-14 173115" src="https://github.com/user-attachments/assets/2d0a13fe-a042-4c36-a0a1-eab0c45feaee" />
<img width="1320" height="610" alt="Captura de pantalla 2026-04-14 173209" src="https://github.com/user-attachments/assets/af5a41a5-26c5-49a2-84e6-eaba1d709b24" />


---

## Dificultades y Soluciones

|Problema| solución|
|---|---|
|Problema con la conexion a la BD| Modificar el archivo env. especificamente en **Session_Drive** colocando que sea **file**|

---

## Referencias

1. **Laravel Documentation** — Documentación oficial del framework, instalación, autenticación y migraciones.
   🔗 https://laravel.com/docs/13.x

2. **Laravel/UI Package** — Repositorio oficial del paquete de scaffolding de autenticación.
   🔗 https://github.com/laravel/ui

3. **Composer Documentation** — Referencia de comandos `install`, `update` y `require`.
   🔗 https://getcomposer.org/doc/
---

---

## Fecha de Ejecución del Laboratorio

| Campo | Detalle |
|---|---|
| **Fecha de realización** | 8 de Abril 2026 |
| **Fecha límite de entrega** | 15 de abril de 2026 |
| **Semestre** | I Semestre 2025 |

---

<div align="center">

### Información 
Este laboratorio ha sido desarrollado por el estudiante de la Universidad Tecnológica de Panamá: 
| Campo | Detalle |
|---|---|
| **Nombre** | `Aaron Ortiz` |
| **Correo** | `aaron.ortiz@utp.ac.pa` |
| **Curso** | Desarrollo Web VII |
| **Instructor** | Ing. Irina Fong |
---

*Laboratorio #2 — Implementación del Login en Laravel | Unidad I: Patrón MVC*

</div>
