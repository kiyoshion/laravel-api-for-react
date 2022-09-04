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

```app/Models/Post.php
class Post extends Model
{
    use HasFactory;

    protected $primaryKey = 'id';

    public $incrementing = false;

    protected $keyType = 'string';

    protected $fillable = [
        'id', 'title', 'body',
    ];

}
```

### Modify migration file

```
public function up()
{
    Schema::create('posts', function (Blueprint $table) {
        $table->uuid('id')->primary();
        $table->string('title');
        $table->text('body');
        $table->timestamps();
    });
}
```

Then command like this:
```
php artisan migrate
```

## Add CRUD on contoller

### Index

```
public function index()
{
    $posts = Post::all();

    return response()->json([
        'posts' => $posts
    ], 200);
}
```

### Store

```
public function store(Request $request)
{
    $uuid = uniqid();
    $post = Post::firstOrCreate([
        'id' => $uuid,
        'title' => $request->input('title'),
        'body' => $request->input('body')
    ]);

    return response()->json([
        'post' => $post
    ], 201);
}
```

### Show

```
public function show($id)
{
    $post = Post::findOrFail($id);

    return response()->json([
        'post' => $post
    ], 200);
}
```

### Update

```
public function update(Request $request, $id)
{
    $post = Post::updateOrCreate(
        ['id' => $id],
        [
            'title' => $request->input('title'),
            'body' => $request->input('body')
        ]
    );

    return response()->json([
        'post' => $post
    ], 201);
}
```

### Destroy

```
public function destroy($id)
{
    $post = Post::findOrFail($id);
    $post->delete();

    $posts = Post::all();

    return response()->json([
        'posts' => $posts
    ], 201);
}
```
