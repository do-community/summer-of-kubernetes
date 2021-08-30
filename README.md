# DO Summer of Kubernetes 

## Learning Objectives
_By the end of this session, you will:_  
1. Create and connect to a DigitalOcean Managed Kubernetes cluster 
1. Install the latest version of the kube-prometheus-stack
1. Login and look around the Grafana Dashboard 
1. ​​Install the Ambassador Edge Stack 
1. Setup a custom Grafana Dashboard to show data from the Ambassador Edge Stack
1. Describe how Prometheus and Grafana work together in a Kubernetes Cluster
1. Explain at least three benefits of being able to observe components in your Kubernetes Cluster


### 1. Create and connect to a DigitalOcean Managed Kubernetes cluster 
#### Login to your DigitalOcean account using doctl 

* From the [DigitalOcean Console](https://cloud.digitalocean.com/account/api/tokens) generate an access token with Read and Write permissions. 
* Copy the token to your clipboard
* Run the `doctl` auth command: 

```bash
doctl auth init
``` 
* Paste your authentication token and hit return

#### Create a Kubernetes cluster

Copy and paste the following command: 
```bash
doctl kubernetes cluster create summer-of-k8s \
--auto-upgrade=false \
--node-pool "name=basicnp;size=s-4vcpu-8gb;count=3;tag=cluster2;label=type=basic;auto-scale=true;min-nodes=2;max-nodes=4" \
--region sfo3
``` 

A couple of things to note: 
* In the command above, the VMs (AKA droplets) for this cluster are located in San Franciso, California, USA. If you'd like to find a data center region closer to you, run the command 
```bash
doctl kubernetes options regions
```
...and replace your region after the `--region` flag (for example, `--region blr1` will spin up droplets in DigitalOcean's Bangalore data center)
* If you'd prefer, you can create a Kubernetes cluster through the DigitalOcean Cloud Console
~!@#$$%^%% (find the quickstart video showing this)

### 2. Install the latest version of the kube-prometheus-stack

#### Find your DOKS cluster id 
```bash
doctl kubernetes cluster list
```
#### Install the [DigitalOcean Kubernetes Monitoring Stack 1-Click App](https://marketplace.digitalocean.com/apps/kubernetes-monitoring-stack)

```bash
doctl kubernetes 1-click install <your_cluster_id> --1-clicks monitoring
```

#### Check that the installation if complete by looking for `Running` pods in the `kube-prometheus-stack` namespace
```bash
kubectl -n kube-prometheus-stack get pods
```

3. Login and look around the Grafana Dashboard 
4. ​​Install the Ambassador Edge Stack 
5. Setup a custom Grafana Dashboard to show data from the Ambassador Edge Stack
6. Describe how Prometheus and Grafana work together in a Kubernetes Cluster
7. Explain at least three benefits of being able to observe components in your Kubernetes Cluster




## ​​Install Ambassador Edge Stack 

How to Install Ambassador Edge: 
https://www.getambassador.io/docs/edge-stack/latest/tutorials/getting-started/

Note: getting some CRD webhook errors during installation. Be wary if using kube 1.22. 1.21 will be okay 

## 


