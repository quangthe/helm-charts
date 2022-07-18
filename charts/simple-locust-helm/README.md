# simple-locust-helm

A simple helm chart to deploy [Locust](https://locust.io/) - An open source load testing tool.

## Quickstart 

Prepare Locust file, see [How to write Locust file](https://docs.locust.io/en/stable/writing-a-locustfile.html)

Example: `locustfile.py`
```py
from locust import HttpUser, task, between 

class QuickstartUser(HttpUser): 
    wait_time = between(0.7, 1.3) 

    @task(1) 
    def visit_website(self): 
        with self.client.get("/", headers={"User-Agent": "Mozilla"}, timeout=0.2, catch_response=True) as response: 
            if response.request_meta["response_time"] > 200: 
                response.failure("Homepage failed") 
            else: 
                response.success() 
```

Create config map to store the Locust file: `kubectl create cm locust-script --from-file=locustfile.py=locustfile.py`

Install Locust: `helm install loadtest helm-charts/simple-locust-helm --set locustConfigMap=locust-script`

### Access Locust web UI

Use the Helm NOTE to get the node ip and port:
```
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services locust-simple-locust-helm)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT
```

See [Locust’s web interface](https://docs.locust.io/en/stable/quickstart.html#locust-s-web-interface)
