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



# Comandos para Laravel:
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
- [ ] Instalar Laravel na Pasta
- [ ] Iniciar um Projeto dentro da Pasta
- [ ] Instalar Autenticação e Boostrap
- [ ] Instalar npm e dar run
- [ ] Editar ficheiro .env (acesso BD)
- [ ] Migrar BD para phpMyAdmin
- [ ] Iniciar/Reiniciar Web Server