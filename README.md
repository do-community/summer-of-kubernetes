# DigitalOcean Summer of Kubernetes Session 
## Observability: Configuring Prometheus and Grafana on DigitalOcean Managed Kubernetes

### Learning Objectives 
_By the end of this session, you will:_  
1. Create and connect to a DigitalOcean Managed Kubernetes cluster 
1. Install the latest version of the kube-prometheus-stack
1. Login and look around the Grafana Dashboard 
1. ​​Install the Ambassador Edge Stack 
1. Setup a custom Grafana Dashboard to show data from the Ambassador Edge Stack
1. Describe how Prometheus and Grafana work together in a Kubernetes Cluster
1. Explain at least three benefits of being able to observe components in your Kubernetes Cluster

### Step-by-Step Instructions 
#### 1. Create and connect to a DigitalOcean Managed Kubernetes cluster 
* Install `doctl`
    * If you haven't already, read through [the documentation](https://github.com/digitalocean/doctl#installing-doctl) and install `doctl`.

* Login to your DigitalOcean account using `doctl`
    * From the [DigitalOcean Console](https://cloud.digitalocean.com/account/api/tokens) generate an access token with Read and Write permissions. 
    * Copy the token to your clipboard
    * Run the `doctl` auth command: 

        ```bash
        doctl auth init
        ``` 
    * Paste your authentication token and hit return

* Create a Kubernetes cluster
    * Copy and paste the following command: 
        ```bash
        doctl kubernetes cluster create summer-of-k8s \
        --auto-upgrade=false \
        --node-pool "name=node;size=s-4vcpu-8gb;count=3;tag=cluster2;label=type=basic;auto-scale=true;min-nodes=3;max-nodes=4" \
        --region sfo3
        ``` 

    * Two things to note: 
        * 💡 In the command above, the VMs (AKA droplets) for this cluster are located in San Franciso, California, USA. If you'd like to find a data center region closer to you, run the command 
            ```bash
            doctl kubernetes options regions
            ```
            * Put your preferred region after the `--region` flag (for example, `--region blr1` will spin up droplets in DigitalOcean's Bangalore data center)
        * 💡 If you'd like to skip the command line setup, you can [create a Kubernetes cluster through the DigitalOcean Cloud Console](https://www.youtube.com/watch?v=k50reywjO5U&list=PLseEp7p6EwibbSz6yvFFrvBJo6L7X_rVj&index=2). 

#### 2. Install the latest version of the kube-prometheus-stack
* Find your DOKS cluster id 
    ```bash
    doctl kubernetes cluster list
    ```
*  Install the [DigitalOcean Kubernetes Monitoring Stack 1-Click App](https://marketplace.digitalocean.com/apps/kubernetes-monitoring-stack)
    ```bash
    doctl kubernetes 1-click install <your_cluster_id> --1-clicks monitoring
    ```
* Check that the installation if complete by looking for the pods in the `kube-prometheus-stack` namespace
    ```bash
    kubectl -n kube-prometheus-stack get pods
    ```
* When all pods are reporting a status of `Running`, go on to the next step. 

#### 3. Login and look around the Grafana Dashboard 
* Follow the instructions for how to start and login to the Grafana Dashboard by working through the [DigitalOcean Kubernetes Monitoring Stack Documentation](https://marketplace.digitalocean.com/apps/kubernetes-monitoring-stack)
* Stop after completing the steps under the heading **Grafana**. 
* Once you're logged-in to Grafana, click on the magnifiying glass icon to find and explore the pre-made Kubernetes dashboards. 

#### 4. ​​Install the Ambassador Edge Stack (AES)
*  Install the Edge Stack with Helm
    * Install the Edge Stack using Ambassador Labs's [Helm 3 instructions](https://www.getambassador.io/docs/edge-stack/latest/tutorials/getting-started/#1-installation).  
        * 💡 Note: You may see some CRD and webhook warnings during installation. If you are using Kubernetes versions 1.16 - 1.21, you will be fine. 
    * Look at what you've installed by inspecting the `ambassador` namespace
        ```bash
        kubectl -n ambassador get all
        ```
    * Go to the Ambassador Edge Stack Dashboard by finding the external-ip of the ambassador service.
        ```bash
        kubectl -n ambassador get svc
        ```
    * Copy the external ip address, and paste it in your browser. Say hi to [Edgy the Blackbird](https://www.getambassador.io/about-us/history-of-edgy/) and explore the dashboard. 

#### 5. Setup a custom Grafana Dashboard to show data from the Ambassador Edge Stack
* Install a Mapping and ServiceMonitor so Prometheus can scrape metrics for the AES 
    ```bash
       kubectl apply -f templates/mapping.yaml && \
       kubectl apply -f templates/service-monitor.yaml
    ```
*  Import the Ambassador Edge Stack Grafana Dashboard
    * Visit the [Ambassador Labs Grafana AES Dashboard](https://grafana.com/grafana/dashboards/13758)
    * Copy the dashboard ID 
    * Return to Grafana
    * Find the Plus Sign Icon, and go to `Import`
    * Enter the dashboard ID (`13758`) and click 'Load'
    * Explore the dashboard! 

#### 6. Describe how Prometheus and Grafana work together in a Kubernetes Cluster
* Read the article [Monitoring with Prometheus vs Grafana: Understanding the Difference
](https://www.sumologic.com/blog/prometheus-vs-grafana/) and then answer the question "How do Prometheus and Grafana work together in a Kubernetes Cluster?" 

#### 7. Explain at least three benefits of being able to observe components in your Kubernetes Cluster
* Watch Jason Yee's video [What is Observability?](https://www.youtube.com/watch?v=orsxOxQNzDQ) (and do your own research as needed) to be able to explain at least three benefits of being able to observe components in your Kubernetes Cluster. 

#### 8. Delete your cluster 
* Find your DOKS cluster id 
    ```bash
    doctl kubernetes cluster list
    ```
* Delete your cluster
    ```bash
    doctl kubernetes cluster delete --dangerous <your_cluster_id>
    ```
* When you see the confirmation question, choose `y`.  
    ```bash 
    Warning: Are you sure you want to delete this Kubernetes cluster? (y/N) ?
    ```
