---
title: Configure Docker Toolbox Behind NTLM Proxy
comments: true
---
It started when i was trying docker toolbox, and i couldn't build my source into docker image. It said `network is unreachable`.

If your company behind MS Proxy Server that using the proprietary NTLM protocol and then get frustated when try to configure your docker machine, i will give hint to solve this.

## Prequisite
* [CNTLM](http://cntlm.sourceforge.net/)
* [Docker ToolBox](https://www.docker.com/products/docker-toolbox)

## Setup CNTLM
Make sure you already installed the CNTLM. After CNTLM had been installed, generate the hash for your password:
```powershell
cd C:\Program Files (x86)\Cntlm
```
then run
```powershell
cntlm -H
```
after that copy and paste that result into `cntlm.ini`, this is sample of configuration of mine:
```
Username    putu_p
Domain		  mitrais
Workstation mtpc4xx
Proxy       xxxx.mitrais.com:8080
Proxy		    123.19.123.6:8080
Listen		  3128
Allow		    127.0.0.1
NoProxy		  localhost, 192.168.122.1, 127.0.0.*, 10.*, 192.168.*
PassLM      A5D1254C3A188EF0197EEB862F9AXXXX
PassNT      8CDF8943D83AF8103B7C6849146BXXXX
PassNTLMv2  AF53BD9898560A113E82FCA0C8DFXXXX    # Only for user 'putu_p', domain 'mitrais'
```
next one is you can restart `CNTLM Authentication Proxy` Service

![Imgur](http://i.imgur.com/eHbtxOL.jpg)

Setup cntlm is done! let's move out to proxy configuration on docker machine.

## Setup Proxy on Docker Machine
Open the docker quickstart terminal. 
Then go into inside docker machine with
```bash
$ docker-machine ssh default
```
then edit the boot2docker profile to append the proxy configuration
```bash
docker@default:~$ sudo vi /var/lib/boot2docker/profile 
```
Add the proxy like this:
```
export http_proxy=http://10.0.2.2:3182
export https_proxy=http://10.0.2.2:3182
export no_proxy=localhost,127.0.0.1,10.0.2.*
```
Now restart and try search image from docker hub
```bash
docker@default:~$ sudo /etc/init.d/docker restart
docker@default:~$ exit
$ docker search ubuntu
```
If you success, then welcome abroad capt! If not just put your comment below.
