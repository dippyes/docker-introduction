# Goals

1. Run a dev enviorenment in Docker
    - Nginx/PHP
    - MySQL
    - Redis
2. `Dockerfile` to build a Nginx/PHP image.
3. Re-use existing images for MySQL & Redis.

## Execution Logic:
- Create a new volume for persistent storing of information in regards of MySQL:
```bash
docker volume create mysql
```
-Create a new network by which the containers will talk between each other:
```bash
docker network create appnet
```
- Build the docker image provided within **docker/app**:
```bash
docker build -f \
{Dockerfile location from current directory} {Dockerfile_location_from_current_working_directory}
```
-  Create your application folder: structure must be application/public/index.php.
- Run your application container.
```bash
docker run --rm -d \
    --name=app \
    --network=appnet\
     shippingdocker/app:latest
```
- Build and run your MySQL image - using Laravel defaults here {you may use different version of MySQL}:
```bash
docker run --rm -d \
    --name=mysql \
    --network=appnet \
    -v mysql:/var/lib/mysql  \
    -e MYSQL_ROOT_PASSWORD=root \
    -e MYSQL_DATABASE=homestead \
    -e MYSQL_USER=homestead \
    -e MYSQL_USER_PASSWORD=secret \
    Mysql:5.7
```
- Enjoy :)

### Additional information:
If you wish to run Laravel, you could build the application from within the container.
After you have run the App container, you could pull Laravel through **composer**
**Make sure you don't have the folder _application_!** since this will generate the folder with the brand new Laravel project.
```bash
docker exec -it -w /var/www/html app composer create-project laravel/laravel application
```
Via the docker exec command we can run any command required by the application, both **composer** and **artisan**, if no _docker compose_ is used.