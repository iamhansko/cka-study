# Cluster Maintenance

pod-eviction-timeout => "Node Dead"

## Upgrade

`kubectl drain node01` = Deployments/ReplicaSets Moved + Unschedulable

`kubectl cordon node01` = Unschedulable

`kubectl uncordon node01` = Schedulable Again

Kubernetes Software Version

- v1.29.4 = vMAJOR.MINOR.PATCH
- Kubernetes API [1](https://kubernetes.io/docs/concepts/overview/kubernetes-api/) / [2](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md) / [3](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api_changes.md)
- [Kubeadm Upgrade -> Kubelet+Kubectl Upgrade](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)
- [Change Package Repository](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/change-package-repository/#switching-to-another-kubernetes-package-repository)
- **X** : kube-apiserver
- **X-1** : controller-manager, kube-scheduler
- **X-2** : kubelet, kube-proxy
- **X-1~X+1** : kubectl
- **Y** : ETCD Cluster
- **Z** : CoreDNS
- ControlPlane -> Worker1 Down -> Worker1 Up -> Worker2 Down -> Worker2 Up -> ...

## Backup

Resource Config Backup

- `kubectl get all -A -o yaml` > all.yaml

***Or***

ETCD Backup
- ETCD Version != ETCDCTL Version
- `etcdctl snapshot save -h`
- `etcdctl snapshot restore -h`
- `ETCDCTL_API=3 etcdctl snapshot save snapshot.db`
    
    ```
    ETCDCTL_API=3 etcdctl snapshot save snapshot.db \
    --endpoints=https://127.0.0.1:2379 \
    --cacert=/etc/etcd/ca.crt \
    --cert=/etc/etcd/etcd-server.crt \
    --key=/etc/etcd/etcd-server.key
    ```
- `ETCDCTL_API=3 etcdctl snapshot restore snapshot.db --data-dir /var/lib/etc-from-backup`
- ETCD Cluster 
 
    - `service kube-apiserver stop`
    - `chown -R etcd:etcd /var/lib/etcd-from-backup`
    - /etc/systemd/system/etcd.service

        ```
        ...
        --data-dir=/var/lib/etcd-from-backup
        ...
        ```
    - `systemctl daemon-reload`
    - `service etcd restart`
    - `service kube-apiserver start`

    ***Or***

    - /etc/kubernetes/manifests/etcd.yaml

        ```
        ...
        volumes:
        - hostPath:
            path: /var/lib/etcd-from-backup
            type: DirectoryOrCreate
        name: etcd-data
        ...
        ```

    - Number of Nodes in ETCD Cluster

        ```
        ETCDCTL_API=3 etcdctl \
        --endpoints=https://127.0.0.1:2379 \
        --cacert=/etc/etcd/pki/ca.pem \
        --cert=/etc/etcd/pki/etcd.pem \
        --key=/etc/etcd/pki/etcd-key.pem \
        member list
        ```