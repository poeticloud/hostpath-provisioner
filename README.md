# Hostpath provisioner for Kubernetes

Fork basic code from [upstream](https://github.com/kubernetes-sigs/sig-storage-lib-external-provisioner/tree/master/examples/hostpath-provisioner)

```bash
make
```

## Build & Push Image to Docker Hub

```shell
export IMAGE=poeticloud/hostpath-provisioner:v1.0.0
make
docker push $IMAGE
```

**IMPORTANT** If your os is not Linux , use docker to run above command.

```shell
docker run -it --rm -v $(pwd):/build -w /build golang:1.14 make hostpath-provisioner
export IMAGE={YOUR_USERNAME}/hostpath-provisioner:v1.0.X
make image
```
