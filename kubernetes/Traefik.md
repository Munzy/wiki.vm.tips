---
title: Traefik
description: 
published: true
date: 2020-04-23T04:41:18.222Z
tags: kubernetes, traefik
---

# Traefik

## YAML

```
---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: traefik-ingress-controller
rules:
  - apiGroups:
      - ""
    resources:
      - services
      - endpoints
      - secrets
    verbs:
      - get
      - list
      - watch
  - apiGroups:
      - extensions
    resources:
      - ingresses
    verbs:
      - get
      - list
      - watch
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: traefik-ingress-controller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: traefik-ingress-controller
subjects:
- kind: ServiceAccount
  name: traefik-ingress-controller
  namespace: kube-system
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: traefik-ingress-controller
  namespace: kube-system
---
apiVersion: v1
kind: Service
metadata:
  name: traefik
  namespace: kube-system
  labels:
    k8s-app: traefik-ingress-lb
spec:
  selector:
    k8s-app: traefik-ingress-lb
  ports:
    - port: 80
      name: http
    - port: 443
      name: https
---
apiVersion: v1
kind: Service
metadata:
  name: traefik-console
  namespace: kube-system
  labels:
    k8s-app: traefik-ingress-lb
spec:
  selector:
    k8s-app: traefik-ingress-lb
  ports:
    - port: 8080
      name: web
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: traefik-conf
  namespace: kube-system
data:
  traefik.toml: |
    # traefik.toml
    defaultEntryPoints = ["http","https"]
    InsecureSkipVerify = true
    [entryPoints]
      [entryPoints.http]
      address = ":80"
      [entryPoints.http.redirect]
      entryPoint = "https"
      [entryPoints.https]
      address = ":443"
      [entryPoints.https.tls]
    [acme]
    email = "admin@gmail.com"
    storage = "/acme/acme.json"
    entryPoint = "https"
    onDemand = true
    onHostRule = true
    caServer = "https://acme-v01.api.letsencrypt.org/directory"
    [acme.tlsChallenge]
      entryPoint = "https"
    [web]
      address = ":8080"
      [web.metrics.prometheus]
        Buckets=[0.1,0.3,1.2,5.0]
---
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: traefik-ingress-controller
  namespace: kube-system
  labels:
    k8s-app: traefik-ingress-lb
spec:
  revisionHistoryLimit: 0
  template:
    metadata:
      labels:
        k8s-app: traefik-ingress-lb
        name: traefik-ingress-lb
    spec:
      hostNetwork: true
      terminationGracePeriodSeconds: 60
      volumes:
        - name: config
          configMap:
            name: traefik-conf
        - name: acme
          hostPath:
            path: /etc/traefik/acme
      containers:
        - image: traefik:v1.7.5
          name: traefik-ingress-lb
          imagePullPolicy: Always
          volumeMounts:
            - mountPath: "/config"
              name: "config"
            - mountPath: "/acme"
              name: "acme"
          ports:
            - containerPort: 80
              hostPort: 80
            - containerPort: 443
              hostPort: 443
            - containerPort: 8080
          args:
            - --configfile=/config/traefik.toml
            - --web
            - --web.metrics.prometheus
            - --web.metrics.prometheus.buckets=0.1,0.3,1.2,5.0
            - --kubernetes
            - --logLevel=DEBUG
      tolerations:
      - key: CriticalAddonsOnly
        operator: Exists
      - key: node-role.kubernetes.io/master
        operator: Exists
        effect: NoSchedule
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: traefik-ingress
  namespace: kube-system
  annotations:
    kubernetes.io/ingress.class: "traefik"
    ingress.kubernetes.io/auth-type: "basic"
    ingress.kubernetes.io/auth-secret: "kubesecret"
spec:
  rules:
    - host: traefik.vespa.sfo.munroenet.com
      http:
        paths:
          - backend:
              serviceName: traefik-console
              servicePort: web
```