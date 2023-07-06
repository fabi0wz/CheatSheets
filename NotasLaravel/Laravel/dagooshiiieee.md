# da goo shiiieee

## **1 - Instalação: (EXECUTAR NO CMD) (só é preciso executar se o laravel nao estiver instalado no PC)**

---

`composer global require laravel/installer`****

**2 - Iniciar um Projeto**

---

`composer create-project --prefer-dist laravel/laravel:^7.0` teste

**3 - Instalar autenticação e bootstrap:**

---

> todos os passos daqui para a frente
tem de ser realizado dentro da pasta do projecto:
> 

`composer require laravel/ui:^2.4`

`php artisan ui vue --auth`****

esta a usar versao 4.6 do bootstrap

**4 - Instalar npm**

---

`npm install && npm run dev` (linux / mac)

(windows)

`npm install`

`npm run dev`

> podemos correr os dois em separado para evitar erros****
> 

**5 - Editar ficheiro de acesso a BD**

---

No ficheiro `.env` é onde defenimos as informações das bases de dados

alterar  o `DB_DATABASE=NomedaNossaDB`

***Iniciar Apache e MySql no XAMPP***

e criar a database que vamos utilizar por exemplo ATEC

---

## 1 - Criar os models controllers e migrations

no cmd

```bash
php artisan make:model Project -a
php artisan make:model Category -a
php artisan make:model Product -a
```

## 2 - Estruturar as tabelas

De seguida vamos as Database/Migrations e declaramos os campos que as tabelas irão ter e as foreign keys que pretendemos. (verificar os campos e corrigir/adicionar novos)

```php
//projects table

public function up()
    {
        Schema::create('projects', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('location');
            $table->timestamps();
        });
    }
```

```php
//categories table

public function up()
    {
        Schema::create('categories', function (Blueprint $table) {
            $table->id();
            $table->string('type');
            $table->timestamps();
        });
    }
```

```php
//products table

public function up()
    {
        Schema::create('products', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('description');
            $table->unsignedBigInteger('category_id');
            $table->unsignedBigInteger('project_id');
            $table->timestamps();
            
            $table->foreign('category_id')->references('id')->on('categories');
            $table->foreign('project_id')->references('id')->on('projects');
        });
    }
```

## 3 - Criar relações das Tabelas

Produtos N-1 Projetos

Produtos N-1 Categorias

```php
//no ficheiro Category.php
class Category extends Model
{
    public function products()
    {
        return $this->hasMany(Product::class);
    }
}
```

```php
//no ficheiro Project.php
class Project extends Model
{
    public function product()
    {
        return $this->hasMany(Product::class);
    }
}
```

```php
//no ficheiro Product.php
class Product extends Model
{
    public function project()
    {
        return $this->belongsTo(Project::class);
    }

    public function category()
    {
        return $this->belongsTo(Category::class);
    }
}
```

## 4 - Criar Seeder Categories

Depois podemos ir ao Database/seeds/CategorySeeder e fazer algo do tipo:

```php
public function run()
    {
        DB::Table('categories')->insert([
            'type' => 'Quarto',
        ]);
        DB::Table('categories')->insert([
            'type' => 'Sala',
        ]);
        DB::Table('categories')->insert([
            'type' => 'Cozinha',
        ]);
        DB::Table('categories')->insert([
            'type' => 'Casa de Banho',
        ]);
    }
```

## 5 - Factory para gerar 100 Produtos

Depois temos de ir ao Database/Seeder/PlayerSeeder e chamar a factory:

```php
//ficheiro ProductFactory
$factory->define(Product::class, function (Faker $faker) {
    return [
        'name' => $faker->unique()->firstName,
        'description' => $faker->sentence,
        'category_id' => $faker->numberBetween(1, 4),
    ];
});
```

```php
//ficheiro ProductSeeder.php
public function run()
    {
        $projID = 1;
        for ($i = 0; $i<20; $i++) //aqui ja estamos a associar 5 produtos a projeto
        {
            factory(App\Product::class, 5)->create([
                'project_id' => $projID,
            ]);
            $projID++;
        }
    }
```

## 6 - Factory para gerar 20 projetos (cada com 5 produtos)

E por fim vamos ao ficheiro DatabaseSeeder:

```php
//no ficheiro ProjectFactory
//ja associamos 5 produtos no ficheiro anterior
$factory->define(Project::class, function (Faker $faker) {
    return [
        'name' => $faker->firstName,
        'location' => $faker->country,
    ];
});
```

```php
//no ficheiro ProjectSeeder.php

public function run()
    {
        factory(App\Product::class, 20)->create();
    }
```

## 7 - Inicializar os Seeders no ficheiro DatabaseSeeder

