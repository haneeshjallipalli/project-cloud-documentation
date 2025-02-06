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
You can then navigate to http://IP:16686 to access the Jaeger UI.

The container exposes the following ports:

Port	Protocol	Component	Function
5775	UDP	agent	accept zipkin.thrift over compact thrift protocol
6831	UDP	agent	accept jaeger.thrift over compact thrift protocol
6832	UDP	agent	accept jaeger.thrift over binary thrift protocol
5778	HTTP	agent	serve configs
16686	HTTP	query	serve frontend
14268	HTTP	collector	accept jaeger.thrift directly from clients
9411	HTTP	collector	Zipkin compatible endpoint
