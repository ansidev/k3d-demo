# k3d-demo

## Introduction

This project demonstrates how to setup a local k8s cluster (specifically on macOS) using k3d.

## Usages

> Make sure brew and docker is installed.

1. Install tools:

```sh
make prepare
```

2. Deploy k3s cluster using one of following commands:


```sh
task single-node:deploy
```

```sh
task multi-nodes:deploy
```

```sh
task multi-nodes-ingress-nginx:deploy
```

3. Deploy an example app to k3s:

```sh
task deploy-app -- http_echo.yaml
```

If you deploy the k3s cluster using `task multi-nodes-ingress-nginx:deploy`, use the below command:

```sh
task deploy-app -- http_echo_ingress_nginx.yaml
```

4. Test app

```sh
curl --resolve k3s.local:8080:127.0.0.1 http://k3s.local:8080/
```

You should see this result:

```
hello
```

## Author

Le Minh Tri [@ansidev](https://ansidev.xyz/about).

## License

This source code is released under the [MIT License](/LICENSE).
