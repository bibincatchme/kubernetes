
*kubectl get roles

*kubectl get rolebindings

*kubectl describe role developer

*kubectl describe rolebinding devuser-developer-binding


--

* kubectl auth can-i create pods
* kubectl auth can-i create pods --as dev-user
* kubectl auth can-i list pods --namespace jenkins



//
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["create","delete","get","list","patch","update","watch"]
  resourceNames: ["blue","orange"] #we have 5 pods in same namespace and allow 2 pods for role
  //
