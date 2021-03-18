# k8s
# kubectl and yaml samples

1. First pod
 1.1 Create yaml file imperatively 
   
   `k run pod-nginx --image=nginx -o yaml --dry-run=client > pod-nginx.yaml`
 
 1.2 Create pod
   
     `k apply -f pod-nginx.yaml`
 
 1.3 Verify
 
   `k get pod pod-nginx `

`
    NAME        READY   STATUS    RESTARTS   AGE
    pod-nginx   1/1     Running   0          15s
`    
 
2. Multiple Containers

 ` k run pod-multiple-nginx --image=nginx -o yaml --dry-run=client > pod-multiple-nginx.yaml`
  
  Edit the file to add second container

  `- image: nginx
    name: container2`
   
  Verify
    `k get pods`
    NAME                 READY   STATUS    RESTARTS   AGE
    pod-multiple-nginx   2/2     Running   0          5s
