# Instructions

1. Install Ingress NGINX Controller for bare metal server

   ```sh
   kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.10.1/deploy/static/provider/baremetal/deploy.yaml
   ```

   > See https://kubernetes.github.io/ingress-nginx/deploy/#bare-metal-clusters

2. Check pod status

   ```sh
   kubectl get all,ingress --all-namespaces
   ```

   Result:

   ```
   NAMESPACE       NAME                                           READY   STATUS      RESTARTS   AGE
   kube-system     pod/local-path-provisioner-75bb9ff978-wsrpt    1/1     Running     0          2m8s
   kube-system     pod/coredns-576bfc4dc7-bs78g                   1/1     Running     0          2m8s
   kube-system     pod/metrics-server-557ff575fb-8c82z            1/1     Running     0          2m8s
   ingress-nginx   pod/ingress-nginx-admission-create-29tdz       0/1     Completed   0          96s
   ingress-nginx   pod/ingress-nginx-admission-patch-6n964        0/1     Completed   1          96s
   ingress-nginx   pod/ingress-nginx-controller-5cdfb74dc-577hr   1/1     Running     0          96s

   NAMESPACE       NAME                                         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
   default         service/kubernetes                           ClusterIP   10.43.0.1       <none>        443/TCP                      2m24s
   kube-system     service/kube-dns                             ClusterIP   10.43.0.10      <none>        53/UDP,53/TCP,9153/TCP       2m19s
   kube-system     service/metrics-server                       ClusterIP   10.43.43.240    <none>        443/TCP                      2m18s
   ingress-nginx   service/ingress-nginx-controller             NodePort    10.43.22.190    <none>        80:31944/TCP,443:31285/TCP   96s
   ingress-nginx   service/ingress-nginx-controller-admission   ClusterIP   10.43.100.145   <none>        443/TCP                      96s

   NAMESPACE       NAME                                       READY   UP-TO-DATE   AVAILABLE   AGE
   kube-system     deployment.apps/local-path-provisioner     1/1     1            1           2m19s
   kube-system     deployment.apps/coredns                    1/1     1            1           2m19s
   kube-system     deployment.apps/metrics-server             1/1     1            1           2m18s
   ingress-nginx   deployment.apps/ingress-nginx-controller   1/1     1            1           96s

   NAMESPACE       NAME                                                 DESIRED   CURRENT   READY   AGE
   kube-system     replicaset.apps/local-path-provisioner-75bb9ff978    1         1         1       2m8s
   kube-system     replicaset.apps/coredns-576bfc4dc7                   1         1         1       2m8s
   kube-system     replicaset.apps/metrics-server-557ff575fb            1         1         1       2m8s
   ingress-nginx   replicaset.apps/ingress-nginx-controller-5cdfb74dc   1         1         1       96s

   NAMESPACE       NAME                                       STATUS     COMPLETIONS   DURATION   AGE
   ingress-nginx   job.batch/ingress-nginx-admission-create   Complete   1/1           20s        96s
   ingress-nginx   job.batch/ingress-nginx-admission-patch    Complete   1/1           21s        96s
   ```

3. Install Ingress NGINX

   ```sh
   kubectl apply -f ingress-nginx.yaml
   ```

   Result:

   ```
   NAMESPACE       NAME                                           READY   STATUS      RESTARTS   AGE
   kube-system     pod/local-path-provisioner-75bb9ff978-wsrpt    1/1     Running     0          6m31s
   kube-system     pod/coredns-576bfc4dc7-bs78g                   1/1     Running     0          6m31s
   kube-system     pod/metrics-server-557ff575fb-8c82z            1/1     Running     0          6m31s
   ingress-nginx   pod/ingress-nginx-admission-create-29tdz       0/1     Completed   0          5m59s
   ingress-nginx   pod/ingress-nginx-admission-patch-6n964        0/1     Completed   1          5m59s
   ingress-nginx   pod/ingress-nginx-controller-5cdfb74dc-577hr   1/1     Running     0          5m59s
   kube-system     pod/svclb-ingress-nginx-4544cbab-b6fzw         2/2     Running     0          67s
   kube-system     pod/svclb-ingress-nginx-4544cbab-mjh7b         2/2     Running     0          67s
   kube-system     pod/svclb-ingress-nginx-4544cbab-4nvfb         2/2     Running     0          67s

   NAMESPACE       NAME                                         TYPE           CLUSTER-IP      EXTERNAL-IP                                 PORT(S)                      AGE
   default         service/kubernetes                           ClusterIP      10.43.0.1       <none>                                      443/TCP                      6m47s
   kube-system     service/kube-dns                             ClusterIP      10.43.0.10      <none>                                      53/UDP,53/TCP,9153/TCP       6m42s
   kube-system     service/metrics-server                       ClusterIP      10.43.43.240    <none>                                      443/TCP                      6m41s
   ingress-nginx   service/ingress-nginx-controller             NodePort       10.43.22.190    <none>                                      80:31944/TCP,443:31285/TCP   5m59s
   ingress-nginx   service/ingress-nginx-controller-admission   ClusterIP      10.43.100.145   <none>                                      443/TCP                      5m59s
   ingress-nginx   service/ingress-nginx                        LoadBalancer   10.43.26.109    192.168.165.2,192.168.165.3,192.168.165.4   80:32690/TCP,443:30976/TCP   67s

   NAMESPACE     NAME                                          DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
   kube-system   daemonset.apps/svclb-ingress-nginx-4544cbab   3         3         3       3            3           <none>          67s

   NAMESPACE       NAME                                       READY   UP-TO-DATE   AVAILABLE   AGE
   kube-system     deployment.apps/local-path-provisioner     1/1     1            1           6m42s
   kube-system     deployment.apps/coredns                    1/1     1            1           6m42s
   kube-system     deployment.apps/metrics-server             1/1     1            1           6m41s
   ingress-nginx   deployment.apps/ingress-nginx-controller   1/1     1            1           5m59s

   NAMESPACE       NAME                                                 DESIRED   CURRENT   READY   AGE
   kube-system     replicaset.apps/local-path-provisioner-75bb9ff978    1         1         1       6m31s
   kube-system     replicaset.apps/coredns-576bfc4dc7                   1         1         1       6m31s
   kube-system     replicaset.apps/metrics-server-557ff575fb            1         1         1       6m31s
   ingress-nginx   replicaset.apps/ingress-nginx-controller-5cdfb74dc   1         1         1       5m59s

   NAMESPACE       NAME                                       STATUS     COMPLETIONS   DURATION   AGE
   ingress-nginx   job.batch/ingress-nginx-admission-create   Complete   1/1           20s        5m59s
   ingress-nginx   job.batch/ingress-nginx-admission-patch    Complete   1/1           21s        5m59s
   ```

4. Deploy http_echo_hello.yaml

   ```sh
   kubectl --namespace k3s-local apply --filename http_echo_hello.yaml
   ```

5. Test app

   ```sh
   curl --resolve k3s.local:8080:127.0.0.1 http://k3s.local:8080/hello
   ```

   You should see this result:

   ```
   hello
   ```

6. Deploy http_echo_xinchao.yaml

   ```sh
   kubectl --namespace k3s-local apply --filename http_echo_xinchao.yaml
   ```

7. Test app

   ```sh
   curl --resolve k3s.local:8080:127.0.0.1 http://k3s.local:8080/xinchao
   ```

   You should see this result:

   ```
   xinchao
   ```

7. Destroy deployed resources

   ```sh
   kubectl --namespace k3s-local delete --filename http_echo_hello.yaml
   kubectl --namespace k3s-local delete --filename http_echo_xinchao.yaml
   ```
