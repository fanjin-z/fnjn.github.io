layout: post
title:  "Turning Chromebook into a Terminal of Your Powerful Home Desktop"
date:   2018-12-03 00:00:00 -0800
categories: Chromebook, X11, Linxu
---

This tutorials will show how to turn a Chromebook (or any Debian based computer) into a terminal of any other Linux computer. These two computers can be in different networks, as long as both have Internet connection. This is super helpful if you have one cheap but portable laptop and one powerful home desktop.

## Requirement:
- A domain name.
- A server or VPS with public IP.
- Chrome OS with linux beta enabled or in development mode. [How to Set Up and Use Linux Apps on Chrome OS](https://www.howtogeek.com/363331/how-to-set-up-and-use-linux-apps-on-chrome-os/)


## Tools
- Fast Reverse Proxy (FRP v0.21) [GitHub](https://github.com/fatedier/frp)
- openssh-server
- Atom (Optional)
- Atom remote-ftp extension (Optional)
- Jupyter Notebook (Optional)
- GNU screen (Optional)


## Setup Environment
- Desktop Ubuntu 16.04
- Chromebook Version 70
- Server Debian 9.6

### Configuration
Dowload FRP ([release page](https://github.com/fatedier/frp/releases)) to Desktop, Chromebook and Server.
#### Server Configuration
Edit *frp/frps.ini*
```shell
[common]
bind_port = 7000
# Bonus Feature
vhost_http_port = 8080
```

Start frps on Server
```shell
frp/frps -c frp/frps.ini
```


### Desktop Configuration
Edit *frp/frpc.ini*
```shell
[common]
server_addr = your_server_public_ip
server_port = 7000

[secret_ssh]
type = stcp
sk = your_password
local_ip = 127.0.0.1
local_port = 22

[secret_ftp]
type = stcp
sk = your_password
local_ip = 127.0.0.1
local_port = 21

# Bonus Feature
[jupyter_notebook]
type = http
local_port = 8888
custom_domains = www.your_domian.com
```

Start frpc on Desktop
```shell
frp/frpc -c frp/frpc.ini
```

Install Openssh Server
```shell
sudo apt-get update
sudo apt-get install openssh-server
```
Make sure to enable [key-based authentication](https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server) and check out [this askubuntu answer](https://askubuntu.com/questions/420652/how-to-setup-a-restricted-sftp-server-on-ubuntu) for more security configuration details.


### Chromebook Configuration
Edit  *frp/frpc.ini*
```shell
[common]
server_addr = your_server_public_ip
server_port = 7000

[secret_ssh_visitor]
type = stcp
role = visitor
server_name = secret_ssh
sk = your_password
bind_addr = 127.0.0.1
bind_port = 6000

[secret_ftp_visitor]
type = stcp
role = visitor
server_name = secret_ftp
sk = your_password
bind_addr = 127.0.0.1
bind_port = 6001
```

Start frpc on Chromebook
```shell
frp/frpc -c frp/frpc.ini
```

ssh into Desktop
```shell
ssh -oPort=6000  [username]@127.0.0.1
```
sftp into Desktop
```shell
sftp -oPort=6000  [username]@127.0.0.1
```
Extra details on [frp/REAMDE](https://github.com/fatedier/frp/blob/master/README.md)


## Bonus Features
I find these bonus features very useful for developers.

### Jupyter Notebook
Add to Server's *frp/frps.ini*
```shell
vhost_http_port = 8080
```
Add to Desktop's *frp/frps.ini*
frp/frpc.ini
```shell
[jupyter_notebook]
type = http
local_port = 8888
custom_domains = www.your_domian.com
```

ssh into Desktop, start Jupyter Notebook and copy the one-time token. Enter Chromebook's browser
```
http://www.your_domian.com:8080/?token=[your one time token]
```

### X11 Display
Make sure x11 forwording is enabled in the machine you ssh into.
```shell
ssh -oPort=6000 -X [username]@127.0.0.1
xeyes
```
You will see a window with 2 eyes pop up.


### Atom Remote Editing
Install Atom on Chromebook
```shell
sudo apt-get update
sudo apt-get install atom
```
Install [remote-ftp](https://atom.io/packages/remote-ftp) package. This will allow remote editing and keeping files sync.

### GNU screen
```shell
sudo apt-get update
sudo apt-get install screen
```
It creates virtual terminals on top of ssh shell and allow quick resuming sessions even if network is interrupted. [screen quick reference](http://aperiodic.net/screen/quick_reference)
