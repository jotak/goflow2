# k8s-enrich

## Build image

```bash
podman build -t quay.io/jotak/goflow:v2-k8s .
podman push quay.io/jotak/goflow:v2-k8s
```

## Run in kube

Example of usage in kube (assuming built image `quay.io/jotak/goflow:v2-k8s`)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: goflow
  name: goflow
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: goflow
  template:
    metadata:
      labels:
        app: goflow
    spec:
      containers:
      - command:
        - /bin/sh
        - -c
        - /goflow2 -loglevel "debug" | /k8s-enrich
        image: quay.io/jotak/goflow:v2-k8s
        imagePullPolicy: IfNotPresent
        name: goflow
---
apiVersion: v1
kind: Service
metadata:
  name: goflow
  namespace: default
  labels:
    app: goflow
spec:
  ports:
  - port: 2055
    protocol: UDP
  selector:
    app: goflow
```
