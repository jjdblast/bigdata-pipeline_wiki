## **Beta**

### Create Swarm Cluster
```
docker run --rm swarm create
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

### Docker Commands

* pipeline-master (ports 3xxxx)
```
docker run -it --privileged --name pipeline -h docker -m 6g -v ~/pipeline/data_persist:/root/pipeline/data_persist -p 80:80 -p 36042:6042 -p 39160:9160 -p 39042:9042 -p 39200:9200 -p 37077:7077 -p 38080:38080 -p 38081:38081 -p 36060:6060  -p 36061:6061 -p 36062:6062 -p 36063:6063 -p 36064:6064 -p 36065:6065 -p 32181:2181 -p 38090:8090 -p 30000:10000 -p 30070:50070 -p 30090:50090 -p 39092:9092 -p 36066:6066 -p 39000:9000 -p 39999:19999 -p 36081:6081 -p 35601:5601 -p 37979:7979 -p 38989:8989 -p 34040:4040 -p 34041:4041 -p 34042:4042 -p 34043:4043 -p 34044:4044 -p 34045:4045 -p 34046:4046 -p 34047:4047 -p 34048:4048 -p 34049:4049 -p 34050:4050 -p 34051:4051 -p 34052:4052 -p 34053:4053 -p 34054:4054 -p 34055:4055 -p 34056:4056 -p 34057:4057 -p 34058:4058 -p 34059:4059 -p 34060:4060 -p 36379:6379 -p 38888:8888 -p 34321:54321 -p 38099:8099 -p 38754:8754 -p 37379:7379 -p 36969:6969 -p 36970:6970 -p 36971:6971 -p 36972:6972 -p 36973:6973 -p 36974:6974 -p 36975:6975 -p 36976:6976 -p 36977:6977 -p 36978:6978 -p 36979:6979 -p 36980:6980 -p 35050:5050 fluxcapacitor/pipeline bash
```

* pipeline-worker-1 (1xxxx)
```
docker run -it --privileged --name pipeline-worker-1 -h docker -m 6g -v ~/pipeline/data_persist:/root/pipeline/data_persist -p 10080:80 -p 16042:6042 -p 19160:9160 -p 19042:9042 -p 19200:9200 -p 17077:7077 -p 18080:38080 -p 18081:38081 -p 16060:6060  -p 16061:6061 -p 16062:6062 -p 16063:6063 -p 16064:6064 -p 16065:6065 -p 12181:2181 -p 18090:8090 -p 10000:10000 -p 10070:50070 -p 10090:50090 -p 19092:9092 -p 16066:6066 -p 19000:9000 -p 19999:19999 -p 16081:6081 -p 15601:5601 -p 17979:7979 -p 18989:8989 -p 14040:4040 -p 14041:4041 -p 14042:4042 -p 14043:4043 -p 14044:4044 -p 14045:4045 -p 14046:4046 -p 14047:4047 -p 14048:4048 -p 14049:4049 -p 14050:4050 -p 14051:4051 -p 14052:4052 -p 14053:4053 -p 14054:4054 -p 14055:4055 -p 14056:4056 -p 14057:4057 -p 14058:4058 -p 14059:4059 -p 14060:4060 -p 16379:6379 -p 18888:8888 -p 14321:54321 -p 18099:8099 -p 18754:8754 -p 17379:7379 -p 16969:6969 -p 16970:6970 -p 16971:6971 -p 16972:6972 -p 16973:6973 -p 16974:6974 -p 16975:6975 -p 16976:6976 -p 16977:6977 -p 16978:6978 -p 16979:6979 -p 16980:6980 -p 15050:5050 fluxcapacitor/pipeline bash
```

* pipeline-worker-2 (2xxxx)
```
docker run -it --privileged --name pipeline-worker-2 -h docker -m 6g -v ~/pipeline/data_persist:/root/pipeline/data_persist -p 20080:80 -p 26042:6042 -p 29160:9160 -p 29042:9042 -p 29200:9200 -p 27077:7077 -p 28080:38080 -p 28081:38081 -p 26060:6060  -p 26061:6061 -p 26062:6062 -p 26063:6063 -p 26064:6064 -p 26065:6065 -p 22181:2181 -p 28090:8090 -p 20000:10000 -p 20070:50070 -p 20090:50090 -p 29092:9092 -p 26066:6066 -p 29000:9000 -p 29999:19999 -p 26081:6081 -p 25601:5601 -p 27979:7979 -p 28989:8989 -p 24040:4040 -p 24041:4041 -p 24042:4042 -p 24043:4043 -p 24044:4044 -p 24045:4045 -p 24046:4046 -p 24047:4047 -p 24048:4048 -p 24049:4049 -p 24050:4050 -p 24051:4051 -p 24052:4052 -p 24053:4053 -p 24054:4054 -p 24055:4055 -p 24056:4056 -p 24057:4057 -p 24058:4058 -p 24059:4059 -p 24060:4060 -p 26379:6379 -p 28888:8888 -p 24321:54321 -p 28099:8099 -p 28754:8754 -p 27379:7379 -p 26969:6969 -p 26970:6970 -p 26971:6971 -p 26972:6972 -p 26973:6973 -p 26974:6974 -p 26975:6975 -p 26976:6976 -p 26977:6977 -p 26978:6978 -p 26979:6979 -p 26980:6980 -p 25050:5050 fluxcapacitor/pipeline bash
```
