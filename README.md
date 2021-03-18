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

3. Create busybox with an echo date and sleep 
   - `k run pod-busybox --image=busybox --dry-run=client -o yaml -- /bin/sh -c 'while true;do echo $(date); sleep 10; done;' > pod-busybox.yaml`
   - `k apply -f pod-busybox.yaml`
   - Verify `k logs pod-busybox`

4. Create nginx with resource requests and limits
   - `k run pod-with-limits --image=nginx --dry-run=client -o yaml > pod-with-limits.yaml`
   - Edit to add limits
   `resources:
      requests:
        memory: "50Mi"
        cpu: "250m" 
      limits:
        memory: "75Mi"
        cpu: "300m"`
    - Verify `k get pod pod-with-limits`
  
5. Deployment with 3 replicas
   - Create yaml file `k create deployment webapp --image=nginx --replicas=3 -o yaml --dry-run=client > webapp.yaml`
   - `k apply -f webapp.yaml`
   - Verify `k get deployments.apps webapp`
6. Create service for above deployment
   - `k create service clusterip webapp-srv --tcp=8080:80 --dry-run=client -o yaml > webapp-srv.yaml`
   - Edit webapp-srv.yaml file to add Pod.spec.selector, this will slected the pods from above deployment `app: webapp`
   - `k apply -f webapp-srv.yaml `
   - Verify, you should see 3 endpoints with Ips and port in the o/p `k get endpoints webapp-srv `
   
