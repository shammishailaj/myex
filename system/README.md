# Pre-Script: This page is totally disorganized. Either wait for me to organize it or raise a [PR](https://github.com/shammishailaj/myex/compare)

[Alpine Linux](../alpine-linux/) | [Docker](../docker/) | [S3CMD](../s3cmd/) | [Go](../go/) | [System](../system/) | [Security](../security/) | [MySQL](../mysql/) | [InfluxDB](../influxdb/)

- System Crons

Default crons that must be set-up on every unix/linux machine for proper performance

```
0 * * * * /bin/sync;/bin/echo 3 > /proc/sys/vm/drop_caches
0 * * * * /usr/sbin/ntpdate time.nist.gov
```

- Load-Test a web URL via shell

```
for i in {0..20}; do (curl -Is https://www.example.com/ | head -n1 &) 2>/dev/null; done
```


- Packages

```
systemd-services -- provides timedatectl on ubuntu 16.04
sysstat -- provides iostat
```


- Commands

See I/O stats

```
iostat -d | tail -n +4 | head -n -1 | awk '{s+=$2} END {print s}'
```

```
iostat -d | tail -n +4 | head -n -1 | awk '{s+=$2} END {print s}'
148.66
```
```
iostat -dx nvme0n1p1 | awk '{ print $4; }'
```

List all the attached block devices
```
root@xxx:~# lsblk

NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  300G  0 disk 
└─xvda1 202:1    0  300G  0 part /
loop0     7:0    0 17.9M  1 loop /snap/amazon-ssm-agent/1068
loop1     7:1    0 87.9M  1 loop /snap/core/5328
loop2     7:2    0 89.3M  1 loop /snap/core/6673
loop3     7:3    0 12.7M  1 loop /snap/amazon-ssm-agent/495
loop4     7:4    0 89.4M  1 loop /snap/core/6818
```

Print the number of read operations per second

```
root@xxx:~# iostat -dx xvda1 | awk '{ print $4; }'
```


- Installing Filebeat v7.3 via APTtitude
1. Download and install the Public Signing Key:

```wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -```

2. You may need to install the apt-transport-https package on Debian before proceeding:

```sudo apt-get install apt-transport-https```

3. Save the repository definition to  ```/etc/apt/sources.list.d/elastic-7.x.list```:

```echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list```

OR Install alternative package which contains only features that are available under the Apache 2.0 license

```echo "deb https://artifacts.elastic.co/packages/oss-7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list```

4. Run apt-get update, and the repository is ready for use. For example, you can install Filebeat by running

```sudo apt-get update && sudo apt-get install filebeat```

OR only upgrade *filebeat*

```sudo apt-get update && sudo apt-get install --only-upgrade filebeat```

5. To configure Filebeat to start automatically during boot, run

```sudo update-rc.d filebeat defaults 95 10```

- Set-up a executable as a service on Ubuntu and compatible machines

Create a file in /etc/systemd/system/webhook.service

Put the following contents

```
[Unit]
Description=Webhook service
ConditionPathExists=/usr/local/bin/webhook
ConditionPathExists=/etc/webhook/hooks.json
After=network.target

[Service]
Type=simple
User=example_user
Group=example_user
LimitNOFILE=1024

Restart=on-failure
RestartSec=10
startLimitIntervalSec=60

WorkingDirectory=/etc/webhook
ExecStart=/usr/local/bin/webhook -hooks /etc/webhook/hooks.json -port 3001 -hotreload -nopanic

# make sure log directory exists and owned by syslog
PermissionsStartOnly=true
ExecStartPre=/bin/mkdir -p /etc/webhook
ExecStartPre=/bin/mkdir -p /var/log/webhook
ExecStartPre=/bin/chown syslog:adm /var/log/webhook
ExecStartPre=/bin/chmod 755 /var/log/webhook
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=webhook

[Install]
WantedBy=multi-user.target
```

To start the service

```# systemctl start webhook```

And automatically get it to start on boot:

```# systemctl enable webhook```

### Starting in the right order

You may have wondered what the *After=* directive did. It simply means that your service must be started after the network is ready. If your program expects the networking to be up and running, you should add:

```After=network.target```

You may also specify the name of another service here to load this service only once that service has started. For example, here we tell systemd to start our service only when the MySQL service has already started

```After=mysql.service```


### Restarting on exit

