# k-code
Example of kubernetes manifests for running Code Server on Kubernetes by using the https://github.com/coder/code-server with nvidia GPU examples

## Usage

The manifests for creating code server consisted of 

1. ConfigMap: Contains the startup script (Install the codeserver)
2. Service: For expose the codeserver
3. Deployment: The codeserver

You can modify the ConfigMap to meet your requirements.

### CPU only codeserver

```bash
kubectl apply -f cpu/code.yaml

kubectl get pod

NAME                                 READY   STATUS    RESTARTS   AGE
codeserver-7944bf48c6-2wzrk          1/1     Running   0          10s

# Wait until the codeserver script to finished
kubectl logs codeserver-7944bf48c6-2wzrk

Deploy code-server for your team with Coder: https://github.com/coder/coder
[2024-08-23T08:28:19.716Z] info  Wrote default config file to /root/.config/code-server/config.yaml
[2024-08-23T08:28:19.980Z] info  code-server 4.92.2 de65bfc9477f61bc22d0b1a23085d1f18bb25202
[2024-08-23T08:28:19.981Z] info  Using user-data-dir /root/.local/share/code-server
[2024-08-23T08:28:19.993Z] info  Using config file /root/.config/code-server/config.yaml
[2024-08-23T08:28:19.993Z] info  HTTP server listening on http://127.0.0.1:8080/
[2024-08-23T08:28:19.993Z] info    - Authentication is disabled
[2024-08-23T08:28:19.993Z] info    - Not serving HTTPS
[2024-08-23T08:28:19.993Z] info  Session server listening on /root/.local/share/code-server/code-server-ipc.sock


# Port-forward then connect to localhost:8080
kubectl port-forward service/codeserver 8080:8080
```

To obtain a password, you can execute this command
```bash
kubectl exec deployments/codeserver -- cat /root/.config/code-server/config.yaml
```

### With Nvidia GPU

```bash
kubectl apply -f nvidia/cuda-code.yaml


# Port-forward then connect to localhost:8080
kubectl port-forward service/cuda-code 8080:8080
```