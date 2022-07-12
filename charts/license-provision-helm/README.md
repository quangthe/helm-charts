# license-provision-helm
A simple license provision for Magnolia PaaS

## Install helm chart
```
helm repo add helm-charts https://quangthe.github.io/helm-charts
helm repo update

# example: Magnolia PaaS instances are in dev namespace
helm --namespace dev upgrade -i \
--set namespace=dev \
--set customer="test" \
--set magnoliaVersion="6.2.19" \
--set license.server="https://license.int.magnolia-platform.com" \
--set license.username="test" \
--set license.password="test" \
boot helm-charts/license-provision
```

## Clean up
```
helm -n del boot
```