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


<img width="1306" height="314" alt="image" src="https://github.com/user-attachments/assets/35e143bd-24d0-474d-bce2-a3e939c983d9" />
