# WordPress-Docker environment
## Docker
first you need to make sure that Docker is already installed in your operating system, if not, install Docker from www.Docker.com
## Docker Containers
Set up WordPress container on Docker, in order to set up WordPress on Docker, two methods are available ‒ the CLI and Docker compose. We will use the Docker compose 
method as it’s more straightforward and systematic. It’s worth noting that all required images are acquired from Docker Hub :
• WordPress : the official WordPress Docker image. Includes all WordPress 
files, Apache server, and PHP. https://hub.docker.com/_/wordpress
• MySQL : required for MySQL root user, password, and database connection 
variables. https://hub.docker.com/_/mysql
• phpMyAdmin : a web application for managing databases. https://hub.docker.com/_/phpmyadmin
## docker-compose.yml file
you need to set the configurations for the wordpress environments like this:
```console
version: "3.8"

services:
  # Database
  db:
    image: mysql:8.0
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    networks:
      - wpsite
  # phpmyadmin
  phpmyadmin:
    depends_on:
      - db
    image: phpmyadmin/phpmyadmin
    restart: always
    ports:
      - "8080:80"
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: password
    networks:
      - wpsite
  # Wordpress
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8000:80"
    restart: always
    volumes: ["./:/var/www/html"]
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
    networks:
      - wpsite
networks:
  wpsite:
volumes:
  db_data:
```
With the Docker Compose file created, run the following command in the 
same WordPress directory to create and start the containers:
```console
docker-compose up -d
```
## WordPress installation 
Complete WordPress installation on a Web browser, Open your browser and enter http://localhost:8000 WordPress setup screen will appear. Select the preferred 
language and continue.
