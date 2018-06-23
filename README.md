# Cluster Kubernetes using Kubeadm

This repository create a cluster of Kubernetes using Kubeadm provisioning machines with Vagrant and Ansible. The operating system used is a box `centos/7`.

## Requeriments

The tools that were used to automate the creation of the cluster were:

- Ansible latest version installed by pip in guests machines;
- Vagrant latest version 2.0.2 with vagrant-hosts plugin installed;
- Kubernetes tools latest version 1.9;

## Get started

I am using the vagrant my structure of the `vagrant-yenv` repository to configure the machine environment. See more about: github.com/vinixavier/vagrant-yenv

To create the cluster, perform the following steps:

```bash
- git clone https://github.com/vinixavier/vagrant-kubeadm
- cd vagrant-kubeadm
- vagrant up --provision
- vagrant ssh kub01
- kubectl get nodes
```
