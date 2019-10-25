# Install Kubernetes

This is how to install a Kubernetes cluster on bare metal.

```sh
IP=your-ip-address

wget -O install.sh https://raw.githubusercontent.com/peterconnects/kubernetes-scripts/master/install2.sh && chmod u=rwx install.sh

sudo ./install.sh attendant $IP  2>&1 | tee outfile
```

Reset:

```sh
sudo kubeadm reset --force && \
  sudo rm -rf kubeadm-config.yaml helm* install.sh && \
  sudo rm -rf /tmp/installed
sudo rm -rf ~/.kube && sudo rm -rf ~/.helm
```

## Ingress & SQL Server on a single node cluster without a load balancer

What if we want to expose MSSQL over port 1433?

We need to set the hostNetwork to true, as documented here:  
https://kubernetes.github.io/ingress-nginx/deploy/baremetal/#via-the-host-network

We need to expose the extra TCP port like so:  
https://kubernetes.github.io/ingress-nginx/user-guide/exposing-tcp-udp-services

The tcp service configmap looks like so:

```yaml
kind: ConfigMap
apiVersion: v1
metadata:
  name: tcp-services
  namespace: ingress-nginx
  labels:
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/part-of: ingress-nginx
data:
  1433: "default/mssql-service:1433"
```

When deploying mssql, we need to expose mssql via a service like so:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mssql-service
  labels:
    release: {{ .Release.Name }}-mssql
spec:
  selector:
    app: mssql
  ports:
    - protocol: TCP
      port: 1433
      targetPort: 1433
  type: NodePort

```
