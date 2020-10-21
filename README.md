# Sharepath provisioner for Kubernetes

Fork basic code from [upstream](https://github.com/kubernetes-sigs/sig-storage-lib-external-provisioner/tree/master/examples/sharepath-provisioner)

```bash
make
```

## Intro

This provisioner is for private kubernetes cluster deploy.

![](./design/sharepath-deploy.png)

## Deploy into your kubernetes cluster

```shell
kubectl apply -f https://raw.githubusercontent.com/poeticloud/sharepath-provisioner/main/deploy/sharepath-provisioner.yml
```

**IMPORTANT** You can change the default directory name ( `/sharepath` ) to anything in above YAML file.

## Test

Create StorageClass :

```shell
cat <<EOF | kubectl apply -f -
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: example-sharepath
provisioner: poeticloud.com/sharepath
EOF
```

Create PVC:

```shell
cat <<EOF | kubectl apply -f -
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: sharepath-pvc
spec:
  storageClassName: "example-sharepath"
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1Mi
EOF
```

After create pvc:

```plain
[root@node-1 ~]# tree -al /sharepath/
/sharepath/
└── default-sharepath-pvc
```

Delete PVC:

```shell
kubectl delete pvc sharepath-pvc
```

After delete pvc:

```plain
[root@node-1 ~]# tree -al /sharepath/
/sharepath/
├── ._archived
│   └── default-sharepath-pvc
```

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
