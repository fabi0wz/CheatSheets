# Criar rota, controller e view

Criado em: June 16, 2023 8:49 PM

## 1 - No ficheiro `routes\web.php` registamos a rota para esta view

A route lida com o request do browser para aceder aquela pagina, verifica se está registada e se estiver chama o Controller a essa rota associado, que por si vai retornar a view

```php
Route::get('/players', 'PlayerController@index')->name('players');
```

## 2 - No Controller do nosso player criamos o return da view e os nossos metodos

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

Se por exemplo a tabela users tiver uma foreign key Country_id e quisermos consultar o nome do pais podemos fazer

`{{ $player->country->name }}`

vai a tabela players e ve o country id, e depois atraves desse country id vai ver o name na tabela dos countries

## Paginas
[Iniciar novo Projeto](IniciarNovoProjeto.md)\
[Criar Factory, Seeder e Migration](CriarFactorySeederMigration.md)\
[My beloved Laravel cheat sheet](MyBelovedLaravelCheatSheet.md)\
[Migrations - dump](MigrationsDump.md)\
[Da goo shiiieee](dagooshiiieee.md)