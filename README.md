# phyt'lab
Wiki pour phytothérapie

[![Angular Logo](https://www.vectorlogo.zone/logos/laravel/laravel-icon.svg)](https://laravel.com) [![Webpack Logo](https://www.vectorlogo.zone/logos/js_webpack/js_webpack-icon.svg)](https://webpack.js.org)

[![Travis Build Status][build-badge]][build]
[![Dependencies Status][dependencyci-badge]][dependencyci]
[![License](https://img.shields.io/badge/License-GPLv3-blue.svg)](LICENSE.md)


### Configuration
 You can set DB config in `docker-compose.yml` <br>
 Default `.env` file is created on image build you can overwrite it buy creating a `.env` file from `.env.example`

### Quickstart
With Docker Compose is a Quickstart very easy. Run the following command:

```
docker-compose up
```

and after that open your Browser and go to [http://localhost:8080](http://localhost:8080) .

### How to use the Image without Docker compose
Networking changed in Docker v1.9, so you need to do one of the following steps.

#### Docker < v1.9
1. MySQL Container:
```bash
docker run -d --name phytlab-mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=phytlab -e MYSQL_USER=phytlab -e MYSQL_PASSWORD=secret mysql:5.7.21
```
2. Phytlab Container:
```bash
docker run --name my-phytlab -d --link phytlab-mysql:mysql -p 8080:80 madicorp/phytlab:0.25.2
```

#### Docker 1.9+

1.Create a shared network:

```bash
docker network create phytlab_nw
```

2.MySQL container :
```bash
docker run -d --net phytlab_nw  \
-e MYSQL_ROOT_PASSWORD=secret \
-e MYSQL_DATABASE=phytlab \
-e MYSQL_USER=phytlab \
-e MYSQL_PASSWORD=secret \
 --name="phytlab_db" \
 mysql:5.7.21
```


3.Create BookStack Container

```bash
docker run -d --net phytlab_nw  \
-e DB_HOST=phytlab_db:3306 \
-e DB_DATABASE=phytlab \
-e DB_USERNAME=phytlab \
-e DB_PASSWORD=secret \
-p 8080:80 \
 madicorp/phytlab:0.25.2
```

After the steps you can visit [http://localhost:8080](http://localhost:8080) . You can login with username 'admin@admin.com' and password 'password'.


### Inspiration

This is a fork of [solidnerd/docker-bookstack](https://github.com/solidnerd/docker-bookstack). Solidnerd did the intial work, but I want to go in a different direction.



[build-badge]: https://travis-ci.com/madicorp/phytlab.svg?branch=master
[build]: https://travis-ci.org/madicorp/phytlab
[dependencyci-badge]: https://dependencyci.com/github/madicorp/phytlab/badge
[dependencyci]: https://dependencyci.com/github/madicorp/phytlab
[license-badge]: https://img.shields.io/badge/License-GPLv3-blue.svg?style=flat
[license]: https://github.com/madicorp/phytlab/blob/master/LICENSE.md