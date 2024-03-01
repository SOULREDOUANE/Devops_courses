### Configuration

1. View the `kubeconfig` file content located at  home/ .kube/config
    ```
    kubectl config view 
2.  Switch to a different context 
    ```
    kubectl config use-context prod-user@production 
3. Set the kubeconfig file location (this tels kubectl to use kubeconfig file in this path only for executing this command with not touching the default kubeconfig file )
    ``` 
    kubectl config --kubeconfig=/root/my-kube-config use-context research  
4. Add user to kubeconfig 
kubectl config set-cluster cluster-node4 --server=https://192.168.0.15:6443 --certificate-authority=/etc/kubernetes/pki/certificate.crt && \
kubectl config set-credentials redouane-node4-user --client-certificate=/etc/kubernetes/pki/certificate2.crt --client-key=/etc/kubernetes/pki/new.key && \
kubectl config set-context redouane-node4 --cluster=cluster-node4 --user=redouane-node4-user --namespace=default && \
kubectl config use-context redouane-node4






## Kubernetes Configuration Management

This document provides instructions for managing Kubernetes configuration using `kubectl`.

1. To view the contents of the `kubeconfig` file located at `~/.kube/config`, use the following command:
    ```
     kubectl config view
2. To switch to a different context, use the `kubectl config use-context `command followed by the context name. For example, to switch to the `prod-user@production` context:
    ```
    kubectl config use-context prod-user@production
3. To specify a custom location for the kubeconfig file for a specific command without affecting the default `kubeconfig` file, use the `--kubeconfig` flag. For example, to use the kubeconfig file located at `/root/my-kube-config` for setting the context to `research`:
    ```
    kubectl config --kubeconfig=/root/my-kube-config use-context research

4. Set the cluster configuration:

    ```bash 
    kubectl config set-cluster cluster-node4 \
    --server=https://192.168.0.15:6443 \
    --certificate-authority=/etc/kubernetes/pki/certificate.crt
    ```

5. Set the user credentials:
    ```bash 
   kubectl config set-credentials redouane-node4-user \
    --client-certificate=/etc/kubernetes/pki/certificate2.crt \
    --client-key=/etc/kubernetes/pki/new.key
    ```

6. 

##  Roles and Rolebinding

1. Get all apiGroups for later use in role yaml file 
    ```bash 
    kubectl api-resources 
    ```
2. list all roles 
    ```bash 
    kubectl get roles 
    ```
3. list all rolebindings 
    ```bash
    kubectl get rolebindings
    ```
4. describe rolebinding 
    ```bash 
    kubectl describe rolebindings  name-of-rolebinding 
    ```
5. check if you can you have certain priveliges 
    ```bash 
    kubctl auth can-i create deployments 
    # verbs: list, create, update, delete, watch, get 
     ```
6. check if a certain user has prvileges 
    ```bash 
    kubctl auth can-i create deployments  --as dev-user
    # this commands is used to impersonate another user 
    kubectl auth can-i delete pods --as dev-user  --namespace dev
    ```
8. 

## cluster  kubernetes 

1. check of your kubernetes version 
    ```bash 
    kubeadm upgrade plan 
    ```
2. make the upgrade to a certain version 
    ```bash 
    kubeadm upgrade apply v1.12.0
    ```
3. upgrade kubeadm 
    ```bash 
    apt-get upgrade -y kubeadm=1.12.0-00
4. Upgrade the kublet 
    ```bash 
    apt get upgrade -y kubelet=1.12.0-00
    # after upgrading remember to restart the kubelet
    sudo systemctl restart kubelet
    ```
5. upgrade a node requires that you  `drain` the node first 
    ```bash 
    kubectl drain node-1
    ```
6. after you do all the configuration in 1, 2, 3 and 4 section  remember to `uncordon` the node, make it ready to accept scheduling nodes in it 
    ```bash 
    kubectl uncordon 
    ```
7. 

## All pod's  imperative commands 

1. Create a pod 
    ```bash 
    kubectl run nginx --image=nginx 
    ``` 
2. create a pod with `--command`, `--env`, `-l`, `--port`,and `--restart` 
    ```bash 
     kubectl run my-pod \
    --image=nginx \
    --command \
    --env="APP_ENV=production" \
    --labels="app=myapp,version=v1"\
    --port=80 \
    --restart=Never
    ```
3. create a pod and expose a service for it using the `--expose` flag:
    ```bash 
    kubectl run nginx --image=nginx --expose 
4. create a pod  yml file using imperative command with the `--dry-run` falg: 
    ```bash 
    kubectl run my-pod --image=nginx --dry-run=client -o yaml --port=80 --restart=Always > pod.yaml
    ```
5. delete a pod 
    ```bash 
    kubectl delete po  nginx 
    ## "nginx"is  pod's name  that we created already 
    ```
