### 📦 Kubernetes Namespace — Explained
A Namespace in Kubernetes is a way to logically isolate resources (like pods, services, etc.) within the same cluster. It’s like having multiple virtual clusters within one physical cluster.

### 🧠 Why Use Namespaces?
Namespaces help with:

✅ Environment separation: dev, staging, production
<br>
✅ Multi-team isolation: team-a, team-b
<br>
✅ Resource limits: apply quotas to specific namespaces
<br>
✅ RBAC: role-based access control per namespace

```
kubectl create ns dev
kubectl describe ns dev
```
<img width="472" height="199" alt="image" src="https://github.com/user-attachments/assets/4792ebbb-fc4e-4252-884f-21442f990dad" />

#### lets allocate quota in this namespace
nano dev.yml
```

apiVersion: v1 
kind: ResourceQuota
metadata:
  name: mem-cpu-quota
  namespace: dev 
#inside the dev namespace 
spec:
  hard:
    requests.cpu: "1" #every pod can request or guranteed that they can use 1Core of RAM
    requests.memory: "1Gi"
    limits.cpu: "2" #the pod can use max 2 core processor
    limits.memory: "2Gi"
```
<br>
#### request means all the time inside my pod a container will get this much cpu and memory

####limite means the maximum amount of cpu and memory a container can use

kubectl apply -f dev.yml

kubectl describe ns dev

<img width="559" height="254" alt="image" src="https://github.com/user-attachments/assets/c9955bdc-0bd4-4b23-96d4-d23cfeff8f42" />

#lets create a pod and utlized the resources 
```
nano q.yml
apiVersion: v1 

kind: Pod
metadata:
  name: quota-pod-1
  namespace: dev
spec:
  containers:
  - name: quota-pod-1 
    image: nginx 
    resources:
      limits:
        memory: "800Mi"
        cpu: "800m"
      requests:
        memory: "600Mi" 
        cpu: "400m"
    ports:
      - containerPort: 80
```

`kubectl apply -f q.yml`

  #lets create a pod to over utlzied the resource 
    nano q2.yml
```
apiVersion: v1 

kind: Pod
metadata:
  name: quota-pod-2
  namespace: dev
spec:
  containers:
  - name: quota-pod-2
    image: nginx 
    resources:
      limits:
        memory: "1Gi"
        cpu: "800m"
      requests:
        memory: "700Mi" # This pod requests more memory than quota allows
        cpu: "400m"
    ports:
      - containerPort: 80
```
`kubectl apply -f q2.yml`

<img width="1792" height="410" alt="image" src="https://github.com/user-attachments/assets/67f5fea4-3624-4ad5-aaa4-0994c3a22faa" />

