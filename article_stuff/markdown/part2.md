# Laravel Básico - Parte 2 (hasManyThrough)
- [Parte 1 - Setup do projeto, hasMany relationship e Laravel Tinker](https://github.com/leandroramos/laravel_basico/tree/part1)
- [Parte 3 - Usando Factories](https://github.com/leandroramos/laravel_basico/tree/part3)

## Criar o model Authors
- php artisan make:model Author -m
- Colocar as colunas na tabela authors em database/migrations/create_authors_table.php
    ```php
    public function up()
    {
        Schema::create('authors', function (Blueprint $table) {
            $table->increments('id');
            $table->string('name');
            $table->string('email');
            $table->string('bio')->nullable();
            $table->timestamps();
        });
    }
    ```

## Adicionar o ID do autor ao Post
- php artisan make:migration add_author_id_to_posts --table=posts
- Colocar a coluna de chave estrangeira em database/migrations/add_author_id_to_posts.php
    ```php
    public function up()
    {
            Schema::table('posts', function (Blueprint $table) {
                $table->integer('author_id')->unsigned();

                $table->foreign('author_id')->references('id')->on('authors');
            });
        }

        /**
         * Reverse the migrations.
         *
         * @return void
         */
        public function down()
        {
            Schema::table('posts', function (Blueprint $table) {
                $table->dropForeign('posts_author_id_foreign')
                $table->dropColumn('author_id');
            });
        }
    }
    ```

## Rodar as migrations
- Antes de rodar as migrations, temos que dar refresh no banco de dados, pois podem existir dados que impediriam a adição de novas chaves estrangeiras (PDOException::("SQLSTATE[23000]):
    - php artisan migrate:refresh
    - php artisan migrate

## Adicionar os relacionamentos
- app/Post.php

    ```php
    <?php

    namespace App;

    use Illuminate\Database\Eloquent\Model;

    class Post extends Model
    {
        public function comments()
        {
            return $this->hasMany('App\Comment');
        }

        public function author()
        {
            return $this->belongsTo('App\Author');
        }
    }
    ```

    - app/Author.php
        ```php
        <?php

        namespace App;

        use Illuminate\Database\Eloquent\Model;

        class Author extends Model
        {
            public function posts()
            {
                return $this->hasMany('App\Post');
            }
            public function comments()
            {
                return $this->hasManyThrough('App\Comment', 'App\Post');
            }
        }
        ```

## Sugestões para testar o banco de dados no Tinker
    - php artisan tinker
    - $author = new Author
    - $author->name = 'Leandro Ramos'
    - $author->email = 'leandroramos@usp.br'
    - $author->save()
    - $post = new Post
    - $post->title = 'Um Grande Post!'
    - $post->content = 'Um excelente texto explicando sobre uma grande novidade'
    - $post->author_id = 1
    - $post = new Post
    - $post->title = 'Segundo Grande Post!'
    - $post->content = 'Segundo texto espetacular sobre um grande post.'
    - $post->author_id = 1
    - $post = new Post
    - $post->title = 'Um Grande Post!'
    - $post->content = 'Um excelente texto explicando sobre uma grande novidade'
    - $post->author_id = 1 
    - $post->save()
    - $post = new Post
    - $post->title = 'Segundo Grande Post!'
    - $post->content = 'Segundo texto espetacular sobre um grande post.'
    - $post->author_id = 1 
    - $post->save()
    - $comment = new Comment
    - $comment->author_email = 'comentador@site.com'
    - $comment->content = 'Mas que post sensacional!'
    - $comment->post_id = 1
    - $comment->save()
    - $comment = new Comment
    - $comment->author_email = 'oriboncina@site.com'
    - $comment->content = 'Sei não... estás enganado!!'
    - $comment->post_id = 1
    - $comment->save()
    - $comment = new Comment
    - $comment->author_email = 'eu@site.com'
    - $comment->content = 'Estão faltando alguns links no post.'
    - $comment->post_id = 1
    - $comment->save()
    - $comment = new Comment
    - $comment->author_email = 'eu@site.com'
    - $comment->content = 'Obrigado, ajudou muito.'
    - $comment->post_id = 2
    - $comment->save()
    - 
    - Mais testes:
        - $author = Author::first()
        - $author->posts
        - $author->comments
        - $author->comments->where('post_id', '2')
        - $comment = Comment::first()
        - $comment->post
        - $comment->post->author
        - $comment->post->author->name
 
## Links de referência
- [Laravel 5.6 - Eloquent Relationships](https://laravel.com/docs/5.6/eloquent-relationships#has-many-through)
- [Laravel 5.6 - Database Migrations](https://laravel.com/docs/5.6/migrations#generating-migrations)
