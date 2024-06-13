Example Scenario: Managing RBAC in a Namespace
Step 1: Create a namespace for the demo:
```
kubectl create namespace rbac-demo
```

Step 2: Create a Role with escalate and bind Verbs
```
# escalate-and-bind-role.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: rbac-demo
  name: escalate-and-bind-role
rules:
- apiGroups: ["rbac.authorization.k8s.io"]
  resources: ["roles", "rolebindings", "clusterrolebindings"]
  verbs: ["escalate", "bind","get", "list", "create"]
```

Step 4: Apply this Role
```
kubectl apply -f 01-escalate-and-bind-role.yaml
```



Step 5: Create a User to Use This Role
```
kubectl apply -f  02-SA-Demo-User.yaml
```

Step 6: Create a RoleBinding to bind the escalate-and-bind-role to the demo-user:
```
# demo-user-binding.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: demo-user-binding
  namespace: rbac-demo
subjects:
- kind: ServiceAccount
  name: demo-user
  namespace: rbac-demo
roleRef:
  kind: Role
  name: escalate-and-bind-role
  apiGroup: rbac.authorization.k8s.io
```
```
kubectl apply -f  03-demo-user-binding.yaml
```

Step 7: Now check the user privillages
```
kubectl get pods -n rbac-demo --as=system:serviceaccount:rbac-demo:demo-user

Error from server (Forbidden): pods is forbidden: User "system:serviceaccount:rbac-demo:demo-user" cannot list resource "pods" in API group "" in the namespace "rbac-demo"
```
```
kubectl auth can-i get pods -n rbac-demo --as=system:serviceaccount:rbac-demo:demo-user
no
```


Step 8: Now, assume the identity of demo-user to perform actions using their permissions. For this demo, you can use kubectl with --as option, Create a new Role with higher privileges:
```
# high-privilege-role.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: rbac-demo
  name: high-privilege-role
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["create", "delete", "update", "get", "list"]
```
```
kubectl apply -f 04-high-privilege-role.yaml --as=system:serviceaccount:rbac-demo:demo-user
```


Step 9: Binding the High-Privilege Role to Another User
```
kubectl apply -f 05-SA-Another-User.yaml
```

Step 10: Create a RoleBinding to bind the high-privilege-role to another-user:
```
# another-user-binding.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: another-user-binding
  namespace: rbac-demo
subjects:
- kind: ServiceAccount
  name: another-user
  namespace: rbac-demo
roleRef:
  kind: Role
  name: high-privilege-role
  apiGroup: rbac.authorization.k8s.io
```

```
kubectl apply -f 06-Another-user-binding.yaml --as=system:serviceaccount:rbac-demo:demo-user
```


Step 12: Verify Permissions , Switch to the context of another-user to verify that they have the high-privilege permissions:
```
kubectl auth can-i create pods --as=system:serviceaccount:rbac-demo:another-user -n rbac-demo
kubectl auth can-i delete pods --as=system:serviceaccount:rbac-demo:another-user -n rbac-demo
```

