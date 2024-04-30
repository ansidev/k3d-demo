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

   > **NOTE:** Kubernetes dashboard will be enabled by default.

3. Open Kubernetes dashboard and paste the bearer token into the form to login (The bearer token will be copied to clipboard automatically).

   ```sh
   task dashboard:open
   ```

   To get the bearer token, run

   ```sh
   task dashboard:get-admin-token
   ```

4. Deploy an example app to k3s

   ```sh
   task deploy-app -- http_echo.yaml
   ```

   > **NOTE:** The default namespace for app will be `k3s-local` and can be customized via [Taskfile.yaml](./Taskfile.yaml) variable `KUBE_NAMESPACE`.

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
