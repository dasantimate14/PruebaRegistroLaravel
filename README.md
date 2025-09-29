<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400" alt="Laravel Logo"></a></p>

<p align="center">
<a href="https://github.com/laravel/framework/actions"><img src="https://github.com/laravel/framework/workflows/tests/badge.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

## README — Laboratorio: Implementación de Login en Laravel
> **Proyecto:** Login con Laravel (Laboratorio MVC)

> **Instructor:** Ing. Irina Fong

> **Repositorio:** https://github.com/dasantimate14/PruebaRegistroLaravel

---

## Índice

1. [Descripción del proyecto](#descripción-del-proyecto)
2. [Requisitos previos](#requisitos-previos)
3. [Estructura MVC (breve explicación)](#estructura-mvc-breve-explicación)
4. [Pasos detallados (guía de instalación y ejecución)](#pasos-detallados-guía-de-instalación-y-ejecución)
5. [Base de datos y migraciones](#base-de-datos-y-migraciones)
6. [Compilación de assets / Frontend (Vite / npm)](#compilación-de-assets--frontend-vite--npm)
7. [Resultados y artefactos (capturas / backup)](#resultados-y-artefactos-capturas--backup)
8. [Dificultades encontradas y soluciones aplicadas](#dificultades-encontradas-y-soluciones-aplicadas)
9. [Referencias y recursos consultados](#referencias-y-recursos-consultados)
10. [Footer — Información del estudiante](#footer--información-del-desarrollador--estudiante)

---

## Descripción del proyecto

Este repositorio contiene la implementación del módulo de **autenticación (login y registro)** en Laravel. El objetivo del laboratorio es familiarizarse con la estructura de un proyecto Laravel bajo el patrón **MVC**, configurar autenticación básica y documentar el flujo de trabajo, migraciones y problemas comunes encontrados durante la práctica.

---

## Requisitos previos

Antes de ejecutar el proyecto asegúrate de tener instalado/configurado:
[![PHP](https://img.shields.io/badge/PHP-8.0+-8892BF?logo=php)()
[![Laravel](https://img.shields.io/badge/Laravel-8+-FF2D20?logo=laravel)]()
[![Composer](https://img.shields.io/badge/Composer-2+-000000?logo=composer)]()
[![Node.js](https://img.shields.io/badge/Node.js-22.19.0-339933?logo=node.js)]()
[![MySQL](https://img.shields.io/badge/MySQL-5.7+/4479A1?logo=mysql)]()

* PHP `8.0` o superior.
* Composer (última versión estable).
* Node.js (recomendado: **node-v22.19.0** — ver *Soluciones* para problemas con npm).
* NPM (incluido con Node.js).
* Servidor web local (XAMPP, WampServer, Laragon, MAMP, etc.).
* Servidor HTTP: Apache o Nginx.
* Base de datos: **MySQL** o **MariaDB** en ejecución.
* Editor de código: **Visual Studio Code** (recomendado).
* Laravel installer o crear proyecto con `composer create-project` o `laravel new`.
---

## Estructura MVC (breve explicación)

Explicación rápida de las carpetas principales relacionadas con MVC en Laravel:

* `app/Http/Controllers/` — **Controladores**: lógica que procesa peticiones y devuelve vistas/respuestas.
* `routes/web.php` — **Rutas web**: mapeo de URLs a controladores/vistas.
* `resources/views/` — **Vistas (Blade)**: plantillas que renderizan la UI (login, register, etc.).
* `app/Models/` — **Modelos (Eloquent)**: representan tablas de la base de datos (por ejemplo `User`).
* `database/migrations/` — **Migraciones**: scripts PHP que crean/modifican tablas.

**Comandos de migración usados** (ver sección detalle más abajo):

```bash
php artisan migrate
php artisan migrate:rollback
```

---

## Pasos detallados — Guía de instalación y ejecución

A continuación están los pasos exactos ejecutados durante el laboratorio (orden recomendado). Copia y pega en tu terminal dentro de la carpeta donde quieras crear el proyecto.

### 1. Crear el proyecto (opcional si ya existe)

```bash
# Opción 1: Usando Laravel installer
laravel new login-lab

# Opción 2: Usando Composer
composer create-project laravel/laravel login-lab
```

### 2. Ingresar al proyecto e instalar dependencias PHP

```bash
cd login-lab
composer install
```

### 3. Configurar `.env`

* Copiar `.env.example` a `.env`:

```bash
cp .env.example .env
```

* Actualizar parámetros importantes en `.env`:

```
APP_NAME=LoginLab
APP_URL=http://127.0.0.1:8000

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=login_lab_db
DB_USERNAME=root
DB_PASSWORD=
```

Generar key de la aplicación:

```bash
php artisan key:generate
```

Si editas `.env` y los cambios no se reflejan, ejecutar:

```bash
php artisan config:clear
php artisan config:cache
```

### 4. Instalar scaffolding de autenticación (ejemplo con laravel/ui + Bootstrap)

> Nota: en versiones modernas también puedes usar `laravel/breeze`. Aquí se documenta `laravel/ui` como ejemplo de la guía.

```bash
composer require laravel/ui
php artisan ui bootstrap --auth
```

### 5. Instalar dependencias de Node y compilar assets

```bash
npm install
# o si prefieres via composer script (si está definido en composer.json)
# composer run dev

npm run dev    # modo desarrollo (hot-reload)
# npm run build # para producción
```

> Si usas `vite`, el proyecto puede venir configurado para `vite` y los scripts en `package.json` o `composer.json` deben existir. (Ver sección troubleshooting si `composer run dev` falla.)

### 6. Ejecutar migraciones (crear tablas)

Antes de migrar, asegúrate de crear la base de datos `login_lab_db` (o la que tengas en .env). Después:

```bash
php artisan migrate
```

Si la base de datos no existe, Laravel puede sugerir crearla (dependiendo del entorno) o tendrás que crearla manualmente desde phpMyAdmin o `mysql` CLI.

### 7. Levantar servidor de desarrollo

```bash
php artisan serve
# Por defecto estará en http://127.0.0.1:8000
```

Visita `http://127.0.0.1:8000` y prueba las rutas de autenticación (Login / Register).

---

## Base de datos y migraciones

* Archivo de configuración de DB: `.env`.
* Migraciones: `database/migrations/` — se ejecutan con `php artisan migrate`.
* Recomendación: después de la migración genera un respaldo (backup) de la base de datos y súbelo al repositorio en la carpeta `/database_backups/`.

**Ejemplo de comando mysqldump para crear backup:**

```bash
# desde terminal (ajusta usuario/contraseña y nombre de DB)
mysqldump -u root -p login_lab_db > database_backups/backup_login_lab.sql
```

**Nota:** Guarda `database_backups/backup_login_lab.sql` en el repo (no subir credenciales reales).

---

## Compilación de assets / Frontend (Vite / npm)

* `npm install` instalará paquetes listados en `package.json`.
* `npm run dev` compila en modo desarrollo (Vite con hot-reload).
* Alternativa: `composer run dev` si el script `dev` está declarado en `composer.json`. Si al ejecutar `composer run dev` obtienes `Script dev not defined`, usa `npm run dev`.

**Comandos útiles:**

```bash
# Limpiar caché de configuración si cambios en .env no se ven
php artisan config:clear
php artisan config:cache
```

---

## Resultados y artefactos (capturas / backup)

Incluye en el repositorio las siguientes rutas/archivos (se muestran como ejemplo; debes agregarlos):

* `/screenshots/login_home.png` — **Captura** de la página inicial del login (obligatorio por la guía).
* `/database_backups/backup_login_lab.sql` — **Backup** de la base de datos creada.
* `README.md` — este archivo.

> **Instrucción:** Toma una captura de pantalla del formulario de login y súbela a `/screenshots/login_home.png` en el repo.

---

## Dificultades encontradas y soluciones aplicadas

### Dificultad 1 — Errores de ejecución con los comandos `npm`

* **Síntoma:** `npm` fallaba o no encontraba módulos.
* **Solución aplicada:** Se instaló Node.js en la versión **node-v22.19.0** y se agregó la ruta a las variables de entorno del sistema para que `npm` y `node` estén disponibles en la terminal.

### Dificultad 2 — Error al ejecutar `npm` en la terminal PowerShell de VS Code

* **Síntoma:** Algunos scripts `.ps1` bloqueados impiden ejecutar comandos npm/shell scripts.
* **Solución aplicada:** Ejecutar en PowerShell (solo durante la sesión):

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

Esto permite ejecutar los comandos npm únicamente durante la sesión actual de la consola.

### Dificultad 3 — Errores al ejecutar migraciones porque la base de datos no existía

* **Síntoma:** `php artisan migrate` falla indicando que la base de datos no existe.
* **Solución aplicada:** Crear la base de datos manualmente (phpMyAdmin, CLI) o aceptar la sugerencia en consola si Laravel la ofrece (responder `yes` según el prompt). Luego reintentar:

```bash
php artisan migrate
```

### Otras dificultades comunes (documentadas en la guía)

* **APP_KEY vacío** → Solución: `php artisan key:generate`.
* **Configuración `.env` no aplicada** → Solución: `php artisan config:clear` y/o `php artisan config:cache`.
* **`composer run dev` -> Script dev not defined** → Uso `npm run dev` o agregar script `dev` en `composer.json`.

---

## Referencias y recursos consultados

1. Documentación del instructivo **"Customizar el Repositorio del Login de Laravel"**, instrucciones y requisitos del laboratorio. 
2. **Guía paso a paso: Cómo crear un Login y Registro en Laravel** (guía de laboratorio utilizada para los comandos y explicación de Vite/composer/npm). 
3. Documentación oficial de **Laravel** — guías y referencia sobre instalación, migraciones y autenticación. (Ej.: [https://laravel.com/docs](https://laravel.com/docs)).

> **Nota:** La guía del laboratorio (puntos 1–7) se siguió y adaptó para este README; revisa los archivos PDF insertados en el repositorio para ver capturas y notas del instructor.  

---

## Fecha de ejecución y entrega

* **Fecha de ejecución del laboratorio:** `2025-09-27`.
* **Fecha límite de entrega (instructor):** `2025-09-29`.

---

## Comandos rápidos (resumen)

```bash
# Clonar / crear proyecto
composer create-project laravel/laravel login-lab
cd login-lab

# PHP / Composer
composer install
php artisan key:generate
php artisan migrate
php artisan config:clear
php artisan serve

# Autenticación (laravel/ui ejemplo)
composer require laravel/ui
php artisan ui bootstrap --auth

# Node / Assets
npm install
npm run dev
# o
composer run dev  # solo si el script dev está definido
```

---

## Footer — Información del desarrollador / estudiante

Este laboratorio ha sido desarrollado por el estudiante de la Universidad Tecnológica de Panamá:

* **Nombre:** Diego Santimateo
* **Correo:** diego.santimateo@utp.ac.pa
* **Curso:** Ingeniería Web
* **Instructor del Laboratorio:** Ing. Irina Fong

---

## About Laravel

Laravel is a web application framework with expressive, elegant syntax. We believe development must be an enjoyable and creative experience to be truly fulfilling. Laravel takes the pain out of development by easing common tasks used in many web projects, such as:

- [Simple, fast routing engine](https://laravel.com/docs/routing).
- [Powerful dependency injection container](https://laravel.com/docs/container).
- Multiple back-ends for [session](https://laravel.com/docs/session) and [cache](https://laravel.com/docs/cache) storage.
- Expressive, intuitive [database ORM](https://laravel.com/docs/eloquent).
- Database agnostic [schema migrations](https://laravel.com/docs/migrations).
- [Robust background job processing](https://laravel.com/docs/queues).
- [Real-time event broadcasting](https://laravel.com/docs/broadcasting).

Laravel is accessible, powerful, and provides tools required for large, robust applications.

## Learning Laravel

Laravel has the most extensive and thorough [documentation](https://laravel.com/docs) and video tutorial library of all modern web application frameworks, making it a breeze to get started with the framework.

You may also try the [Laravel Bootcamp](https://bootcamp.laravel.com), where you will be guided through building a modern Laravel application from scratch.

If you don't feel like reading, [Laracasts](https://laracasts.com) can help. Laracasts contains thousands of video tutorials on a range of topics including Laravel, modern PHP, unit testing, and JavaScript. Boost your skills by digging into our comprehensive video library.

## Laravel Sponsors

We would like to extend our thanks to the following sponsors for funding Laravel development. If you are interested in becoming a sponsor, please visit the [Laravel Partners program](https://partners.laravel.com).

### Premium Partners

- **[Vehikl](https://vehikl.com)**
- **[Tighten Co.](https://tighten.co)**
- **[Kirschbaum Development Group](https://kirschbaumdevelopment.com)**
- **[64 Robots](https://64robots.com)**
- **[Curotec](https://www.curotec.com/services/technologies/laravel)**
- **[DevSquad](https://devsquad.com/hire-laravel-developers)**
- **[Redberry](https://redberry.international/laravel-development)**
- **[Active Logic](https://activelogic.com)**

## Contributing

Thank you for considering contributing to the Laravel framework! The contribution guide can be found in the [Laravel documentation](https://laravel.com/docs/contributions).

## Code of Conduct

In order to ensure that the Laravel community is welcoming to all, please review and abide by the [Code of Conduct](https://laravel.com/docs/contributions#code-of-conduct).

## Security Vulnerabilities

If you discover a security vulnerability within Laravel, please send an e-mail to Taylor Otwell via [taylor@laravel.com](mailto:taylor@laravel.com). All security vulnerabilities will be promptly addressed.

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
