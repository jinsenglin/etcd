# Usage

```bootstrap
kubectl create -f etcd.yml
kubectl get po --show-all

kubectl create -f vulcand.yml
kubectl get po --show-all

kubectl create -f agent1.yml
kubectl get po --show-all

kubectl create -f agent2.yml
kubectl get po --show-all
```

```cleanup
kubectl delete po etcd0
kubectl delete po etcd1
kubectl delete po etcd2
kubectl delete po vulcand
kubectl delete po agent1
kubectl delete po agent2
kubectl delete svc etcd-client
kubectl delete svc etcd0
kubectl delete svc etcd1
kubectl delete svc etcd2
```

# Files and Objects

* etcd.yml
  * pod x 3
  * service x 4
* vulcand.yml
  * pod x 1
* agent1.yml
  * pod x 1
* agent2.yml
  * pod x 1

# References

* https://github.com/coreos/etcd/blob/master/hack/kubernetes-deploy/etcd.yml
* https://github.com/coreos/etcd/blob/master/hack/kubernetes-deploy/vulcand.yml
* https://coreos.com/etcd/docs/latest/v2/docker_guide.html
