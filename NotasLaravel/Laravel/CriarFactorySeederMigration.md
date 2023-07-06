# Criar Factory, Seeder e Migration

Criado em: May 25, 2023 7:57 PM

## 1 - Criar o Model

Ao corrermos este comando cria ja a migration, seeder e factory, para comandos individuais verificar a cheatsheet

```php
php artisan make:model Player -a

//!!! SEMPRE NO SINGULAR
//USER / COUNTRY / TODO (se for no plural vai dar erro em todo o programa)
```

## 2 - Estruturar as tabelas

De seguida vamos as Database/Migrations e declaramos os campos que as tabelas irão ter e as foreign keys que pretendemos.

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

// quando estamos a fazer foreign keys com o tipo Id na outra table tem 
// de ser unsignedBigInteger 
// se nao gera erro que a foreign key esta mal estruturada
```

## 3 - Criar relações das Tabelas

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

## 4 - Criar Factory

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

[Tipos de Dados no Faker](Criar%20Factory,%20Seeder%20e%20Migration%20388fe76b238b4c38936c1c6b339597f6/Tipos%20de%20Dados%20no%20Faker%20518bb9c1e70d4175bb47e5bdd80a29b1.md)

Por cada campo na tabela, preenchemos com data usando o gerador de fake data faker

## 5 - Seeder

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
use Illuminate\Support\Facades\DB;

    \DB::table('pets')->insert([
        'columnName1' => 'Pet 1',
        'columnName2' => '2000-1-5',
        'columnName3' => 'brown',
        'person_id' => 1,
    ]);
```

## 6 - Inicializar os Seeders

E por fim vamos ao ficheiro DatabaseSeeder:

```php
public function run()
    {
        $this->call(CountrySeeder::class);
        $this->call(PlayerSeeder::class);
        // $this->call(UserSeeder::class);
    }
```

no fim é só correr o comando `php artisan migrate:fresh —seed`