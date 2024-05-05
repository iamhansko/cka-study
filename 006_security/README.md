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