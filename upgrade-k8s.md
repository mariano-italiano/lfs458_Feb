# Upgrade K8s cluster - procedure

## Master - Control Plane node
1. Upgrade kubeadm:
```shell
sudo apt-get install -y --allow-change-held-packages kubeadm=1.25.1-00
```

2. Drain the node to evacuate kube-system pods running on the master node:
```
kubectl drain master --ignore-daemonsets
```

3. Perform the upgrade:
```
sudo kubeadm upgrade plan v1.25.1
sudo kubeadm upgrade apply v1.25.1
```

4. Upgrade kubelet and kubectl:
```
sudo apt-get install -y --allow-change-held-packages kubectl=1.25.1-00 kubelet=1.25.1-00
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```

5. Uncordon the master node - to exit the maintenance
```
kubectl uncordon master
```

## Worker Nodes

1. Upgrade kubeadm (locally on worker node):
```shell
sudo apt-get install -y --allow-change-held-packages kubeadm=1.25.1-00
```

2. Drain the node to evacuate workload pods running on this node (executed on control plane node):
```
kubectl drain worker01 --ignore-daemonsets
kubectl drain worker01 --ignore-daemonsets --force
```

3. Upgrade the worker node (locally on worker node):
```
sudo kubeadm upgrade node
```
4. Upgrade kubelet and kubectl (locally on worker node):
```
sudo apt-get install -y --allow-change-held-packages kubectl=1.25.1-00 kubelet=1.25.1-00
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```
5. Uncordon the worker node - to exit the maintenance
```
kubectl uncordon master
```
