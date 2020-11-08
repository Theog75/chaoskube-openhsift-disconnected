# chaoskube-openhsift-disconnected

Author: Liran Cohen

Email: liran@sosiv.io

## What is chaoskube?

[Chaoskube](https://github.com/linki/chaoskube) is a piece of software designed to test you application/enrvionment runniing on Kubernetes for recovery from failures.

The basics of chaos engineering is to deliberately inject failures and misconfigurations into an applications or an environment to test for resilience in turbulent conditions, thus finding the places in code or configuration that require attention in case of instability and making them as resilient as possible to failures.

## Quick start

- Download the chaoskube image onto you local repo:
```
docker pull quay.io/linki/chaoskube:v0.21.0
```

Note: the image version may change - at the point of writing this document, V0.21.0 is the latest chaoskube version

- Retag the image to your local repo:

```
docker tag quay.io/linki/chaoskube:v0.21.0 <your.local.repo>/chaoskube:v0.21.0
```

- Push the image to your local repo:
```
docker push <your.local.repo>/chaoskube:v0.21.0
```

- edit the yaml file "chaoskube.yml" and change the following:

    * Replace eyJhdXRocyI6eyJkb...FpXeHNhUT09In19fQ== with a base64 encoding of the secret used to pull images from your container repository
    ```
    - apiVersion: v1
      data:
        .dockerconfigjson: eyJhdXRocyI6eyJkb...FpXeHNhUT09In19fQ==
      kind: Secret
      metadata:
        namespace: chaoskube
        name: repocreds
      type: kubernetes.io/dockerconfigjson
      ```
      * Replace <your.local.repository> with the url to your local container repository:
      ```
      spec:
        serviceAccountName: chaoskube
        containers:
        - name: chaoskube
          image: <your.local.repository>/chaoskube:v0.21.0
          args:
      ```
      



