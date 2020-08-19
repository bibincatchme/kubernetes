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
