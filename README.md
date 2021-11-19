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

Now you are ready to create a new project. `CD` into the `src`folder and run the following: 

    docker-compose run --rm composer create-project laravel/laravel .

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

## How to run composer, artisan and npm

-   `docker-compose run --rm composer update`
-   `docker-compose run --rm npm run dev`
-   `docker-compose run --rm artisan migrate`


## Stopping the containers

To stop the containers, run this command

    docker-compose down

To stop the containers and delete the associated volumes, run the following command

    docker-compose down -v
