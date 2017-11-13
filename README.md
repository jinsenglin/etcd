# Usage

```
kubectl create -f etcd.yml
kubectl get po --show-all

kubectl create -f vulcand.yml
kubectl get po --show-all
```

# Files and Objects

* etcd.yml
  * pod x 3
  * service x 4
* agent1-deployment.yaml
  * pod x 1
* agent2-deployment.yaml
  * pod x 1

# References

* https://github.com/coreos/etcd/blob/master/hack/kubernetes-deploy/etcd.yml
* https://github.com/coreos/etcd/blob/master/hack/kubernetes-deploy/vulcand.yml
* https://coreos.com/etcd/docs/latest/v2/docker_guide.html
