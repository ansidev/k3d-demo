# Instructions

1. Deploy pod

   ```sh
   kubectl --namespace k3s-local apply --filename nginx_init.yaml
   ```

2. Check pod status

   ```sh
   kubectl --namespace k3s-local get pod
   ```

   Result:

   ```
   NAME            READY   STATUS    RESTARTS   AGE
   nginx-init      1/1     Running   0          1m
   ```

3. Port forwarding to be able to access pod from outside

   ```sh
   kubectl -n k3s-local port-forward pod/nginx-init 8080:80
   ```
   > `8080` is the host port, `80` is the container port.

   Result:

   ```
   Forwarding from 127.0.0.1:8080 -> 80
   Forwarding from [::1]:8080 -> 80
   ```

4. Open another terminal and test

   ```sh
   curl http://127.0.0.1:8080/
   ```

   Result:
   ```
   <p>This is updated by init container</p>
   ```

5. Destroy pod

   ```sh
   kubectl --namespace k3s-local delete --filename nginx_init.yaml
   ```
