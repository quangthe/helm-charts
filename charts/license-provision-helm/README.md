# license-provision-helm
A simple license provision for Magnolia PaaS

## Install helm chart

Example: Magnolia PaaS instances are in dev namespace

Create `docker-registry` secret named `license-provision` in the `dev` namespace.
```
kubectl -n dev create secret docker-registry license-provision \
--docker-server=https://registry.your-company.com \
--docker-username="aaa" \
--docker-password="bbb"
```

```
helm repo add helm-charts https://quangthe.github.io/helm-charts
helm repo update

helm --namespace dev upgrade -i \
--set namespace=dev \
--set customer="test" \
--set magnoliaVersion="6.2.19" \
--set license.server="https://license.int.magnolia-platform.com" \
--set license.username="test" \
--set license.password="test" \
boot helm-charts/license-provision-helm
```

## Clean up
```
helm -n dev del boot
```