By default, systemd does not restart your service if the program exits for whatever reason. This is usually not what you want for a service that must be always available, so we’re instructing it to always restart on exit:

```Restart=always```

You could also use `on-failure` to only restart if the exit status is not 0.
By default, systemd attempts a restart after 100ms. You can specify the number of seconds to wait before attempting a restart, using:

```RestartSec=1```


### Avoiding the trap: the start limit
I personally fell into this one more than once. By default, when you configure Restart=always as we did, systemd gives up restarting your service if it fails to start more than 5 times within a 10 seconds interval. Forever.
There are two ```[Unit]``` configuration options responsible for this:


	StartLimitBurst=5
	StartLimitIntervalSec=10


The ```RestartSec``` directive also has an impact on the outcome: if you set it to restart after 3 seconds, then you can never reach 5 failed retries within 10 seconds.
The simple fix that always works is to set ```StartLimitIntervalSec=0```. This way, systemd will attempt to restart your service forever.
It’s a good idea to set RestartSec to at least 1 second though, to avoid putting too much stress on your server when things start going wrong.

As an alternative, you can leave the default settings, and ask systemd to restart your server if the start limit is reached, using ```StartLimitAction=reboot```.

*See: [Creating a Linux service with systemd](https://medium.com/@benmorel/creating-a-linux-service-with-systemd-611b5c8b91d6)*
*See: [Redirect Systemd Service logs to a filebeat](https://unix.stackexchange.com/questions/321709/redirect-systemd-service-logs-to-file)*

- Set the locale

```apt-get clean && apt-get update && apt-get install -y locales```

```locale-gen en_US.UTF-8```


- ERROR FIX::[Org.freedesktop.DBus.Error.Spawn.PermissionsInvalid: The permission of the setuid helper is not correct](https://askubuntu.com/a/826626).

usually indicates that the permissions for the dbus are incorrect. Run the following command and check your permission for the file:

```
ls -al /usr/lib/dbus-1.0/dbus-daemon-launch-helper
```

You should see something very similar to the following. Make sure that the permissions are the same:

```
-rwsr-xr-- 1 root messagebus 42992 Apr  1 10:41 /usr/lib/dbus-1.0/dbus-daemon-launch-helper*
```

If the permissions are not -rwsr-xr-- run the following command to fix the permissions:

```
sudo chmod 4754 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
```

or 

```
sudo chmod u+s /usr/lib/dbus-1.0/dbus-daemon-launch-helper
```

Then for good measure, fix the ownership if it is incorrect as well:

```
sudo chown root:messagebus /usr/lib/dbus-1.0/dbus-daemon-launch-helper
```

Try a reboot after permissions and ownership are changed.


# To ignore something in a grep command

```
grep -v 'red\|green\|blue'
```

Or 

```
grep -v red | grep -v green | grep -v blue
```

See: https://unix.stackexchange.com/a/416367/91242


# Play with line-numbers

See: https://unix.stackexchange.com/questions/288521/with-the-linux-cat-command-how-do-i-show-only-certain-lines-by-number

- Display contents of a file with line numbers

```
cat -n /path/to/file.ext
```

- Display contents of a file as per line numbers specified

- Remove line number 11 from the output

```
cat -n text.txt | sed '11d'
```

- Remove all lines from the output except line number 11

```
cat -n text.txt | sed '11!d'
```

- Remove all lines from the output except line numbers 9 through 12

```
cat -n text.txt | sed '9,12!d'
```

- Remove all lines from the output except line numbers 9 through 12 without using the `cat` command

```
sed '9,12!d' text.txt
```

- Sample File

```
$ cat file
Line 1
Line 2
Line 3
Line 4
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
```

- To print one line (5)

```
$ sed -n 5p file
Line 5
```

- To print multiple lines (5 & 8)

```
$ sed -n -e 5p -e 8p file
Line 5
Line 8
```

- To print specific range (5 - 8)

```
$ sed -n 5,8p file
Line 5
Line 6
Line 7
Line 8
```

- To print range with other specific line (5 - 8 & 10)

```
$ sed -n -e 5,8p -e 10p file
Line 5
Line 6
Line 7
Line 8
Line 10
```

- Return lines 41 through 50
```
cat /var/log/syslog -n | head -n 50 | tail -n 10
```

or 

```
cat /var/log/syslog -n | grep " 50" -b10 -a10
```

will show lines 40 through 60. The problem with the grep method is that you need to account for padding of the line numbers (notice the space)


- In awk, $1 is the first field, so the following command prints all lines whose first fields are

i) between 4 and 8 (inclusive) or,
ii) 12 or,
iii) 42

