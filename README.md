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

    - Replace eyJhdXRocyI6eyJkb...FpXeHNhUT09In19fQ== with a base64 encoding of the secret used to pull images from your container repository

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

    - Replace <your.local.repository> with the url to your local container repository:

        ```
        spec:
          serviceAccountName: chaoskube
          containers:
          - name: chaoskube
            image: <your.local.repository>/chaoskube:v0.21.0
            args:
        ```

## Configuration options

Configuration is done using environment variables which can be configured via the deployment yaml in the following manner:
```
args:
- --interval=1m
- --labels=environment=test
```

### Interval
This configuration option determines the interval between pod terminations, i.e. what is the duraiton of time to wait from one pod termination reuest to another.

Example:
```
--interval=1m
```

### Interval
This configuration option determines the interval between pod terminations, i.e. what is the duraiton of time to wait from one pod termination reuest to another.

Example:
```
--interval=1m
``` 
### Labels

Pods with these labels will be the only ones terminated. This option is an "AND" function, meaning, if there are 2 or 3 labels configured here only pods with ALL of the configured labels will be terminated using chaoskube.

Example:
```
--labels=environment=test
```





