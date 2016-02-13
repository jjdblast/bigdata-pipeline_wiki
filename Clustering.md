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

### List the Swarm Nodes through Docker
```
docker run swarm list token://<cluster-id>
```

### List the Swarm Nodes through `docker-machine`
```
docker-machine ls
```

### Run an Image in a Container (not named `frontend`)
```
docker run -itd -e constraint:itype!=frontend 
```

### Kill all docker-machine's
```
docker-machine kill $(docker-machine ls -q)
```