# Networking

[Networking Model](https://kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-implement-the-kubernetes-networking-model)
- Every Pod has IP
- Every Pod communicates every other Pod in the same Node
- Every Pod communicates every other Pod on other Nodes without NAT

[Ports](https://kubernetes.io/ko/docs/reference/networking/ports-and-protocols/#%EC%BB%A8%ED%8A%B8%EB%A1%A4-%ED%94%8C%EB%A0%88%EC%9D%B8)

Cluster Network
```
ip link
ip addr
ip addr show type bridge
ip addr add 192.168.1.10/24 dev eth0
ip route
ip route add 192.168.1.0/24 via 192.168.2.1
cat /proc/sys/net/ipv4/ip_forward
arp
netstat -plnt
route
```

CNI Plugin
```
ps -aux | grep -i kubelet
ls /opt/cni/bin # Candidates
ls /etc/cni/net.d # Selected
```

Service
- [kube-proxy](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/)
    - new Service -> kube-proxy Rules updated
    - proxy mode : iptables(default) | ipvs | kernelspace
- ClusterIP : only accessible within cluster
- NodePort : on port on all nodes
- LoadBalancer : 

CoreDNS
- Pod -> kube-system -> CoreDNS
    - `kubectl get service -n kube-system`
- /etc/coredns/Corefile
- DNS
    - Service
        - http://SERVICENAME.NAMESPACE
        - http://SERVICENAME.NAMESPACE.svc
        - http://SERVICENAME.NAMESPACE.svc.cluster.local
    - Pod
        - http://XXX-XXX-XXX-XXX.NAMESPACE.pod.cluster.local

Ingress
- Single URL
- Routing (Host, Path)
- HTTPS
- Ingress Controller
    - Nginx Ingress Controller (Deployment + ConfigMap + Service + ...)
    - Ha Proxy
    - Traefik
- [rewrite-target : /](https://kubernetes.github.io/ingress-nginx/examples/rewrite/)
    ```
    # Without rewrite-target
    http://<ingress-service>:<ingress-port>/watch -> http://<watch-service>:<port>/watch
    http://<ingress-service>:<ingress-port>/wear -> http://<wear-service>:<port>/wear

    # With rewrite-target
    http://<ingress-service>:<ingress-port>/watch -> http://<watch-service>:<port>/
    http://<ingress-service>:<ingress-port>/wear -> http://<wear-service>:<port>/
    ```