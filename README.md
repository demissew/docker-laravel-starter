# Setup

To get started, first clone this repository into your dev machine. Then edit the following in the `docker-compose.yml` file.

    services:
      orion:
        build:

Change the service name into something different if you wish.

Then run 

    docker-compose up --build orion
    
OR 

    docker-compose up -d --build orion

to run the containers in a detached mode.

Now you are ready to create a new project.  Run the following: 

    docker-compose run --rm composer create-project laravel/laravel src

After laravel has completed installing, update your .env file to match the environment variables in the docker-compose.yml mysql service

## docker-compose.yml mysql service

    MYSQL_DATABASE: orion
    MYSQL_USER: orion
    MYSQL_PASSWORD: password
    MYSQL_TCP_PORT: 3307

## .env file

    DB_CONNECTION=mysql
    DB_HOST=mysql
    DB_PORT=3307
    DB_DATABASE=orion
    DB_USERNAME=orion
    DB_PASSWORD=password

Make sure to set `MYSQL_TCP_PORT` and `DB_PORT`to something different than the default mysql port `3306` (in case  you have mysql running in your machine already). 

I tried port mapping in order to use a different port for mysql container so that I can keep my main mysql installation running while using another mysql service in a container, but I was not able to connect to the database in the mysql container no matter what. Everytime I run `artisan migration`, I keep on getting this error. The only fix I found for this issue is to set `MYSQL_TCP_PORT`to a different port and use the same port in Laravel's `.env` file.

      SQLSTATE[HY000] [2002] Connection refused (SQL: select * from information_schema.tables where table_schema = orion and table_name = migrations and table_type = 'BASE TABLE')
    
      at vendor/laravel/framework/src/Illuminate/Database/Connection.php:703
        699▕         // If an exception occurs when attempting to run a query, we'll format the error
        700▕         // message to include the bindings with SQL, which will make this exception a
        701▕         // lot more helpful to the developer instead of just the database's errors.
        702▕         catch (Exception $e) {
      ➜ 703▕             throw new QueryException(
        704▕                 $query, $this->prepareBindings($bindings), $e
        705▕             );
        706▕         }
        707▕     }
    
          +36 vendor frames 
      37  artisan:37
          Illuminate\Foundation\Console\Kernel::handle(Object(Symfony\Component\Console\Input\ArgvInput), Object(Symfony\Component\Console\Output\ConsoleOutput))



## How to run composer, artisan and npm

-   `docker-compose run --rm composer update`
-   `docker-compose run --rm npm run dev`
-   `docker-compose run --rm artisan migrate`


## Stopping the containers

To stop the containers, run this command

    docker-compose down

To stop the containers and delete the associated volumes, run the following command

    docker-compose down -v

## Credit

Most of the code is taken from this repository: https://github.com/aschmelyun/docker-compose-laravel

