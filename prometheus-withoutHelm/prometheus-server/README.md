cAdvisor
cAdvisor is an open-source agent integrated into the kubelet binary that monitors resource usage and analyzes the performance of containers. It collects statistics about the CPU,
memory, file, and network usage for all containers running on a given node (it does not operate at the pod level). In addition to core metrics, it also monitors events as well. 
Metrics can be accessed directly, using commands like kubectl top or used by the scheduler to perform orchestration (for example with autoscaling).



To access the cAdvisor UI, we need to proxy between our machine and Kubernetes API server. Start a local instance of the proxy server by typing:


cAdvisor metrics endpoint
Among the output, metrics you can look for include:

CPU:
container_cpu_user_seconds_total: Cumulative “user” CPU time consumed in seconds
container_cpu_system_seconds_total: Cumulative “system” CPU time consumed in seconds
container_cpu_usage_seconds_total: Cumulative CPU time consumed in seconds (sum of the above)
Memory:
container_memory_cache: Number of bytes of page cache memory
container_memory_swap: Container swap usage in bytes
container_memory_usage_bytes: Current memory usage in bytes, including all memory regardless of when it was accessed
container_memory_max_usage_bytes: Maximum memory usage in byte
Disk:
container_fs_io_time_seconds_total: Count of seconds spent doing I/Os
container_fs_io_time_weighted_seconds_total: Cumulative weighted I/O time in seconds
container_fs_writes_bytes_total: Cumulative count of bytes written
container_fs_reads_bytes_total: Cumulative count of bytes read
Network:
container_network_receive_bytes_total: Cumulative count of bytes received
container_network_receive_errors_total: Cumulative count of errors encountered while receiving
container_network_transmit_bytes_total: Cumulative count of bytes transmitted
container_network_transmit_errors_total: Cumulative count of errors encountered while transmitting
Some additional useful metrics can be found here:

/healthz: Endpoint for determining whether cAdvisor is healthy
/healthz/ping: To check connectivity to etcd
/spec: Endpoint returns the cAdvisor MachineInfo()
For example, to see the cAdvisor MachineInfo()





Kubelet
The Kubelet is one of the most important components in Kubernetes. Basically, it’s an agent that runs on each node and is responsible for watching the API Server for pods that are bound to its node and making sure those pods are running (it talks to the Docker daemon using the API over the Docker socket to manipulate containers lifecycle). It then reports back to the API Server the status of changes regarding those pods.
The main Kubelet responsibilities include:
Run the pods containers.
Report the status of the node and each pod to the API Server.
Run container probes.
Retrieve container metrics from cAdvisor, aggregate and expose them through the Kubelet Summary API for components (such as Heapster) to consume.
The last responsibility listed above will change in the future as cAdvisor will be turned off for container stats collection and replaced by the Container Runtime Interface.
The Kubelet also starts an internal HTTP server on port 10255 and exposes some endpoints (mostly for debugging, stats, and for one-off container operations such as kubectl logs or kubectl exec), such as /metrics, /metrics/cadvisor, /pods, /spec, and so on.
cAdvisor
The Kubelet ships with built-in support for cAdvisor, which collects, aggregates, processes and exports metrics (such as CPU, memory, file and network usage) about running containers on a given node. cAdvisor includes a built-in web interface available on port 4194 (just open your browser and navigate to http://<node-ip>:4194/).







Kubernetes metrics server
Starting from Kubernetes 1.8, the resource usage metrics coming from the kubelets and cadvisor are available through the Kubernetes metrics server API the same way Kubernetes API is exposed.

This service doesn’t allow us to store values over time either, and lacks visualization or analytics. Kubernetes metrics server is used for Kubernetes advanced orchestration like Horizontal Pod Autoscaler for autoscaling.
