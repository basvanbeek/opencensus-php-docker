# opencensus-php-docker
OpenCensus PHP Ecosystem using Docker Compose for quick prototyping &amp;
testing.

This docker-compose demo uses the following container set-up:

```
                                                                                       ┌────────┐
┌────────────┐                ┌───────────┐         ┌────────────┐          ┌─────────▶│ Zipkin │
│ PHP APACHE │──unix socket──▶│ OC Daemon │──gRPC──▶│  OC Agent  │──export──┤          └────────┘
└────────────┘                └───────────┘         └────────────┘          │          ┌────────────┐
                                                                            └─────────▶│ Prometheus │
                                                                                       └────────────┘
```

### starting the ecosystem

To start the ecosystem, simply run:
```shell
docker-compose up
```

After a while you should have the following available services:

service    | description               | URL
-----------|---------------------------|-----
Zipkin     | Tracing UI                | http://localhost:9411
Prometheus | Metrics UI                | http://localhost:9090
OC Agent   | zPages                    | http://localhost:55679/debug/tracez<br>http://localhost:55679/debug/rpcz
OC Agent   | Prometheus scrape address | http://localhost:8888/metrics
Apache PHP | Apache PHP Server         | http://localhost:8080

If one of the above ports is already in use, you can adjust the ports in the
`docker-compose.yml` file e.g.

```
  ports:
    # Alternate host port 9190 for the Prometheus UI
    - 9190:9090
```

### running example code

The `data/php` folder is the PHP container's document root and the
`data/php/html` directory is the Apache web root. This allows you to have code
live outside the Apache path (e.g. a compose vendor directory).

A small example script has been added as `data/php/html/helloworld/index.php`.
To run it you need to install the code's dependencies first with `composer`.

The Apache PHP container which by default is named `ocphp_php` has `composer`
installed so getting the dependencies ready can be done by running:

```shell
docker exec -ti ocphp_php composer install
```

After this a `vendor` directory will be available under `data/php` and contain
the OpenCensus library and its dependencies.

The script is similar to the typical Helloworld examples found in other
OpenCensus language libraries to highlight how similar usage is in a polyglot
environment while still trying to be idiomatic for each different language.
