## TIPS AND TRICKS
### How to get the cluster info?
kubectl cluster-info

### How to get the info of a specific service or pod?
- kubectl describe pod pod_name
- kubectl describe svc service_name
- kubectl describe ingress ingress_name

### How to list multiple resources with detailed metadata
Using options `-o wide` and `--show-labels=true`, we can combine different types of resources separating by a comma. For example:
```
kubectl -n bmdev get pvc,svc,po,deployment,ingress,configmap --show-labels=true -o wide
```


### How to manage multiple installed versions of kubectl
Read 
- https://gist.github.com/ntung/466e6f2724da29b65d768ee2757ffd53
- https://formulae.brew.sh/formula/kubernetes-cli

### How to enable sticky session or session affinity?
Sticky session makes sure the request is being served from the same post as long long the session hasn't been expired (see a clearer address [here](https://medium.com/@zhimin.wen/sticky-sessions-in-kubernetes-56eb0e8f257d)). See [this answer](https://stackoverflow.com/a/55471792/865603) as an example. Read another paper presenting [how to build a stateful services with Kubernetes](https://sookocheff.com/post/kubernetes/building-stateful-services/).

### How to deal with the issue "kubectl describe ingress not working"
There is an incompatibility between client and server version. To fix this issue, downgrading the client is an easier option because to what extent it is tough to change the static setting for nodes in kubernetes server. See some links below:
- https://github.com/kubernetes/kubectl/issues/675
- https://github.com/kubernetes/ingress-nginx/issues/4382

**Notes**: In my machine, there is two installations of kubectl client running in the same time. One goes with docker and the other was installed separately. Currently, the former is being used so that you need to bear in mind which one is being called.

### How to fix the issue "backend - 404"
This bug often pops over when the path property in Ingress points to a wrong location or non-existing service. You sometimes have a tiny typo so that Ingress does not know where to locate resource.

### How to fix the issue "503 Service Temporarily Unavailable"
This error is often caused due to a wrong declaration of service. Please check your yaml file. Read a solution to this issue [here](https://serverfault.com/a/869044/141514) on [serverfault](https://serverfault.com/).

### How to copy directories and files to and from Kubernetes Container
It might be handy to copy necessary files or directories between host and k8s pods, also among pods. This [blog](https://medium.com/@nnilesh7756/copy-directories-and-files-to-and-from-kubernetes-container-pod-19612fa74660) gives us some basic instructions to do what we need. To copy resources in docker, we can consult [this tutorial](https://dev.to/mfahlandt/copy-files-from-and-to-kubernetes-pods-and-docker-container-4lgh) as well.

### How to run tomcat-based applications on k8s
Very useful to read topics about how to enable sticky session for applications running in Tomcat on k8s. Let's have a look at [this hint](https://cwiki.apache.org/confluence/display/TOMCAT/ClusteringCloud).

### How to look for environment variables in a specific pod without being inside it ###
Normally, we exec a specific pod and run `printenv` to check environment variables when logging in into it. However, running the following command is likely to archieve the same outcome.

```
kubectl -n bmdev exec -it jummp-589b8fdb57-hfwf4 env | grep DB_
```
This command helps look up the list of environment variables in the pod named `jummp-589b8fdb57-hfwf4` and do search for ones having the prefix `DB_`.

### Learning resources ###
[The latest official documentation of Kubernetes.io](https://kubernetes.io/docs/home/)
[Kubernetes By Example](https://kubernetesbyexample.com/)
