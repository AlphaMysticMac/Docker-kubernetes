In Kubernetes, a NodePort is a type of Service that exposes your application running in a pod to external traffic (outside the cluster) by opening a specific port on each Node in the cluster.

🔧 How NodePort Works:
Pod: You have a running application inside a pod.

Service (NodePort): You expose that pod using a service of type NodePort.

Port Allocation:

- Kubernetes assigns a port from a range (default: 30000–32767) on all worker nodes.

External Access:

 - You can access the service using: http://<NodeIP>:<NodePort>

⚙️ Basic Example (YAML)
```
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
    - port: 80          # Port exposed inside the cluster
      targetPort: 8080  # Pod's container port
      nodePort: 30080   # Optional: Kubernetes will auto-assign if omitted
This exposes the pod's port 8080 on <NodeIP>:30080.
```



####lets expose our application outside k8s cluster using nodeport
`kubectl expose deploy my-first-deployment --type=NodePort --port=80 --name=my-first-service`
` kubectl get svc`
<img width="697" height="70" alt="image" src="https://github.com/user-attachments/assets/ddc30f8d-a091-4891-a590-1ea62c348266" />

`kubectl set image deploy my-first-deployment nginx=nulldevil/phpsysinfo`

` kubectl describe deploy my-first-deployment`
<img width="1840" height="821" alt="image" src="https://github.com/user-attachments/assets/dcb26c82-5069-4f38-b524-72f3b170d235" />

<img width="1478" height="804" alt="image" src="https://github.com/user-attachments/assets/90da3d69-51f2-4ba2-aa64-d456a6410243" />

<img width="1193" height="139" alt="image" src="https://github.com/user-attachments/assets/3d01e742-390c-4da3-b1ed-bb3c081d126c" />

####################refresh the page you will see that the traffic is moving towards different pod after every minutes

##INGRESS controller


🚪 Kubernetes Ingress Controllers – Explained
An Ingress Controller is a special type of Kubernetes controller that manages external access (usually HTTP/HTTPS) to services in a Kubernetes cluster. It reads Ingress resources and configures a reverse proxy (like NGINX) to route traffic.

📌 Key Concepts
Term	Meaning
| Term                   | Meaning                                                                                   |
| ---------------------- | ----------------------------------------------------------------------------------------- |
| **Ingress**            | A Kubernetes object that defines HTTP/HTTPS routing rules to Services inside the cluster. |
| **Ingress Controller** | A pod that watches `Ingress` resources and configures a reverse proxy to serve traffic.   |

🚀 Popular Ingress Controllers
| Controller        | Notes                                                |
| ----------------- | ---------------------------------------------------- |
| **NGINX**         | Most widely used, open source and configurable.      |
| **Traefik**       | Dynamic config, great for microservices and metrics. |
| **HAProxy**       | High-performance, suitable for edge deployments.     |
| **Istio Gateway** | Works with Istio service mesh.                       |


📄 Basic Ingress YAML Example
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-service
            port:
              number: 80
```
📌 This routes traffic from http://example.com/ to the my-service service inside your cluster.


### #lets install the ingress controller 
- kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.13.0/deploy/static/provider/cloud/deploy.yaml
##### once you install the ingress controller you can see the deploy and a pod against which the ingress controller is working 
- kubectl get deploy -A


-----------------------------------------------------------------------------------------------------------------------------------
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: minimal-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - http:
      paths:
      - path: /fashion
        pathType: Prefix
        backend:
          service:
            name: fashionsrv
            port:
              number: 80
      - path: /mobile
        pathType: Prefix
        backend:
          service:
            name: mobilesrv
            port:
              number: 80
```

<img width="1196" height="854" alt="image" src="https://github.com/user-attachments/assets/21239275-7ce6-4c44-bf53-915836a8f350" />
##after the url put /fashion or /mobile 
