# Hostpath provisioner for Kubernetes

Fork basic code from [upstream](https://github.com/kubernetes-sigs/sig-storage-lib-external-provisioner/tree/master/examples/hostpath-provisioner)

```bash
make
```

## Build & Push Image to Docker Hub

```shell
IMAGE=poeticloud/hostpath-provisioner:v1.0.0
make
docker push $IMAGE
```
