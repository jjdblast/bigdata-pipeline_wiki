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
Make sure to replace `<pipeline-home>` with the correct directory where your pipeline code lives
```
local-macosx-or-linux$ <pipeline-home>/bin/run-docker-container.sh
```

### Windows
Make sure to replace `###pipelinedirectory###` with the correct directory where your pipeline code lives **in 2 places**:
* Inside the run-docker-container.bat script
* In the command below:
```
local-windows$ ###pipelinedirectory###/bin/run-docker-container.bat
```