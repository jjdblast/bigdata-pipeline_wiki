Follow the Steps Below to Setup Github Integration for your Customizations:

## Configure Git with your Email and Name
```
git config --global user.email "chris@fregly.com"
git config --global user.name "Chris Fregly"
```

## Update the Remote Origin URL
```
git remote set-url origin git@github.com:fluxcapacitor/pipeline.git
```

## Start your SSH Authentication Agent
```
eval $(ssh-agent -s)
```


## Setup Private (github_rsa) and Public (github_rsa.pub) Keys
* Put your private (github_rsa) and public (github_rsa.pub) keys in `~/.ssh/`.

**You must modify the permissions.**
```
chmod 600 ~/.ssh/github_rsa
```

## Register SSH Keys Locally
* Run `ssh-add` and enter the passphrase used when creating the key pair

**If you see the following error, make sure you ran the `chmod 600` command above.**
```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for '/root/.ssh/github_rsa' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
```
```
ssh-add ~/.ssh/github_rsa
Enter passphrase for /root/.ssh/github_rsa: <your-passphrase>
```