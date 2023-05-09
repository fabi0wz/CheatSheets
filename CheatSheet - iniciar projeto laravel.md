## Instalar:

XAMPP\
Composer (Version 2.5.5)\
Node (v16.19.1)\
(estamos a usar a versao 7.4.33 de php)


# Commands cmd para verificar as instalações:

``php -v``
``Composer``
``Node -v``


Correr ficheiros de PHP na consola:
Comandos: \
``php index.php``



# Comandos para iniciar um projeto Laravel:
## 1 - Instalação: (EXECUTAR NO CMD)

``composer global require laravel/installer``

## 2 - Iniciar um Projeto
``Composer create-project --prefer-dist laravel/laravel:^7.0 blog``

>blog vai ser o nome do projeto
7.0 especificamos a versão do laravel

de seguida podemos fazer CD para o projeto ou abrir a pasta no terminal no IDE

## 3 - Instalar autenticação e bootstrap:
    todos os passos daqui para a frente 
    tem de ser realizado dentro da pasta do projecto:


``composer require laravel/ui:^2.4``\
``php artisan ui vue --auth``

## 4 - Instalar npm
``npm install && npm run dev``  (linux / mac)

``npm install; npm run dev``  (windows)

``npm install``\
``npm run dev``
    
>podemos correr os dois em separado para evitar erros

## 5 - Editar ficheiro de acesso a BD

No ficheiro .env é onde defenimos as informacoes das bases de dados

***Iniciar Apache e MySql no XAMPP*** 
### linha 9 a 14

``DB_DATABASE=laravel`` alterar para ``DB_DATABASE=ATEC`` 

>nome da base de dados que criarmos
ou estejamos a utilizar no phpmyadmin

``DB_USERNAME=root`` alterar para ``DB_USERNAME=root``

>nome do user que tem acesso a BD
default é root

``DB_PASSWORD=root`` alterar para ``DB_PASSWORD=``

>e password do mesmo
default é vazio


## 6 - Migrar BD para phpmyadmin
Passa a estrutura das tabelas para o phpmyadmin
(verificar no php my admin -> designer)\
``php artisan migrate``

## 7 - Iniciar um WebServer - > XAMPP
``php artisan serve``

>se alguma alteracao for feita aos ficheiros 
é necessário reiniciar o web server

>CTRL + C e voltamos a correr o comando\
``php artisan serve``

# PASSOS:
- [] Instalar Laravel
- [ ] Iniciar um Projeto dentro da Pasta
- [ ] Instalar Autenticação e Boostrap
- [ ] Instalar npm e dar run
- [ ] Editar ficheiro .env (acesso BD)
- [ ] Migrar BD para phpMyAdmin
- [ ] Iniciar/Reiniciar Web Server

# Coomeçar trocar nomeasdasd

## 1 - Criar controlador
`php artisan make:controller HelloWorldController`

>HelloWorldController é o nome do controlador que criamos

Verificar: **Dentro do projeto:** app\Http\Controllers

## 2 - Dentro do controlador adicionar nossos metodos

criamos variavel com um texto e enviamos para o controlador

```
public function index()
    {
        $helloWorld = 'Hello World!';
        return view('hello_world.index', ['helloWorld' => $helloWorld]);
    }
```
>hello_world.index -> hello_world é o nome da pasta . (concatenar) index, por exemplo se estivesse na raiz era so 'index'

podemos retornar varias variaveis ao html

```
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
```

## 3 - Criar ficheiro index

ir a resources\views e adicionar o ficheiro ``index.blade.php``

dentro do ficheiro

`<h1>{{ $helloWorld }}</h1>`

    <h1>{{ $helloWorld }}</h1>
    
    *same as*

    <h1>
        <?php echo $helloWorld?>
    </h1>

    {{ $variavel }} é o equivalente a fazer echo de uma variavel


## 4 - Registar uma Rota

No web.php (ficheiro das rotas):

``Route::get('/hello-world', 'HelloWorldController@index');``

>primeiro parametro é o nome da rota\
>
>---
>segundo parametro é o nome do controller e o metodo que vai ser executado\
>
>---
>@separa o parametro do metodo






# Paths nos projetos
- Controllers -> ``\app\Http\Controllers``
- Views -> ``\resources\views``
- Routes -> ``\routes\web.php``

**tree view**

    ├── app
    │   └── Http
    │       └── Controllers
    ├── Resources
    │   └── Views
    ├── routes
    │   └── web.php



# Juntar HTML e importa-lo para o ficheiro main

