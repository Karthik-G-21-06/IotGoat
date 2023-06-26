# IOTGOAT challenges

## weak and guessable/harcoded passwords
for the first challenge since the password is hardcoded we need to find the password from the firmware. So download the [image](https://github.com/OWASP/IoTGoat/releases/download/v1.0/IoTGoat-raspberry-pi2.img) use binwalk to exract the firmware .

``` binwalk -e filename```  

Now acess the squashfs-root directory. Most of  the unix-based filesystem have all the important information and system configuration in the /etc direcctory. inside this directory we the passwd file which should contain our required username and password.

```
➜  squashfs-root cd etc && cat passwd
root:x:0:0:root:/root:/bin/ash
daemon:*:1:1:daemon:/var:/bin/false
ftp:*:55:55:ftp:/home/ftp:/bin/false
network:*:101:101:network:/var:/bin/false
nobody:*:65534:65534:nobody:/var:/bin/false
dnsmasq:x:453:453:dnsmasq:/var/run/dnsmasq:/bin/false
iotgoatuser:x:1000:1000::/root:/bin/ash
```
In this iotgoatuser is just defined with password, groupid username etc. but a=in the field of password there is a 'x' which means the passsord is hashed and saved in etc/shadow file. 
```
➜  squashfs-root cd etc && cat shadow
root:$1$Jl7H1VOG$Wgw2F/C.nLNTC.4pwDa4H1:18145:0:99999:7:::
daemon:*:0:0:99999:7:::
ftp:*:0:0:99999:7:::
network:*:0:0:99999:7:::
nobody:*:0:0:99999:7:::
dnsmasq:x:0:0:99999:7:::
dnsmasq:x:0:0:99999:7:::
iotgoatuser:$1$79bz0K8z$Ii6Q/if83F1QodGmkb4Ah.:18145:0:99999:7:::
acess this file to get the hashed file 
```
