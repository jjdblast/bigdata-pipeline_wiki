# Update the Remote Origin URL
```
local-laptop$ git remote set-url origin git@github.com:fluxcapacitor/pipeline.git
```

# Register SSH Keys Locally
* Put your private (github_rsa) and public (github_rsa.pub) keys are in the `~/.ssh/` directory.
* Run the following command and enter the passphrase used when you created the key pair:
```
local-laptop$ ssh-add ~/.ssh/github_rsa
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
* If you see this error when trying to run `ssh-add`:
```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for '/root/.ssh/github_rsa' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
```
Run the following command to tighten up access to the private key:
```
chmod 600 ~/.ssh/github_rsa
```