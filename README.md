# Introdução ao AdonisJS

O AdonisJS é um framework Node.js que oferece uma estrutura pronta e pré-configurada para o desenvolvimento de aplicativos web, incluindo recursos como autenticação, upload de arquivos, validação de dados, envio de e-mails e comunicação com banco de dados. Ele segue a filosofia de escrever menos código para produzir mais, assim como outros frameworks populares, como Laravel, Django e Ruby on Rails.

## Alguns Pacotes Adonis

- npm i @adonisjs/auth: para recursos de autenticação.
- npm i @adonisjs/mail: para recursos de envio de e-mails.
- npm i @adonisjs/lucid: para recursos de comunicação com banco de dados. O Lucid é um ORM que oferece suporte a diversos bancos de dados.

## Arquitetura Adonis

O AdonisJS adota a arquitetura MVC (Model-View-Controller) para organizar o código de suas aplicações:

- Model (M): Camada responsável pela manipulação dos dados. Aqui, você define como os dados são armazenados e recuperados.

- View (V): Camada de interação com o usuário. Ela lida com a apresentação e a interface do seu aplicativo.

- Controller (C): Camada controladora que contém a lógica de negócios da aplicação. Ela faz a ponte entre o Model e a View.

O AdonisJS permite que você trabalhe tanto na parte frontend quanto na parte backend de sua aplicação. Isso pode levar a um acoplamento maior entre as camadas, mas também oferece flexibilidade para escolher as melhores ferramentas para cada parte do projeto.

Um cenário comum é usar o Vue.js ou o Nuxt.js como frontend (View) e o AdonisJS como backend (Model e Controller) para criar uma aplicação web desacoplada.

### Fluxo de Dados

O fluxo de dados em um aplicativo AdonisJS típico funciona da seguinte maneira:

1. O usuário interage com a interface do frontend (View).

2. O frontend faz uma requisição HTTP para a API desenvolvida com o AdonisJS (backend).
3. No backend, a requisição chega primeiro ao Controller, onde são aplicadas regras de negócios.

4. O Controller se comunica com o Model para buscar, atualizar, criar ou excluir dados no banco de dados.

5. O Model realiza as operações no banco de dados e retorna os resultados para o Controller.

6. O Controller envia a resposta de volta para a camada View, que a apresenta ao usuário.

### Criar um Projeto AdonisJS

Para criar um novo projeto AdonisJS, você pode usar o seguinte comando:

```bash
npx create-adonis-ts-app nomeProjeto
```

### CLI Adonis

O AdonisJS possui um CLI (Command Line Interface) interno que simplifica o desenvolvimento e a execução de tarefas comuns. Você pode listar todos os comandos disponíveis executando node ace -h.

Aqui estão alguns comandos úteis:

`node ace serve --watch`: Inicia o servidor da aplicação e monitora os arquivos para recarregar automaticamente quando houver alterações.

`node ace make:controller nomeController`: Cria um novo controller com o nome especificado.

`node ace make:model nomeModel`: Cria um novo model com o nome especificado.

### Primeira API com AdonisJS7

Para criar sua primeira API com AdonisJS, você pode seguir os seguintes passos:

1. Crie um controller usando o comando node ace make:controller NomeController.

2. Dentro do controller criado, defina um método, por exemplo, index(), que retornará os dados que você deseja expor pela API.

3. Crie uma rota para a API no arquivo start/routes.ts. Por exemplo:

```javascript
Route.get('/api/rotaDesejada', 'NomeController.index')
```

Agora, quando você acessar /api/rotaDesejada, a rota chamará o método index() do controller e retornará os dados correspondentes.
