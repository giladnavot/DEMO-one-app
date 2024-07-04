---
title: Docker Configuration for Performance Testing
---
This document provides a detailed walkthrough of how Docker is utilized in the performance directory of the DEMO-one-app repository. It focuses on the configuration and usage of Docker, specifically within the context of the performance testing setup.

<SwmSnippet path="/__performance__/docker-compose.yml" line="3">

---

## Docker Network Configuration

The Docker network `one-app-performance` is defined. This network is used to connect all the services defined in this Docker Compose file.

```yaml
networks:
  one-app-performance:
```

---

</SwmSnippet>

<SwmSnippet path="/__performance__/docker-compose.yml" line="8">

---

## InfluxDB Service Configuration

The `influxdb` service is configured to use the `influxdb:1.8.10` image. It is connected to the `one-app-performance` network and exposes port 8086. An environment variable `INFLUXDB_DB` is set to `k6`, which is the database that will store the k6 metrics.

```yaml
  influxdb:
    image: influxdb:1.8.10
    networks:
      - one-app-performance
    ports:
      - "8086:8086"
    environment:
      - INFLUXDB_DB=k6
```

---

</SwmSnippet>

<SwmSnippet path="/__performance__/docker-compose.yml" line="18">

---

## Prometheus Service Configuration

The `prometheus` service is configured to use the `prom/prometheus:v2.51.2` image. It is connected to the `one-app-performance` network and exposes port 9090. The `./prometheus` directory is mounted to `/etc/prometheus` in the container, which is where Prometheus expects its configuration files to be.

```yaml
  prometheus:
    image: prom/prometheus:v2.51.2
    networks:
      - one-app-performance
    volumes:
      - ./prometheus:/etc/prometheus
    ports:
      - "9090:9090"
```

---

</SwmSnippet>

<SwmSnippet path="/__performance__/docker-compose.yml" line="28">

---

## Grafana Service Configuration

The `grafana` service is configured to use the `grafana/grafana:10.4.2` image. It is connected to the `one-app-performance` network and maps port 3030 on the host to port 3000 in the container. Several environment variables are set to configure Grafana's authentication settings. Two volumes are mounted to the container: `./grafana/provisioning` to `/etc/grafana/provisioning` and `./grafana/dashboards` to `/var/lib/grafana/dashboards`. The `grafana` service depends on the `influxdb` service.

```yaml
  grafana:
    image: grafana/grafana:10.4.2
    networks:
      - one-app-performance
    ports:
      - "3030:3000"
    environment:
      - GF_AUTH_ANONYMOUS_ORG_ROLE=Admin
      - GF_AUTH_ANONYMOUS_ENABLED=true
      - GF_AUTH_BASIC_ENABLED=false
    volumes:
      - ./grafana/provisioning:/etc/grafana/provisioning
      - ./grafana/dashboards:/var/lib/grafana/dashboards
    depends_on:
      - "influxdb"
```

---

</SwmSnippet>

<SwmSnippet path="/__performance__/docker-compose.yml" line="45">

---

## K6 Service Configuration

The `k6` service is configured to use the `grafana/k6:0.50.0` image. It is connected to the `one-app-performance` network and exposes port 6565. Two volumes are mounted to the container: `./scripts` to `/scripts` and `./results` to `/results`. An environment variable `K6_OUT` is set to send the k6 output to the InfluxDB service.

```yaml
  k6:
    image: grafana/k6:0.50.0
    networks:
      - one-app-performance
    ports:
      - "6565:6565"
    volumes:
      - ./scripts:/scripts
      - ./results:/results
    environment:
      - K6_OUT=influxdb=http://influxdb:8086/k6
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="general-build-tool"><sup>Powered by [Swimm](/)</sup></SwmMeta>
