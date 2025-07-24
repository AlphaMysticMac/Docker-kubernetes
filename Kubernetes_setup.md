# cks-master
sudo -i
bash <(curl -s https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/cluster-setup/latest/install_master.sh)


# cks-worker
sudo -i
bash <(curl -s https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/cluster-setup/latest/install_worker.sh)


### run the printed kubeadm-join-command from the master on the worker

- master node :
<img width="570" height="124" alt="image" src="https://github.com/user-attachments/assets/858337bb-b82a-4349-a27d-ecbca5951902" />


| Term          | Description                                                                                    |
| ------------- | ---------------------------------------------------------------------------------------------- |
| **Cluster**   | A set of machines (nodes) running Kubernetes; includes control plane and worker nodes.         |
| **Node**      | A physical or virtual machine in the cluster. Can be **Master (Control Plane)** or **Worker**. |
| **Pod**       | The **smallest deployable unit** in Kubernetes; a wrapper around one or more containers.       |
| **Container** | A lightweight, portable unit of software (e.g., Docker container) running inside a Pod.        |
| **Namespace** | Virtual cluster within a Kubernetes cluster; used for **organizing and isolating** resources.  |

<br>
<br>


| Term                         | Description                                                                            |
| ---------------------------- | -------------------------------------------------------------------------------------- |
| **kube-apiserver**           | Central management interface (all `kubectl` commands go here).                         |
| **etcd**                     | Distributed **key-value store** for all cluster data.                                  |
| **kube-scheduler**           | Assigns Pods to Nodes based on resource availability and policies.                     |
| **kube-controller-manager**  | Runs controllers (e.g., replication, node, job controllers) to maintain desired state. |
| **cloud-controller-manager** | Manages cloud provider-specific logic (e.g., AWS, GCP, Azure integration).             |

### CUSTOM POD CREATION 
- kubectl run my-second-pod --image=nginx --port=80 --dry-run=client -o yaml > pod1.yml
- cat pod1.yml
- kubectl apply -f pod1.yml
- kubectl get pod
- kubectl get nodes
- kubectl get pod -A -o wide
- cat /root/.kube/config

  
#### replica pod creation :
 - kubectl create deployment my-first-deployment --image=nginx --port=80 --replicas=2 --dry-run=client -o yaml
 - kubectl create deployment my-first-deployment --image=nginx --port=80 --replicas=2 --dry-run=client -o yaml > dep.yml
 - kubectl apply -f dep.yml
 - kubectl get deployment
 - kubectl delete pod my-first-deployment-77bdf869fc-9qrvk
 - kubectl get pod

<img width="483" height="448" alt="image" src="https://github.com/user-attachments/assets/849fefce-4c9e-41ec-88ae-940de44f7d73" />


<img width="1306" height="314" alt="image" src="https://github.com/user-attachments/assets/35e143bd-24d0-474d-bce2-a3e939c983d9" />


------------------------------------------------------------------------------------------------------------------------------------------------
<br>
### this will create your first deployment and give you two pod 
<br>    - 83  kubectl create deployment my-first-deployment --image=nginx --port=80 --replicas=2 --dry-run=client -o yaml
<br>    - 84  kubectl create deployment my-first-deployment --image=nginx --port=80 --replicas=2 --dry-run=client -o yaml > dep.yml
<br>    - 85  kubectl apply -f dep.yml
<br>    - 86  kubectl get deployment
<br>   ######this will show you two pod attach to your deployment 
<br>    - 87  kubectl get pod
<br>   ######change the pod name your pod name will not be same like mine
<br>    - 88  kubectl delete pod my-first-deployment-77bdf869fc-92gmv
<br>   ######once you delete the pod as replicas is always 2 means it will create a new pod 
<br>    - 89  kubectl get pod
<br> ######deployment explanation
<br> ###we apply the file kubectl apply -f dep.yml
<br> apiVersion: apps/v1 #########deployment api version is apps/v1
<br> kind: Deployment ###we are creating a deployment
<br> metadata:
<br>   creationTimestamp: null
<br>   labels:
<br>     app: my-first-deployment
<br>   name: my-first-deployment
<br> spec: ###specification of the deployment
<br> ###replicas , selector ,template
<br>   replicas: 2 
<br>   selector: ###.spec.selector 
<br>   ###.spec.selector.matchLabels kind of match expression 
<br>     matchLabels:
<br>       app: my-first-deployment
<br>       ###the label is in key value format
<br>   strategy: {}
<br>   template: ###isnide the pod we need to create containers 
<br>   ###how this template will be mapped to the pod 
<br>     metadata:
<br>       creationTimestamp: null
<br>       labels: ###same label we are using 
<br>         app: my-first-deployment
<br>     spec: ###this template has an specificattion which contianer the container def 
<br>       containers:
<br>       - image: nginx
<br>         name: nginx
<br>         ports:
<br>         - containerPort: 80
<br>         resources: {}
<br> status: {}
<br> ###relationship between dep+rs=pod
<br> [root@ip-172-31-115-211 ~]$ kubectl get deploy
<br> NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
<br> my-first-deployment   2/2     2            2           38m
<br> [root@ip-172-31-115-211 ~]$ kubectl get rs
<br> NAME                             DESIRED   CURRENT   READY   AGE
<br> my-first-deployment-77bdf869fc   2         2         2       38m
<br> [root@ip-172-31-115-211 ~]$ kubectl get pod
<br> NAME                                   READY   STATUS    RESTARTS   AGE
<br> my-first-deployment-77bdf869fc-f9djv   1/1     Running   0          38m
<br> my-first-deployment-77bdf869fc-rwscn   1/1     Running   0          37m
<br> my-first-pod                           1/1     Running   0          148m


