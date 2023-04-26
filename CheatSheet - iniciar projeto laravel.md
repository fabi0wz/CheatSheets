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
- [ ] Instalar Laravel
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
