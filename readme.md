## Steps

- git clone https://github.com/laravel/laravel webapp
- cd webapp
- rm -rf .git
- git init
- git add .
- git commit -m 'initial commit'
- git submodule add https://github.com/LaraDock/laradock.git
- cd laradock
- cp env-example .env (this file must be modified, takes too much time and installs unnecessary packages)
- docker-compose up -d nginx mysql
- cd ..
- cp .env.example .env
- vim .env
  - DB_HOST=127.0.0.1 => DB_HOST=mysql
- cd laradock/
- docker-compose exec workspace bash
  - composer install
  - php artisan key:generate

## Mysql

- docker-compose exec mysql mysql -u root -proot (laradock folder)
  - create database homestead
  - CREATE USER 'homestead'@'localhost' IDENTIFIED WITH mysql_native_password BY 'secret';
  - GRANT ALL PRIVILEGES ON *.* TO 'homestead'@'localhost' WITH GRANT OPTION;

  - CREATE USER 'homestead'@'%' IDENTIFIED WITH mysql_native_password BY 'secret';
  - GRANT ALL PRIVILEGES ON *.* TO 'homestead'@'%' WITH GRANT OPTION;



## Passport
- docker-compose exec workspace bash
  - composer require laravel/passport
  - vim config/app.php (add passport to providers' array)
    - Laravel\Passport\PassportServiceProvider::class,
  - php artisan migrate
  - php artisan passport:install
  - vim app/User.php
    - use Laravel\Passport\HasApiTokens;
    - use HasApiTokens, Notifiable; (inside class)
  - vim app/Providers/AuthServiceProvider.php
    - use Laravel\Passport\Passport;
    - 'App\Model' => 'App\Policies\ModelPolicy', (uncomment)
    - Passport::routes(); (in boot function)
  - vim config/auth.php
    ```
    'guards' => [
        'web' => [
            'driver' => 'session',
            'provider' => 'users',
        ],

        'api' => [
            'driver' => 'passport',
            'provider' => 'users',
        ],
    ],
    ```
