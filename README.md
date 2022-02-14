# Home Cluster

This repository holds everything needed to manage my k3s homeCluster in a GitOps fashion.

## Current Hardware

* 1x Raspberry Pi 4B 2GB (Node)

* 1x Raspberry Pi 4B 4GB (Master)

## Infrastructure Standup

Ansible is used to spin up and configure all the infrastructure in my cluster. To bootstrap the cluster, begin by installing the operating system of your choice to your SD Card.

Modify the `ansible_host` variables within your [hosts.ini](ansible/inventory/piCluster/hosts.ini) file to reflect the IP Addresses that the Raspberry Pis get.

After the `hosts.ini` file is set, ansible is ready to run. The following command sets the hostnames of the clusters, configures the necessary settings and packages needed to run k3s on raspberry pi, install k3s to the master, join the agent to the master as another node, and copy the kubeconfig file to your local `.kube/config`.

To run:

```bash
ansible-playbook ansible/site.yml -i ansible/inventory/piCluster/hosts.ini
```

After the playbook run completes, run check to ensure that the cluster came up successfully.

```bash
$ kubectl get nodes

NAME         STATUS   ROLES                  AGE     VERSION
pi-kube-n1   Ready    <none>                 9m46s   v1.22.3+k3s1
pi-kube-m1   Ready    control-plane,master   10m     v1.22.3+k3s1
```

To ensure the pods came up successfully

```bash
$ kubectl get pods -A

NAMESPACE     NAME                                     READY   STATUS    RESTARTS   AGE
kube-system   local-path-provisioner-64ffb68fd-b95fn   1/1     Running   0          10m
kube-system   coredns-85cb69466-q45gv                  1/1     Running   0          10m
```
