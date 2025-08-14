# Change from Ubuntu Desktop to Server

It is possible to convert Ubuntu Desktop to a CLI-only server.

You can do this with the following commands, but note that it may not remove everything:
```
sudo apt purge ubuntu-desktop gnome-shell gdm3
sudo apt autoremove --purge
sudo apt purge firefox libreoffice* thunderbird
sudo reboot
```

After reboot, you will get the terminal instead of the GUI. This should result in better performance.

Shut down the VM and configure the VirtualBox host NAT with port forwarding:
```
Name: SSH
Protocol: TCP
Host IP: (leave blank)
Host Port: 2222
Guest IP: (leave blank)
Guest Port: 22
```

Then install and enable SSH:
```
sudo apt update
sudo apt install -y openssh-server
sudo systemctl enable --now ssh
```

To connect via SSH (replace `<your-username>` with your actual username):
```
ssh -p 2222 <your-username>@localhost
```

for super weird reasons git will ask you to add your ame and email now when in the cli...???
```
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```