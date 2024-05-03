# Application Lifecycle Management

`kubectl rollout status deployment/DEPLOYMENT_NAME`

`kubectl rollout history deployment/DEPLOYMENT_NAME`

`kubectl rollout undo deployment/DEPLOYMENT_NAME`

Deployment Strategy
- Recreate : All down All up
- RollingUpdate : One by One Up+Down 

`kubectl set image deployment/DEPLOYMENT_NAME CONTAINER_NAME=IMAGE_NAME`

Configure Applications
- Command + Arguments
- Environment Variables
- Secrets

    - [Encryption](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/)
    - [K8S Secrets + AWS Secrets Manager](https://www.youtube.com/watch?v=MTnQW9MxnRI)

        1. `head -c 32 /dev/urandom | base64`
        1. enc.yaml
        1. /etc/kubernetes/manifests/kube-apiserver.yaml
            
            ```
            --encryption-provider=[Path_To_enc.yaml]
            
            ...

            volumeMounts:
            - name: enc

            ...

            volume:
            - name: enc

            ...
        
            ```

Multi Container Pod
- Scale Up/Down toegether
- Shared Localhost
- [Shared Volume](https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume)
- Design Patterns

    - Sidecar
    - Adapter
    - Ambassador

[Init Container](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)