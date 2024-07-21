# guardia
<fieldset>
- <a href="#introduction">Introduction</a><br>
- <a href="#version">Version</a><br>
- <a href="#support-os">Support OS</a><br>
- <a href="#installation">Installation</a><br>
- <a href="#how-to-use">How to Use</a><br>
- <a href="#uninstallation">Uninstallation</a><br>
</fieldset>

---
## Introduction
If you have a linux machine connected to Internet, you may see various attempt to SSH access from anywhere in this planet.
May you thought "I want to prevent these unauthorized person not to connect my machine never again".

However, we are not able to make firewall rules manually because there are so many failed ssh access logs in a day
 and there would be also false log caused by authorized user who just missing to type 'b' at the end of the password.

I, who loves linux, experienced the same situation. 
Therefore, I made a simple software 'guardia' to create linux firewall rules in response to the so called 'intruders' who try to the brute force attack.

'guardia' reacts in real time when the intruder excess the number of max_try SSH connection attempt.
When 'guardia' detected suspicious behavior, it will block the remote ip by creating an applying drop rule on active firewall zones.
In addition, 'guardia' have a function to send syslog of blocked ip and evidences.
You can easily integrate these information from multiple linux machine, by sending syslog to one device which gathers logs such as log-server.

If you worried about that you are blocked by 'guardia' because you forget the password or mistype the key, do not worry.
In 'guardia' you can register your host ip and username as a whitelist. Once whitelist recognise your username and access ip, 
you can try access to your machine unlimitedly with whitelist information without being blocked.

