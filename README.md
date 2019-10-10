# Install Kubernetes

This is how to install a Kubernetes cluster on bare metal.

```sh
sudo kubeadm reset --force && sudo rm -rf kubeadm-config.yaml helm* install.sh && sudo rm -rf /tmp/installed"
sudo rm -rf ~/.kube && sudo rm -rf ~/.helm
sudo ./install.sh attendant 192.168.1.154 attendant attendant.domain.local 2>&1 | tee outfile
```
