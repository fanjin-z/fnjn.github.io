layout: post
title:  "Complete Guide for Hosting a CS:GO Dedicated Server"
date:   2018-05-06 00:00:00 -0800
categories: csgo-server, docker
---

This guide is tested on Debian Stretch (naive installation) and Jessie (LinuxGSM installation).

*To keep your server safe, it's always recommended to enable both key based and password login authentication.*
(Guide: [Setup SSH Authentication](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-1604)
, [Enable both authentication](https://security.stackexchange.com/questions/17931/possible-to-use-both-private-key-and-password-authentication-for-ssh-login))<br>

## Install CS:GO Dedicated Server

### Naive Installation (Not Recommended)

Create an user account name `steam` (or whatever name you like). You will be prompted to create a password.
```
adduser steam
```

Give your created account `sudo` privilege.
```
adduser steam sudo
```

Switch to your account.
```
su - steam
```

Go to your home directory.
```
cd /home/steam
```

Create a directory for SteamCMD and switch to it.
```
mkdir ~/Steam && cd ~/Steam
```

Install dependency library.
```
sudo apt-get install lib32gcc1 && sudo apt-get install lib32stdc++6
```

Download and extract SteamCMD.
```
curl -sqL "https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz" | tar zxvf -
```

Run SteamCMD and updates will automatically be download. If success, `Steam>` prompts.
```
~/Steam/steamcmd.sh
```

Login your steam account. Your will be required to enter your steam password (, and Two-factor code if Steam Guard is enabled).
```
login [Steam Username]
```

Designate install path for csgo server.
```
force_install_dir /path/to/csgo-ds/
```

Login as anonymous.
```
login anonymous
```

Install csgo dedicated server. This step takes quite a while, approx. 17 GB space will be used.
```
app_update 740 validate
```

When installation completes, quit SteamCmd.
```
quit
```

Start a casual game on dust 2 (see note). More script info [Source Dedicated Server](https://developer.valvesoftware.com/wiki/Source_Dedicated_Server).
```
/path/to/csgo-ds/srcds_run -game csgo -console -usercon +game_type 0 +game_mode 0 +mapgroup mg_active +map de_dust2 +sv_setsteamaccount [token]
```

Open CS:GO game and connect to your server. On CS:GO game console.
```
connect IP[:PORT]
```
or Search your server on community server pages `PLAY > BROWSE COMMUNITY SERVERS`. HF.

Note: To host public servers, you need `Steam Game Server Login Token (GSLT)`. Register on [Steam Game Server Account Management](https://steamcommunity.com/dev/managegameservers).

### Installation with LinuxGSM (Highly Recommended)
*Complete LinuxGSM [official guide](https://linuxgsm.com/lgsm/csgoserver/)*

Create an user account name `csgoserver` (or whatever name you like). You will be prompted to create a password.
```
adduser csgoserver
```

Give your created account `sudo` privilege.
```
adduser csgoserver sudo
```

Switch to your account.
```
su - csgoserver
```

Go to your home directory.
```
cd /home/csgoserver
```

Register Steam Game Server Login Token (GSLT) on [Steam Game Server Account Management](https://steamcommunity.com/dev/managegameservers).<br><br>

Install dependencies (Debian 64 bits). (Note: dependencies are slightly vary for different OS, please check [here](https://linuxgsm.com/lgsm/csgoserver/#ubuntu) )
```
sudo dpkg --add-architecture i386; sudo apt update; sudo apt install mailutils postfix curl wget file bzip2 gzip unzip bsdmainutils python util-linux ca-certificates binutils bc tmux lib32gcc1 libstdc++6 libstdc++6:i386
```

Download and run the script.
```
wget https://linuxgsm.com/dl/linuxgsm.sh && chmod +x linuxgsm.sh && bash linuxgsm.sh csgoserver
```

Run the installer and follow the instructions.
```
./csgoserver install
```

Start the server.
```
./csgoserver start
```

Open CS:GO game and connect to your server. On CS:GO game console.
```
connect IP[:PORT]
```
or Search your server on community server pages `PLAY > BROWSE COMMUNITY SERVERS`. HF.

## More on server management
### Setup admin
Download [Source Mod](https://www.sourcemod.net/downloads.php?branch=stable)
Download [Metamode: Source](https://www.sourcemm.net/downloads.php?branch=stable)
Download [Metamode vdf](https://www.sourcemm.net/vdf). (Note: select game `Counter-Strike: Global Offensive`)

Untar (or unzip) `sourcemod` and `sourcemm` to same folder.
```
tar -xzvf sourcemod-xxx.tar.gz -C /target/directory
tar -xzvf mmsource-xxx.tar.gz  -C /target/directory
```
Replace with newly downloaded `metamod.vdf`
```
cp /download/directory/metamod.vdf /target/directory/addons
```

Add yourself to admin. Append your SteamID and privilege to `/target/directory/addons/sourcemod/configs/admins_simple.ini` in following format.
```
"STEAM_0:1:16"		"z"
```
Your can find your steamID By [Steam ID Finder](https://steamidfinder.com/). "z" represents root privilege. More privilege info in `admin_levels.cfg` of the same folder.

Upload everything under `/target/directory` to server `csgo` directory. (If install with LinuxGSM, the path is `~/serverfiles/csgo`)
```
scp -r /target/directory csgoserver@[Server IP]:~/serverfiles/csgo
```

Start the server and connect to the server in-game.
Toggle admin by enter `!admin` on game chat or enter `say "!admin"` on console.


### Further Reading
CS:GO Dedicated Server Guide – How To Setup and Install [[link](https://segmentnext.com/2012/08/23/csgo-dedicated-server-guide-how-to-setup-and-install/)]. This article provides excellent explanation on major configuration files (e.g. gamemodes_server.txt, server.cfg). <br>
Linux GSM: Developing LGSM [[link](https://github.com/GameServerManagers/LinuxGSM/wiki/Developing-LGSM)]. This article explains how LinuxGSM works.<br>
Linux GSM: Workshop(https://github.com/GameServerManagers/LinuxGSM/wiki/Workshop)].<br>
Deathmatch Goes Advanced [[link](https://forums.alliedmods.net/showthread.php?t=233685)]. An excellent plugin for Deathmatch server.<br>

Reference:
Valve developer wiki: [SteamCMD](https://developer.valvesoftware.com/wiki/SteamCMD).<br>
Valve developer wiki: [Counter-Strike: Global Offensive Dedicated Servers](https://developer.valvesoftware.com/wiki/Counter-Strike:_Global_Offensive_Dedicated_Servers).<br>
Linux GSM: [Getting Started with csgoserver](https://linuxgsm.com/lgsm/csgoserver/#gettingstarted).<br>
Linux GSM: [Multiple Game Servers](https://github.com/GameServerManagers/LinuxGSM/wiki/Multiple-Game-Servers).<br>
How to set up Admin (sourcemod) on a CS:GO Dedicated Server [[YouTube link](https://www.youtube.com/watch?v=rpQwOMpuUQ4)].<br><br>
