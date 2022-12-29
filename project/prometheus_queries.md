## Availability SLI
### The percentage of successful requests over the last 5m


```sum(rate(prometheus_http_requests_total{code=~"200"}[5m])) / sum(rate(prometheus_http_requests_total[5m]))```

## Latency SLI
### 90% of requests finish in these times

```histogram_quantile(0.90, sum(rate(prometheus_http_request_duration_seconds_bucket[5m])) by (le, verb)) ```


## Throughput
### Successful requests per second

``sum(rate(prometheus_http_requests_total{code=~"200"}[5m]))``

## Error Budget - Remaining Error Budget
### The error budget is 20%

