layout: post
title:  "Building GLFW and GLM Project on Ubuntu"
date:   2018-11-16 00:00:00 -0800
categories: GLFW, GLM
---

*This guide is tested Ubuntu on 16.04.5*

Install necessary packages
```
sudo apt-get update
sudo apt-get install build-essential cmake git
sudo apt-get install libglm-dev libglfw3-dev libglew-dev
```


Clone starter code from course website.
```
git clone git@github.com:Fnjn/CSE167StarterCode.git
```

# Makefile
```
cd CSE167StarterCode
make
./mytest
```

![Success Run](/images/Building-GLFW-and-GLM-Project-on-Ubuntu/success_run2.png)

# Eclipse IDE

Download and install Eclipse CDT from https://www.eclipse.org/cdt/downloads.php<br><br>


Create an empty C++ project and copy-paste all files from Starter code.
![Project Files](/images/Building-GLFW-and-GLM-Project-on-Ubuntu/project_files.png)


Right click project >> Properties >> C/C++ General >> Paths and Symbols >> libraries, then add following library links as showed in picture.
![Link Libraries](/images/Building-GLFW-and-GLM-Project-on-Ubuntu/library_links.png)

Build >> Run
![Success Run](/images/Building-GLFW-and-GLM-Project-on-Ubuntu/success_run.png)
