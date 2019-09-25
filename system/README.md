Pre-Script: This page is totally disorganized. Either wait for me to organize it or raise a [PR](https://github.com/shammishailaj/myex).

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
