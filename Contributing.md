## Prepare Your Environment to Submit Pull Requests

### Configure Git with your Email and Name
```
git config --global user.email "<your email address>"
git config --global user.name "<your username>"
```

### Update the Remote Origin URL
```
cd $PIPELINE_HOME
git remote set-url origin git@github.com:fluxcapacitor/pipeline.git
```

### Start your SSH Authentication Agent
```
eval $(ssh-agent -s)
```

### Setup Github Keys
* Put your private `github_rsa` and public `github_rsa.pub` keys into `~/.ssh/`
* Modify permissions on this `github_rsa` file
```
chmod 600 ~/.ssh/github_rsa
```

### Register SSH Keys Locally
* Run `ssh-add` and enter the passphrase used when creating the key pair
```
ssh-add ~/.ssh/github_rsa
Enter passphrase for ~/.ssh/github_rsa: <your-passphrase>
```

### Troubleshooting
* If you see the following error, make sure you ran modified the permissions on your `github_rsa` file above
```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for '/root/.ssh/github_rsa' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
```