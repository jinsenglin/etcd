# Usage

```cli
kubectl run -it --rm --image=quay.io/coreos/etcd:latest --restart=Never etcdctl -- sh

# export ETCDCTL_API=3
# etcdctl --endpoints=http://$ETCD_CLIENT_PORT_2379_TCP_ADDR:2379 member list
# etcdctl --endpoints=http://$ETCD_CLIENT_PORT_2379_TCP_ADDR:2379 put foo bar
# etcdctl --endpoints=http://$ETCD_CLIENT_PORT_2379_TCP_ADDR:2379 get foo
# etcdctl --endpoints=http://$ETCD_CLIENT_PORT_2379_TCP_ADDR:2379 put /node/foo foo
# etcdctl --endpoints=http://$ETCD_CLIENT_PORT_2379_TCP_ADDR:2379 put /node/bar bar
# etcdctl --endpoints=http://$ETCD_CLIENT_PORT_2379_TCP_ADDR:2379 get --prefix /node
# etcdctl --endpoints=http://$ETCD_CLIENT_PORT_2379_TCP_ADDR:2379 get --print-value-only --prefix /node
```

```bootstrap1
kubectl create -f etcd.yml
kubectl get po --show-all

kubectl create -f vulcand.yml
kubectl get po --show-all

kubectl create -f agent1.yml
kubectl get po --show-all

kubectl create -f agent2.yml
kubectl get po --show-all
```

```bootstrap2
export HostIP=127.0.0.1

docker run -d -v /usr/share/ca-certificates/:/etc/ssl/certs -p 4001:4001 -p 2380:2380 -p 2379:2379 \
 --name etcd quay.io/coreos/etcd:v2.3.8 \
 -name etcd0 \
 -advertise-client-urls http://${HostIP}:2379,http://${HostIP}:4001 \
 -listen-client-urls http://0.0.0.0:2379,http://0.0.0.0:4001 \
 -initial-advertise-peer-urls http://${HostIP}:2380 \
 -listen-peer-urls http://0.0.0.0:2380 \
 -initial-cluster-token etcd-cluster-1 \
 -initial-cluster etcd0=http://${HostIP}:2380 \
 -initial-cluster-state new
```

```bootstrap3
export NODE1=127.0.0.1

REGISTRY=quay.io/coreos/etcd

docker run \
  -p 2379:2379 \
  -p 2380:2380 \
  --volume=${DATA_DIR}:/etcd-data \
  --name etcd ${REGISTRY}:v3.0.17 \
  /usr/local/bin/etcd \
  --data-dir=/etcd-data --name node1 \
  --initial-advertise-peer-urls http://${NODE1}:2380 --listen-peer-urls http://0.0.0.0:2380 \
  --advertise-client-urls http://${NODE1}:2379 --listen-client-urls http://0.0.0.0:2379 \
  --initial-cluster node1=http://${NODE1}:2380
```

```bootstrap4
./etcd
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
* https://coreos.com/etcd/docs/latest/dev-guide/local_cluster.html
* https://github.com/coreos/etcd/blob/master/Documentation/op-guide/container.md
