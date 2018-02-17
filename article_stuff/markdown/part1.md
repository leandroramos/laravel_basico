# Laravel Básico - Parte 1
- [Parte 2 - Adicionando colunas às tabelas e hasManyThrough relationship](https://github.com/leandroramos/laravel_basico/tree/part2)
- [Parte 3 - Usando Factories](https://github.com/leandroramos/laravel_basico/tree/part3)

## Criação do banco de dados
- Criar o banco de dados no SGBD preferido

## Criação do projeto Laravel
- composer create-project laravel/laravel laravel_basico

## Configurar as variáveis de ambiente
- Editar o arquivo .env com nossos dados de conexão ao banco de dados

## Criação dos models e migrations
- php artisan make:model Post -m
- php artisan make:model Comment -m

### Criação dos campos nas tabelas
- database/migrations/create_posts_table.php
  ```php
  public function up()
  { 
      Schema::create('posts', function (Blueprint $table) {
          $table->increments('id');
          $table->string('title');
          $table->text('content');
          $table->timestamps();
      }); 
  }
  ```

- database/migrations/create_comments_table.php
  ```php
  public function up()
  { 
      Schema::create('comments', function (Blueprint $table) {
          $table->increments('id');
          $table->string('author_email');
          $table->text('content');
          $text->integer('post_id')->unsigned();
          $table->timestamps();

          $table->foreign('post_id')->references('id')->on('posts');
      }); 
  }
  ```

### Rodar as migrations
- php artisan migrate

### Colocar o relacionamento 1:N nos models
- app/Post.php
    ```php
    public function post()
    {
        return $this->belongsTo('App\Post');
    }
    ```
    
- app/Comment.php
    ```php
    public function comments()
    {
        return $this->hasMany('App\Comment');
    }
    ```

## Links de referência
- [Laravel 5.6 - Configuration](https://laravel.com/docs/5.6/configuration)
- [Laravel 5.6 - Eloquent Relationships](https://laravel.com/docs/5.6/eloquent-relationships#one-to-many)
