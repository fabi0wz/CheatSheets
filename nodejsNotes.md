# iniciar projeto de vite com nodejs

`npm create vite@latest`

package.json é o ficheiro central do projeto e é onde tem toda a informação necessária para o projeto funcionar.

Um projeto NodeJS por padrão na raiz tem muitos ficheiros por isso é recomendado todos os ficheiros do nosso projeto serem colocados numa pasta src.

## Package.json

Propriedades importantes deste ficheiro:

***name***: Nome do Projeto\
***version: versão do projeto\
***main***: ponto de entrada da aplicação, por default é index.js mas podemos alterar para o que quisermos\
***scripts***: aqui podemos definir alguns comandos, como por exemplo para iniciar um servidor de desenvolvimento ou para corrigir a identação do nosso código\
***dependencies***: lista de dependências que a nossa aplicação precisa para funcionar.\
***devDependencies***: dependências de desenvolvimento como por exemplo typescript ou nodemon. São bibliotecas que nos vão poder ajudar como programadores mas não são usadas na lõgica da nossa aplicação.

## Comandos NPM

***npm install nome_modulo_npm***: Instalar uma dependêmncia como por exemplo o Lodash, Em vez de install podemos abreviar para i\
***npm install nome_modulo_npm -D***: o -D vai instalar a dependência como dependência de desenvolvimento\
***npm unistall nome_modulo_npm***: Remove uma dependência da nossa aplicação.\
***npm audit***: vai auditar os vários módulos da nossa aplicação e nos informar se há alguma vulnerabilidade conhecida\
***npm update***: atualizar os modulos\
***npm help***: mostra a documentação para os comandos do npm\
***npm help [comando]***: mostra a documentação para o comando do npm

## Modulos

Existem 3 fontes de modulos:

    1 - Modulos Próprios
        -apenas adicionamos o ficheiro no nosso projeto e importamos com o caminho incluindo o .js

    2 - Modulos do NodeJS / Core
        -apenas importamos pelo nome do módulo

    3 - Modulos de terceiros (Instalados pelo npm)
        -Instalar ao módulo com o npm
        -Importar como se fosse um módulo oferecido plo NodeJs

### Importar modulos nativamente vs commonJS(NodeJS)

ES Native Modules:\
`import { registerFormEvents} from "./components/contactForm.js"`

Common JS (NodeJs)\
`const fs = require('fs')`

## Componentes

Componentes são partes do nosso código que podem ser reutilizadas em vários locais da nossa aplicação.
Podem ser construídos com um ou mais elementos e também podem conter outros componentes.\
Idealmente os componentes funcionam de forma isolada e não contêm lógica de negócio.

Os componentes normalmente tem 3 partes e funcionam como se fossem pequenos sites. Já que apresentam sempre uma componente de HTML para definir a estrutura, CSS para definir os estilos e JS com a implementação das interações com o utilizador.

### Tipos de componentes

    1 -Stateless: não tem estado, ou seja, não tem lógica de negócio, apenas apresenta o conteúdo que lhe é passado
    
    2 -Stateful: tem estado, ou seja, tem lógica de negócio, por exemplo um formulário de login que tem de validar os dados introduzidos pelo utilizador

### The elements of Atomic Design

<https://xd.adobe.com/ideas/process/ui-design/atomic-design-principles-methodology-101/>

    1 - Atoms
        -elemento mais basico e pequeno do nosso sistema (buttons, inputs, labels e outros pequenos elementos que são reutilizados)

    2 - Molecules
        -são compostos por 2 ou mais átomos (ex: um input com um label) 
    
    3 - Organisms
        -são compostos por 2 ou mais moléculas (ex: um formulário com vários inputs e labels)

    4 - Templates
        -são compostos por 2 ou mais organismos (ex: um formulário de login com um formulário de registo)

    5 - Pages
        -são compostos por 2 ou mais templates (ex: a página de login que contém o formulário de login e o formulário de registo)