```
$ cat -n file | awk '$1>=4 && $1<=8 || $1==12 || $1==42'
 4  Line 4
 5  Line 5
 6  Line 6
 7  Line 7
 8  Line 8
12  Line 12
42  Line 42
```


- To remove the field added by cat -n to get the original lines from the file, you can do:

```
$ cat -n file | awk '$1>=4 && $1<=8 || $1==12 || $1==42{sub(/^\s*[0-9]+\s*/,""); print}'
Line 4
Line 5
Line 6
Line 7
Line 8
Line 12
Line 42
```

- Use `awk` directly to display a line number

```
awk 'NR==1' file.txt
```

will display line number 1 from file.txt


# LSOF [Source](https://www.tecmint.com/10-lsof-command-examples-in-linux/)

- See long listing of open files some of them are extracted for better understanding which displays the columns like Command, PID, USER, FD, TYPE etc.

```
# lsof

COMMAND    PID      USER   FD      TYPE     DEVICE  SIZE/OFF       NODE NAME
init         1      root  cwd      DIR      253,0      4096          2 /
init         1      root  rtd      DIR      253,0      4096          2 /
init         1      root  txt      REG      253,0    145180     147164 /sbin/init
init         1      root  mem      REG      253,0   1889704     190149 /lib/libc-2.12.so
init         1      root   0u      CHR        1,3       0t0       3764 /dev/null
init         1      root   1u      CHR        1,3       0t0       3764 /dev/null
init         1      root   2u      CHR        1,3       0t0       3764 /dev/null
init         1      root   3r     FIFO        0,8       0t0       8449 pipe
init         1      root   4w     FIFO       0,8       0t0       8449 pipe
init         1      root   5r      DIR       0,10         0          1 inotify
init         1      root   6r      DIR       0,10         0          1 inotify
init         1      root   7u     unix 0xc1513880       0t0       8450 socket
```

Most sections and their respective values are self-explanatory.

**FD** – stands for File descriptor and may seen some of the values as:

* **cwd** current working directory
* **rtd** root directory
* **txt** program text (code and data)
* **mem** memory-mapped file

Also in **FD** column numbers like **1u** is actual file descriptor and followed by **u**,**r**,**w** of it’s mode as:

* **r** for read access.
* **w** for write access.
* **u** for read and write access.


**TYPE** – of files and it’s identification.

**DIR** – Directory
**REG** – Regular file
**CHR** – Character special file.
**FIFO** – First In First Out


- List User Specific Opened Files

```
# lsof -u sam

COMMAND  PID    USER   FD   TYPE     DEVICE SIZE/OFF   NODE NAME
sshd    1838     sam  cwd    DIR      253,0     4096      2 /
sshd    1838     sam  rtd    DIR      253,0     4096      2 /
sshd    1838     sam  txt    REG      253,0   532336 188129 /usr/sbin/sshd
sshd    1838     sam  mem    REG      253,0    19784 190237 /lib/libdl-2.12.so
sshd    1838     sam  mem    REG      253,0   122436 190247 /lib/libselinux.so.1
sshd    1838     sam  mem    REG      253,0   255968 190256 /lib/libgssapi_krb5.so.2.2
sshd    1838     sam  mem    REG      253,0   874580 190255 /lib/libkrb5.so.3.3
```

The command above displays the list of all opened files for the user named **sam**

- Find Processes running on Specific Port

```
# lsof -i TCP:22

COMMAND  PID    USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
sshd    1471    root    3u  IPv4  12683      0t0  TCP *:ssh (LISTEN)
sshd    1471    root    4u  IPv6  12685      0t0  TCP *:ssh (LISTEN)
```

The command above shows all the running process for a specific port (TCP 22 in this case)


- List Only IPv4 & IPv6 Open Files

