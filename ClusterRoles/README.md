*kubectl get clusterroles --no-headers | wc -l


How many ClusterRoleBindings exist on the cluster?
*kubectl get clusterrolebindings --no-headers | wc -l



What user/groups are the cluster-admin role bound to?
The ClusterRoleBinding for the role is with the same name
*kubectl describe clusterrolebinding cluster-admin
