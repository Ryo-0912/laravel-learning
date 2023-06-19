laravelコンテナ、mysqlコンテナとは、docker-compose.ymlに記載されているサービス名各々のこと。

```
version: '3'
services:
    **①laravel.inter_edu**:
        build:
            context: ./vendor/laravel/sail/runtimes/8.1
            dockerfile: Dockerfile
            args:
                WWWGROUP: '${WWWGROUP}'
        image: sail-8.0/app
        ports:
            - '${APP_PORT:-80}:80'
        environment:
            WWWUSER: '${WWWUSER}'
            LARAVEL_SAIL: 1
        volumes:
            - '.:/var/www/html'
        networks:
            - sail
        depends_on:
            - mysql
    ②**mysql**:
        image: 'mysql:8.0'
        ports:
            - '${FORWARD_DB_PORT:-3307}:3306'
        environment:
            MYSQL_ROOT_PASSWORD: '${DB_PASSWORD}'
            MYSQL_DATABASE: '${DB_DATABASE}'
            MYSQL_USER: '${DB_USERNAME}'
            MYSQL_PASSWORD: '${DB_PASSWORD}'
            MYSQL_ALLOW_EMPTY_PASSWORD: 'yes'
        volumes:
            - 'sailmysql:/var/lib/mysql'
        networks:
            - sail
        healthcheck:
            retries: 3
            timeout: 5s
networks:
    sail:
        driver: bridge
volumes:
    sailmysql:
        driver: local
    sail-mysql:
        driver: local
```

上のコードでいうと、laravel.inter_eduとmysqlがそれぞれコンテナ名。

各コンテナにアクセスするには、次のようなコードでアクセスできる。

- **laravelコンテナへアクセス**

```
docker-compose exec laravel.inter_edu bash
```

- **mysqlコンテナへアクセス**

```
docker-compose exec mysql bash
```

これらの設定をするにあたり、.envは次のようにする。

### **.env**

```
DB_CONNECTION=mysql
DB_HOST=mysql    
**注意 : ↑はdocker-compose.ymlのサービス名で該当するもの、DBなので、laravel.inter_eduではなく、mysqlをセット**

DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=password
DB_TIME_ZONE=Asia/Tokyo
```
