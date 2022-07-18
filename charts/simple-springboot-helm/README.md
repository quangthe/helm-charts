# simple-springboot-app
A simple helm chart to quickly deploy Spring Boot application on K8S

## Quick start
Tested with Spring Boot version: `2.7.1`

### Run with default app config
```
helm install demo helm-charts/simple-springboot-helm
```

### Run with external app config

Prepare app config map

For example: `application-k8s.yaml`

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config-k8s
  namespace: default
data:
  application-k8s.yaml: |-
    spring:
      datasource:
        url: "jdbc:postgresql://postgres:5432/demo"
        username: postgres
        password: postgres
        driverClassName: org.postgresql.Driver
```

Create config map: `kubectl create cm app-config-k8s --from-file=application-k8s.yaml=application-k8s.yaml`

```
helm install demo helm-charts/simple-springboot-helm --set appConfig=app-config-k8s --set profiles.active=k8s
```

### Key helm chart values

```yaml
image:
  repository: pcloud/simple-springboot-app
  tag: ""

profiles:
  default: default
  active: k8s

appConfig: ""

actuator:
  port: 8081
  livenessPath: /actuator/health/livenessState
  readinessPath: /actuator/health/readinessState
```