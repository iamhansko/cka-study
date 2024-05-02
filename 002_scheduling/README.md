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