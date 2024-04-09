# Monitoring report 

I'll be using the two hosts from the previous project: vm-server and vm-client.

## vm-client Ubuntu 22.04

For some reason I don't have a file /var/log/auth.log in my ubuntu client VM.
Why ? 

`apt list -a rsyslog` shows that it is installed

```
rsyslog/jammy-updates,jammy-security 8.2112.0-2ubuntu2.2 arm64  
rsyslog/jammy 8.2112.0-2ubuntu2 arm64
```

But `systemctl status rsyslog` show that it's not running (and not found)

I reinstalled it with `sudo apt install --reinstall rsyslog` and now when I run `dpkg -l | grep rsyslog`, I have the following result that indicates that it is correctly installed.

```
ii  rsyslog    8.2112.0-2ubuntu2.2   arm64   reliable system and kernel logging
```

And I now have the file `/var/log/auth.log` :) 


rsyslog.conf includes all files contained in /etc/rsyslog.d/


## Ubuntu server

### Display processes

![ps -ef output](assets/ps_output_01.png)

![Untitled](02%20-%20Monitoring%20101%2066aca3d5b5de4a7895715e7984b19bd7/Untitled%207.png)

![Untitled](02%20-%20Monitoring%20101%2066aca3d5b5de4a7895715e7984b19bd7/Untitled%208.png)

ps -ef output

![Untitled](02%20-%20Monitoring%20101%2066aca3d5b5de4a7895715e7984b19bd7/Untitled%209.png)

connection via ssh to the server

![Untitled](02%20-%20Monitoring%20101%2066aca3d5b5de4a7895715e7984b19bd7/Untitled%2010.png)



The above is the output for the command ps -ef on my linux server machine from the previous project. I'm connected to it via my m
pts/0 is the session I started via ssh from my main machine with which I connected to ssh to the server.
ow, whereas tty1 is a session on the system's first virtual console, possibly accessed directly from the machine's physical input and output devices.


In the server, I have the following file with the default configuration `/etc/rsyslog.d/50-default.conf`

### Enable logs reception from other hosts to my server

sudo nano /etc/rsyslog.conf

```bash
#################
#### MODULES ####
#################
# ...
# provides UDP syslog reception
#module(load="imudp")
#input(type="imudp" port="514")
```

We uncomment the 2 following lines to activate logs reception  
```bash
module(load="imudp")
input(type="imudp" port="514")
```
We chose UDP rather than TCP for performance reason.
The transmission will be done over port 514.

We restart rsyslog so the changes take effect
`systemctl restart rsyslog`

Now we need to configure the machine that will send the logs (here the vm-client)



