# Instructions

1. Deploy pod

   ```sh
   kubectl --namespace k3s-local apply --filename nginx_sidecar.yaml
   ```

2. Check pod status

   ```sh
   kubectl --namespace k3s-local get pod
   ```

   Result:

   ```
   NAME            READY   STATUS    RESTARTS   AGE
   nginx-sidecar   1/1     Running   0          1m
   ```

3. Check nginx log

   ```sh
   kubectl --namespace k3s-local logs nginx-sidecar --container nginx-log
   ```

   Result: empty log.

4. Port forwarding to be able to access pod from outside

   ```sh
   kubectl -n k3s-local port-forward pod/nginx-sidecar 8080:80
   ```
   > `8080` is the host port, `80` is the container port.

   Result:

   ```
   Forwarding from 127.0.0.1:8080 -> 80
   Forwarding from [::1]:8080 -> 80
   ```

5. Open another terminal and test

   ```sh
   curl http://127.0.0.1:8080/
   ```

6. Check nginx log again

   ```sh
   kubectl --namespace k3s-local logs nginx-sidecar --container nginx-log
   ```

   Result: something looks like below

   ```
   127.0.0.1 - - [01/Jan/2024:00:00:00 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/8.4.0" "-"
   ```

5. Destroy pod

   ```sh
   kubectl --namespace k3s-local delete --filename nginx_sidecar.yaml
   ```
