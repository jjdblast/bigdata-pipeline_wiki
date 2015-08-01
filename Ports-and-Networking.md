## Apache Zeppelin
* Explicitly configured to run on port 38080 in the Docker container versus the default of 8080.  
* We do this because Apache Zeppelin automatically starts a Web Socket connection on http port + 1; where + 1 is relative to the Docker Container port (38081, in this case).  
* If we use the default port 8080, and map port 8080 to 38080 from the `docker run` command, Zeppelin will create a Web Socket connection of 8080 + 1 = 8081 instead of 38081 because it is not aware of the `docker run` mapping.
