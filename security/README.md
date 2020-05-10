[Alpine Linux](../alpine-linux/) | [Docker](../docker/) | [S3CMD](../s3cmd/) | [Go](../go/) | [System](../system/) | [Security](../security/) | [MySQL](../mysql/) | [InfluxDB](../influxdb/)

- Generate a secure random alpha-numeric string of 18 characters

```
LC_CTYPE=C;tr -dc '[:alnum:]' < /dev/urandom | head -c ${1:-18}; echo;
```

# How to Configure a Firewall on Your Server

### UFW Overview
A firewall controls incoming and outgoing network traffic based on predefined security rules. Typically it establishes a barrier between a trusted (internal) network and untrusted external network, like the Internet.

UFW, or [Uncomplicated FireWall](https://en.wikipedia.org/wiki/Uncomplicated_Firewall), is a frontend for [IPTables](https://en.wikipedia.org/wiki/Iptables) to simplify the configuration of your firewall. It has in particular the advantage to create rules at the same time for IPv4 and for IPv6], wich simplifies the creation.

It is the the software we use in this tutorial.


- Requirements

* You have an account and are logged into [console.scaleway.com](https://console.scaleway.com/)
* You have [configured your SSH Key](https://www.scaleway.com/en/docs/configure-new-ssh-key/)
* You have a root account or sudo rights on the server

### Installing UFW

UFW is available as a pre-built package in the apt repositories of Ubuntu. It can be installed easily via apt:

```
sudo apt-get install ufw
```

### Configuring Security Policies

This tutorial is based on Virtual Cloud Instances. If you are using a BareMetal instance, make sure [not to block the nbd ports](https://www.scaleway.com/en/docs/configure-ufw-firewall-on-ubuntu-bionic-beaver/#-Enabling-ufw-on-a-BareMetal-instance) before applying the rules.

Security policies applied by the firewall on your server depend on your needs and the applications you use.

The most secure configuration is to block all traffic, inbound and outbound by default and to allow ports on a a case by case policy.

In this tutorial a policy will be configured that blocks inbound packets and authorizes outbound traffic by default.

1 . Start by defining the policy, that refuses everything by default:

```
sudo ufw default deny
```

2 . Enable outgoing traffic

```
sudo ufw default allow outgoing
```

### Establishing rules
To define rules, you have to know which services are running on the server and which are their associtated ports.

In this example, a SSH server, HTTP(S) and a DNS server are running on the machine.

Every known protocol uses an associated port from the [Well Known Ports list](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers).

The services running on the machine, used in this example have need for the following ports:

* Port 22 / TCP for SSH
* Port 80 / TCP for HTTP
* Port 443 / TCP for HTTPS
* Port 53 / TCP & UDP for DNS


1 . Authorize SSH:
```
sudo ufw allow 22/tcp
```

2 . Authorize HTTP:
```
sudo ufw allow 80/tcp
```

3 . Authorize HTTPS:
```
sudo ufw allow 443/tcp
```

4 . Authorize DNS:
```
sudo ufw allow 53
```

In this case `TCP` has not to be specified, as both, `TCP` and `UDP` are needed.

5 . Activate the new rules:
```
sudo ufw enable
```

6 . Verify the configuration:
```
sudo ufw status numbered
```

A list of all configured rules will appear:
```
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 22/tcp                     ALLOW IN    Anywhere
[ 2] 80/tcp                     ALLOW IN    Anywhere
[ 3] 443/tcp                    ALLOW IN    Anywhere
[ 4] 53                         ALLOW IN    Anywhere
[ 5] 22/tcp (v6)                ALLOW IN    Anywhere (v6)
[ 6] 80/tcp (v6)                ALLOW IN    Anywhere (v6)
[ 7] 443/tcp (v6)               ALLOW IN    Anywhere (v6)
[ 8] 53 (v6)                    ALLOW IN    Anywhere (v6)
```

Now you have a very basic, but working firewall on your server!

### Adding more Rules

As the firewall is running now, it is possible to add more rules to it:

1 . Allow the connection to port 25 (SMTP) via TCP to the server:
```
sudo ufw allow 25/TCP
```

### Deleting Rules

Over the time you may recognize, that some of the rules you defined previously don’t match your requirements anymore.

1 . Display the list of all defined rules:

```
sudo ufw status numbered
```

The numbers at the beginning of each row are the number of the rule in UFW.

2 . To delete a rule, find its number and type:

```
sudo ufw delete NUMBER
```

Make sure you are deleting the correct rule before deleting it.


### Enabling ufw on a BareMetal Instance

Scaleway BareMetal instances are using the [NBD protocol](https://en.wikipedia.org/wiki/Network_block_device) to connect the volumes. When enabling a firewall, you have to make sure not to block these ports.

1 . Change the default INPUT-policy to ACCEPT and no longer to DROP. Open the file `/etc/default/ufw` and change the entry `DEFAULT_INPUT_POLICY` as following:

```
DEFAULT_INPUT_POLICY="ACCEPT"
```

2 . Add a `DROP-ALL` rule at the end of the file `/etc/ufw/after.rules`, just before the final `COMMIT` line:

```
-A ufw-reject-input -j DROP
```

3 . Disable the UFW logging:

```
sudo ufw logging off
```

4 . Enable SSH to be able to connect to the server once the firewall is activated:

```
sudo ufw allow ssh
```

5 . Enable UFW:

```
sudo ufw enable
```

### Going further

UFW is very practical because it allows you to setup a firewall easily.
The tool is not limited to the blockage of ports. You can, for example, configure a rate-limit with it (limitation of numbers of connections per IP) and a lot of other things.

Don’t hesitate to read its manpage: `man ufw` and the [official website](https://wiki.ubuntu.com/UncomplicatedFirewall) for more information.

Source: [Scaleway Documentation](https://www.scaleway.com/en/docs/configure-ufw-firewall-on-ubuntu-bionic-beaver/)