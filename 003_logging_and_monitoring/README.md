# Logging & Monitoring

Metrics Server
- 1 metrics-server : 1 cluster
- In Mermoy
- cAdvisor (containers -> (metrics) -> metrics-server)
- `kubectl top node`
- `kubectl top pod`

`kubectl logs -f POD_NAME`
`kubectl logs -f POD_NAME CONTAINER_NAME`