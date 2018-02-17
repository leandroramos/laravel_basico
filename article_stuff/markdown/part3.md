# Parte 3 - Factories
- [Parte 1 - Setup do projeto, hasMany relationship e Laravel Tinker](https://github.com/leandroramos/laravel_basico/tree/part1)
- [Parte 2 - Adicionando colunas às tabelas e hasManyThrough relationship](https://github.com/leandroramos/laravel_basico/tree/part2)

## Criar as factories
    - php artisan make:factory AuthorFactory --model="App\\Author"
    - php artisan make:factory PostFactory --model="App\\Post"
    - php artisan make:factory CommentFactory --model="App\\Comment"

## Código das factories
- AuthorFactory

    ```php
    <?php

    use Faker\Generator as Faker;

    $factory->define(App\Author::class, function (Faker $faker) {
        return [
            'name'  => $faker->name,
            'email' => $faker->unique()->safeEmail,
            'bio'   => $faker->paragraph(1),
        ];
    });
    ```
        
- PostFactory

    ```php
    <?php

    use App\Author;
    use Faker\Generator as Faker;

    $factory->define(App\Post::class, function (Faker $faker) {
        return [
            'title'     => $faker->sentence(4),
            'content'   => $faker->paragraph(4),

            'author_id' => function () {
                return Author::orderByRaw("RAND()")
                    ->take(1)
                    ->first()
                    ->id;
            }
        ];
    });
    ```
        
- CommentFactory

    ```php
    <?php

    use App\Post;
    use Faker\Generator as Faker;

    $factory->define(App\Comment::class, function (Faker $faker) {
        return [
            'author_email'  => $faker->unique()->safeEmail,
            'content'       => $faker->paragraph(2),

            'post_id' => function () {
                return Post::orderByRaw("RAND()")
                    ->take(1)
                    ->first()
                    ->id;
            }
        ];
    });
    ```

## Código do DatabaseSeeder
- database/seeds/DatabaseSeeder.php

    ```php
    <?php

    use Illuminate\Database\Seeder;

    class DatabaseSeeder extends Seeder
    {
        /**
         * Run the database seeds.
         *
         * @return void
         */
        public function run()
        {
            // $this->call(UsersTableSeeder::class);
            echo "Criando 10 autores...\n";
            factory(App\Author::class, 10)->create();

            echo "Criando 36 posts relacionados a autores aleatórios...\n";
            factory(App\Post::class, 36)->create();

            echo "Criando 67 comentários relacionados a posts aleatórios...\n";
            factory(App\Comment::class, 67)->create();
        }
    }
    ```
  

## Links de referência
- [Laravel 5.6 - Database Testing](https://laravel.com/docs/5.6/database-testing)
- [Faker - Documentação (e repositório)](https://github.com/fzaninotto/Faker)
