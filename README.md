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

## Manipulação de Banco de Dados com AdonisJS

O AdonisJS oferece suporte a duas abordagens para manipulação de bancos de dados: Query Builder e ORM (Object-Relational Mapping).

### Query Builder

O Query Builder é uma forma de construir consultas SQL de maneira programática por meio de métodos. Ele oferece uma maneira mais legível e segura de criar consultas em comparação com a escrita direta de SQL. Com o Query Builder, você pode encadear métodos para construir consultas complexas. Veja um exemplo de como criar uma consulta com Query Builder:

```javascript
const Database = use('Database')

const users = await Database.select('id', 'username').from('users').where('active', true)
```

### ORM (Object-Relational Mapping)

O AdonisJS também inclui um ORM chamado Lucid que mapeia registros do banco de dados para objetos JavaScript. Isso permite que você trabalhe com registros de banco de dados como se fossem objetos, tornando o código mais legível e expressivo. Aqui está como você pode usar o Lucid ORM:

Primeiro, instale o pacote Lucid com o comando `npm install @adonisjs/lucid`.

Em seguida, execute `node ace invoke @adonisjs/lucid` para selecionar o driver do banco de dados que você deseja usar e configurar as informações no arquivo .env.

Você deve criar um Model para cada tabela que deseja interagir. Use o comando node ace make:model NomeModel para criar um Model.

Agora, você pode usar o Model para realizar operações no banco de dados de forma mais fácil e expressiva. Aqui está um exemplo:

``` javascript
const Post = use('App/Models/Post')

const post = new Post()
post.title = 'Meu primeiro post'
post.content = 'Conteúdo do post'
await post.save()

// Consulta
const posts = await Post.query().where('published', true).fetch()

```

## Migrations

As Migrations são usadas para criar, atualizar e excluir tabelas no banco de dados de forma programática e controlada. Elas ajudam a manter o controle das mudanças no esquema do banco de dados ao longo do tempo.

Para criar uma Migration no AdonisJS, você pode usar o comando `node ace make:migration nomeMigration`. Isso criará um arquivo de Migration na pasta `database/migrations`.

Em seguida, você pode definir as colunas da tabela no método `up()` da Migration. Aqui está um exemplo de criação de uma tabela:

```javascript
public async up() {
  this.schema.createTable('posts', (table) => {
    table.increments('id')
    table.string('title')
    table.text('content', 'longtext')
    table.timestamp('created_at', { useTz: true })
    table.timestamp('updated_at', { useTz: true })
  })
}
```

Para reverter as mudanças feitas por uma Migration, você define o método `down()` correspondente. Por exemplo, para reverter a criação da tabela, você faria:

```javascript
public async down() {
  this.schema.dropTable('posts')
}
```

Para executar as Migrations, use o comando node ace migration:run. Para reverter a última Migration, use `node ace migration:rollback`.

### Atualização de Tabelas com Migrations

Para atualizar tabelas existentes, você pode criar uma nova Migration que adiciona ou remove colunas da tabela. Use o comando `node ace make:migration nomeMigration --table=nomeTabela` para criar uma Migration específica para uma tabela.

No método `up()` da Migration, você pode usar `table.alterTable()` para adicionar ou modificar colunas existentes. Aqui está um exemplo:

```javascript
public async `up()` {
  this.schema.alterTable('posts', (table) => {
    table.string('slug')
  })
}
```

No método `down()`, você define as ações reversas para reverter as alterações. Por exemplo:

```javascript
public async `down()` {
  this.schema.alterTable('posts', (table) => {
    table.dropColumn('slug')
  })
}
```

Lembre-se de executar as Migrations para aplicar as alterações ou reverter as ações, conforme necessário.

## Definindo Models no AdonisJS

No AdonisJS, os modelos são usados para representar as tabelas do banco de dados e definir a estrutura das colunas. Você pode criar um modelo com o seguinte 

comando:
```bash
node ace make:model NomeDoModelo
```

Aqui está um exemplo de como um modelo pode ser definido:

```javascript
import { DateTime } from 'luxon'
import { BaseModel, column } from '@ioc:Adonis/Lucid/Orm'

export default class Post extends BaseModel {
  @column({ isPrimary: true })
  public id: number

  @column()
  public title: string

  @column()
  public content: string

  @column.dateTime({ autoCreate: true })
  public createdAt: DateTime

  @column.dateTime({ autoCreate: true, autoUpdate: true })
  public updatedAt: DateTime
}
```

Neste exemplo, o modelo Post representa uma tabela com as colunas id, title, content, createdAt e updatedAt.

### Aplicando a Lógica do Controller (Operações CRUD)

O controlador (Controller) é usado para manipular as operações CRUD (Criar, Ler, Atualizar e Excluir) em seus modelos. Você pode criar um controlador com os métodos CRUD padrão usando o seguinte comando:

```bash
node ace make:controller NomeDoController -r
```

Isso criará um controlador com os métodos index, create, store, show, edit, update, e destroy já definidos. Você pode personalizá-los conforme necessário.

Aqui está um exemplo simplificado do controlador PostsController:

```javascript
import { HttpContextContract } from '@ioc:Adonis/Core/HttpContext'
import Post from 'App/Models/Post'

export default class PostsController {
  public async index({}: HttpContextContract) {
    const posts = await Post.all()
    return posts
  }

  public async store({ request }: HttpContextContract) {
    const data = request.only(['title', 'content'])
    const post = await Post.create(data)
    return post
  }

  public async show({ params }: HttpContextContract) {
    const post = await Post.findOrFail(params.id)
    return post
  }

  public async update({ request, params }: HttpContextContract) {
    const post = await Post.findOrFail(params.id)
    const data = request.only(['title', 'content'])

    post.merge(data)
    await post.save()

    return post
  }

  public async destroy({ params }: HttpContextContract) {
    const post = await Post.findOrFail(params.id)
    await post.delete()
  }
}
```

Neste exemplo, o PostsController contém os métodos CRUD padrão para o modelo Post. Isso permite que você liste todos os posts, crie um novo post, exiba um post específico, atualize um post existente e exclua um post.

### Query Builder no AdonisJS

O AdonisJS oferece suporte a consultas SQL usando Query Builder. Você pode importar o módulo Database e usar métodos fluentes para construir consultas SQL de maneira programática. Aqui está um exemplo de uso do Query Builder no controlador PostsController:

```javascript
import { HttpContextContract } from '@ioc:Adonis/Core/HttpContext'
import Database from '@ioc:Adonis/Lucid/Database'

export default class PostsController {
  public async show({ params }: HttpContextContract) {
    const post = await Database.rawQuery(`SELECT * FROM posts WHERE id = ${params.id}`)
    return post
  }
}
```

No exemplo acima, uma consulta SQL simples é executada usando o Query Builder para recuperar um post específico com base no ID fornecido.
