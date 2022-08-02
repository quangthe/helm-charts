# simple-oauth2-proxy-helm

Deploy [oauth2-proxy](https://oauth2-proxy.github.io/oauth2-proxy/) to protect application URL in K8S.

## Quickstart

Given a domain defined in an k8s ingress, we would like to protect that domain with `oauth2-proxy`. For example: `web.mydomain.com`

Create oauth2-proxy config secret

```bash
kubectl create secret generic my-oauth2-proxy-secret \
--from-literal=clientId=xxx \
--from-literal=clientSecret=yyy \
--from-literal=cookieSecret=zzz
```

```bash
helm repo add helm-charts https://quangthe.github.io/helm-charts/
helm repo update

helm upgrade --install my helm-charts/simple-oauth2-proxy-helm -f values.yaml \
--set oauth2.oidcIssuerUrl=https://dev-aaa.okta.com/oauth2/bbb \
--set oauth2.configSecret=my-oauth2-proxy-secret \
--set oauth2.domain=web.mydomain.com \
--set oauth2.tlsSecretName=my-domain-tls
```

Then add the following annotations to the ingress contains the domain

```
    nginx.ingress.kubernetes.io/auth-signin: "https://$host/oauth2/start?rd=$escaped_request_uri"
    nginx.ingress.kubernetes.io/auth-url: "https://$host/oauth2/auth"
```

Go to `https://web.mydomain.com`, it should redirect to authentication page of OIDC provider.
