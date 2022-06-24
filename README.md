# helm-charts
My helm charts for demo and devops

- `simple-magnolia-helm`: Deploy single Magnolia instance with your external Postgres DB (RDS or Postgres pod)

## simple-magnolia-helm

values.yaml
```
ingress:
  enabled: true
  className: "nginx"
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod 
  hosts:
    - host: travel.demo.example.com
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls:
    - secretName: travel-demo
      hosts:
        - travel.demo.example.com

war:
  repository: pcloud/magnolia-travel-demo
  pullPolicy: IfNotPresent
  # Overrides the image tag whose default is the chart appVersion.
  tag: "latest"

magnoliaMode: "author"

resources:
  limits:
    cpu: 1500m
    memory: 2Gi
  requests:
    cpu: 500m
    memory: 2Gi

db:
  host: acid-travel
  name: magnolia
  username: mgnl
  passwordFrom: "db-secret"
  passwordFromKey: password
```

Install helm chart
```
helm repo add helm-charts https://quangthe.github.io/helm-charts/
helm repo update
helm upgrade -i demo helm-charts/simple-magnolia-helm -f values.yaml
```

Clean up
```
helm del demo

# then delete the pvc
kubectl get pvc -l tier=app --no-headers -o name | xargs -I {} kubectl delete {}
```