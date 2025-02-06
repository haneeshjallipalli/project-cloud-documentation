## Running Prometheus on Docker
```
docker run -d -p --name=prometheus 9090:9090 prom/prometheus
```
## Running Grafana on Docker
```
docker run -d -p 3000:3000 --name=grafana grafana/grafana-enterprise
```

## Running Jaeger on Docker
```
docker run -d --name jaeger \
  -e COLLECTOR_ZIPKIN_HTTP_PORT=9411 \
  -p 5775:5775/udp \
  -p 6831:6831/udp \
  -p 6832:6832/udp \
  -p 5778:5778 \
  -p 16686:16686 \
  -p 14268:14268 \
  -p 9411:9411 \
  jaegertracing/all-in-one:1.6.0

```
