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

You may have wondered what the ```After=``` directive did. It simply means that your service must be started after the network is ready. If your program expects the networking to be up and running, you should add:

```After=network.target```

You may also specify the name of another service here to load this service only once that service has started. For example, here we tell systemd to start our service only when the MySQL service has already started

```After=mysql.service```


### Restarting on exit

By default, systemd does not restart your service if the program exits for whatever reason. This is usually not what you want for a service that must be always available, so we’re instructing it to always restart on exit:

```Restart=always```

You could also use on-failure to only restart if the exit status is not 0.
By default, systemd attempts a restart after 100ms. You can specify the number of seconds to wait before attempting a restart, using:

```RestartSec=1```


### Avoiding the trap: the start limit
I personally fell into this one more than once. By default, when you configure Restart=always as we did, systemd gives up restarting your service if it fails to start more than 5 times within a 10 seconds interval. Forever.
There are two ```[Unit]``` configuration options responsible for this:

```
StartLimitBurst=5
StartLimitIntervalSec=10
```

The ```RestartSec``` directive also has an impact on the outcome: if you set it to restart after 3 seconds, then you can never reach 5 failed retries within 10 seconds.
The simple fix that always works is to set ```StartLimitIntervalSec=0```. This way, systemd will attempt to restart your service forever.
It’s a good idea to set RestartSec to at least 1 second though, to avoid putting too much stress on your server when things start going wrong.

As an alternative, you can leave the default settings, and ask systemd to restart your server if the start limit is reached, using ```StartLimitAction=reboot```.

*See: [Creating a Linux service with systemd](https://medium.com/@benmorel/creating-a-linux-service-with-systemd-611b5c8b91d6)*

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
