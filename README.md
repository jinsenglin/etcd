# etcd

# deployment

name: my-cluster

svcs:
* svc 1: my-cluster-metadata-server

pods:
* pod 1: etcd-server
* pod 2: etcd-client1
* pod 3: etcd-client2
