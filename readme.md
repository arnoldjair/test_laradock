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
