# Introduction

Debugging a crashed container which could not be started normally.

# Instructions

1. Deploy pod

   ```sh
   kubectl --namespace k3s-local run crashed-pod --image=busybox -- false
   ```

2. Check pod status

   ```sh
   kubectl --namespace k3s-local get pod
   ```

   Result:

   ```
   NAME            READY   STATUS             RESTARTS     AGE
   crashed-pod     0/1     CrashLoopBackOff   3 (1s ago)   1m
   ```

3. Check pod information

   ```sh
   kubectl --namespace k3s-local describe pod crashed-pod
   ```

   Result should look like below:

   ```
   Containers:
     crashed-pod:
       Container ID:  containerd://ccbe88309ccce2a129dac595f0da54461ac8abd8e256742b9e8720689ff34362
       Image:         busybox
       Image ID:      docker.io/library/busybox@sha256:6776a33c72b3af7582a5b301e3a08186f2c21a3409f0d2b52dfddbdbe24a5b04
       Port:          <none>
       Host Port:     <none>
       Args:
         false
       State:          Waiting
         Reason:       CrashLoopBackOff
       Last State:     Terminated
         Reason:       Error
         Exit Code:    1
         Started:      Thu, 02 May 2024 05:16:29 +0700
         Finished:     Thu, 02 May 2024 05:16:29 +0700
       Ready:          False
       Restart Count:  5
       Environment:    <none>
       Mounts:
         /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-bxfp2 (ro)
   ```

   **NOTE:** The state of the container `crashed-pod` is `Waiting`, Reason is `CrashLoopBackOff`.

4. Debugging the failed container with the below command will not work.

   ```sh
   kubectl -n k3s-local debug -it crashed-pod --image=busybox --target=crashed-pod
   ```

   **NOTE**: Trying to access the container `crashed-pod` inside the pod `crashed-pod` using the container image `busybox`, which has built-in shell.

   Result should look like below:

   ```
   Targeting container "crashed-pod". If you don't see processes from this container it may be because the container runtime doesn't support this feature.
   Defaulting debug container name to debugger-zghv5.
   Warning: container debugger-zghv5: failed to generate container "40186728994c4907b50ab010c585ba0050a95f8ffbca6e90402bbdbb69844912" spec: invalid target container: container "b3ccf5c3f1e132eed9537c0ecdaad02996c0b27b5ac89be39c463d9543c9bef1" is not running - in state CONTAINER_EXITED
   ```

   **Reason**: The container is exited.

5. Debugging the failed container with the below command will work.

   ```sh
   kubectl -n k3s-local debug -it crashed-pod --copy-to=crashed-pod-debug --container=crashed-container-debug --image=busybox -- sh
   ```

   **NOTE**: Create a copy of the pod `crashed-pod` named `crashed-pod-debug` and use the container `crashed-container-debug` (with the container image `busybox`) for debugging with the shell command `sh`.

   Result should look like below:

   ```
   If you don't see a command prompt, try pressing enter.
   / #
   ```

   Now you can access the target container and debug.

6. Destroy pod

   ```sh
   kubectl --namespace k3s-local delete pod crashed-pod crashed-pod-debug
   ```
