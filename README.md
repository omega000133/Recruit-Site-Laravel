# Recruit Site Laravel

A Laravel-based recruitment and job-listing platform developed for the **Hyogo Prefecture Automobile Maintenance Promotion Association** (兵庫県自動車整備振興会).

The application provides a public-facing recruitment website for browsing job opportunities and submitting inquiries, together with management interfaces for maintaining companies, vacancies, qualifications, users, and core site information.

**Live website:** https://recruit-kyujin-haspa.net/

---

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Application Areas](#application-areas)
- [Tech Stack](#tech-stack)
- [System Requirements](#system-requirements)
- [Local Installation](#local-installation)
- [Environment Configuration](#environment-configuration)
- [Database Setup](#database-setup)
- [Running the Application](#running-the-application)
- [Frontend Development](#frontend-development)
- [Available Routes](#available-routes)
- [Project Structure](#project-structure)
- [Data Models](#data-models)
- [Email Configuration](#email-configuration)
- [File Uploads](#file-uploads)
- [Testing](#testing)
- [Code Quality](#code-quality)
- [Production Deployment](#production-deployment)
- [Security Notes](#security-notes)
- [Troubleshooting](#troubleshooting)
- [License](#license)

---

## Overview

Recruit Site Laravel is a full-stack recruitment management application built with Laravel 10.

It supports two main user experiences:

1. **Public recruitment website**
   - View the recruitment portal
   - Browse job information
   - Review individual job or company details
   - Read application rules and related information
   - Submit an inquiry or application form
   - Receive a completion/thank-you screen

2. **Management interface**
   - Authenticate administrators
   - Maintain user and company records
   - Import user data from CSV
   - Create, update, and remove job records
   - Maintain degree or qualification options
   - Update basic company/site information
   - Upload business or company images
   - Manage primary recruitment information
   - Extend or update listing-related information

The project follows Laravel's MVC architecture and uses Blade templates for server-rendered pages.

---

## Key Features

### Public Job Portal

- Recruitment landing page
- Job listing and job-detail pages
- Organization/company information pages
- Application rule pages
- Contact or application form
- Email submission workflow
- Thank-you page after successful submission
- Responsive frontend assets built with Vite

### Job Management

- Create job postings
- View job records
- Update existing vacancies
- Delete vacancies
- Maintain detailed recruitment information
- Extend or renew recruitment-related records

### User and Company Management

- Create users or business records
- Update stored records
- Delete records
- Import records from CSV
- Maintain basic organization information
- Upload organization or profile images

### Qualification Management

- Create qualification or degree options
- Update qualification records
- Delete qualification records
- Reuse qualification data in recruitment workflows

### Authentication

- Laravel UI authentication scaffolding
- Login and registration routes generated through `Auth::routes()`
- Authenticated home/dashboard page

### Communication

- SMTP-based email delivery
- Dedicated email templates
- Inquiry/application form submission
- Configurable sender name and address

---

## Application Areas

The Blade views are organized around the following functional modules:

| Module | Purpose |
|---|---|
| `Job` | Public job browsing and vacancy details |
| `Item` | Supporting listing or content-item pages |
| `Form` | Inquiry/application form and email submission |
| `Rule` | Rules, terms, or application guidance |
| `JobManage` | Administrative vacancy management |
| `DegreeManage` | Qualification/degree management |
| `BasicManage` | Basic company and image information |
| `MainInfo` | Main recruitment information management |
| `auth` | Login, registration, password, and authentication pages |
| `emails` | Outbound email templates |
| `layouts` | Shared Blade layouts |

---

## Tech Stack

### Backend

- **PHP 8.1+**
- **Laravel 10**
- **Laravel UI 4**
- **Laravel Sanctum**
- **Laravel Tinker**
- **Guzzle HTTP Client**
- **MySQL**

### Frontend

- **Blade**
- **JavaScript**
- **SCSS/Sass**
- **Bootstrap 5**
- **Popper.js**
- **Axios**
- **Vite 5**

### Development and Testing

- **Composer**
- **npm**
- **PHPUnit 10**
- **Laravel Pint**
- **Laravel Sail**
- **Faker**
- **Mockery**
- **Laravel Ignition**

---

## System Requirements

Install the following software before running the project locally:

- PHP `8.1` or newer
- Composer `2.x`
- Node.js `18+`
- npm `9+`
- MySQL `8.x` or compatible MariaDB version
- PHP extensions commonly required by Laravel:
  - BCMath
  - Ctype
  - cURL
  - DOM
  - Fileinfo
  - JSON
  - Mbstring
  - OpenSSL
  - PDO
  - PDO MySQL
  - Tokenizer
  - XML

Check your installed versions:

```bash
php -v
composer --version
node -v
npm -v
mysql --version
```

---

## Local Installation

### 1. Clone the repository

```bash
git clone https://github.com/omega0106/recruit-site-laravel.git
cd recruit-site-laravel
```

### 2. Install PHP dependencies

```bash
composer install
```

### 3. Install frontend dependencies

```bash
npm install
```

### 4. Create the environment file

```bash
cp .env.example .env
```

On Windows Command Prompt:

```bat
copy .env.example .env
```

### 5. Generate the Laravel application key

```bash
php artisan key:generate
```

### 6. Configure the database

Create a MySQL database:

```sql
CREATE DATABASE recruit_site
    CHARACTER SET utf8mb4
    COLLATE utf8mb4_unicode_ci;
```

Update `.env`:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=recruit_site
DB_USERNAME=root
DB_PASSWORD=your_password
```

### 7. Create or import the database schema

Choose the method appropriate for the repository version you are using.

#### Option A: Run Laravel migrations

```bash
php artisan migrate
```

#### Option B: Import the included SQL dump

The repository includes `laravel.sql`, which may contain the database structure and existing application data.

```bash
mysql -u root -p recruit_site < laravel.sql
```

Do not run both methods blindly against the same non-empty database. Review the SQL dump and migration history first to prevent duplicate tables or conflicting data.

### 8. Create the storage link

```bash
php artisan storage:link
```

### 9. Clear cached configuration

```bash
php artisan optimize:clear
```

---

## Environment Configuration

A typical local `.env` configuration looks like this:

```env
APP_NAME="Recruit Site"
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://127.0.0.1:8000

LOG_CHANNEL=stack
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=recruit_site
DB_USERNAME=root
DB_PASSWORD=

BROADCAST_DRIVER=log
CACHE_DRIVER=file
FILESYSTEM_DISK=local
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120

MAIL_MAILER=smtp
MAIL_HOST=127.0.0.1
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="noreply@example.com"
MAIL_FROM_NAME="${APP_NAME}"

VITE_APP_NAME="${APP_NAME}"
```

Never commit the real `.env` file or production credentials.

---

## Database Setup

The application contains Laravel migrations and an included SQL export.

Relevant domain models include:

- `User`
- `Job`
- `Degree`
- `Main`
- `Extend_verify_token`

The exact table structure should be confirmed from:

```text
database/migrations/
laravel.sql
app/Models/
```

For a clean Laravel-managed database:

```bash
php artisan migrate
```

To reset the local database:

```bash
php artisan migrate:fresh
```

Use `migrate:fresh` only in a disposable development environment because it deletes all existing tables.

To inspect migration status:

```bash
php artisan migrate:status
```

---

## Running the Application

### Start the Laravel development server

```bash
php artisan serve
```

The backend will normally be available at:

```text
http://127.0.0.1:8000
```

### Start Vite in development mode

Open another terminal:

```bash
npm run dev
```

For a one-command local workflow on Unix-like systems:

```bash
php artisan serve & npm run dev
```

---

## Frontend Development

The frontend uses Vite to compile JavaScript and Sass/SCSS resources.

### Development server

```bash
npm run dev
```

### Production build

```bash
npm run build
```

The main frontend dependencies are:

- Bootstrap
- Popper.js
- Axios
- Sass
- Laravel Vite Plugin

Frontend source files are stored under:

```text
resources/js/
resources/css/
resources/sass/
resources/views/
```

Compiled production assets are written to:

```text
public/build/
```

---

## Available Routes

The application defines public resources, authentication routes, and management actions.

### Public and General Pages

| Method | URI | Purpose |
|---|---|---|
| `GET` | `/top` | Recruitment landing page |
| `GET` | `/thank` | Submission completion page |
| `GET` | `/home` | Authenticated home/dashboard |
| Resource | `/job` | Job listing and job-detail workflow |
| Resource | `/item` | Supporting content/item workflow |
| Resource | `/form` | Inquiry/application form workflow |
| Resource | `/rule` | Rules or guidance pages |
| `POST` | `/form/sendMail` | Send inquiry/application email |

### Authentication

Laravel UI registers standard authentication endpoints through:

```php
Auth::routes();
```

These typically include login, logout, registration, and password-reset routes, depending on the enabled Laravel UI configuration.

### User Management

| Method | URI | Purpose |
|---|---|---|
| `POST` | `/user/store` | Create a user record |
| `POST` | `/user/store/csv_upload` | Import user records from CSV |
| `POST` | `/user/update` | Update a user record |
| `POST` | `/user/delete` | Delete a user record |

### Job Management

| Method | URI | Purpose |
|---|---|---|
| `GET` | `/jobManage` | Job-management page |
| `POST` | `/jobManage/store` | Create a job |
| `POST` | `/jobManage/update` | Update a job |
| `POST` | `/jobManage/delete` | Delete a job |

### Qualification Management

| Method | URI | Purpose |
|---|---|---|
| `GET` | `/degreeManage` | Qualification-management page |
| `POST` | `/degreeManage/store` | Create a qualification |
| `POST` | `/degreeManage/update` | Update a qualification |
| `POST` | `/degreeManage/delete` | Delete a qualification |

### Basic Information Management

| Method | URI | Purpose |
|---|---|---|
| `GET` | `/basicManage` | Basic-information management page |
| `POST` | `/basicManage/store` | Create basic information |
| `POST` | `/basicManage/store/picture_upload` | Upload an image |
| `POST` | `/basicManage/update` | Update basic information |
| `POST` | `/basicManage/edit` | Retrieve or prepare a record for editing |
| `POST` | `/basicManage/delete` | Delete basic information |
| `POST` | `/basicManage/show` | Retrieve basic-information details |

### Main Recruitment Information

| Method | URI | Purpose |
|---|---|---|
| `GET` | `/mainInfo` | Main-information management page |
| `POST` | `/mainInfo/store` | Create main recruitment information |
| `POST` | `/mainInfo/update` | Update main recruitment information |
| `POST` | `/mainInfo/extend` | Extend or renew information |
| `POST` | `/mainInfo/show` | Retrieve information details |

To see the authoritative route list for your checked-out version:

```bash
php artisan route:list
```

Filter routes:

```bash
php artisan route:list --path=job
php artisan route:list --path=basicManage
php artisan route:list --method=POST
```

---

## Project Structure

```text
recruit-site-laravel/
├── app/
│   ├── Console/
│   ├── Exceptions/
│   ├── Http/
│   │   ├── Controllers/
│   │   ├── Middleware/
│   │   └── Kernel.php
│   ├── Mail/
│   ├── Models/
│   │   ├── Degree.php
│   │   ├── Extend_verify_token.php
│   │   ├── Job.php
│   │   ├── Main.php
│   │   └── User.php
│   └── Providers/
├── bootstrap/
├── config/
├── database/
│   ├── factories/
│   ├── migrations/
│   └── seeders/
├── public/
├── resources/
│   ├── css/
│   ├── js/
│   ├── lang/
│   ├── sass/
│   └── views/
│       ├── BasicManage/
│       ├── DegreeManage/
│       ├── Form/
│       ├── Item/
│       ├── Job/
│       ├── JobManage/
│       ├── MainInfo/
│       ├── Rule/
│       ├── auth/
│       ├── emails/
│       └── layouts/
├── routes/
│   ├── api.php
│   ├── channels.php
│   ├── console.php
│   └── web.php
├── storage/
├── tests/
├── .env.example
├── artisan
├── composer.json
├── laravel.sql
├── package.json
├── phpunit.xml
└── vite.config.js
```

---

## Data Models

### User

Represents authenticated users and/or managed company/member records.

Related workflows include:

- Record creation
- Record updates
- Record deletion
- CSV bulk import
- Authentication

### Job

Represents vacancies displayed on the public recruitment portal.

Related workflows include:

- Public job listing
- Job details
- Administrative creation
- Administrative update
- Administrative deletion

### Degree

Represents education, qualification, license, or experience options used by recruitment records.

### Main

Stores primary recruitment or organization information used across public and management pages.

### Extend Verify Token

Appears to support an extension or verification workflow associated with recruitment information. Review the model, migration, and controller implementation before changing this flow.

---

## Email Configuration

The contact/application form sends email through Laravel's mail system.

For local development, Mailpit or MailHog can be used:

```env
MAIL_MAILER=smtp
MAIL_HOST=127.0.0.1
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="noreply@example.com"
MAIL_FROM_NAME="${APP_NAME}"
```

For production, configure a real provider such as Amazon SES, SendGrid, Mailgun, Postmark, or a trusted SMTP service.

Example SMTP configuration:

```env
MAIL_MAILER=smtp
MAIL_HOST=smtp.example.com
MAIL_PORT=587
MAIL_USERNAME=your_username
MAIL_PASSWORD=your_password
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS="recruit@example.com"
MAIL_FROM_NAME="Recruit Site"
```

After changing mail settings:

```bash
php artisan config:clear
```

Email templates are located under:

```text
resources/views/emails/
```

---

## File Uploads

The basic-information module includes a picture-upload endpoint:

```text
POST /basicManage/store/picture_upload
```

For local uploads:

```bash
php artisan storage:link
```

Ensure these directories are writable:

```text
storage/
bootstrap/cache/
```

On Linux:

```bash
chmod -R 775 storage bootstrap/cache
```

The web-server user must own or have write access to those directories.

Validate uploaded files by:

- MIME type
- File extension
- Maximum size
- Image dimensions
- Generated server-side filename

Do not trust the original client filename.

---

## CSV Import

The user-management workflow provides a CSV upload endpoint:

```text
POST /user/store/csv_upload
```

A sample `user.csv` file is included in the repository.

Before importing data:

1. Back up the database.
2. Confirm the expected column order.
3. Check the character encoding.
4. Remove duplicate records.
5. Validate required fields.
6. Test with a small file first.

Japanese CSV files may use UTF-8, UTF-8 with BOM, or Shift_JIS. Confirm the parser behavior before importing production data.

---

## Testing

Run the complete test suite:

```bash
php artisan test
```

Or run PHPUnit directly:

```bash
./vendor/bin/phpunit
```

Run a specific test:

```bash
php artisan test --filter=ExampleTest
```

Recommended test coverage:

- Authentication and authorization
- Public job listing
- Job-detail retrieval
- Job CRUD operations
- User CRUD operations
- CSV validation and import
- Image upload validation
- Inquiry form validation
- Email dispatch
- Qualification management
- Main-information extension workflow

---

## Code Quality

### Format PHP code

```bash
./vendor/bin/pint
```

Check formatting without modifying files:

```bash
./vendor/bin/pint --test
```

### Clear Laravel caches

```bash
php artisan optimize:clear
```

### Rebuild Composer autoload files

```bash
composer dump-autoload
```

### Inspect registered routes

```bash
php artisan route:list
```

---

## Production Deployment

### 1. Configure the production environment

```env
APP_ENV=production
APP_DEBUG=false
APP_URL=https://your-domain.example
```

Use production database, mail, session, cache, and logging credentials.

### 2. Install optimized PHP dependencies

```bash
composer install --no-dev --prefer-dist --optimize-autoloader
```

### 3. Install and build frontend assets

```bash
npm ci
npm run build
```

### 4. Run database migrations

```bash
php artisan migrate --force
```

Use the SQL dump instead only when that is the intended deployment procedure.

### 5. Create the storage link

```bash
php artisan storage:link
```

### 6. Cache Laravel configuration

```bash
php artisan config:cache
php artisan route:cache
php artisan view:cache
```

If route caching reports closure-related errors, resolve the affected route definitions before enabling it.

### 7. Configure the web server

Point the document root to:

```text
/path/to/recruit-site-laravel/public
```

Do not point the public web root at the project root.

### 8. Configure permissions

```bash
chown -R www-data:www-data storage bootstrap/cache
chmod -R 775 storage bootstrap/cache
```

Adapt the web-server user to your hosting environment.

### 9. Schedule Laravel tasks

Add the scheduler to cron if scheduled commands are used:

```cron
* * * * * cd /path/to/recruit-site-laravel && php artisan schedule:run >> /dev/null 2>&1
```

### 10. Configure queues

The default `.env.example` uses the synchronous queue driver:

```env
QUEUE_CONNECTION=sync
```

For higher-volume email or import processing, consider a database or Redis queue and run a supervised worker.

---

## Security Notes

Before deploying publicly:

- Set `APP_DEBUG=false`.
- Rotate all credentials that may have been exposed.
- Do not commit `.env`.
- Protect management routes with authentication and authorization middleware.
- Validate every POST request with Laravel Form Requests.
- Verify CSRF protection is active.
- Restrict image and CSV upload types and sizes.
- Escape user-generated output in Blade.
- Use parameterized Eloquent or query-builder operations.
- Apply rate limiting to login and public form submission.
- Use HTTPS.
- Configure secure session cookies.
- Add spam protection to public forms.
- Log administrative changes.
- Back up the MySQL database and uploaded files.
- Review imported SQL and CSV files before using them in production.

Because the repository contains explicit management POST endpoints, confirm that every administrative route is protected at the controller or route-middleware level.

---

## Troubleshooting

### `No application encryption key has been specified`

```bash
php artisan key:generate
```

### `SQLSTATE[HY000] [1045] Access denied`

Verify:

```env
DB_HOST
DB_PORT
DB_DATABASE
DB_USERNAME
DB_PASSWORD
```

Then clear cached configuration:

```bash
php artisan config:clear
```

### `Vite manifest not found`

Run:

```bash
npm install
npm run build
```

For development:

```bash
npm run dev
```

### Uploaded images are not visible

Run:

```bash
php artisan storage:link
```

Then verify storage permissions.

### Changes to `.env` are ignored

```bash
php artisan optimize:clear
```

### `Class not found` after adding or moving PHP files

```bash
composer dump-autoload
```

### Email is not delivered

- Check SMTP host and port.
- Check encryption settings.
- Confirm sender address.
- Review `storage/logs/laravel.log`.
- Use Mailpit locally.
- Clear cached configuration.

### CSV import produces corrupted Japanese characters

Confirm the source encoding and convert the file to UTF-8 before import when necessary.

Example with `iconv`:

```bash
iconv -f SHIFT_JIS -t UTF-8 user.csv > user-utf8.csv
```

### Permission denied errors

Ensure the web server can write to:

```text
storage/
bootstrap/cache/
```

---

## Recommended Future Improvements

- Add complete feature and integration tests
- Introduce Form Request classes for all management actions
- Group management routes under `auth` and authorization middleware
- Replace action-style POST endpoints with consistent RESTful resource routes
- Add role-based access control
- Move CSV imports and email delivery to queues
- Add audit logs for administrative changes
- Add pagination and filters for large datasets
- Add spam and abuse protection to public forms
- Add deployment automation with GitHub Actions
- Add database and uploaded-file backup automation
- Add API documentation if external integrations are introduced
- Remove obsolete or duplicated root-level PHP files
- Resolve the merge-conflict markers currently visible in the existing README

---

## License

This repository is based on Laravel, which is released under the MIT License.

Confirm the intended license for the custom recruitment-site source code with the repository owner before redistributing or reusing it commercially.

---

## Acknowledgements

- [Laravel](https://laravel.com/)
- [Bootstrap](https://getbootstrap.com/)
- [Vite](https://vitejs.dev/)
- [Hyogo Prefecture Automobile Maintenance Promotion Association](https://recruit-kyujin-haspa.net/)
