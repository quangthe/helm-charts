# helm-charts

My helm charts for demo and devops

- `simple-magnolia-helm`: Deploy single Magnolia instance with your external Postgres DB (RDS or Postgres pod)
- `license-provision-helm`: A simple license provision for Magnolia PaaS
- `simple-locust-helm`: A simple helm chart to deploy [Locust](https://locust.io/) - An open source load testing tool.
- `simple-springboot-helm`: A simple helm chart to quickly deploy Spring Boot application on K8S.
- `static-site-helm`: A simple nginx static site to verify ingress and cert-manager setup for a K8S.
- `simple-keycloak-helm`: A Helm chart for deploying demo Keycloak server
- `simple-oauth2-proxy-helm`: A simple helm chart to deploy OAuth2 proxy

## simple-magnolia-helm

<details>
  <summary>values.yaml</summary>

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
</details>

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

## license-provision-helm

See [instruction](https://github.com/quangthe/helm-charts/tree/main/charts/license-provision-helm)

## simple-locust-helm

See [instruction](https://github.com/quangthe/helm-charts/tree/main/charts/simple-locust-helm)

## simple-springboot-helm

See [instruction](https://github.com/quangthe/helm-charts/tree/main/charts/simple-springboot-helm)

## static-site-helm

See [instruction](https://github.com/quangthe/helm-charts/tree/main/charts/static-site-helm)

## simple-keycloak-helm

See [instruction](https://github.com/quangthe/helm-charts/tree/main/charts/simple-keycloak-helm)

## simple-oauth2-proxy-helm

See [instruction](https://github.com/quangthe/helm-charts/tree/main/charts/simple-oauth2-proxy-helm)
