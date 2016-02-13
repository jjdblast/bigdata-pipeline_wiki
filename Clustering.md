### Create Swarm Cluster
```
docker run swarm create
<cluster-id> <-- cluster id used for swarm discovery
```

### Create Swarm Master
```
docker-machine create -d virtualbox --swarm --swarm-master --swarm-discovery token://<cluster-id> swarm-master
```

### Create Swarm Nodes
* Front-end (1 node)
```
docker-machine create -d virtualbox --engine-label itype=frontend --swarm --swarm-discovery token://<cluster-id> swarm-node-01
```
* Back-end (2 nodes)
```
docker-machine create -d virtualbox --swarm --swarm-discovery token://<cluster-id> swarm-node-02
docker-machine create -d virtualbox --swarm --swarm-discovery token://<cluster-id> swarm-node-03
```

### Configure Shell to Communicate with Swarm Master
```
eval "$(docker-machine env --swarm swarm-master)"
```

### List the Swarm Nodes
* Verify `swarm-master` is ACTIVE for your shell
```
docker-machine ls
NAME            ACTIVE      DRIVER       STATE     URL                         SWARM                   DOCKER    ERRORS
default         -           virtualbox   Running   tcp://192.168.99.100:2376                           v1.10.1
swarm-master    * (swarm)   virtualbox   Running   tcp://192.168.99.101:2376   swarm-master (master)   v1.10.1
swarm-node-01   -           virtualbox   Running   tcp://192.168.99.102:2376   swarm-master            v1.10.1
swarm-node-02   -           virtualbox   Running   tcp://192.168.99.103:2376   swarm-master            v1.10.1
swarm-node-03   -           virtualbox   Running   tcp://192.168.99.104:2376   swarm-master            v1.10.1
```

### Run an Image in a Container (not named `frontend`)
```
docker run -itd -e constraint:itype!=frontend 
```

### Kill all docker-machine's
```
docker-machine kill $(docker-machine ls -q)
```