```php
//DatabaseSeeder.php

public function run()
    {
        $this->call(ProjectSeeder::class);
        $this->call(CategorySeeder::class);
        $this->call(ProductSeeder::class);
    }
```

no fim é só correr o comando `php artisan migrate:fresh —seed`

---

# 1 - No ficheiro `routes\web.php` as rotas

```php
Route::get('/projects', 'ProjectController@index')->name('project');
Route::get('/products', 'ProductController@index')->name('product');
Route::get('/products/{product}', 'ProductController@list')->name('category');
```

## 2 - No Controllers dos nossos projects e products criamos o return da view e os nossos metodos

```php
//ProjectController
public function index()
    {
        $projects = Project::all();
        return response(view('projects', [
            'projects' => $projects,
        ]));
    }
```

```php
//ProductController
public function index()
{
    $products = Product::all();
    return response(view('products', [
        'products' => $products,
    ]));
}
public function list($id)
{
		$product = Product::Find($id);
		return response(view('category', [
		'product'=> $product,
    ]));
}
```

## 3 - Criamos dentro das views criamos a pasta master

com os ficheiros 

```html
{{-- footer.blade.php --}}

<footer>
    <p align="center">This is a footer</p>
</footer>
```

```php
{{-- header.blade.php --}}

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
 {{-- main.blade.php --}}

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

## 4 - Na pasta views criamos os nossos ficheiros dos controllers:

```php
{{-- category.blade.php --}

@extends('master.main')

@section('styles')
@endsection

@section('scripts')
@endsection

@section('content')
    @component('components.categoriesTable', [
        'product' => $product,
    ])
    @endcomponent
@endsection
```

```php
{{-- projects.blade.php --}}

@extends('master.main')

@section('styles')
@endsection

@section('scripts')
@endsection

@section('content')
    @component('components.projectsTable', [
        'projects' => $projects,
    ])

    @endcomponent
@endsection
```

```php
{{-- products.blade.php --}}

@extends('master.main')

@section('styles')
@endsection

@section('scripts')
@endsection

@section('content')
    @component('components.productsTable', [
        'products' => $products,
    ])

    @endcomponent
@endsection
```

## 5 - Criamos a pasta views/components, onde colocamos os componentes individuais

Dentro das views agora podemos utilizar as variaveis que enviamos através do controller

```php
{{-- projectsTable.blade.php --}}

<table class="container table table-striped">
    {{-- cabeçalho --}}
    <thead>
    <tr>
        <th scope="col">id</th>
        <th scope="col">Name</th>
        <th scope="col">Location</th>

    </tr>
    </thead>
    {{-- body --}}
    <tbody>
    @foreach ($projects as $project)
        <tr>
            <th scope="row">{{ $project->id }}</th>
            <td>{{ $project->name }}</td>
            <td>{{ $project->location }}</td>
        </tr>
    @endforeach
    </tbody>
</table>

```

```php
{{-- ProductsTable.blade.php --}}

<table class="container table table-striped">
    {{-- cabeçalho --}}
    <thead>
    <tr>
        <th scope="col">id</th>
        <th scope="col">Name</th>
        <th scope="col">Description</th>
        <th scope="col">Category ID</th>
        <th scope="col">Project ID</th>
        <th scope="col">Product Information</th>

    </tr>
    </thead>
    {{-- body --}}
    <tbody>
    @foreach ($products as $product)
        <tr>
            <th scope="row">{{ $product->id }}</th>
            <td>{{ $product->name }}</td>
            <td>{{ $product->description }}</td>
            <td>{{ $product->category_id }}</td>
            <td>{{ $product->project_id }}</td>
            <td><a href="/products/{{ $product->id }}">Product Information</a>
            </td>
        </tr>
    @endforeach
    </tbody>
</table>
```

```php
{{-- CategoriesTable.blade.php --}}
<table class="container table table-striped">
    {{-- cabeçalho --}}
    <thead>
    <tr>
        <th scope="col">id</th>
        <th scope="col">Name</th>
        <th scope="col">Type</th>

    </tr>
    </thead>
    {{-- body --}}
    <tbody>
        <tr>
            <th scope="row">{{ $product->id }}</th>
            <td>{{ $product->name }}</td>
            <td>{{ $product->category->type }}</td>
        </tr>
    </tbody>
</table>
```

## Paginas
[Pagina Inicial](../NotasLaravel.md)\
[Iniciar novo Projeto](IniciarNovoProjeto.md)\
[Criar Factory, Seeder e Migration](CriarFactorySeederMigration.md)\
[Criar rota, controller e views](CriarRotaControllerViews.md)\
[My beloved Laravel cheat sheet](MyBelovedLaravelCheatSheet.md)\
[Migrations - dump](MigrationsDump.md)