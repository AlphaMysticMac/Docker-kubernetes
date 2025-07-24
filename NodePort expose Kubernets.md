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
