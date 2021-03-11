# INSTALL

## Mandatory

#### PHP (Language)

Download and install (**check the version required**):

- Directly from the PHP website: <https://www.php.net/downloads>
- With XAMPP (contains Apache + MariaDB + PHP + Perl): <https://www.apachefriends.org/download.html>

#### Composer (Package Manager)

Download and install: <https://getcomposer.org/download/>

#### PostgreSQL (Database)

Download and install: <https://www.postgresql.org/download/>

#### Node (Package Manager)

Download and install: <https://nodejs.org/en/download/>

#### Git (Version Management)

Download and install: <https://git-scm.com/downloads>

## Optional (but recommended)

#### Visual Studio Code (IDE)

Download and install: <https://code.visualstudio.com/download>

#### Git Extensions (Git Management)

Download and install: <https://github.com/gitextensions/gitextensions/releases/latest>

#### DBeaver (Database Administration)

Download and install: <https://dbeaver.io/download/>

#### Docker Desktop (Container Management)

Download and install: <https://www.docker.com/products/docker-desktop>

---

## Run

### Setup

- Clone your project with `git clone` and open a terminal at the root of the project
- Install all dependencies

  `composer install`

- Serve your application

  `php artisan serve`

### Useful commands

- List all commands

  `php artisan list`

- Open a php command line (for testing)

  `php artisan tinker`

- Generate application key

  `php artisan key:generate`

- Run database migrations and seeders

  `php artisan migrate:fresh --seed`

- Add a new package

  `composer require [package name]`

### Compile Javascript

- Install all dependencies

  `npm install`

- Compile javascript assets

  `npm run dev` (compiles for development)

  `npm run watch` (compiles after every code change)

  `npm run prod` (compiles for production)

- Add a new package

  `npm install [package name]`
