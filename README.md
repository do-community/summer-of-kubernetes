# DO Summer of Kubernetes 

## Learning Objectives
_By the end of this session, you will:_  
1. Create and connect to a DOKS cluster 
1. Install the latest version of the kube-prometheus-stack
1. Login and look around the Grafana Dashboard 
1. (WIP) ​​Install Ambassador Edge Stack or Emissary-ingress to display some metrics via the Grafana dashboards
1. Setup a custom Grafana Dashboard to show data from some part of the Kubernetes cluster
1. Describe how Prometheus and Grafana work together in a Kubernetes Cluster

Bikram's blueprint https://github.com/bikram20/Kubernetes-Starter-Kit-Developers#prometheus-monitoring-stack-


## Create and connect to a DOKS cluster 

```bash
~ doctl kubernetes cluster create bg-cluster-2 \
--auto-upgrade=false \
--maintenance-window "saturday=21:00" \
--node-pool "name=basicnp;size=s-2vcpu-4gb;count=3;tag=cluster2;label=type=basic;auto-scale=true;min-nodes=3;max-nodes=5" \
--region sfo3 \
``` 

## Install the latest version of the kube-prometheus-stack
https://github.com/bikram20/Kubernetes-Starter-Kit-Developers#prometheus-monitoring-stack-

or DO Marketplace app 

## Login and look around the Grafana Dashboard 


## ​​Install Ambassador Edge Stack or Emissary-ingress to display some metrics via the Grafana dashboards

How to Install Ambassador Edge: 
https://www.getambassador.io/docs/edge-stack/latest/tutorials/getting-started/

Note: getting some CRD webhook errors during installation. Be wary if using kube 1.22. 1.21 will be okay 

## 


