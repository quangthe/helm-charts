# Simple static site helm chart

Verify ingress and cert-manager setup for a K8S cluster

Some important default values:
- `ingress.enabled: false`
- `image.repository: pcloud/nginx-static-site`
- `image.tag: "1.0"`

Install helm chart
```
helm repo add helm-charts https://quangthe.github.io/helm-charts
helm repo update

helm upgrade -i demo helm-charts/static-site-helm

# with custom values myvalues.yaml
helm upgrade -i -f myvalues.yaml demo helm-charts/static-site-helm
```

Clean up
```
helm del demo
```