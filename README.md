# K8s_Network-Policies

Kindnet is the Default CNI for KIND K8s setup 

![image](https://github.com/user-attachments/assets/26a72b67-58c8-4d94-b3b8-17e4c4bc2fa4)

1. Create a Kind Cluster with Disabling the Deault CNI - kindnet using the config file as `kind-cluster-nodes-no-cni.yml`

2. Installing the CNI Addon for Weave Net (Old) Use Calico for latest
```
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
```

3. Verify the Weave Net and the Nodes
```
k get ds -n=kube-system
```
![image](https://github.com/user-attachments/assets/3cc33267-8890-4914-bd46-bf5420300d13)

```
k get nodes
```
![image](https://github.com/user-attachments/assets/95a2b112-66d0-4c0a-ae18-244d0a751b52)

```
k get pods -n=kube-system |grep weave
```
![image](https://github.com/user-attachments/assets/f86e410d-c29c-49e8-ac9f-ce04bf93e148)

4. Create an Application
```
k apply -f application.yml
```
![image](https://github.com/user-attachments/assets/c6add838-756d-404d-9fe1-992851ec2548)

5. List the Pods
```
k get pods
```
![image](https://github.com/user-attachments/assets/4e69183e-279a-4a8e-babe-a5747dcc305f)

6. Login to a Frontend Pod
```
k exec -it frontend -- bash
```

7. You can do a curl on Backend service exposed on Port 80
```
curl backend:80
```
![image](https://github.com/user-attachments/assets/2c11f4f0-e136-42b1-9192-b3d70efee1f0)

8. You can't do curl for Database so install Telnet in the same Frontend pod
```
apt-get update && apt-get install telnet
```

9. Perform a Telnet to Database 3306
```
telnet db 3306
```
![image](https://github.com/user-attachments/assets/af3c1860-1774-4f5d-bdbd-51c7bfb88a1e)

10. Now we need to restrict this using a Network Policy
```
k apply -f network_policy.yml
```

11. Verify the Network Policy created
```
k get netpol
```
![image](https://github.com/user-attachments/assets/e9c5704e-2e3f-482c-bfdd-a45cffaeacdd)
```
k describe netpol db-test
```
![image](https://github.com/user-attachments/assets/44948710-5ada-4b9f-a019-d736f2b2b344)

12. Now Login to a Frontend Pod and do a Telnet to the Database
```
telnet db 3306
```
![image](https://github.com/user-attachments/assets/f5bcf3b7-8759-4101-a937-883eb81956d7)




