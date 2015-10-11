# Update the Remote Origin URL
```
local-laptop$ git remote set-url origin git@github.com:fluxcapacitor/pipeline.git
```

# Register SSH Keys Locally
* Make sure your private (github_rsa) and public (github_rsa.pub) keys are in the `~/.ssh/` directory.
* Run the following command and enter the passphrase used when you created the key pair:
```
local-laptop$ ssh-add github_rsa
Enter passphrase for /root/.ssh/github_rsa: <your-passphrase>
```
* If you see this error or similar when trying to use `ssh-add`:
```
Could not open a connection to your authentication agent.
``` 
Run this command to start the authentication agent and try again:
```
eval $(ssh-agent -s)
```