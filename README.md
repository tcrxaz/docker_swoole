# docker_swoole

使用 **docker-compose** 快速搭建php环境

## Start

配置 `.env`

## Environment

### php

php 镜像来自官方 `php:fpm-alpine`

在此基础上增加了一下拓展:

- swoole
- redis
- mysqli
- pdo_mysql
- gd
- ...

手动添加了 `composer` 并修改了时区 (`Asia/Shanghai`)

### nginx

nginx 镜像来自官方 `nginx:alpine`

### mariadb

mariadb 镜像来自官方 `mariadb:latest`

## License

MIT



