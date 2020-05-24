This is a sample/proof of concept about installing Packet Tracer on a server and provide remote access to the users.

Special Thanks:
Contributors: Elaine Horn, Justin Valentino, Paul Fedele, Steve Stiles
NetAcad product team: Farsheed Tari, Giuseppe Cinque, Jozef Janitor, Kang Liu
NetAcad field team: Clydene Stangvik, Jessie Pagan, Marie Zwickert, Rebecca Chisolm
NetAcad technical field team: Echo Rantanen

Hardware & Software requirement:
To install on your server, the server should have 
Minimal memory: 1G
Minimal storage: 15G
Recommanded memory: 4G or more
The script was tested on Ubuntu 18.04.4 LTS server.

Installation:
Copy the script and "PacketTracer_730_amd64.deb" into the same folder.
You can get a copy of the Packet Tracer and learn the basic features by registering the free "Introduction to Packet Tracer" course at 
https://www.netacad.com/courses/packet-tracer 
Run the script and follow the instruction, then it will download and install the other required packages.

Update the home page:
The web page and configuration for the page can be found from a sub folder inside your installation file folder.
The ptweb.conf is the configuration file, the files in "www" folder are the default sample homepage.
ptweb-vnc
└── pt-nginx
    ├── conf
    │   └── ptweb.conf
    └── www

Default Users:
HIGHLY SUGGEST CHANGE THE PASSWORD FOR THE DEFAULT USERS!
Admin username: ptadmin
Default admin password: IlovePT
Users: pt01 (pt02-pt10 also avaiable)
Default password: 123

User Management:
Login with the admin user, goto the pulldown menu on the top right and click Settings
Click the "Users" tab, and choose the user from the list and modify the password and other settings.
In case if you break something, the default setting can be restored with the following command:
(go to the installation folder first)
sudo docker exec -i guacamole-mariadb mysql -uptdbuser -pptdbpass <  ptweb-vnc/db-dump.sql

Manage Packet Tracer activity files:
Use the following command to copy the file into the container:
(use ptvnc1 as example, please change it to the actual container name that you need to copy the files into)
sudo docker cp <source file> ptvnc1:/home/ptuser/Desktop/
to copy the files from the container, use the following command:
(Assume the file is saved on Desktop)
sudo docker cp ptvnc1:/home/ptuser/Desktop/<filename> .
You can consider mount a folder into the docker and give access to the user. 
For more details: https://docs.docker.com/storage/bind-mounts/
Please feel free to modify the script and make it work for you. 

Clipboard and Session logout:
After login to the Apache Guacamole, press the shortcut key "Ctrl+Alt+Shift", a window will popup on the left side of the screen.
Then you can use the Clipboard to copy texts into and from the remote session.
To logout, click the username on the top right of the popup window, then click Logout.
Users content will stay, they can resume the work after login again.

Known Issue:
Some container may not start due to various reasons, including but not limit to the sequence of startup, high CPU load and limited available memory.
Workaround:
Check which docker did not start and start it manually with the following steps:
use "sudo docker ps -a" to check if all the dockers are running
Sample Output:
# docker ps -a
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS                       PORTS                NAMES
d3af8c36dff4        nginx                 "nginx -g 'daemon of…"   6 minutes ago       Exited (255) 5 minutes ago   0.0.0.0:80->80/tcp   pt-nginx1
182da42ee992        guacamole/guacamole   "/opt/guacamole/bin/…"   6 minutes ago       Exited (255) 5 minutes ago   8080/tcp             pt-guacamole
cfd1e9630003        guacamole/guacd       "/bin/sh -c '/usr/lo…"   6 minutes ago       Up 5 minutes                 4822/tcp             pt-guacd
1f605ebfa2dd        ptvnc                 "/start"                 7 minutes ago       Up 5 minutes                                      ptvnc10
63ff68c02527        ptvnc                 "/start"                 7 minutes ago       Up 5 minutes                                      ptvnc9
fd506a1f16b5        ptvnc                 "/start"                 7 minutes ago       Up 5 minutes                                      ptvnc8
af505c45adf5        ptvnc                 "/start"                 7 minutes ago       Up 5 minutes                                      ptvnc7
79bb270cdb71        ptvnc                 "/start"                 7 minutes ago       Up 5 minutes                                      ptvnc6
f2694744219a        ptvnc                 "/start"                 8 minutes ago       Up 5 minutes                                      ptvnc5
8f2213b31511        ptvnc                 "/start"                 8 minutes ago       Up 5 minutes                                      ptvnc4
d3aecc321e67        ptvnc                 "/start"                 8 minutes ago       Up 5 minutes                                      ptvnc3
2fdff69bed34        ptvnc                 "/start"                 8 minutes ago       Up 5 minutes                                      ptvnc2
6b15fed09a37        ptvnc                 "/start"                 8 minutes ago       Up 5 minutes                                      ptvnc1
1f4fdbb8fdf8        mariadb:latest        "docker-entrypoint.s…"   8 minutes ago       Up 5 minutes                 3306/tcp             guacamole-mariadb

In this sample, the first 2 docker did not start, then you can start them manually with the following command:
sudo docker start pt-guacamole
sudo docker start pt-nginx1

In case if student have problem with the container assigned, you can restart the container with the following command:
(use ptvnc1 as example, please change it to the actual container name that have problem)
sudo docker restart ptvnc1
If the command above still can not solve the problem, you can recreate it with the following commands:
(CAUTION: this will delete all the files stored inside the container)
sudo docker stop ptvnc1
sudo docker rm ptvnc1
sudo docker run -d --name ptvnc1 --restart unless-stopped --cpus=0.1 -m 512M --kernel-memory 64M --oom-kill-disable --ulimit nproc=512 --ulimit nofile=1024:1024 ptvnc


