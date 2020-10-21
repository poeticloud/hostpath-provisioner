# Sharepath provisioner for Kubernetes

Fork basic code from [upstream](https://github.com/kubernetes-sigs/sig-storage-lib-external-provisioner/tree/master/examples/sharepath-provisioner)

```bash
make
```

## Intro

This provisioner is for private kubernetes cluster deploy.

![](./design/sharepath-deploy.png)

## Build & Push Image to Docker Hub

```shell
export IMAGE=poeticloud/sharepath-provisioner:v1.0.0
make
docker push $IMAGE
```

## FAQ

### If your os is not Linux , you should use `docker` insteed of `make` to build

```shell
docker run -it --rm -v $(pwd):/build -w /build golang:1.14 make sharepath-provisioner
```

### 如果你在国内，编译 golang 需要设置代理

请在执行编译命令前，执行：

```shell
go env -w GOPROXY=https://goproxy.cn,direct
go env -w GOSUMDB="sum.golang.google.cn"
```

### If you want to use vendor for build

```shell
go mod tidy
go mod vendor
CGO_ENABLED=0 go build -mod=vendor -a -ldflags '-extldflags "-static"' -o sharepath-provisioner .
```

## Thanks

- https://github.com/kubernetes-sigs/sig-storage-lib-external-provisioner/tree/master/examples/hostpath-provisioner
- https://github.com/torchbox/k8s-hostpath-provisioner
- https://github.com/rimusz/hostpath-provisioner