6. get the logs of the `nginx` pod
    ```bash
    kubectl logs nginx 
    ```
7. get all specification and status of `nginx` pod 
    ```bash 
    kubectl describe pod nginx

8. Run commands inside the pod `nginx`
    ```bash 
    #get the interactive terminal of the pod
    kubectl exec -it nginx -- bash
    #create a folder inside the pod
    kubectl exec nginx -- mkdir -p /etc/nginx-folder
    ```
9. get wide information about `nginx` pod
    ```bash 
    kubectl get pods -o wide 
    ```
10. List pods Sorted by Restart Count
    ```bash 
    kubectl get pods --sort-by='.status.containerStatuses[0].restartCount'
    ```
13. get pods info with their labels 
    ```bash 
    kubectl get pods --show-labels
    ```
14. get the number of running pods 
    ```bash 
    kubectl get po --no-headers | wc -l 
    ```
15. check if the pod's image is correct 
    ```bash
    kubectl get pods -oyaml | grep image 
    ```
16. Get metrics of node
    ```bash 
    kubectl top node my-node 
    # Show metrics for a given node
    ```

 ## All node's  imperative commands 

1. List all the nodes 
    ```bash 
    kubectl get no 
    ```
2. List all nodes with more details 
    ```bash
    kubectl get no -o wide 
    ```
3. Get yaml file of a node, `node01` for example
    ```bash
    kubectl get no node01 -oyaml 
    ```
4. Taint the node `node01` 
    ```bash
    kubectl taint node node01 app=frent:NoSchedule
    # NoSchedule, PreferNoSchedule, NoExecute
    ```
5. Untaint the node `node01`
    ```bash
    kubectl taint node node01 app-
    ```
6. Label the node `node01`
    ```bash
    kubectl label node node01 app:back
    ```
7. Unlabel the node `node01`
    ```bash
    kubectl label node node01 app-
8. Drain Node: Evict all pods from a node and mark it as unschedulable for maintenance.
    ```bash 
    kubectl drain node01  --ignore-deomonsets
    ```
9. Cordon Node: Mark a node as unschedulable, preventing new pods from being scheduled onto it.
    ```bash 
    kubectl cordon  node01
    ```
10. Uncordon Node: Mark a node as schedulable, allowing new pods to be scheduled onto it.
    ```bash 
    kubectl uncordon node01
    ```
11. Delete node `node01`
    ```bash
    kubectl delete node01
    ```
12. Get metrics of node
    ```bash 
    kubectl top node my-node 
    # Show metrics for a given node
    ```
## All Deployment Imperative commands 

1. Create a deployment 
    ```bash
    kubectl create deployment  nginx-deployment --image=nginx
    ```
2. Scale a Deployment: Scale the number of replicas of  `nginx-deployment`
    ```bash 
    kubectl scale deployment nginx-deployment --replicas=3
    ```
3. Rolling Update: Update the image of the `nginx-deployment` in a rolling manner to avoid downtime.
    ```
    kubectl set image deployment/nginx-deployment <container-name>=<new-image>
14. Check the history of updates for `nginx-deployment`
    ```bash 
    kubectl rollout history deployment nginx-deployment 
15. Roll back to pervious deployment's version 
    ```bash
    kubectl rollout undo deployment nginx-deployment --to-revision=3
    # or to the last version by 
    kubectl rollout undo deployment nginx-deployemt 
16. Describe Deployment: Describe a deployment in detail, including events and annotations.
    ```bash 
    kubectl describe deployment nginx-deployment
    ```
17. Create a deployment from yml file 
    ```bash
    kubectl create -f nginx-deployment.yml --record
    ```
18. Edit a deployment 
    ```bash
    kubectl edit deployemnt nginx-deployemnt
    ```
17. Delete a deployment
    ```bash
    kubectl delete deploymetn nginx-deployment 

## All service imperative commands

1. Create a Service: Use `kubectl create service` and ` --tcp=<port>:<targetPort>` to create a new service.
    ```bash
    kubectl create service nginx-service  --tcp=80:80
    # you can use other Protocols instead of tcp like UDP
    ```
2. Expose a Deployment: Expose a deployment as a service.
    ```bash
    kubectl expose deployment <deployment-name> --port=<port> --target-port=<target-port> --type=<service-type>
    # there are three service types ClusterIp, NodePort and LoadBalancer
4. Edit a Service: Edit the definition of a service.
    ```bash 
    kubectl edit svc nginx-service 
    ```
5. Describe a Service: Get detailed information about a service, including its endpoints.
    ```bash 
    kubeclt  descirbe svc nginx-service
    ```
6. Label a Service: Add labels to a service.
    ```bash
    kubectl label svc nginx-service app:frent
7. Label a Service: Add labels to a service.
    ```bash 
    kubectl annotate service nginx-service  <annotation-key>=<annotation-value>
    ```
8. 

## All secret imperative commands 

1. 


