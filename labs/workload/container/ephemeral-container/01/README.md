# Introduction

Debugging a container which has no debugging utilities.

# Instructions

1. Deploy pod

   ```sh
   kubectl --namespace k3s-local apply --filename ephemeral_pod.yaml
   ```

2. Check pod status

   ```sh
   kubectl --namespace k3s-local get pod
   ```

   Result:

   ```
   NAME            READY   STATUS    RESTARTS   AGE
   ephemeral-pod   1/1     Running   0          1m
   ```

3. Try to exec into it will fail.

   ```sh
   kubectl --namespace k3s-local exec -it ephemeral-pod -- sh
   ```

   Result should look like below:

   ```
   error: Internal error occurred: error executing command in container: failed to exec in container: failed to start exec "a841b24e5ed861d9a77bf71caf1bc9c3b93f746c6fc624887471bc9d6eeb7844": OCI runtime exec failed: exec failed: unable to start container process: exec: "sh": executable file not found in $PATH: unknown
   ```

   **NOTE:** The reason is the container image `registry.k8s.io/pause:latest` has no shell.

4. Debug the failed container

   ```sh
   kubectl -n k3s-local debug -it ephemeral-pod --image=busybox --target=ephemeral-container
   ```

   **NOTE**: Trying to access the container `ephemeral-container` inside the pod `ephemeral-pod` using the container image `busybox`, which has built-in shell.

   Result should look like below:

   ```
   Targeting container "ephemeral-container". If you don't see processes from this container it may be because the container runtime doesn't support this feature.
   Defaulting debug container name to debugger-59m2g.
   If you don't see a command prompt, try pressing enter.
   / #
   / #
   / #
   ```

   Now you can access the target container and debug.

5. Destroy pod

   ```sh
   kubectl --namespace k3s-local delete --filename ephemeral_pod.yaml
   ```
