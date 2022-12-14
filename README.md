# Wordpress Theme

Custom Wordpress theme development environment.
Built from this source:

> https://gist.github.com/bradtraversy/faa8de544c62eef3f31de406982f1d42# Wordpress Theme

## Build yaml file

```
$ docker-compose up -d

# To Tear Down
$ docker-compose down --volumes
```

## Create yaml file

- Start docker
- Create and move to directory 
```
$ mkdir wordpress
$ cd wordpress
```
- Create doker-compose.yaml file
```
$ touch doker-compose.yaml
```
- Populate doker-compose.yaml
```
version: '3'

services:
  # Database
  db:
    image: mysql:5.7
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
    image: phpmyadmin
    restart: always
    ports:
      - '8080:80'
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
      - '8000:80'
    restart: always
    volumes: ['./:/var/www/html']
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
- Build wordpress environment
```
$ docker-compose up -d

# To Tear Down
$ docker-compose down --volumes
```