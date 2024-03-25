# Kubernetes hackathon

## Prerequisites
- Kubectl installation
	- https://kubernetes.io/docs/tasks/tools/
- Minikube installation
	- https://minikube.sigs.k8s.io/docs/start/
- Start the cluster using 
	 ```minikube start```
- Ensure minikube cluster context is set in **kubectl** command


## Task 1
List all kubernetes nodes using `kubectl get nodes` command and paste the output below
	    
```
NAME       STATUS   ROLES           AGE     VERSION
minikube   Ready    control-plane   2d23h   v1.28.3

```

## Task 2
describe one kubernetes node using `kubectl describe node/<node-name>` command and paste the section from the terminal output below

**Number of cpu**:     
```
Capacity:
  cpu:                8
  ephemeral-storage:  1055762868Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             3930092Ki
  pods:               110
```
**Hostname**: 
```
  InternalIP:  192.168.49.2
  Hostname:    minikube
```
**Cpu and Memory usage**:
```
  Resource           Requests    Limits
  --------           --------    ------
  cpu                750m (9%)   0 (0%)
  memory             170Mi (4%)  170Mi (4%)
```

## Task 3
List all kubernetes namespaces using `kubectl get namespace` command and paste the output below
	    
```
NAME              STATUS   AGE
default           Active   2d23h
kube-node-lease   Active   2d23h
kube-public       Active   2d23h
kube-system       Active   2d23h

```

## Task 4
List all the pods in '*kube-system*' namespace using `kubectl get pods -n <namespce command>`
	    
```
NAME                               READY   STATUS    RESTARTS        AGE
coredns-5dd5756b68-gmjm8           1/1     Running   0               2d23h
etcd-minikube                      1/1     Running   0               2d23h
kube-apiserver-minikube            1/1     Running   0               2d23h
kube-controller-manager-minikube   1/1     Running   1 (2d23h ago)   2d23h
kube-proxy-475ht                   1/1     Running   0               2d23h
kube-scheduler-minikube            1/1     Running   0               2d23h
storage-provisioner                1/1     Running   1 (2d23h ago)   2d23h

```


## Task 5

Create a namespace with name `my-namespace` using `kubectl create <namespace name>` command

List all namespaces like  in **Task 3**  and paste output below
```
PS C:\WINDOWS\system32> kubectl get namespace
NAME              STATUS   AGE
default           Active   2d23h
kube-node-lease   Active   2d23h
kube-public       Active   2d23h
kube-system       Active   2d23h
my-namespace      Active   11s

```
## Task 6

Create a Pod with name `nginx` image `nginx:latest` in `my-namespace` using yaml configurations.

1. Create yaml configuration to create a pod should run on the namespace you created in the last **Task 5**

	Refference: https://kubernetes.io/docs/concepts/workloads/pods/#using-pods
3. Use `kubectl apply -f <filename>` command to apply the configuration


Copy the yaml content below
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80

```

List all the pods in the namespace `my-namespace` using `kubectl get pods -n <namespace>` command
```
PS C:\WINDOWS\system32> kubectl get pods -n my-namespace
NAME    READY   STATUS              RESTARTS   AGE
nginx   0/1     ContainerCreating   0          3s

```

Get extra details about the pods in the namespace `my-namespace` using `kubectl get pods -n <namespace> -o wide` command
```
PS C:\WINDOWS\system32> kubectl get pods -n my-namespace -o wide
NAME    READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          83s   10.244.0.4   minikube   <none>           <none>

```

## Task 7

Get details about `nginx` pod in `my-namespace`  using `kubectl describe pods/<pod-name> -n <namespace>` command


```
Name:             nginx
Namespace:        my-namespace
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Mon, 25 Mar 2024 11:40:20 +0530
Labels:           <none>
Annotations:      <none>
Status:           Running
IP:               10.244.0.4
IPs:
  IP:  10.244.0.4
