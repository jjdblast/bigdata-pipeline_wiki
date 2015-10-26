## Configure Git with your Email and Name
```
git config --global user.email "chris@fregly.com"
git config --global user.name "Chris Fregly"
```

## Update the Remote Origin URL
```
local-laptop$ git remote set-url origin git@github.com:fluxcapacitor/pipeline.git
```

## Start your SSH Authentication Agent
```
eval $(ssh-agent -s)
```

## Register SSH Keys Locally
* Put your private (github_rsa) and public (github_rsa.pub) keys are in the `~/.ssh/` directory.
* Run the following command to tighten up access to the private key:
```
chmod 600 ~/.ssh/github_rsa
```
or you may see this error when trying to run `ssh-add` next:
```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for '/root/.ssh/github_rsa' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
```
* Run `ssh-add` and enter the passphrase used when creating the key pair:
```
local-laptop$ ssh-add ~/.ssh/github_rsa
Enter passphrase for /root/.ssh/github_rsa: <your-passphrase>
```