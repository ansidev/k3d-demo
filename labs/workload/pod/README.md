# Instructions

1. Deploy pod

   ```sh
   kubectl --namespace k3s-local apply --filename http_echo_pod.yaml
   ```

2. Check pod status

   ```sh
   kubectl --namespace k3s-local get pod
   ```

   Result:

   ```
   NAME            READY   STATUS    RESTARTS   AGE
   http-echo-app   1/1     Running   0          1m
   ```

3. Port forwarding to be able to access pod from outside

   ```sh
   kubectl -n k3s-local port-forward pod/http-echo-app 8080:5678
   ```
   > `8080` is the host port, `5678` is the container port.

   Result:

   ```
   Forwarding from 127.0.0.1:8080 -> 5678
   Forwarding from [::1]:8080 -> 5678
   ```

4. Open another terminal and test

   ```sh
   curl http://127.0.0.1:8080/
   ```

   Result:
   ```
   hello
   ```

5. Destroy pod

   ```sh
   kubectl --namespace k3s-local delete --filename http_echo_pod.yaml
   ```
