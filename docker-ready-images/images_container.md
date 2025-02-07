Many DevOps tools are designed to run efficiently in containers, making them easy to deploy, scale, and manage. Here are some essential DevOps tools that can run in Docker containers:  

### **CI/CD Tools**  
1. **Jenkins** - `jenkins/jenkins`  
2. **GitLab CI/CD** - `gitlab/gitlab-ce`  
3. **ArgoCD** - `argoproj/argocd`
```
docker run -d -p 8080:8080 --name jenkins jenkins/jenkins
docker run -d -p 80:80 --name gitlab gitlab/gitlab-ce
docker run -d -p 8088:8088 --name argocd argoproj/argocd
``` 

### **Version Control & Code Collaboration**  
4. **GitLab** - `gitlab/gitlab-ce`
```
docker run -d -p 3000:3000 --name gitea gitea/gitea
```

### **Containerization & Orchestration**  
5. **Docker** - `docker:dind` (Docker-in-Docker)  
6. **Kubernetes Tools** (kubectl, helm, kustomize)  
7. **K3s** (Lightweight Kubernetes)
```
docker run -d --privileged --name docker-dind docker:dind
```

### **Monitoring & Observability**  
8. **Prometheus** - `prom/prometheus`  
9. **Grafana** - `grafana/grafana`  
10. **Loki (Log Aggregation)** - `grafana/loki`  
11. **Jaeger (Tracing)** - `jaegertracing/all-in-one`  
12. **Elasticsearch** - `elasticsearch/elasticsearch`  
13. **Fluentd** - `fluent/fluentd`  
14. **Splunk Universal Forwarder** - `splunk/universalforwarder`
```
docker run -d -p 9090:9090 --name prometheus prom/prometheus
docker run -d -p 3000:3000 --name grafana grafana/grafana
docker run -d -p 3100:3100 --name loki grafana/loki
docker run -d -p 14268:14268 -p 16686:16686 --name jaeger jaegertracing/all-in-one
docker run -d -p 9200:9200 --name elasticsearch elasticsearch:7.10.2
docker run -d -p 24224:24224 --name fluentd fluent/fluentd

```

### **Configuration Management & Infrastructure as Code**  
15. **Ansible AWX** - `ansible/awx`  
16. **Terraform** - `hashicorp/terraform`  
17. **Puppet Server** - `puppet/puppetserver`  
18. **Chef Infra Server** - `chef/chef-server`
```
docker run -d -p 8052:8052 --name awx ansible/awx
docker run -d --name terraform hashicorp/terraform
docker run -d -p 8140:8140 --name puppetserver puppet/puppetserver
docker run -d -p 443:443 --name chef-server chef/chef-server

```

### **Security & Compliance**  
19. **Trivy (Container Scanning)** - `aquasec/trivy`  
20. **Clair (Vulnerability Scanning)** - `quay/clair`  
21. **Falco (Runtime Security)** - `falcosecurity/falco`
```
docker run -d --name trivy aquasec/trivy
docker run -d -p 6060:6060 --name clair quay/clair
docker run -d -p 8765:8765 --name falco falcosecurity/falco

```

### **Artifact Management**  
22. **Nexus Repository Manager** - `sonatype/nexus3`  
23. **Harbor (Container Registry)** - `goharbor/harbor-core`  
24. **JFrog Artifactory** - `jfrog/artifactory-oss`
```
docker run -d -p 8081:8081 --name nexus sonatype/nexus3
docker run -d -p 5000:5000 --name harbor goharbor/harbor-core
docker run -d -p 8082:8082 --name artifactory jfrog/artifactory-oss

```

### **Message Queues & Service Mesh**  
25. **RabbitMQ** - `rabbitmq`  
26. **Kafka** - `confluentinc/cp-kafka`  
27. **Istio** - `istio/proxyv2`
```
docker run -d -p 5672:5672 -p 15672:15672 --name rabbitmq rabbitmq:management
docker run -d -p 9092:9092 --name kafka confluentinc/cp-kafka
docker run -d -p 15021:15021 --name istio istio/proxyv2

```



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

## Jenkins in docker
```
docker run -p 8080:8080 -p 50000:50000 --restart=on-failure jenkins/jenkins:lts-jdk17
```
