# Install Kubernetes

This is how to install a Kubernetes cluster on bare metal.

```sh
wget -O install.sh https://raw.githubusercontent.com/jacqinthebox/kubernetes-scripts/master/install2.sh && chmod u=rwx install.sh

sudo ./install.sh attendant 192.168.1.154 attendant attendant.domain.local 2>&1 | tee outfile

sudo kubeadm reset --force && sudo rm -rf kubeadm-config.yaml helm* install.sh && sudo rm -rf /tmp/installed
sudo rm -rf ~/.kube && sudo rm -rf ~/.helm
```