```
# lsof -i 4

COMMAND    PID    USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
rpcbind   1203     rpc    6u  IPv4  11326      0t0  UDP *:sunrpc
rpcbind   1203     rpc    7u  IPv4  11330      0t0  UDP *:954
rpcbind   1203     rpc    8u  IPv4  11331      0t0  TCP *:sunrpc (LISTEN)
avahi-dae 1241   avahi   13u  IPv4  11579      0t0  UDP *:mdns
avahi-dae 1241   avahi   14u  IPv4  11580      0t0  UDP *:58600
```
The command above shows list of open IPv4 network files


```
# lsof -i 6

COMMAND    PID    USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
rpcbind   1203     rpc    9u  IPv6  11333      0t0  UDP *:sunrpc
rpcbind   1203     rpc   10u  IPv6  11335      0t0  UDP *:954
rpcbind   1203     rpc   11u  IPv6  11336      0t0  TCP *:sunrpc (LISTEN)
rpc.statd 1277 rpcuser   10u  IPv6  11858      0t0  UDP *:55800
rpc.statd 1277 rpcuser   11u  IPv6  11862      0t0  TCP *:56428 (LISTEN)
cupsd     1346    root    6u  IPv6  12112      0t0  TCP localhost:ipp (LISTEN)
```

The command above shows list of open IPv6 network files


- List Open Files of TCP Port ranges 1-1024

```
# lsof -i TCP:1-1024

COMMAND  PID    USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
rpcbind 1203     rpc   11u  IPv6  11336      0t0  TCP *:sunrpc (LISTEN)
cupsd   1346    root    7u  IPv4  12113      0t0  TCP localhost:ipp (LISTEN)
sshd    1471    root    4u  IPv6  12685      0t0  TCP *:ssh (LISTEN)
master  1551    root   13u  IPv6  12898      0t0  TCP localhost:smtp (LISTEN)
sshd    1834    root    3r  IPv4  15101      0t0  TCP 192.168.0.2:ssh->192.168.0.1:conclave-cpp (ESTABLISHED)
sshd    1838 tecmint    3u  IPv4  15101      0t0  TCP 192.168.0.2:ssh->192.168.0.1:conclave-cpp (ESTABLISHED)
sshd    1871    root    3r  IPv4  15842      0t0  TCP 192.168.0.2:ssh->192.168.0.1:groove (ESTABLISHED)
httpd   1918    root    5u  IPv6  15991      0t0  TCP *:http (LISTEN)
httpd   1918    root    7u  IPv6  15995      0t0  TCP *:https (LISTEN)
```

Run the command above to list all the running process of open files of TCP Port ranges from 1-1024.

- Exclude User with ‘^’ Character

```
# lsof -i -u^root

COMMAND    PID    USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
rpcbind   1203     rpc    6u  IPv4  11326      0t0  UDP *:sunrpc
rpcbind   1203     rpc    7u  IPv4  11330      0t0  UDP *:954
rpcbind   1203     rpc    8u  IPv4  11331      0t0  TCP *:sunrpc (LISTEN)
rpcbind   1203     rpc    9u  IPv6  11333      0t0  UDP *:sunrpc
rpcbind   1203     rpc   10u  IPv6  11335      0t0  UDP *:954
rpcbind   1203     rpc   11u  IPv6  11336      0t0  TCP *:sunrpc (LISTEN)
avahi-dae 1241   avahi   13u  IPv4  11579      0t0  UDP *:mdns
avahi-dae 1241   avahi   14u  IPv4  11580      0t0  UDP *:58600
rpc.statd 1277 rpcuser    5r  IPv4  11836      0t0  UDP *:soap-beep
rpc.statd 1277 rpcuser    8u  IPv4  11850      0t0  UDP *:55146
rpc.statd 1277 rpcuser    9u  IPv4  11854      0t0  TCP *:32981 (LISTEN)
rpc.statd 1277 rpcuser   10u  IPv6  11858      0t0  UDP *:55800
rpc.statd 1277 rpcuser   11u  IPv6  11862      0t0  TCP *:56428 (LISTEN)
```

Here, we have excluded root user. You can exclude a particular user using ‘^’ with command as shown above.

- Find Out who’s Looking What Files and Commands?

```
# lsof -i -u sam

COMMAND  PID    USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
bash    1839     sam  cwd    DIR  253,0    12288   15 /etc
ping    2525     sam  cwd    DIR  253,0    12288   15 /etc
```

Above example shows user **sam** is using command like **ping** and **/etc** directory

- List all Network Connections

