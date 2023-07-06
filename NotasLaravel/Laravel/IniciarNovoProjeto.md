# Iniciar novo Projeto

Criado em: May 25, 2023 8:24 PM

## **1 - Instalação: (EXECUTAR NO CMD)**

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

***Iniciar Apache e MySql no XAMPP***

e criar a database que vamos utilizar por exemplo ATEC

### linha 9 a 14

`DB_DATABASE=laravel` alterar para `DB_DATABASE=ATEC`

> nome da base de dados que criarmos ou estejamos a utilizar no phpmyadmin
> 

`DB_USERNAME=root` alterar para `DB_USERNAME=root`

> nome do user que tem acesso a BD default é root
> 

`DB_PASSWORD=root` alterar para `DB_PASSWORD=`

> e password do mesmo default é vazio
> 

**6 - Migrar BD para phpmyadmin - Editar ficheiro de acesso a BD**

---

Passa a estrutura das tabelas para o phpmyadmin (verificar no php my admin -> designer)

`php artisan migrate`****

**7 - Iniciar um WebServer - > XAMPP**

---

`php artisan serve`

> se alguma alteracao for feita aos ficheiros é necessário reiniciar o web server
> 

> CTRL + C e voltamos a correr o comando
> 
> 
> `php artisan serve`
>

## Paginas
[Pagina Inicial](../NotasLaravel.md)\
[Criar Factory, Seeder e Migration](CriarFactorySeederMigration.md)\
[Criar rota, controller e views](CriarRotaControllerViews.md)\
[My beloved Laravel cheat sheet](MyBelovedLaravelCheatSheet.md)\
[Migrations - dump](MigrationsDump.md)
[da goo shiiieee](dagooshiiieee.md)