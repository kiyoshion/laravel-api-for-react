# Laravel for API

This app doesn't use docker, cause I deploy to LEMP on VPS. So this app is installed by composer.

```
composer create-project laravel/lravel laravel-api-for-react
```

## Setup

Create a database (MySQL or PostgresSQL and so on) in your terminal. Check `.env` file in laravel project, db_name, user, password.

```
mysql -u root
CREATE DATABASE db_name;
CREATE USER 'user'@'localhost' IDENTIFIED BY 'password';
GRANT ALL ON db_name.* TO user;
```

Then migrate.
```
php artisan migrate
```

## Install laravel/breeze for api

Command like this for installing apis.

```
composer require laravel/breeze --dev
php artisan breeze:install react

php artisan migrate
npm install
npm run dev
```

## Controller

### Add controller

If you add options like below, you can make controller resourceful.

```
php artisan make:controller PostC
ontroller --resource --api
```

### Add routes

Don't forget to add 'use' statement.

```routes/api.php
use App\Http\Controllers\PostController;

Route::apiResources([
  'posts' => PostController::class,
]);
```

### Add model

```
php artisan make:model Post --migration
```

Awesome command!
```
php artisan make:model Post --controller --resource --api
```
