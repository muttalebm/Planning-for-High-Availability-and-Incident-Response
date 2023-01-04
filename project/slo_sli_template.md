# API Service

| Category     | SLI                                                      | SLO                                                                                                         |
|--------------|----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| Availability | The number of successful request out of total requests   | 99%                                                                                                         |
| Latency      | 90 percentile of all bucket requests over 30 sec         | 90% of requests below 100ms                                                                                 |
| Error Budget | % of remaining error budget                               | Error budget is defined at 20%. This means that 20% of the requests can fail and still be within the budget |
| Throughput   | Number of successful request per second over last 5 mins | 5 RPS indicates the application is functioning                                                              |