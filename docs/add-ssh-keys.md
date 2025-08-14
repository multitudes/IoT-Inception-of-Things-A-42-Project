# Set up ssh

## From host to guest VM
To add SSH keys for SSH access from your host to the VM, you need to set up key-based authentication. Here are the steps:

## On your host machine:

1. **Generate SSH key pair (if you don't have one):**
   
```sh
   cd ~/.ssh
   ssh-keygen -t ed25519 -C "HtoVM"
   ```
. **Copy your public key to the VM:**


```sh
sudo apt update
sudo apt install -y openssh-server
sudo systemctl enable --now ssh
```
if getting error when reusing a already previously used key when recreating the installation for a second time:
Update the known_hosts with the new hash from the VM:
```sh
ssh-keygen -f "/home/lbrusa/.ssh/known_hosts" -R "[localhost]:2222"
```
then copy the ssh public key to the VM:
```sh
ssh-copy-id -i ~/.ssh/HtoVM.pub -p 2222 alimpens@localhost
```
If this fails because too many auth failures and the ssh-copy-id did not copy the key you can try
```sh
ssh-add -D
ssh-copy-id -o IdentitiesOnly=yes -i ~/.ssh/ubuntuserver22.pub -p 2222 laurent@localhost
```

If you get an error here it means that the ssh agent is giving too many keys befoer the correct one
Specify the exact key to use with -i when connecting:
```sh
ssh -i ~/.ssh/ubuntuserver22 -p 2222 laurent@localhost
```

6. **From host, test SSH access:**
   
```sh
ssh -p 2222 username@vm-ip-address
```


You should now be able to SSH into your VM without entering a password. The host's public key is stored in the VM's `~/.ssh/authorized_keys` file, allowing secure key-based authentication.


# SSH Keys for GitHub in VM

### Steps:

1. **Generate a new SSH key pair inside the VM:**
   -C is just a comment wj=hich will be appended at the end of the key ... optional 
   -f is the name of the file
   -N is the passphrase
   ```sh
      ssh-keygen -t ed25519 -C "GitHub-key" -f ~/.ssh/git -N ''
   ```


2. **Add the public key to your GitHub account:**
   - Show the public key:
     ```sh
     cat ~/.ssh/git.pub
     ```
   - Copy it and add it to GitHub → Settings → SSH and GPG keys.

   - you might want to add github to the ssh config like
   ```
   cd ~/.ssh
   vim config
   ```
   in the config you add
   ```
   Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/git
    IdentitiesOnly yes
   ``` 


   on the mac you need one extra step, add it to the agent
   ```sh
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519
   ```
   By executing eval "$(ssh-agent -s)", you're effectively telling your current shell: "Start the ssh-agent (if it's not running),
   get the commands it outputs to set up communication with it, and then execute those commands right here in this shell session.
   
5. **Test SSH access:**
   ```sh
   ssh -T git@github.com
   ```

