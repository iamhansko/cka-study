# Scheduling

Pod Status Pending -> No Node? No Scheduler?

Filtering -> Labels / Selectors

Restrictions -> Taints(Lock for Nodes) / Tolerations(Key for Pods)
- NoSchedule
- PreferNoSchedule
- NoExecute

Selecting -> NodeSelector / NodeAffinity

NodeAffinity
- requiredDuringSchedulingIgnoredDuringExecution
- preferredDuringSchedulingIgnoredDuringExecution 

DaemonSet -> (NodeAffnity) -> Each Node

Static Pods -> /etc/kubernetes/manifests/ -> XXX.yaml (kubelet : Only Pod)
- /var/lib/kubelet/config.yaml 

Multiple Schedulers
- [Configure Multiple Scheulders](https://kubernetes.io/docs/tasks/extend-kubernetes/configure-multiple-schedulers/)
- [Pod - schedulerName](https://kubernetes.io/docs/tasks/extend-kubernetes/configure-multiple-schedulers/)
- `kubectl get events -o wide`
- `kubectl logs my-custom-scheduler-name --name-space=kube-system`
- [Extension Points](https://kubernetes.io/docs/concepts/scheduling-eviction/scheduling-framework/#interfaces) : Scheduling Queue -> Filtering -> Scoring -> Binding
- [Multiple Profiles](https://kubernetes.io/docs/reference/scheduling/config/#multiple-profiles)
- [How does the Kubernetes scheduler work?](https://jvns.ca/blog/2017/07/27/how-does-the-kubernetes-scheduler-work/)