## Docker Volumes (Persistent Paths)

The following paths will be shared between the Host (ec2-linux or macosx-laptop) and the Docker Container:
### [MacOS X Only] boot2docker Persistent Path
```
/var/lib/boot2docker
```

### Docker Persistent Path
```
/var/lib/docker
```

Notes: 
* The `-v ~/pipeline/notebooks` maps persistent path inside the Docker Container.  This way, you won't lose changes to your Apache Zeppelin or Spark-Notebook notebooks while running inside Docker.
* Consider everything - both in-memory or on-disk - inside the Docker Container to be scoped to the life of just that Container.
* **If the Docker Container crashes or you exit the bash session, you will lose data if you're not using VOLUMES (-v)!**
* If you're other assets that you want to persist, make sure to commit/push them to git or save them outside of the ephemeral Docker Container - otherwise they may disappear.
* Be careful with mapping local paths that are under git's control.  Things get wonky sometimes.  (hand-waving)