# Overview
Kubernetes on CentOS 7 based on [kubeadm](http://kubernetes.io/docs/admin/kubeadm/). Default setup is a single master with three nodes

To setup type:

```
$ sudo ./up.sh
$ sudo vagrant ssh master
[vagrant@master]$ kubectl get nodes
```

> NOTE: currently works on Vagrant libvirt/KVM only, but it shouldn't be too hard to make it work in Virtualbox

# Versions
Currently it uses Kubernetes v1.4.5