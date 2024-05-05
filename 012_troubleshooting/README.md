# Troubleshooting

- [Debug Applications](https://kubernetes.io/docs/tasks/debug/debug-application/)
- Debug Control Plane

    - `kubectl get nodes`
    - `kubectl get pods -n kube-system`
    - `kubectl logs kube-apiserver -n kube-system`

- Debug Worker Nodes
    - `top`
    - `df -h`
    - `kubectl describe node NODE_NAME`
    - `service kubelet status`
    
        - `service kubelet restart`
        - /etc/kubernetes/kubelet.conf
    - `service kube-proxy status`

- Debug CoreDNS

    - Check Lists
        1. a service account named coredns
        1. cluster-roles named coredns and kube-dns
        1. clusterrolebindings named coredns and kube-dns
        1. a deployment named coredns
        1. a configmap named coredns
        1. a service named kube-dns.
    - Debug Network Plugin
    - Disable SELinux
    - coredns Deployment - allowPrivilegeEscalation : true

        ```
        kubectl -n kube-system get deployment coredns -o yaml | \
        sed 's/allowPrivilegeEscalation: false/allowPrivilegeEscalation: true/g' | \
        kubectl apply -f -
        ```
    - kubelet config.yaml -> resolveConf: [path-to-your-real-resolv-conf-file]
    - Disable local DNS cache + Restore /etc/resolv.conf to the original
    - Edit Corefile, replacing forward . /etc/resolv.conf with upstream DNS IP (ex: forward . 8.8.8.8) But this only fixes the issue for CoreDNS, kubelet will continue to forward the invalid resolv.conf to all default dnsPolicy Pods, leaving them unable to resolve DNS.