Containers:
  nginx:
    Container ID:   docker://2c77deb5a91cf39e42e5c8fd24ae333a30c28071265e218e45dddae0dbdfd8a7
    Image:          nginx:latest
    Image ID:       docker-pullable://nginx@sha256:6db391d1c0cfb30588ba0bf72ea999404f2764febf0f1f196acd5867ac7efa7e
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Mon, 25 Mar 2024 11:40:27 +0530
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-9n6js (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  kube-api-access-9n6js:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  2m15s  default-scheduler  Successfully assigned my-namespace/nginx to minikube
  Normal  Pulling    2m13s  kubelet            Pulling image "nginx:latest"
  Normal  Pulled     2m10s  kubelet            Successfully pulled image "nginx:latest" in 2.776s (2.776s including waiting)
  Normal  Created    2m9s   kubelet            Created container nginx
  Normal  Started    2m8s   kubelet            Started container nginx

```

## Task 8

Port forward the `nginx` (running in namespace `my-namespace`) pods's port `80` to your local systems port `9090` using `kubectl port-forward` command 

Refference: 
https://kubernetes.io/docs/reference/kubectl/generated/kubectl_port-forward/

```
PS C:\WINDOWS\system32> kubectl port-forward pod/nginx 9090:80
Forwarding from 127.0.0.1:9090 -> 80
Forwarding from [::1]:9090 -> 80
Handling connection for 9090

```

Open another terminal  and do a curl to the port using given command
`curl http://localhost:9000`

```
StatusCode        : 200
StatusDescription : OK
Content           : <!DOCTYPE html>
                    <html>
                    <head>
                    <title>Welcome to nginx!</title>
                    <style>
                    html { color-scheme: light dark; }
                    body { width: 35em; margin: 0 auto;
                    font-family: Tahoma, Verdana, Arial, sans-serif; }
                    </style...
RawContent        : HTTP/1.1 200 OK
                    Connection: keep-alive
                    Accept-Ranges: bytes
                    Content-Length: 615
                    Content-Type: text/html
                    Date: Mon, 25 Mar 2024 06:23:16 GMT
                    ETag: "65cce434-267"
                    Last-Modified: Wed, 14 Feb 2024 ...
Forms             : {}
Headers           : {[Connection, keep-alive], [Accept-Ranges, bytes], [Content-Length, 615], [Content-Type,
                    text/html]...}
Images            : {}
InputFields       : {}
Links             : {@{innerHTML=nginx.org; innerText=nginx.org; outerHTML=<A href="http://nginx.org/">nginx.org</A>;
                    outerText=nginx.org; tagName=A; href=http://nginx.org/}, @{innerHTML=nginx.com;
                    innerText=nginx.com; outerHTML=<A href="http://nginx.com/">nginx.com</A>; outerText=nginx.com;
                    tagName=A; href=http://nginx.com/}}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 615

```


## Task 9

List all the logs of pod `nginx` running in `my-namespace`  using `kubectl logs` command

Refference: 
https://kubernetes.io/docs/reference/kubectl/generated/kubectl_logs
```
PS C:\WINDOWS\system32> kubectl logs nginx
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/03/25 05:52:31 [notice] 1#1: using the "epoll" event method
2024/03/25 05:52:31 [notice] 1#1: nginx/1.25.4
2024/03/25 05:52:31 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14)
2024/03/25 05:52:31 [notice] 1#1: OS: Linux 5.15.133.1-microsoft-standard-WSL2
2024/03/25 05:52:31 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2024/03/25 05:52:31 [notice] 1#1: start worker processes
2024/03/25 05:52:31 [notice] 1#1: start worker process 29
2024/03/25 05:52:31 [notice] 1#1: start worker process 30
2024/03/25 05:52:31 [notice] 1#1: start worker process 31
2024/03/25 05:52:31 [notice] 1#1: start worker process 32
2024/03/25 05:52:31 [notice] 1#1: start worker process 33
2024/03/25 05:52:31 [notice] 1#1: start worker process 34
2024/03/25 05:52:31 [notice] 1#1: start worker process 35
2024/03/25 05:52:31 [notice] 1#1: start worker process 36
127.0.0.1 - - [25/Mar/2024:06:23:16 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (Windows NT; Windows NT 10.0; en-IN) WindowsPowerShell/5.1.22621.2506" "-"

```

## Task 10
Delete the pod `nginx` using the yaml configurations we created in the **Task 6**

`kubectl delete -f < yaml file name with path from task 6>`

Refference:
https://kubernetes.io/docs/reference/kubectl/generated/kubectl_delete

```
PS C:\WINDOWS\system32> kubectl delete -f "D:\texttosql\demo\k8s-basics-hackathon\nginx.yaml"
pod "nginx" deleted

```

Delete the namespace `my-namespace` using `kubectl delete namespace <namespace>` command

```
PS C:\WINDOWS\system32> kubectl delete namespace my-namespace
namespace "my-namespace" deleted

```
