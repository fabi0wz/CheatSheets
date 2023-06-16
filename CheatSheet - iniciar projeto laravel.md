# Laravel Cheatsheet

## Instalar:

XAMPP (Version 7.4.33 php)

Composer (Version 2.5.5)

Node (v16.19.1)

## **Comandos cmd para verificar as instalações:**


`php -v` `Composer` `Node -v`

verificar se estou com a versao do node correta senão fazer `nvm use 16.19.1`

[Iniciar novo Projeto](#iniciar-novo-projeto)\
[Criar Rota, Controller e View](#criar-rota-controller-e-view)\
[Criar Factory, Seeder e Migration](#criar-factory-seeder-e-migration)\
[My beloved Laravel cheat sheet](#my-beloved-laravel-cheat-sheet)\
[Migrations - Dump](#migrations-dump)

# Paths nos projetos

- Controllers -> `\app\Http\Controllers`
- Views -> `\resources\views`
- Routes -> `\routes\web.php`

**tree view**

```
├── app
│   └── Http
│       └── Controllers
├── Resources
│   └── Views
├── routes
│   └── web.php

```
## Iniciar novo Projeto
## **1 - Instalação: (EXECUTAR NO CMD)**


`composer global require laravel/installer`****

## **2 - Iniciar um Projeto**


`Composer create-project --prefer-dist laravel/laravel:^7.0 blog`

> blog vai ser o nome do projeto 7.0 especificamos a versão do laravel
> 

de seguida podemos fazer CD para o projeto ou abrir a pasta no terminal no IDE****

## **3 - Instalar autenticação e bootstrap:**


> todos os passos daqui para a frente
tem de ser realizado dentro da pasta do projecto:
> 

`composer require laravel/ui:^2.4`

`php artisan ui vue --auth`****

esta a usar versao 4.6 do bootstrap

## **4 - Instalar npm**


`npm install && npm run dev` (linux / mac)

`npm install; npm run dev` (windows)

`npm install`

`npm run dev`

> podemos correr os dois em separado para evitar erros****
> 

## **5 - Editar ficheiro de acesso a BD**


No ficheiro `.env` é onde defenimos as informacoes das bases de dados

***Iniciar Apache e MySql no XAMPP***

## linha 9 a 14

`DB_DATABASE=laravel` alterar para `DB_DATABASE=ATEC`

> nome da base de dados que criarmos ou estejamos a utilizar no phpmyadmin
> 

`DB_USERNAME=root` alterar para `DB_USERNAME=root`

> nome do user que tem acesso a BD default é root
> 

`DB_PASSWORD=root` alterar para `DB_PASSWORD=`

> e password do mesmo default é vazio
> 

## **6 - Migrar BD para phpmyadmin5 - Editar ficheiro de acesso a BD**


Passa a estrutura das tabelas para o phpmyadmin (verificar no php my admin -> designer)

`php artisan migrate`****

## **7 - Iniciar um WebServer - > XAMPP**


`php artisan serve`

> se alguma alteracao for feita aos ficheiros é necessário reiniciar o web server
> 

> CTRL + C e voltamos a correr o comando
> 
> 
> `php artisan serve`
>

## [Back to Top](#laravel-cheatsheet)

# Criar Rota, Controller e View

## 1 - No Controller do nosso player criamos o return da view e os nossos metodos

Dentro do controller criamos as variaveis que pretendemos utilizar nas views e é onde fazemos a lógica para consulta à base de dados e guardamos em estruturas de dados

```php
public function index()
{
    $players = player::all();
    return response(view('players', [
        'players' => $players,
    ]));
}
```

Podemos enviar varias variaveis de uma vez

```php
public function index()
    {
        $helloWorld = 'Hello World!';
        return view('hello_world.index',
            [
                'helloWorld' => $helloWorld,
                'h2' => 'Isto está no h2!',
                'h3' => 'Isto está no h3!',
                'h4' => 'Isto está no h4!'
            ]
        );
    }
//estamos a enviar para a view hello_world.index as variaveis helloworld, h2, h3, h4
```

## 2 - No ficheiro `routes\web.php` registamos a rota para esta view

A route lida com o request do browser para aceder aquela pagina, verifica se está registada e se estiver chama o Controller a essa rota associado, que por si vai retornar a view

```php
Route::get('/players', 'PlayerController@index')->name('players');
```

## 3 - Criamos dentro das views criamos a pasta master

com os ficheiros 

```php
//footer.blade.php
<footer>
    <p align="center">This is a footer</p>
</footer>
```

```php
//header.blade.php
<nav class="navbar navbar-expand-lg navbar-light bg-light">
    <a class="navbar-brand" href="#">Navbar</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
  
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav mr-auto">
        <li class="nav-item active">
          <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Link</a>
        </li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" role="button" data-toggle="dropdown" aria-expanded="false">
            Dropdown
          </a>
          <div class="dropdown-menu">
            <a class="dropdown-item" href="#">Action</a>
            <a class="dropdown-item" href="#">Another action</a>
            <div class="dropdown-divider"></div>
            <a class="dropdown-item" href="#">Something else here</a>
          </div>
        </li>
        <li class="nav-item">
          <a class="nav-link disabled">Disabled</a>
        </li>
      </ul>
      <form class="form-inline my-2 my-lg-0">
        <input class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search">
        <button class="btn btn-outline-success my-2 my-sm-0" type="submit">Search</button>
      </form>
    </div>
  </nav>
```

```php
//main.blade.php
<!DOCTYPE html>
<html lang="{{ app()->getLocale() }}">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>Project</title>

    {{-- STYLE SECTION --}}
    <link rel="stylesheet" href="{!! asset('css/app.css') !!}" media="all" type="text/css" />
    @yield('styles')
    {{-- .STYLE SECTION --}}
</head>

<body>

{{-- Header --}}
@component('master.header')
@endcomponent
{{-- .Header --}}

{{-- Main --}}
<main>
    @yield('content')
</main>
{{-- .Main --}}

{{-- Footer --}}
@component('master.footer')
@endcomponent
{{-- .Footer --}}

{{-- SCRIPTS SECTION --}}
<script src="{!! asset('js/app.js') !!}" type="text/javascript"></script>
@yield('scripts')
{{-- .SCRIPTS SECTION --}}

</body>

</html>
```

## 4 - Na pasta views criamos o nosso ficheiro que o controller esta a chamar, por exemplo players.blade.php onde importamos os ficheiros do master

```php
@extends('master.main')

@section('styles')
@endsection

@section('scripts')
@endsection

@section('content')
    @component('components.table', [
        'players' => $players,
    ])

    @endcomponent
@endsection
```

## 5 - Criamos a pasta views/components, onde colocamos os componentes individuais

Dentro das views agora podemos utilizar as variaveis que enviamos através do controller `$player`

`table.blade.php`

```php
<table class="container table table-striped">
    {{-- cabeçalho --}}
    <thead>
    <tr>
        <th scope="col">id</th>
        <th scope="col">Name</th>
        <th scope="col">Address</th>
        <th scope="col">Description</th>
        <th scope="col">Retired</th>
    </tr>
    </thead>
    {{-- body --}}
    <tbody>
    @foreach ($players as $player)
        <tr>
            <th scope="row">{{ $player->id }}</th>
            <td>{{ $player->name }}</td>
            <td>{{ $player->address }}</td>
            <td>{{ $player->description }}</td>

            {{-- <td>{!! $player->retired ? '<svg fill="#000000" viewBox="0 0 32 32" version="1.1" xmlns="http://www.w3.org/2000/svg"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g><g id="SVGRepo_iconCarrier"> <title>heart</title> <path d="M0.256 12.16q0.544 2.080 2.080 3.616l13.664 14.144 13.664-14.144q1.536-1.536 2.080-3.616t0-4.128-2.080-3.584-3.584-2.080-4.16 0-3.584 2.080l-2.336 2.816-2.336-2.816q-1.536-1.536-3.584-2.080t-4.128 0-3.616 2.080-2.080 3.584 0 4.128z"></path> </g></svg>' : '<svg fill="#000000" viewBox="0 0 32 32" version="1.1" xmlns="http://www.w3.org/2000/svg"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g><g id="SVGRepo_iconCarrier"> <title>alt-battery-1</title> <path d="M0 20q0 2.496 1.76 4.256t4.256 1.76h17.984q2.496 0 4.256-1.76t1.76-4.256h1.984v-8h-1.984q0-2.464-1.76-4.224t-4.256-1.76h-17.984q-2.496 0-4.256 1.76t-1.76 4.224v8zM4 20v-8q0-0.832 0.576-1.408t1.44-0.576h17.984q0.832 0 1.408 0.576t0.608 1.408v8q0 0.832-0.608 1.44t-1.408 0.576h-17.984q-0.832 0-1.44-0.576t-0.576-1.44zM6.016 20h1.984v-8h-1.984v8z"></path> </g></svg>' !!}</td> --}}

            <td>{!! $player->retired ? '<svg fill="#000000" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g><g id="SVGRepo_iconCarrier"><path d="M8,11V9a1,1,0,0,1,2,0v2a1,1,0,0,1-2,0Zm7,1a1,1,0,0,0,1-1V9a1,1,0,0,0-2,0v2A1,1,0,0,0,15,12Zm.225,2.368a4,4,0,0,1-6.45,0,1,1,0,0,0-1.55,1.264,6,6,0,0,0,9.55,0,1,1,0,1,0-1.55-1.264ZM23,12A11,11,0,1,1,12,1,11.013,11.013,0,0,1,23,12Zm-2,0a9,9,0,1,0-9,9A9.01,9.01,0,0,0,21,12Z"></path></g></svg>' : '<svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g><g id="SVGRepo_iconCarrier"> <path d="M16 16.4995C15.0878 15.2854 13.6356 14.5 12 14.5C10.3642 14.5 8.91221 15.2857 8 16.5M15 9V10M9 9V10M12 21C16.9706 21 21 16.9706 21 12C21 7.02944 16.9706 3 12 3C7.02944 3 3 7.02944 3 12C3 16.9706 7.02944 21 12 21Z" stroke="#000000" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path> </g></svg>' !!}</td>
        </tr>
    @endforeach
    </tbody>
</table>
```
## [Back to Top](#laravel-cheatsheet)

# Criar Factory, Seeder e Migration

## 1 - Criar o model

ao corrermos este comando cria ja a migration, seeder e factory, para comandos individuais verificar a cheatsheet

```php
php artisan make:model Player -a
```

## 2 - Estruturar as tabelas

De seguida vamos as Database/Migrations e declaramos os campos que a tabela irá ter e as foreign keys que pretendemos.

```php
public function up()
    {
        Schema::create('players', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('address');
            $table->text('description');
            $table->boolean('retired');
            $table->unsignedBigInteger('country_id');
            $table->foreign('country_id')->references('id')->on('countries');
            $table->timestamps();
        });
    }
```

## 3 - Criar Factory

Depois podemos ir ao Database/PlayerFactory e fazer algo do tipo:

```php
$factory->define(Player::class, function (Faker $faker) {
    return [
        'name' => $faker->name,
        'address' => $faker->address,
        'description' => $faker->text,
        'retired' => $faker->boolean,
        'country_id' => $faker->numberBetween(1, 100)
    ];
});
```

Por cada campo na tabela, preenchemos com data usando o gerador de fake data faker

## 4 - Seeder

Depois temos de ir ao Database/Seeder/PlayerSeeder e chamar a factory:

```php
public function run()
    {
        factory(App\Player::class, 100)->create();
    }
```

Podemos também inserir data individualmente através do seeder:

```php
//Manual insert
    \DB::table('pets')->insert([
        'columnName1' => 'Pet 1',
        'columnName2' => '2000-1-5',
        'columnName3' => 'brown',
        'person_id' => 1,
    ]);
```

## 5 - Inicializar os Seeders

E por fim vamos ao ficheiro DatabaseSeeder:

```php
public function run()
    {
        $this->call(CountrySeeder::class);
        $this->call(PlayerSeeder::class);
        // $this->call(UserSeeder::class);
    }
```

Depois vamos ao model (sao os ficheiros dentro da pasta app) app\Players.php ou app\Country.php

e criamos as relacoes entre as diferentes tables

```php
//no ficheiro Players.php
public function country()
    {
        return $this->belongsTo(Country::class);
    }
```

```php
//no ficheiro Country.php
public function players()
    {
        return $this->hasMany(Player::class);
    }
```

no fim é só correr o comando `php artisan migrate:fresh —seed`

## [Back to Top](#laravel-cheatsheet)

# My beloved Laravel cheat sheet

## Laravel cheat sheet

Project commands

```
// New project
$ laravel new projectName

// Launch server/project
$ php artisan serve

// commands list
$ php artisan list

// command help
$ php artisan help migrate

// Laravel console
$ php artisan tinker

// Route list
$ php artisan route:list

```

Commons commands

```php
// Database migration
$ php artisan migrate

// Data seed
$ php artisan db:seed

// Create table migration
$ php artisan make:migration create_products_table

// Create from model with options:
// -m (migration), -c (controller), -r (resource controllers), -f (factory), -s (seed)
$ php artisan make:model Product -mcf

// Create a controller
$ php artisan make:controller ProductsController

// Update table migration
$ php artisan make:migration add_date_to_blogposts_table

// Rollback latest migration
php artisan migrate:rollback

// Rollback all migrations
php artisan migrate:reset

// Rollback all and re-migrate
php artisan migrate:refresh

// Rollback all, re-migrate and seed
php artisan migrate:refresh --seed

```

Create and update data tables (with migrations)

```php
// Create data table
$ php artisan make:migration create_products_table

// Create table(migration exemple)
Schema::create('products', function (Blueprint $table) {
    // auto-increment primary key
    $table->id();
    // created_at and updated_at
    $table->timestamps();
    // Unique constraint
    $table->string('modelNo')->unique();
    // Not required
    $table->text('description')->nullable();
    // Default value
    $table->boolean('isActive')->default(true);
    // Index
    $table->index(['account_id', 'created_at']);
    // Foreign Key relation
    $table->foreignId('user_id')->constrained('users')->onDelete('cascade');
});

// Update table (migration exemple)
$ php artisan make:migration add_comment_to_products_table

// up()
Schema::table('users', function (Blueprint $table) {
    $table->text('comment');
});

// down()
Schema::table('users', function (Blueprint $table) {
    $table->dropColumn('comment');
});

```

Models (app\* → ficheiros com o nome dos controllers)

In DatabaseSeeder → `$this→call(SeederFile::class);`

```php
// Model mass assignment list exclude attributes
protected $guarded = []; // empty == All

// Or list included attributes
protected $fillable = ['name', 'email', 'password',];

// One to Many relationship (posts with many comments)
public function comments()
{
    return $this->hasMany(Comment::class);
}

// One to Many relationship (comments with one post)
public function post()
{
    return $this->belongTo(Post::class);
}

// One to one relationship (Author with one profile)
public function profile()
{
    return $this->hasOne(Profile::class);
}

// One to one relationship (Profile with one Author)
public function author()
{
    return $this->belongTo(Author::class);
}

// Many to Many relationship
// 3 tables (posts, tags and post_tag)
// post_tag (post_id, tag_id)

// in Tag model...
public function posts()
    {
        return $this->belongsToMany(Post::class);
    }

// in Post model...
public function tags()
    {
        return $this->belongsToMany(Tag::class);
    }

```

Factory

```php
// ex: database/factories/ProductFactory.php
public function definition() {
    return [
        'name' => $this->faker->text(20),
        'price' => $this->faker->numberBetween(10, 10000),
    ];
}
// all fakers options : https://github.com/fzaninotto/Faker

```

Seed

```php
// CALL FACTORY TROUGH SEEDER
// ex: database/seeders/DatabaseSeeder.php
public function run() {
    Product::factory(10)->create();
}

//INSERT INDIVIDUAL DATA IN TABLE
//Seeds file
public function run()
{
    //Manual insert
    \DB::table('pets')->insert([
        'columnName1' => 'Pet 1',
        'columnName2' => '2000-1-5',
        'columnName3' => 'brown',
        'person_id' => 1,
    ]);
}

```

Running Seeders

```
$ php artisan db:seed
// or with migration
$ php artisan migrate --seed

```

Eloquent ORM

```php
// New
$flight = new Flight;
$flight->name = $request->name;
$flight->save();

// Update
$flight = Flight::find(1);
$flight->name = 'New Flight Name';
$flight->save();

// Create
$user = User::create(['first_name' => 'Taylor','last_name' => 'Otwell']);

// Update All:
Flight::where('active', 1)->update(['delayed' => 1]);

// Delete
$current_user = User::Find(1)
$current_user.delete();

// Delete by id:
User::destroy(1);

// Delete all
$deletedRows = Flight::where('active', 0)->delete();

// Get All
$items = Item::all().

// Find one by primary key
$flight = Flight::find(1);

// display 404 if not found
$model = Flight::findOrFail(1);

// Get last entry
$items = Item::latest()->get()

// Chain
$flights = App\Flight::where('active', 1)->orderBy('name', 'desc')->take(10)->get();

// Where
Todo::where('id', $id)->firstOrFail()

// Like
Todos::where('name', 'like', '%' . $my . '%')->get()

// Or where
Todos::where('name', 'mike')->orWhere('title', '=', 'Admin')->get();

// Count
$count = Flight::where('active', 1)->count();

// Sum
$sum = Flight::where('active', 1)->sum('price');

// Contain?
if ($project->$users->contains('mike'))

```

Routes

```php
// Basic route closure
Route::get('/greeting', function () {
    return 'Hello World';
});

// Route direct view shortcut
Route::view('/welcome', 'welcome');

// Route to controller class
use App\Http\Controllers\UserController;
Route::get('/user', [UserController::class, 'index']);

// Route only for specific HTTP verbs
Route::match(['get', 'post'], '/', function () {
    //
});

// Route for all verbs
Route::any('/', function () {
    //
});

// Route Redirect
Route::redirect('/clients', '/customers');

// Route Parameters
Route::get('/user/{id}', function ($id) {
    return 'User '.$id;
});

// Optional Parameter
Route::get('/user/{name?}', function ($name = 'John') {
    return $name;
});

// Named Route
Route::get(
    '/user/profile',
    [UserProfileController::class, 'show']
)->name('profile');

// Resource
Route::resource('photos', PhotoController::class);

GET /photos index   photos.index
GET /photos/create  create  photos.create
POST    /photos store   photos.store
GET /photos/{photo} show    photos.show
GET /photos/{photo}/edit    edit    photos.edit
PUT/PATCH   /photos/{photo} update  photos.update
DELETE  /photos/{photo} destroy photos.destroy

// Nested resource
Route::resource('photos.comments', PhotoCommentController::class);

// Partial resource
Route::resource('photos', PhotoController::class)->only([
    'index', 'show'
]);

Route::resource('photos', PhotoController::class)->except([
    'create', 'store', 'update', 'destroy'
]);

// URL with route name
$url = route('profile', ['id' => 1]);

// Generating Redirects...
return redirect()->route('profile');

// Route Prefix group
Route::prefix('admin')->group(function () {
    Route::get('/users', function () {
        // Matches The "/admin/users" URL
    });
});

// Route model binding
use App\Models\User;
Route::get('/users/{user}', function (User $user) {
    return $user->email;
});

// Route model binding (other than id)
use App\Models\User;
Route::get('/posts/{post:slug}', function (Post $post) {
    return view('post', ['post' => $post]);
});

// Route fallback
Route::fallback(function () {
    //
});

```

Cache

```php
// Route Caching
php artisan route:cache

// Retrieve & Store (key, duration, content)
$users = Cache::remember('users', now()->addMinutes(5), function () {
    return DB::table('users')->get();
});

```

Controllers

```php
// Set validation rules
protected $rules = [
    'title' => 'required|unique:posts|max:255',
    'name' => 'required|min:6',
    'email' => 'required|email',
    'publish_at' => 'nullable|date',
];

// Validate
$validatedData = $request->validate($rules)

// Show 404 error page
abort(404, 'Sorry, Post not found')

// Controller CRUD exemple
Class ProductsController
{

   public function index()
   {
       $products = Product::all();

       // app/resources/views/products/index.blade.php
       return view('products.index', ['products', $products]);
   }

   public function create()
   {
       return view('products.create');
   }

   public function store()
   {
       Product::create(request()->validate([
           'name' => 'required',
           'price' => 'required',
           'note' => 'nullable'
       ]));

       return redirect(route('products.index'));
   }

   //method with model injection
   public function show(Product $product)
   {
       return view('products.show', ['product', $product]);
   }

   public function edit(Product $product)
   {
       return view('products.edit', ['product', $product]);
   }

   public function update(Product $product)
   {
       Product::update(request()->validate([
           'name' => 'required',
           'price' => 'required',
           'note' => 'nullable'
       ]));

       return redirect(route($product->path()));
   }

   public function delete(Product $product)
   {
        $product->delete();
        return redirect("/contacts");
   }
}

// Query Params www.demo.html?name=mike
request()->name //mike

// Form data (or default value)
request()->input('email', 'no@email.com')

```

Template

```
<!-- Route name -->
<a href="{{ route('routeName.show', $id) }}">

<!-- Template inheritance -->
@yield('content')  <!-- layout.blade.php -->
@extends('layout')
@section('content') … @endsection

<!-- Template include -->
@include('view.name', ['name' => 'John'])

<!-- Template variable -->
{{ var_name }}

<!-- raw safe html variable -->
{ !! var_name !! }

<!-- Interation -->
@foreach ($items as $item)
   {{ $item.name }}
   @if($loop->last)
       $loop->index
   @endif
@endforeach

<!-- Conditional -->
@if ($post->id === 1)
    'Post one'
@elseif ($post->id === 2)
    'Post two!'
@else
    'Other'
@endif

<!-- Form -->
<form method="POST" action="{{ route('posts.store') }}">

@method(‘PUT’)
@csrf

<!-- Request path match -->
{{ request()->is('posts*') ? 'current page' : 'not current page' }}

<!-- Route exist? -->
@if (Route::has('login'))

<!-- Auth blade variable -->
@auth
@endauth
@guest

<!-- Current user -->
{{ Auth::user()->name }}

<!-- Validations errors -->
@if ($errors->any())
    <div class="alert alert-danger">
        <ul>
            @foreach ($errors->all() as $error)
                <li>{{ $error }}</li>
            @endforeach
        </ul>
    </div>
@endif

<!-- Check a specific attributes -->
<input id="title" type="text" class="@error('title') is-invalid @enderror">

<!-- Repopulate form with data from previous request -->
{{ old('name') }}

```

Database direct access no model

```php
use Illuminate\Support\Facades\DB;
$user = DB::table('users')->first();
$users = DB::select('select name, email from users');
DB::insert('insert into users (name, email, password) value(?, ?, ?)', ['Mike', 'mike@hey.com', 'pass123']);
DB::update('update users set name = ? where id = 1', ['eric']);
DB::delete('delete from users where id = 1');

```

Helpers

```php
// Display variable content and kill execution
dd($products)

// Create a Laravel collection from array.
$collection = collect($array);

// Sort by description ascending
$ordered_collection = $collection->orderBy(‘description’);

// Reset numbering value
$ordered_collection = $ordered_collection->values()->all();

// Return project full Path
app\ : app_path();
resources\ : resource_path();
database\ :database_path();

```

Flash & Session

```php
// Flash (only next request)
$request->session()->flash('status', 'Task was successful!');

// Flash with redirect
return redirect('/home')->with('success' => 'email sent!');

// Set Session
$request->session()->put('key', 'value');

// Get session
$value = session('key');
If session: if ($request->session()->has('users'))

// Delete session
$request->session()->forget('key');

// Display flash in template
@if (session('message')) {{ session('message') }} @endif

```

HTTP Client

```php
// Use package
use Illuminate\Support\Facades\Http;

//Http get
$response = Http::get('www.thecat.com')
$data = $response->json()

// Http get with query
$res = Http::get('www.thecat.com', ['param1', 'param2'])

// Http post
$res = Http::post('http://test.com', ['name' => 'Steve','role' => 'Admin']);

// Bearer
$res = Http::withToken('123456789')->post('http://the.com', ['name' => 'Steve']);

// Headers
$res = Http::withHeaders(['type'=>'json'])->post('http://the.com', ['name' => 'Steve']);

```

Storage (helper class to store files locally or in the cloud)

```php
// Public config: Local storage/app/public
Storage::disk('public')->exists('file.jpg'))
// S3 config: storage: Amazon cloud ex:
Storage::disk('s3')->exists('file.jpg'))

// Make the Public config available from the web
php artisan storage:link

// Get or put file in the storage folder
use Illuminate\Support\Facades\Storage;
Storage::disk('public')->put('example.txt', 'Contents');
$contents = Storage::disk('public')->get('file.jpg');

// Access files by generating a url
$url = Storage::url('file.jpg');
// or by direct path to public config
<img src={{ asset('storage/image1.jpg') }}/>

// Delete file
Storage::delete('file.jpg');

// Trigger download
Storage::disk('public')->download('export.csv');

```

Install a project from github

```
$ git clone {project http address} projectName
$ cd projectName
$ composer install
$ cp .env.example .env
$ php artisan key:generate
$ php artisan migrate
$ npm install

```

Heroku deployment

```php
// Install Heroku on local machine (macOs)
$ brew tap heroku/brew && brew install heroku

// Login to heroku (create account if none)
$ heroku login

// Create Procile
$ touch Procfile

// Save in Profile
web: vendor/bin/heroku-php-apache2 public/

```

## Rest API (create a Rest API endpoint)

API Routes (All api routes will be prefix with 'api/')

```php
// routes/api.php
Route::get('products', [App\Http\Controllers\ProductsController::class, 'index']);
Route::get('products/{product}', [App\Http\Controllers\ProductsController::class, 'show']);
Route::post('products', [App\Http\Controllers\ProductsController::class, 'store']);

```

API Resource (Layer that sits between your models and the JSON responses)

```
$ php artisan make:resource ProductResource

```

API Resource definition file

```php
// app/resource/ProductResource.php
public function toArray($request)
    {
        return [
            'id' => $this->id,
            'name' => $this->name,
            'price' => $this->price,
            'custom' => 'This is a custom field',
        ];
    }

```

API Controller (Best practice is to place your API controllers inside app/Http/Controllers/Api/v1/)

```php
public function index() {
        //$products = Product::all();
        $products = Product::paginate(5);
        return ProductResource::collection($products);
    }

    public function show(Product $product) {
        return new ProductResource($product);
    }

    public function store(StoreProductRequest $request) {
        $product = Product::create($request->all());
        return new ProductResource($product);
    }

```

## API Token authentication

First you need to create a Token for a specific user.

```php
$user = User::first();
$user->createToken('dev token');
// plainTextToken: "1|v39On3Uvwl0yA4vex0f9SgOk3pVdLECDk4Edi4OJ"

```

You can then use this token is a request

```
GET api/products (Auth Bearer Token: plainTextToken)

```

Authorization rules

You can create a token with pre-define auth rules

```jsx
$user->createToken('dev token', ['product-list']);

// in controllers
if !auth()->user()->tokenCan('product-list') {
    abort(403, "Unauthorized");
}
```

## [Back to Top](#laravel-cheatsheet)

# Migrations dump

# MIGRATIONS

criar nova table migration: `php artisan make:migration create_table_posts --create=posts`

se for necessario que seja executado primeiro (por causa das relacoes das tableas) alteramos nome do ficheiro

```
2014_10_21_000000_create_users_table.php ->  2011_10_21_000000_create_users_table.php

```

`php artisan migrate:fresh --seed`

`php artisan migrate:fresh --seed`

https://laravel.com/docs/7.x/migrations

https://laravel.com/docs/7.x/seeding

Criar seeds de table para users e posts

`php artisan make:seeder UsersTableSeeder`

`php artisan make:seeder PostsTableSeeder`

https://github.com/fzaninotto/Faker#formatters

`php artisan make:factory PostFactory`

`php artisan make:model Post`

`php artisan make:model Todo -a` -> modulo Todo, Controller, Migration, Seeds, Factory plural in tables and singular #Comando Principal `php artisan make:model Todo -all` -> `php artisan make:model Todo -help`

Como criar entries na base de dados em seeder ou factory:

primeiro criamos as factories, migrations e seeds com o comando `php artisan make:model Nome -a`

# Criar Model, Migration, Factory, Seeder e Controller de uma só vez:

`php artisan make:model Singular -a`

## Exemplo de MIGRATION:

```
public function up()
    {
        Schema::create('cars', function (Blueprint $table) {
                $table->id();
                $table->string('registration');
                $table->foreignId('brand_id')->constrained();
                $table->foreignId('owner_id')->constrained();
                $table->year('year_of_manufacturing');
                $table->string('color');
                $table->timestamps();
        });
}

```

## Exemplo de FACTORY:

```
!!-> Não esquecer de fazer os Imports

use App\Brand;
use App\Owner;

$factory->define(Car::class, function (Faker $faker) {

    $brands = Brand::all();

        $owners = Owner::all();

    return [
            'registration' =>$faker->swiftBicNumber,
            'brand_id' =>$brands[rand(0, count($brands)-1)]->id,     >MUITA ATENÇÃO
            'owner_id' =>$owners[rand(0, count($owners)-1)]->id,
            'year_of_manufacturing' =>$faker->year,
            'color' =>$faker->colorName
        ];
});

```

!!!!!-> No caso de querermos inserir um valor decimal por exemplo (wallet_money):

```
  No Factory:

	'wallet_money' =>$faker->randomFloat($nbMaxDecimals = NULL, $min = 0, $max = 1000) // 48.8932

		Se quisermos que um valor seja unico:

			EX:   'email' => $faker->unique()->safeEmail,

```

## Exemplo de SEED:

```
	!!! Atenção se pertender-mos fazer um seed de um numero especifico de algo para um user (por exemplo: Um user tem de ter 2 bicicletas) usamos:

			for ($i = 1; $i <= 50; $i++) {
				factory(\App\Bicycle::class, 2)->create(['user_id' => $i]);
			}

	use App\Car;

	public function run()
	{
    		factory(Car::class,115)->create();
		}

!!!Atenção ao ficheiro DatabaseSeeder.php

	->Por ordem

		EX:    $this->call(CarSeeder::class);

```

!!!-> No caso de querermos inserir um numero limitado de Paises e não usar o faker:

```
Atenção o processo NÃO É feito na Factory mas sim na Seed:

	!-> Factory fica em Branco!

	Exemplo:

	DB::table('countries')->insert(['name' => 'Portugal','created_at'=>now(),'updated_at'=>now()]);
   		DB::table('countries')->insert(['name' => 'Espanha','created_at'=>now(),'updated_at'=>now()]);
    	DB::table('countries')->insert(['name' => 'França','created_at'=>now(),'updated_at'=>now()]);
    	DB::table('countries')->insert(['name' => 'Polónia','created_at'=>now(),'updated_at'=>now()]);

```

## Criar Base de Dados

```
Relações:

	->  1 para N (hasMany)

	->  N para 1 (belongsTo)

	-> Ir ao ficheiro ( .env ) e "linkar" à base de dados

```

# Para fazer os Seeds à Base de dados

```jsx
	-> php artisan migrate:fresh --seed

		!!!-> Muito importante tem em atenção o Ficheiro DataBaseSeeder, temos que inserir lá
			e por ordem os nossos Seeders!
	

		php artisan migrate:fresh --seed
```

## [Back to Top](#laravel-cheatsheet)