---
## Architecture
![image](https://github.com/user-attachments/assets/2269b29d-f695-477a-adc2-52c6ff6a8dfa)

---
## Version

<details>
<summary> Beta Test Version </summary>
<table>
    <tr>
       <th style="width:100px;">version</th>
       <th style="width:150px;">release date</th>
       <th style="width:250px;">ref</th>
       <th style="witdh:100px">derived version</th>
    </tr>
    <tr>
       <td style="text-align:center;">0.1.2_beta</td>
       <td style="text-align:center;">N/A</td>
       <td style="text-align:center;">beta: test version, the latest</td>
       <td style="text-align:center">N/A</td>
    </tr>
    <tr>
       <td style="text-align:center;">0.1.1_beta</td>
       <td style="text-align:center;">N/A</td>
       <td style="text-align:center;">beta: test version</td>
       <td style="text-align:center">N/A</td>
    </tr>
</table>
</details>

---
## Support OS

You can use 'guardia' with,
-  CentOS 8 Stream (X86)
-  Ubuntu 23.04 (amd64)

---
## Installation

<details>
<summary>Download 'guardia' repository on your linux_machine</summary>

```commandline
git clone https://github.com/luna-negra/guardia
```

The command above makes you to have the latest version of 'guardia'.
You can see the guardia folder on your path. That folder contains rpm and deb files.

</details>

<details>
<summary>Install 'guardia' package</summary>

Install 'guardia' package with command 'rpm' or 'dpgk'

[ centos 8 Stream ]
```commandline
rpm -i guardia-0.1.2_beta-1.el8.x86_64.rpm
```

[ ubuntu 23.04 ]
```commandline
dpkg -l guardia-0.1.2_beta-1.amd64.deb
```

After installing package successfully, 
'guardia' service daemon will be started and enabled. You can check it by using command below.
```commandline
systemctl status guardia
systemctl is-enabled guardia
```

</details>

---
## How To Use

Once 'guardia' daemon has started, 'guardia' watches all ssh access by referencing sshd daemon's log. 
Although you don't have to do many things except below.

### 1. Control Service Daemon
<details>
<summary>Controlling Service Daemon</summary>
Controlling 'guardia' Daemon is common on both ubuntu 23.04 and centos 8 stream


-  Start Daemon
```commandline
systemctl start guardia
```

-  Restart Daemon
```commandline
systemctl restart guardia
```
 
- Stop Daemon
```commandline
systemctl stop guardia
```

- Check Status Daemon
```commandline
systemctl status guardia
```

</details>

### 2. Get or Change Configs

<details>
<summary>Command to Check Config</summary>

You can see the config value by using command 'guardia get'
```commandline
guardia get [option]
```

[ options ]
<table>
    <tr>
        <th style="width:100px;">option</th>
        <th style="width:300px;">description</th>
        <th style="width:200px;">returned value</th>
    </tr>
    <tr> 
        <td style="text-align:center;">q_size</td>
        <td style="text-align:center;">set the capacity of built-in log queue.<br>Default is 30</td>
        <td style="text-align:center;">integer between 10 ~ 200</td>
    </tr>
    <tr> 
        <td style="text-align:center;">ip</td>
        <td style="text-align:center;">set the endpoint ip <br>where you want to send syslog.</td>
        <td style="text-align:center;">IP version 4 <br> without prefix or subnet. [x.x.x.x]</td>
    </tr>
    <tr> 
        <td style="text-align:center;">port</td>
        <td style="text-align:center;">set the endpoint port <br>where you want to send syslog.</td>
        <td style="text-align:center;">514 or not a well known-port (1024 ~)</td>
    </tr>
    <tr> 
        <td style="text-align:center;">protocol</td>
        <td style="text-align:center;">set the protocol(tcp/udp) to send syslog</td>
        <td style="text-align:center;">'udp' <br>* This version only support udp</td>
    </tr>
    <tr> 
        <td style="text-align:center;">zone</td>
        <td style="text-align:center;">set the firewall zone where the drop rule will be applied.</td>
        <td style="text-align:center;">[active_zone_name1,active_zone_name2...]</td>
    </tr>
    <tr> 
        <td style="text-align:center;">max_try</td>
        <td style="text-align:center;">Max try number of access attempt to block unauthorized connections</td>
        <td style="text-align:center;">integer between 1~10</td>
    </tr>
    <tr>
        <td style="text-align:center;">whitelist</td>
        <td style="text-align:center;">whitelist with searching keyword in prompt - ip and username</td>
        <td style="text-align:center;">string with whitelist search result</td>
    </tr>
    <tr>
        <td style="text-align:center;">log_level</td>
        <td style="text-align:center;">set the 'guardia' log level.</td>
        <td style="text-align:center;">'info' or 'debug'</td>
    </tr>
</table>

</details>

<details>
<summary>Command to Change Config</summary>

<h6 style="color:yellow;"> If you change the config or reset config, please restart guardia service.</h6>

You can change 'guardia' config values by using command 'guardia put'````

```commandline
guardia put [option] [value]
```

(1) q_size

'q_size' is the capacity of queue which is contained on the built-in 'guardia' module.
This queue stores ssh log of both failure and success temporarily.

Default is set as 30. You can set this value in range between 30 and 200.
If the installed 'guardia' miss some unauthorized access due to the massive attack,
increase this value. 

```commandline
example) guardia put q_size 100
```

(2) ip

'ip' is a ip address of remote host where you want to send syslog.

Default is '127.0.0.1'. You can set the ip with ipv4 only without prefix or subnet.
If the [value] is not match ipv4 format, config would not be changed.

```commandline
example) guardia put ip 192.168.1.1
```

(3) port

'port' is a port number of remote host where you want to send syslog.

Default is 514, basic syslog port number. You can change this value as 514 or not in range of
well known port number.

```commandline
example) guardia put port 1514
```

(4) protocol

'protocol' is the method of sending syslog. 
Default is 'udp'.

Be Advised that 'tcp' protocol is not supported now. you can only use 'udp' 

```commandline
example) guardia put protocol udp
```

(5) zone

'zone' means that the active zone where the newly created firewall rules will be applied.
Default is 'default' and new rich rule will be stored on all active zone.

This value does not accept non-active zone. Therefore, if you want to set this value with non-active zone,
change the zone status as active before set this config.

[value] must be capsuled by square bracket []. 

```commandline
example1) guardia put zone [internal, public]
example2) guardia put zone [internal]
example3) guardia put zone []                  # This means 'default'
```

(6) max_try

'max_try' is the limitation number that 'guardia' accepts unauthorized access for each host.
if one remote host fail to connect ssh with attempts excess this value, 'guardia' immediately block the remote host. 

Default is 3 and you can set this value between 1 and 5

```commandline
example) guardia put max_try 3
```

(7) log_level

'guardia' support log at '/var/log/guardia/guardia.log'. You can set the log level of 'guardia' by
selecting one of 'info' and 'debug'.

Default is 'info'. If you want to see the detail information of SSH access, please set this value as 'debug'

```commandline
example) guardia put log_level debug
```

(8) whitelist

'whitelist' makes you access without worries about mistyping or forgetting password.
Whitelist is composed of ip address and username. Even though you try to access SSH excess the number of max_try, 
'guardia' will not block you if you use whitelisted username from the host with whitelisted ip address.

One whitelist does not affect the others, so you have to register each whitelist manually, if you want to register multiple username with one ip address.
[value] must be one of 'add' and 'remove'

```commandline
example1) guardia put whitelist add        # add whitelist
example2) guardia put whitelist remove     # remove whitelist
```

You can reach input message if you type one of commands above.
Register of remove whitelist information by inputting ip address and username.
If you type username that not exist in local machine, whitelist will not be registered.

</details>

<details>
<summary>Config Reset</summary>

<h6 style="color:yellow;"> If you change the config or reset config, please restart guardia service.</h6>

You can make all configs as factory-reset form with command below.

```commandline
guardia init
```

This will make config file as a form of when installation was finished.

</details>

---

## Uninstallation

<details>
<summary>Uninstall 'guardia' package</summary>

[ CentOS 8 Stream ] 

```commandline
rpm -e guardia
```

[ Ubuntu 23.04 ]

```commandline
dpkg --purge guardia
```

</details>

---
