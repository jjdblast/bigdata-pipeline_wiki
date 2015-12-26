# Verify the Docker Image was Loaded Successfully
### MacOS X or Windows or Linux
```
local-macosx-or-linux-or-windows$ docker images
REPOSITORY               TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
fluxcapacitor/pipeline   latest              b62e5d6e385f        36 hours ago        3.653 GB
```

# Run the Docker Container using the Loaded Image
* **THE RUN COMMAND IS VERY LONG GIVEN ALL OF THE `-p` PORT MAPPINGS.  BE SURE TO COPY/PASTE ALL OF IT!**
* If running on a large-memory host, you may want to increase the `-m 8g` value to something higher.
* If you plan to mimic my live presentation demo, you may want to change the `-p 30080:80` http port mapping to `-p 80:80`.
** Obviously, you still need to setup CNAME's and customize `config/apache2` and `config/fluxcapacitor` Apache Httpd configs.
* Contact us at help@fluxcapacitor.com with any questions or create a Github issue and I'll answer as quickly as possible.

**Ignore this message if you see it.**
```
WARNING: Your kernel does not support swap limit capabilities, memory limited without swap.
```

### MacOS X or Linux
* Confirm that `~/pipeline/data_persist` is a valid directory that will persistently store the application data - even after the Docker Container is stopped
```
local-macosx-or-linux$ docker run -it --privileged --name pipeline -h docker -m 8g -v ~/pipeline/data_persist:/root/pipeline/data_persist -p 30080:80 -p 36042:6042 -p 39160:9160 -p 39042:9042 -p 39200:9200 -p 37077:7077 -p 38080:38080 -p 38081:38081 -p 36060:6060  -p 36061:6061 -p 36062:6062 -p 36063:6063 -p 36064:6064 -p 36065:6065 -p 32181:2181 -p 38090:8090 -p 30000:10000 -p 30070:50070 -p 30090:50090 -p 39092:9092 -p 36066:6066 -p 39000:9000 -p 39999:19999 -p 36081:6081 -p 35601:5601 -p 37979:7979 -p 38989:8989 -p 34040:4040 -p 34041:4041 -p 34042:4042 -p 34043:4043 -p 34044:4044 -p 34045:4045 -p 34046:4046 -p 34047:4047 -p 34048:4048 -p 34049:4049 -p 34050:4050 -p 34051:4051 -p 34052:4052 -p 34053:4053 -p 34054:4054 -p 34055:4055 -p 34056:4056 -p 34057:4057 -p 34058:4058 -p 34059:4059 -p 34060:4060 -p 36379:6379 -p 38888:8888 -p 34321:54321 -p 38099:8099 -p 38754:8754 -p 37379:7379 -p 36969:6969 -p 36970:6970 -p 36971:6971 -p 36972:6972 -p 36973:6973 -p 3694:6974 -p 36975:6975 -p 36976:6976 -p 36977:6977 -p 36978:6978 -p 36979:6979 -p 36980:6980 fluxcapacitor/pipeline bash
```

### Windows
* Make sure to replace `###pipelinedirectory###` with the pipeline installation directory that contains a `notebooks\` subdirectory to persistently store the notebooks after the Docker Container is stopped 
```
local-windows$ docker run -it --privileged --name pipeline -h docker -m 8g -v //c/###pipelinedirectory###/data_persist:/root/pipeline/data_persist -p 30080:80 -p 34042:4042 -p 39160:9160 -p 39042:9042 -p 39200:9200 -p 37077:7077 -p 38080:38080 -p 38081:38081 -p 36060:6060  -p 36061:6061 -p 36062:6062 -p 36063:6063 -p 36064:6064 -p 36065:6065 -p 32181:2181 -p 38090:8090 -p 30000:10000 -p 30070:50070 -p 30090:50090 -p 39092:9092 -p 36066:6066 -p 39000:9000 -p 39999:19999 -p 36081:6081 -p 35601:5601 -p 37979:7979 -p 38989:8989 -p 34040:4040 -p 34041:4041 -p 34042:4042 -p 34043:4043 -p 34044:4044 -p 34045:4045 -p 34046:4046 -p 34047:4047 -p 34048:4048 -p 34049:4049 -p 34050:4050 -p 34051:4051 -p 34052:4052 -p 34053:4053 -p 34054:4054 -p 34055:4055 -p 34056:4056 -p 34057:4057 -p 34058:4058 -p 34059:4059 -p 34060:4060 -p 36379:6379 -p 38888:8888 -p 34321:54321 -p 38099:8099 -p 38754:8754 -p 37379:7379 -p 36969:6969 -p 36970:6970 -p 36971:6971 -p 36972:6972 -p 36973:6973 -p 3694:6974 -p 36975:6975 -p 36976:6976 -p 36977:6977 -p 36978:6978 -p 36979:6979 -p 36980:6980 fluxcapacitor/pipeline bash
```