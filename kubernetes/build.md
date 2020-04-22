---
title: Kubernetes Setup
description: 
published: true
date: 2020-04-22T05:51:14.136Z
tags: 
---

# Kubernetes

How to setup a kuberentes cluster, roughly.

## Install

```
#!/bin/bash


# update
apt update
apt dist-upgrade -y

# basic needs
apt install fail2ban -y
apt install vnstat -y
apt install apt-transport-https -y
apt install htop -y 

# Getting Keys
curl -s https://download.docker.com/linux/ubuntu/gpg | apt-key add -
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

# Getting sources
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" > /etc/apt/sources.list.d/kubernetes.list
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
apt update

# installing docker, and kubernetes
apt install docker-ce=18.06.1~ce~3-0~ubuntu -y
apt-get install kubeadm kubelet kubectl -y

# install firewall
ufw default deny incoming on eth0
ufw allow 22/tcp
ufw allow 80/tcp
ufw allow 179/tcp
ufw allow 443/tcp
ufw allow 10250/tcp
ufw allow 10254/tcp
ufw allow 10255/tcp
ufw --force enable

```

## Master UFW Settings

```
#!/bin/bash

ufw allow 2379/tcp
ufw allow 2380/tcp
ufw allow 6443/tcp
ufw allow 6666/tcp
ufw allow 10251/tcp
ufw allow 10252/tcp

### Update my record
#kubeadm init --apiserver-advertise-address=<MASTER_IP> --pod-network-cidr=192.168.1.0/16
```

## Let's Encrypt

```
apiVersion: certmanager.k8s.io/v1alpha1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    email: email@address.com
    http01: {}
    privateKeySecretRef:
      key: ""
      name: letsencrypt-prod
    server: https://acme-v02.api.letsencrypt.org/directory
```

## Nginx Ingress

```
#!/bin/bash

kubectl create serviceaccount --namespace kube-system tiller
kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'
helm install --namespace kube-system --name nginx-ingress stable/nginx-ingress --set rbac.create=true --set controller.kind=DaemonSet --set controller.daemonset.useHostPort=true
```

## Add Sub Accounts

```
#!/bin/bash

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl apply -f https://docs.projectcalico.org/v3.3/getting-started/kubernetes/installation/hosted/etcd.yaml
kubectl apply -f https://docs.projectcalico.org/v3.3/getting-started/kubernetes/installation/rbac.yaml
kubectl apply -f https://docs.projectcalico.org/v3.3/getting-started/kubernetes/installation/hosted/calico.yaml
```
