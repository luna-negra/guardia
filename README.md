# guardia

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
## Version

<details>
<summary> Beta Version </summary>
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

[ centos 8 ]
```commandline
rpm -ivh guardia-0.1.2_beta-1.el8.x86_64.rpm
```

[ ubuntu 23.04 ]
```commandline
dpkg -l guardia-0.1.2_beta-1.amd64.deb
```

After installing package successfully, 
'guardia' service daemon will be started and enabled. You can check it by using command below.
```commandline
systemctl status guardia
systemctl enable guardia
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
-  Stop Daemon
```commandline
systemctl stop guardia
```

</details>

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
        <th style="width:200px;">value format</th>
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
You can change the config value by using command 'guardia put'

```commandline
guardia put [option] [value]
```

</details>

