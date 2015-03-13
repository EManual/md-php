## Docker 
Beside using Vagrant, another easy way to get a virtual development or production environment up and running is [Docker].
Docker helps you to provide Linux containers for all kind of applications.
There are many helpful docker images which could provide you with other great services without the need to install
these services on your local machine, e.g. MySQL or PostgreSQL and a lot more. Have a look at the [Docker Hub Registry]
[docker-hub] to search a list of available pre-built containers, which you can then run and use in very few steps.

### Example: Runnning your PHP Applications in Docker

After you [installed docker][docker-install] on your machine, you can start an Apache with PHP support in one step.
The following command will download a fully functional Apache installation with the latest PHP version and provide the
directory `/path/to/your/php/files` at `http://localhost:8080`:

```console
docker run -d --name my-php-webserver -p 8080:80 -v /path/to/your/php/files:/var/www/html/ php:apache
```

After running `docker run` your container is initialized and running.
If you would like to stop or start your container again, you can use the provided name attribute and simply run
`docker stop my-php-webserver` and `docker start my-php-webserver` without providing the above mentioned parameters
again.

### Learn more about Docker

The commands mentioned above only show a quick way to run an Apache web server with PHP support but there are a lot
more things that you can do with Docker. One of the most important things for PHP developers will be linking your
web server to a database instance, for example. How this could be done is well described within the [Docker User Guide]
[docker-doc].

* [Docker Website][Docker]
* [Docker Installation][docker-install]
* [Docker Images at the Docker Hub Registry][docker-hub]
* [Docker User Guide][docker-doc]


[Docker]: http://docker.com/
[docker-hub]: https://registry.hub.docker.com/
[docker-install]: https://docs.docker.com/installation/
[docker-doc]: https://docs.docker.com/userguide/
