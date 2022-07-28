## ☸️ kubernetes prometheus Setup

Complete prometheus monitoring stack setup on Kubernetes.

Idea of this repo to understand all the components involved in prometheus setup.

You can find the full tutorial from here--> [Kubernetes Monitoting setup Using Prometheus](https://devopscube.com/setup-prometheus-monitoring-on-kubernetes/)

## ✍️ Other Manifest repos

Kube State metrics manifests: https://github.com/devopscube/kube-state-metrics-configs

Alert manager Manifests: https://github.com/bibinwilson/kubernetes-alert-manager

Grafana manifests: https://github.com/bibinwilson/kubernetes-grafana

Node Exporter manifests: https://github.com/bibinwilson/kubernetes-node-exporter


## Order of Deployment
- **Namespace:** namespace.yaml
    - Creates a namespace 'monitoring' for logical separation.
- **RBAC:** clusterRole.yaml
    - Creates cluster roles and cluster role bindings to allow RBAC 
- **Configuration Isolation:** config-map.yaml
    - Creates a configmap where all the prometheus configuration is held
- **Deployment:** prometheus-deployment.yaml
    - Deploys the server and other components

**At this point the basic setup is ready and can be accessed using port-forwarding**

$ kctl get pods -n monitoring       \#   Grab the PODNAME

$ kctl port-forward --address localhost,[ANY PRIVATE IP] PODNAME 8080:9090 -n monitoring &

**Hit port 8080 for the localhost / or ANY PRIVATE IP configured**

- **Expose:** prometheus-service.yaml
    - Expose the prometheus service via nodeport
- **Map to DNS:** prometheus-ingress.yaml
    - Configure ingress for DNS, SSL etc. NOT DONE


## Additional services
- State Metrics
    - It is a service that captures details of all the API objects like deployments, replicas etc from 
    the kubernetes master cluster. It is packaged with this deployment so no additional work required.

- Alert Manager
    - A service that helps you visualize alerts. I do see a way to use this service's dashboard and configure
    alerts like add a new alert, or integrate with an end point or even add a recipient. It seems all that has to be 
    done by the configuration file (AlertManagerConfigMap.yaml). 
    I tinkered with the configuration file. I have left only one email reciever. However, I do not think this works 
    due to the SMTP configuration. 
    ToDo: Actually integrate with email
    - **Deploy**
        - AlertManagerConfigmap
        - AlertTemplateConfigMap
        - AlertManagerDeployment
        - AlertManagerService
    
    _Forward port as necessary_

- **Grafana**
    - It's a visualization tool. Almost a default with Prometheus. However, it also works with other integrations. 
    - **Deploy**
        - grafana-datasource-config
        - grafana-deployment
        - grafana-service

    _Forward port as necessary_

ToDo:
- SetUp NodeExporter