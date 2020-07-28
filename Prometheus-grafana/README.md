Install
*helm install  prometheus-eks stable/prometheus-operator --set server.service.type=LoadBalancer

Port Forwading
*kubectl port-forward deployment/prmoetheus-grafana 3000

Get Values of prometheus-operator 
*helm inspect values stable/prometheus-operator  > /tmp/prometheus.values

Upgrade
helm upgrade -f values.yaml prometheus-operator stable/prometheus-operator

helm install  stable/prometheus-operator \
              prometheus \
             --namespace prometheus \
             --set alertmanager.persistentVolume.storageClass="gp2",server.persistentVolume.storageClass="gp2",server.service.type=LoadBalancer



helm install  prometheus-eks stable/prometheus-operator --set server.service.type=LoadBalancer --set rbac.create=false

To uninstall/delete the my-release deployment:
helm delete prometheus-operator




https://github.com/helm/charts/tree/master/stable/prometheus
Ingress TLS
If your cluster allows automatic creation/retrieval of TLS certificates (e.g. kube-lego), please refer to the documentation for that mechanism.

To manually configure TLS, first create/retrieve a key & certificate pair for the address(es) you wish to protect. Then create a TLS secret in the namespace:

kubectl create secret tls prometheus-server-tls --cert=path/to/tls.cert --key=path/to/tls.key
Include the secret's name, along with the desired hostnames, in the alertmanager/server Ingress TLS section of your custom values.yaml file:

server:
  ingress:
    ## If true, Prometheus server Ingress will be created
    ##
    enabled: true

    ## Prometheus server Ingress hostnames
    ## Must be provided if Ingress is enabled
    ##
    hosts:
      - prometheus.domain.com

    ## Prometheus server Ingress TLS configuration
    ## Secrets must be manually created in the namespace
    ##
    tls:
      - secretName: prometheus-server-tls
        hosts:
          - prometheus.domain.com




Grafana
Network I/O pressure
sum (machine_memory_bytes{kubernetes_io_hostname=~"^$Node$"})

Cluster memory usage
sum (container_memory_working_set_bytes{id="/",kubernetes_io_hostname=~"^$MYNODES$"}) / sum (machine_memory_bytes{kubernetes_io_hostname=~"^$MYNODES$"}) * 100
  Used: sum (container_memory_working_set_bytes{id="/",kubernetes_io_hostname=~"^$MYNODES$"})
  total: sum (machine_memory_bytes{kubernetes_io_hostname=~"^$MYNODES$"})

