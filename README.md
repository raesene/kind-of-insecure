# Kind of Insecure

This is a collection of [kind](https://kind.sigs.k8s.io/) configuration files that can be used to create deliberately vulnerable clusters, for the purposes of security testing/training.

Each cluster has a single vulnerability which can be exploited.

 - insecure-port.yaml - This has the insecure port enabled on the API server.
 - etcd.yaml - This has client authentication disabled on the ETCD server.
 - unauthenticated-rw-kubelet.yaml - This has the Read/Write Kubelet port (10250/TCP) available without authentication.
 - ro-kubelet.yaml - This has the Read-Only kubelet port (10255/TCP) available without authentication.

After installing kind each test cluster can be brought up using a command like this

`kind --config insecure-port.yaml --name insecure create cluster`

You can then find out the IP address of the container on the `docker0` network with something like

`docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' insecure-control-plane`

and test away from there.