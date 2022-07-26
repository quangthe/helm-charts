# simple-keycloak-helm

## Quickstart
values.yaml
```yaml
# config map contains realm configs. Set "" to not bootstrap realms.
realmConfig: "realm-config"

ingress:
  enabled: true
  className: "nginx"
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod
  hosts:
    - host: idp.example.com
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls:
    - secretName: simple-keycloak-helm-tls
      hosts:
        - idp.example.com
```

```
helm repo add helm-charts https://quangthe.github.io/helm-charts
helm repo update

helm upgrade -i kc helm-charts/simple-keycloak-helm -f values.yaml
```