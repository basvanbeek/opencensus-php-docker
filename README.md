# opencensus-php-docker
OpenCensus PHP Ecosystem using Docker for quick prototyping &amp; testing

This docker-compose demo uses the following set-up:

```
                                                                                       ┌────────┐
┌────────────┐                ┌───────────┐         ┌────────────┐          ┌─────────▶│ Zipkin │
│ PHP APACHE │──unix socket──▶│ OC Daemon │──gRPC──▶│  OC Agent  │──export──┤          └────────┘
└────────────┘                └───────────┘         └────────────┘          │          ┌────────────┐
                                                                            └─────────▶│ Prometheus │
                                                                                       └────────────┘
```