### Rollout 
<br> ###how to update the deployment process
<br> ###check the name of the image the below command will update your image
<br> 107  kubectl set image deploy my-first-deployment nginx=nginx:1.16.1
<br> ############
<br>   108  kubectl get deploy
<br> ###now if you check replicaset you will find two different replicaset one is previos version and new version
<br>   109  kubectl get rs
<br> ###you can check the rollout status
<br>   110  kubectl rollout status deploy my-first-deployment
<br>   111  kubectl get pod
<br>   112  kubectl describe deployment

<img width="1099" height="848" alt="image" src="https://github.com/user-attachments/assets/6f76c5e0-19bc-457c-9c5d-3d6bb5602995" />

<br> ###lets make a mistake with an image so that my deployment hang and i can rollback to previous releaxse
<br>   113  kubectl set image deploy my-first-deployment nginx=nginx:1.161
<br> ###this will show that the deployment is in hang state press ctrl c
<br>   114  kubectl rollout status deploy my-first-deployment
<br> ###now is you look into rs it will show that there are two replicaset in running state but the wrond deploymetn will show not availabel 
<br>   115  kubectl get rs
<br>   116  kubectl get pod
<br>  ######you can check all the rollout
<br>   120  kubectl rollout history deployment my-first-deployment
<br>   121  kubectl rollout history deployment my-first-deployment --revision=1
<br>   122  kubectl rollout history deployment my-first-deployment --revision=2
<br>   123  kubectl rollout history deployment my-first-deployment --revision=3
<br> ######lets undo the deployment 
<br>   124  kubectl rollout undo deployment my-first-deployment
<br> ############now check that the deployment is up and running 
<br> kubectl get deploy
<br> 
<br> ###roll back to specific release
<br> [root@ip-172-31-115-211 ~]$ kubectl rollout undo deployment my-first-deployment --to-revision=1
<br> deployment.apps/my-first-deployment rolled back

<img width="884" height="434" alt="image" src="https://github.com/user-attachments/assets/a4d7353c-1909-4b5b-ba40-a7695399886a" />

<br> ###lets do the manual scaling 
<br>   134  kubectl get deploy
<br>   135  kubectl scale deploy my-first-deployment --replicas=5
<br>   136  kubectl get deploy
<br>   137  kubectl get pod
<br> ###the below command will show the replicas to save the file Press Esc :q!
<br>   138  kubectl edit deploy my-first-deployment
<br> ######autoscaling 
<br> [root@ip-172-31-115-211 ~]$ kubectl autoscale deploy my-first-deployment --min=5 --max=10 --cpu-percent=80
<br> horizontalpodautoscaler.autoscaling/my-first-deployment autoscaled

<img width="970" height="347" alt="image" src="https://github.com/user-attachments/assets/13384fac-5fbb-4b2d-b16c-b192f1d4c524" />

<br> [root@ip-172-31-115-211 ~]$ kubectl get hpa
<br> NAME                  REFERENCE                        TARGETS              MINPODS   MAXPODS   REPLICAS   AGE
<br> my-first-deployment   Deployment/my-first-deployment   cpu: <unknown>/80%   5         10        5          35s
<br> [root@ip-172-31-115-211 ~]$
<br> https://kubernetes.io/docs/concepts/workloads/autoscaling/
