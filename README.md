# prometheus-sample-app

This Prometheus sample app generates all 4 Prometheus metric types (counter, gauge, histogram, summary) and exposes them at the `/metrics` endpoint

At the same time, the generated metrics are also exposed at the `/expected_metrics` endpoint in the Prometheus remote write format

A health check endpoint also exists at `/`

The following is a list of environment variables for configuration:
* `LISTEN_ADDRESS`: (required) this defines the address and port that the sample app is exposed to. This is primarily to conform with the test framework requirements, the address is ignored as localhost is used, however the port is used. 
* `INSTANCE_ID`: (optional) a unique identifier for a batch of metrics from 1 instance of a sample app. Every metric name from a sample app instance will be prefixed by `INSTANCE_ID` if specified
* `METRICS_LOAD`: (optional, default=1) the amount of each type of metric to generate. The same amount of metrics is always generated per metric type.

Steps for running locally:
```bash
docker build . -t prometheus-sample-app
docker run -it -e INSTANCE_ID=test1234 \
-e LISTEN_ADDRESS=127.0.0.1:8080 \
-p 8080:8080 prometheus-sample-app
curl localhost:8080/metrics
```

Note that the port in LISTEN_ADDRESS must match the the second port specified in the port-forward

More functioning examples:

```bash
docker build . -t prometheus-sample-app
docker run -it -e INSTANCE_ID=test1234 \
-e LISTEN_ADDRESS=127.0.0.1:8080 \
-p 9001:8080 prometheus-sample-app
curl localhost:9001/metrics
```

```bash
docker build . -t prometheus-sample-app
docker run -it -e LISTEN_ADDRESS=127.0.0.1:8080 \
-e METRICS_LOAD=10 \
-p 9001:8080 prometheus-sample-app
curl localhost:9001/metrics
```