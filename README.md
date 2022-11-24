Kubernetes Recources web server
This helm chart is to search and monitor the resources in the kubernetes cluster, the chart build with all the required roles and servicesaccounts.
This chard is stand alone chart.

after deploying this chart you will be able to run curl command to see the required recources:

to do that:

$ kubectl run mycurlpod --image=curlimages/curl -i --tty -- sh

after that you can get inside the pod by:

$ kubectl exec -it pod/mycurlpod -- sh

and run any of the required get commands with curl, /namespaces, /services_<namespace>, /deployments_<namespace>, /pods_<deployment>, /metrics


Prerequisites
Kubernetes 1.7+
Installing the Chart

To install the chart with the release name my-release:

## IMPORTANT: if you do not run on GKE and you want to monitor the app, it is required to allow the metricsrols.enabled on the values.yaml file
## Kubernetes Recources web server Helm chart

for istallation, clone theis repository to your machin and run this command:

$ helm install --name my-release ./reServerPack

Uninstalling the Chart
To uninstall/delete the my-release deployment:

$ helm delete my-release
The command removes all the Kubernetes components associated with the chart and deletes the release.

Configuration
The following table lists the configurable parameters of the web server chart and their default values.

│Parameter                       │ Description	                                                                 │  Default
│ replicaCount                   │                                                                               │2
│ image.repository               │                                                                               │gcr.io/the-delight-365908/resources-server
│ image.tag                      │                                                                               │1
│ imagePullSecrets               │                                                                               │[]
│ nameOverride                   │                                                                               │""
│ fullnameOverride               │                                                                               │""
│ serviceAccount.create          │ Specifies whether a service account should be created                         │true
│ serviceAccount.annotations     │ Annotations to add to the service account                                     │{}
│ serviceAccount.name            │ If not set and create is true, a name is generated using the fullname template│""
│ podAnnotations                 │                                                                               │{}
│ podSecurityContext             │                                                                               │{}
│ securityContext                │                                                                               │{}
│ service.type                   │   to be abe to curl from within the cluster do not change this type           │ClusterIP
│ service.port                   │                                                                               │80
│ ingress.enabled                │                                                                               │false
│ ingress.className              │                                                                               │"nginx"
│ ingress.annotations            │   there is cluster issuer within this chart in case of tls is not empty and   │{}
│                                │   ingres enabled certificate and secret will be created                       │      
│                                │   (depends on cert manager helm chart)                                        │      
│ ingress.hosts                  │   list of hosts                                                               │  
│                                │                                                                               │- host: web-server.local
│                                │                                                                               │  paths:
│                                │                                                                               │    - path: /
│                                │                                                                               │      pathType: Prefix
│ ingress.tls:                   │   if tls will be generated it is required to untag the                        │[]
│                                │   "cert-manager.io/cluster-issuer: letsencrypt" annotat                       │      
│ certificationServer            │   in case of certification required                                           │"https://acme-v02.api.letsencrypt.org/directory"
│ email                          │   in case of certification required                                           │"example@domain.com"
│ resources.limits.cpu           │                                                                               │15m  
│ resources.limits.memory        │                                                                               │100Mi
│ resources.requests.cpu         │                                                                               │1m
│ resources.requests.memory      │                                                                               │50Mi
│ autoscaling.enabled            │                                                                               │false
│ autoscaling.minReplicas        │                                                                               │1
│ autoscaling.maxReplicas        │                                                                               │100
│ autoscaling.targetCPUUtilizationPercentage                                                                     │80
│ metricsrols.enabled            │                                                                               │false




Specify each parameter using the --set key=value[,key=value] argument to helm install.

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

$ helm install --name my-release -f values.yaml .
Tip: You can use the default values.yaml