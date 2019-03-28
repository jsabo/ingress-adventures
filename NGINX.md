# nginx ingress

## Overview

Welcome to the adventures of Nginx ingress

## Setup

Let's do some some nginx ingress role playing.

We're going to install [multiple](https://kubernetes.github.io/ingress-nginx/user-guide/multiple-ingress/#multiple-ingress-nginx-controllers) nginx ingress controllers in different configurations to test out things like HTTP/2.

First let's try your standard prision issue nginx ingress setup as `Type: LoadBalancer`.

```
helm install stable/nginx-ingress \
  --name nginx-ingress-elb \
  --set controller.extraArgs.enable-ssl-passthrough="",rbac.create=true,controller.ingressClass=nginx-ingress-elb \
  --namespace nginx-ingress-elb
```

Next let's run it as a `Type: NodePort` service.

```
helm install stable/nginx-ingress \
  --name nginx-ingress-np \
  --set controller.extraArgs.enable-ssl-passthrough="",rbac.create=true,controller.ingressClass=nginx-ingress-np,controller.service.type=NodePort \
  --namespace nginx-ingress-np
```

## Verify Setup

```
$ kubectl get pods -n nginx-ingress-elb
NAME                                                READY   STATUS    RESTARTS   AGE
nginx-ingress-elb-controller-d87f9d6c6-kzbx2        1/1     Running   0          6m5s
nginx-ingress-elb-default-backend-c7b88b4bd-lfbnh   1/1     Running   0          6m5s
```

```
$ kubectl get pods -n nginx-ingress-np
NAME                                                     READY   STATUS    RESTARTS   AGE
nginx-ingress-np-controller-849dc9b44d-z6ch8       1/1   Running   0       2m59s
nginx-ingress-np-default-backend-cb494ddc6-ksgwz   1/1   Running   0       2m59s
```