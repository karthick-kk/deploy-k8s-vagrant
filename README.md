# deploy-k8s-vagrant
vagrant code and ansible playbooks to deploy a multi-node kubernetes cluster

## Requirements:
- [vagrant](https://www.vagrantup.com/downloads.html)

## Howto:
- Clone this repo
```
$ git clone https://github.com/karthick-kk/deploy-k8s-vagrant.git
```
- Replace the value of N with the number of nodes for the cluster
```
sed -i 's/^N =.*/N = 3/' Vagrantfile
```
- Deploy the cluster
```
vagrant up
```

## Manage cluster nodes
```
$ vagrant ssh k8s-master
$ vagrant ssh node-1
$ vagrant ssh node-2
....
```