```
# lsof -i

COMMAND    PID    USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
rpcbind   1203     rpc    6u  IPv4  11326      0t0  UDP *:sunrpc
rpcbind   1203     rpc    7u  IPv4  11330      0t0  UDP *:954
rpcbind   1203     rpc   11u  IPv6  11336      0t0  TCP *:sunrpc (LISTEN)
avahi-dae 1241   avahi   13u  IPv4  11579      0t0  UDP *:mdns
avahi-dae 1241   avahi   14u  IPv4  11580      0t0  UDP *:58600
rpc.statd 1277 rpcuser   11u  IPv6  11862      0t0  TCP *:56428 (LISTEN)
cupsd     1346    root    6u  IPv6  12112      0t0  TCP localhost:ipp (LISTEN)
cupsd     1346    root    7u  IPv4  12113      0t0  TCP localhost:ipp (LISTEN)
sshd      1471    root    3u  IPv4  12683      0t0  TCP *:ssh (LISTEN)
master    1551    root   12u  IPv4  12896      0t0  TCP localhost:smtp (LISTEN)
master    1551    root   13u  IPv6  12898      0t0  TCP localhost:smtp (LISTEN)
sshd      1834    root    3r  IPv4  15101      0t0  TCP 192.168.0.2:ssh->192.168.0.1:conclave-cpp (ESTABLISHED)
httpd     1918    root    5u  IPv6  15991      0t0  TCP *:http (LISTEN)
httpd     1918    root    7u  IPv6  15995      0t0  TCP *:https (LISTEN)
clock-app 2362   narad   21u  IPv4  22591      0t0  TCP 192.168.0.2:45284->www.gov.com:http (CLOSE_WAIT)
chrome    2377   narad   61u  IPv4  25862      0t0  TCP 192.168.0.2:33358->maa03s04-in-f3.1e100.net:http (ESTABLISHED)
chrome    2377   narad   80u  IPv4  25866      0t0  TCP 192.168.0.2:36405->bom03s01-in-f15.1e100.net:http (ESTABLISHED)
```

The command above with option ‘-i’ shows the list of all network connections ‘LISTENING & ESTABLISHED’.


- Search by PID

```
# lsof -p 1

COMMAND PID USER   FD   TYPE     DEVICE SIZE/OFF   NODE NAME
init      1 root  cwd    DIR      253,0     4096      2 /
init      1 root  rtd    DIR      253,0     4096      2 /
init      1 root  txt    REG      253,0   145180 147164 /sbin/init
init      1 root  mem    REG      253,0  1889704 190149 /lib/libc-2.12.so
init      1 root  mem    REG      253,0   142472 189970 /lib/ld-2.12.so
```

The example above only shows whose PID is 1 [One].


- Kill all Activity of Particular User

```
kill -9 `lsof -t -u sam`
```

Command above will kills all the processes of **sam** user.


# Disabling MOTD (Message of The Day) on Ubuntu 20.04

In the first step we will disable dynamic MOTD news. To do so edit the `/etc/default/motd-news` file and change:


FROM:

```
ENABLED=1
```

TO:

```
ENABLED=0
```

This will disable the dynamic MOTD news upon login including SSH login.

The actual MOTD message consists of multiple parts each providing a different information. For example package upgrade information, file-system check etc. The location of MOTD scripts is `/etc/update-motd.d` and by removing the executable permissions of any particular script will disable the script's message to show up on MOTD.

Disabling parts of the MOTD dynamic message is simple. Locate any part of the script you wish to disable and remove executable permissions by using the `chmod` command. For example the following command will disable updates available message:

```
sudo chmod -x /etc/update-motd.d/90-updates-available
```

To disable and hence silent all MOTD messages for all users simply run command:

```
sudo chmod -x /etc/update-motd.d/*
```

To re-enable MOTD messages enter:

```
sudo chmod +x /etc/update-motd.d/*
```

It is also possible to disable MOTD messages on per user basis by creating an empty `.hushlogin` file in users directory. Login to the Ubuntu system you wish to disable MOTD messages and execute:

```
touch $HOME/.hushlogin
```

See:
[Original Article](https://linuxconfig.org/disable-dynamic-motd-and-news-on-ubuntu-20-04-focal-fossa-linux)
Other Articles:
[Customize Your MOTD Login Message In Debian And Ubuntu](https://ownyourbits.com/2017/04/05/customize-your-motd-login-message-in-debian-and-ubuntu/)
