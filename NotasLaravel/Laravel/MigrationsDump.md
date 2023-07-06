# Migrations - dump

Isto é um dump de codigo que pode ser util para criar migrations

```php
# MIGRATIONS

criar nova table migration: `php artisan make:migration create_table_posts --create=posts`

se for necessario que seja executado primeiro (por causa das relacoes das tableas) alteramos nome do ficheiro

```
2014_10_21_000000_create_users_table.php ->  2011_10_21_000000_create_users_table.php

```

`php artisan migrate:fresh --seed`

`php artisan migrate:fresh --seed`

[https://laravel.com/docs/7.x/migrations](https://laravel.com/docs/7.x/migrations)

[https://laravel.com/docs/7.x/seeding](https://laravel.com/docs/7.x/seeding)

Criar seeds de table para users e posts

`php artisan make:seeder UsersTableSeeder`

`php artisan make:seeder PostsTableSeeder`

[https://github.com/fzaninotto/Faker#formatters](https://github.com/fzaninotto/Faker#formatters)

`php artisan make:factory PostFactory`

`php artisan make:model Post`

`php artisan make:model Todo -a` -> modulo Todo, Controller, Migration, Seeds, Factory plural in tables and singular #Comando Principal `php artisan make:model Todo -all` -> `php artisan make:model Todo -help`

Como criar entries na base de dados em seeder ou factory:

primeiro criamos as factories, migrations e seeds com o comando `php artisan make:model Nome -a`

# Criar Model, Migration, Factory, Seeder e Controller de uma só vez:

`php artisan make:model Singular -a`

### Exemplo de MIGRATION:

```php
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

### Exemplo de FACTORY:

```php
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

```

!!!!!-> No caso de querermos inserir um valor decimal por exemplo (wallet_money):

```php
  No Factory:

	'wallet_money' =>$faker->randomFloat($nbMaxDecimals = NULL, $min = 0, $max = 1000) // 48.8932

		Se quisermos que um valor seja unico:

			EX:   'email' => $faker->unique()->safeEmail,

```

### Exemplo de SEED:

```php
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

```php
Atenção o processo NÃO É feito na Factory mas sim na Seed:

	!-> Factory fica em Branco!

	Exemplo:

	DB::table('countries')->insert(['name' => 'Portugal','created_at'=>now(),'updated_at'=>now()]);
   		DB::table('countries')->insert(['name' => 'Espanha','created_at'=>now(),'updated_at'=>now()]);
    	DB::table('countries')->insert(['name' => 'França','created_at'=>now(),'updated_at'=>now()]);
    	DB::table('countries')->insert(['name' => 'Polónia','created_at'=>now(),'updated_at'=>now()]);

```

## Criar Base de Dados

```php
Relações:

	->  1 para N (hasMany)

	->  N para 1 (belongsTo)

	-> Ir ao ficheiro ( .env ) e "linkar" à base de dados

```

# Para fazer os Seeds à Base de dados

```php
	-> php artisan migrate:fresh --seed

		!!!-> Muito importante tem em atenção o Ficheiro DataBaseSeeder, temos que inserir lá
			e por ordem os nossos Seeders!
	

		php artisan migrate:fresh --seed
```

## Paginas
[Pagina Inicial](../NotasLaravel.md)\
[Iniciar novo Projeto](IniciarNovoProjeto.md)\
[Criar Factory, Seeder e Migration](CriarFactorySeederMigration.md)\
[Criar rota, controller e views](CriarRotaControllerViews.md)\
[My beloved Laravel cheat sheet](MyBelovedLaravelCheatSheet.md)\
[da goo shiiieee](dagooshiiieee.md)