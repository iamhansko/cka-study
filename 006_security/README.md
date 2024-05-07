# Security

## Certificate Authority
- *.key(Private Key) + *.crt(Public Key)

- Certificate Authority
    - ca.crt + ca.key (/etc/kubernetes/pki/ ->)

- [Server Certificates](https://github.com/mmumshad/kubernetes-the-hard-way/blob/master/tools/kubernetes-certs-checker.xlsx)

    - apiserver.crt + apiserver.ky

        (/etc/kubernetes/pki/ -> /etc/kubernetes/manifests/)
    - etcdserver.crt + etcdserver.key

        (/etc/kubernetes/pki/ -> /etc/kubernetes/manifests/)
    - kubelet.crt + kubelet.key 
    
        (/var/lib/kubelet/pki/ -> /etc/kubernetes/manifests/)

- Client Certificates (Things using Kube API Server, ...)

    - admin.crt + admin.key (-> .kube/config)
    - scheduler.crt + scheduler.key
    - controller-manager.crt + controller-manager.key
    - kube-proxy.crt + kube-proxy.key

- Certificates API

    - Controller Manager (CSR-APPROVING, CSR-SIGNING)
    - `cat XXX.csr | base64`
    - `vim csr.yaml`
    - `kubectl apply -f csr.yaml`
    - `kubectl get csr`
    - `kubectl certificate approve YYY`
    - `kubectl certificate deny YYY`

## $HOME/.kube/config

- Context = User + Context (+ Namespace)
- `kubectl config view`
- `kubectl config current-context`
- `kubectl config use-context CONTEXT_NAME`
- `kubectl config --kubeconfig=/root/my-kube-config use-context research`
- ``

## RBAC
- [Api Groups](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/)
    - core "/api" -> /v1 -> pods, namespaces, nodes, services, pv, pvc, ...
    - named "/apis" -> /apps -> /v1 -> /deployments, /replicasets, ...
        
        -> /networking.k8s.io -> /v1 -> /networkpolicies
    - kube proxy != kubectl proxy

    ```
    kubectl api-resources --namespaced=true
    kubectl api-resources --namespaced=false
    ```

- kube-apiserver
    
    --authorization-mode=

        - AlwaysAllow
        - AlwaysDeny
        - Node
        - RBAC
        - ABAC
        - Webhook
    
- RoleBinding / ClusterRoleBinding

    - Role / ClusterRole

        ```
        kubectl auth can-i create deployments
        kubectl auth can-i delete nodes
        kubectl auth can-i create deployments --as dev-user
        kubectl auth can-i create pods --as dev-user
        kubectl auth can-i create pods --as dev-user -n test
        ```

    - Subjects
        - User
        - Service Account (-> Create Token)
        
            ```
            kubectl exec -it kubernetes-dashboard -- ls /var/run/secrets/kubernetes.io/serviceaccount
            ```

## Security Context
[Reference](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

## Network Policy
Rule : Ingress / Egress
Supported
- weave-net
- kube-router
- calico
- romana
- ~~flannel~~