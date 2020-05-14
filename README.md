** Education project K8s on GKE **

* Ready to use NEG
* Uses Ingress resource integrated with gce
* Uses persitent volume claims

** Create cluster **
```
gcloud container clusters create demo-cluster-neg \
    --enable-ip-alias  \
    --create-subnetwork=""  \
    --network=default  \
    --zone=us-central1-a
```

** Some notes **

1. Healthcheck Kibana problem
When Ingress creates balancer, automatically created healthcheck for backend verifies / , and expect 200 .
It doesn't work for Kibana, as it responses with 302 . In order to fix it, readiness probe can be configured
for Pod, but the fact is that it doesn't change healthcheck behavior, until ports with containerPort is undefined.
So this part is required to change behavior of the healthcheck
```
        ports:
        - containerPort: 5601
        readinessProbe:
          httpGet:
            port: 5601
            path: /api/status
```