primeiro criar uma pasta master com os ficheiros:

- footer.blade.php
- header.blade.php
- main.blade.php

no header colocamos o que estará presente em todas as paginas do nosso website, por exemplo a navbar e o mesmo para o footer

no main podemos colocar por exemplo

```
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

depois podemos criar uma pasta components e la dentro por partes de codigo html que vao ser repetidos, por exemplo criamos um card no ficheiro card.blade.php e no index.blade.php só importamos assim:

```
@section('content')
    <div class="container">
        <div class="row">
            <div class="col-md-12">
                @component('components.cards')
                @endcomponent
            </div>
        </div>
    </div>
@stop
```

@component('nomedocomponente')\
@endcomponent


no ficheiro cards:
```
@component('components.card.card', [
    'title'  => 'Title teste',
    'description' => 'description',
    'image' => 'https://imageggegem'
])
```


no card, substituir texto e links por variaveis:\
 {{$variavel}}


 #MIGRATIONS

criar nova table migration:
`php artisan make:migration create_table_posts --create=posts`


 se for necessario que seja executado primeiro (por causa das relacoes das tableas) alteramos nome do ficheiro

    2014_10_21_000000_create_users_table.php ->  2011_10_21_000000_create_users_table.php

`php artisan migrate:fresh --seed`

`php artisan migrate:fresh --seed`

https://laravel.com/docs/7.x/migrations

https://laravel.com/docs/7.x/seeding


Criar seeds de table para users e posts\
`php artisan make:seeder UsersTableSeeder`\
`php artisan make:seeder PostsTableSeeder`

https://github.com/fzaninotto/Faker#formatters

`php artisan make:factory PostFactory`

`php artisan make:model Post`



`php artisan make:model Todo -a` -> modulo Todo, Controller, Migration, Seeds, Factory plural in tables and singular #Comando Principal
`php artisan make:model Todo -all` -> 
`php artisan make:model Todo -help`




Como criar entries na base de dados em seeder ou factory:

primeiro criamos as factories, migrations e seeds com o comando
`php artisan make:model Nome -a`

# Criar Model, Migration, Factory, Seeder e Controller de uma só vez:


`php artisan make:model Singular -a`


### Exemplo de MIGRATION: 

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
			


### Exemplo de FACTORY: 	


    !!-> Não esquecer de fazer os Imports

    
    use App\Brand;
    use App\Owner;
    
    $factory->define(Car::class, function (Faker $faker) {

        $brands = Brand::all();

            $owners = Owner::all();


        return [
                'registration' =>$faker->swiftBicNumber,
                'brand_id' =>$brands[rand(0, count($brands)-1)]->id,      --->MUITA ATENÇÃO
                'owner_id' =>$owners[rand(0, count($owners)-1)]->id,
                'year_of_manufacturing' =>$faker->year,
                'color' =>$faker->colorName          
            ];
    });

		
 !!!!!-> No caso de querermos inserir um valor decimal por exemplo (wallet_money):

	  No Factory:

		'wallet_money' =>$faker->randomFloat($nbMaxDecimals = NULL, $min = 0, $max = 1000) // 48.8932

		
			Se quisermos que um valor seja unico:


				EX:   'email' => $faker->unique()->safeEmail,
	



### Exemplo de SEED:


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




  !!!-> No caso de querermos inserir um numero limitado de Paises e não usar o faker:

	Atenção o processo NÃO É feito na Factory mas sim na Seed:

		!-> Factory fica em Branco!

		Exemplo:    
							
		DB::table('countries')->insert(['name' => 'Portugal','created_at'=>now(),'updated_at'=>now()]);
       		DB::table('countries')->insert(['name' => 'Espanha','created_at'=>now(),'updated_at'=>now()]);
        	DB::table('countries')->insert(['name' => 'França','created_at'=>now(),'updated_at'=>now()]);
        	DB::table('countries')->insert(['name' => 'Polónia','created_at'=>now(),'updated_at'=>now()]);

## Criar Base de Dados


	Relações:

		
		->  1 para N (hasMany)

		->  N para 1 (belongsTo)

		-> Ir ao ficheiro ( .env ) e "linkar" à base de dados

# Para fazer os Seeds à Base de dados

		-> php artisan migrate:fresh --seed


			!!!-> Muito importante tem em atenção o Ficheiro DataBaseSeeder, temos que inserir lá
				e por ordem os nossos Seeders!
		

	
			php artisan migrate:fresh --seed
