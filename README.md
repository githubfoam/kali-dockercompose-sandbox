# kali-dockercompose-sandbox


~~~~
[vagrant@linuxsrv-01 kalilinux]$ hostnamectl
   Static hostname: linuxsrv-01
         Icon name: computer-vm
           Chassis: vm
        Machine ID: bd4f7f0c41c8408f80dbccc43f774a17
           Boot ID: 9a138bd0e6224d3b9e72866a263424d5
    Virtualization: kvm
  Operating System: CentOS Linux 7 (Core)
       CPE OS Name: cpe:/o:centos:centos:7
            Kernel: Linux 3.10.0-1062.9.1.el7.x86_64
      Architecture: x86-64


[vagrant@linuxsrv-01 ~]$ whoami
vagrant
[vagrant@linuxsrv-01 ~]$ id vagrant
uid=1000(vagrant) gid=1000(vagrant) groups=1000(vagrant),993(docker)
sudo mkdir /mnt/share-kali
docker build -t kalicmd . --file=/vagrant/dockerfiles/kalilinux/toolkali

[vagrant@linuxsrv-01 ~]$ docker-compose --file /vagrant/dockerfiles/kalilinux/docker-compose.yml run kali-service
Creating network "kalilinux_default" with the default driver
builder@ef8fe742850c:~$ whoami
builder
builder@ef8fe742850c:~$ id builder
uid=998(builder) gid=998(builder) groups=998(builder)
builder@ef8fe742850c:~$ exit


echo "alias kali='docker-compose -f /vagrant/dockerfiles/kalilinux/docker-compose.yml run kali-service'" >> .bashrc && source .bashrc

[vagrant@linuxsrv-01 ~]$ kali dnsrecon -d hackingloops.com
[*] Performing General Enumeration of Domain: hackingloops.com
[-] DNSSEC is not configured for hackingloops.com
[*]      SOA dora.ns.cloudflare.com 173.245.58.108
[*]      NS dora.ns.cloudflare.com 173.245.58.108
[*]      Bind Version for 173.245.58.108 20171212
[*]      NS dora.ns.cloudflare.com 2606:4700:50::adf5:3a6c
[*]      NS vin.ns.cloudflare.com 173.245.59.245
[*]      Bind Version for 173.245.59.245 20171212
[*]      NS vin.ns.cloudflare.com 2606:4700:58::adf5:3bf5
[*]      MX hackingloops-com.mail.protection.outlook.com 104.47.56.138
[*]      MX hackingloops-com.mail.protection.outlook.com 104.47.58.138
[*]      A hackingloops.com 104.31.75.228
[*]      A hackingloops.com 104.31.74.228
[*]      AAAA hackingloops.com 2606:4700:3033::681f:4ae4
[*]      AAAA hackingloops.com 2606:4700:3035::681f:4be4
[*]      TXT hackingloops.com NETORGFT1870407.onmicrosoft.com
[*]      TXT hackingloops.com v=spf1 include:spf.protection.outlook.com -all
[*]      TXT hackingloops.com google-site-verification=WvsGTMKUbSd-N5uKL1zskvE0h54u9haHvDZ8bYrm3ag
[*]      TXT hackingloops.com ca3-8c3edaad747d4ecd8141eeb0c6fff07a
[*] Enumerating SRV Records
[*]      SRV _sip._tls.hackingloops.com sipdir.online.lync.com 52.113.64.147 443 1
[*]      SRV _sip._tls.hackingloops.com sipdir.online.lync.com 2603:1047:0:8::f 443 1
[*]      SRV _sip._tls.hackingloops.com sipdir.online.lync.com 2603:1047:0:2::b 443 1
[*]      SRV _sip._tls.hackingloops.com sipdir.online.lync.com 2603:1047:0:1::b 443 1
[*]      SRV _sip._tls.hackingloops.com sipdir.online.lync.com 2603:1047:0:5::b 443 1
[*]      SRV _sip._tls.hackingloops.com sipdir.online.lync.com 2603:1047:0:9::f 443 1
[*]      SRV _sipfederationtls._tcp.hackingloops.com sipfed.online.lync.com 52.113.64.147 5061 1
[*]      SRV _sipfederationtls._tcp.hackingloops.com sipfed.online.lync.com 2603:1047:0:8::f 5061 1
[*]      SRV _sipfederationtls._tcp.hackingloops.com sipfed.online.lync.com 2603:1047:0:9::f 5061 1
[*]      SRV _sipfederationtls._tcp.hackingloops.com sipfed.online.lync.com 2603:1047:0:2::b 5061 1
[*]      SRV _sipfederationtls._tcp.hackingloops.com sipfed.online.lync.com 2603:1047:0:5::b 5061 1
[*]      SRV _sipfederationtls._tcp.hackingloops.com sipfed.online.lync.com 2603:1047:0:1::b 5061 1
[+] 12 Records Found

~~~~

~~~~
the default root password – “toor“, without the quotes.  So the username = root and password = toor.
builder@71ca5b328df4:~$ su -
Password:
su: Authentication failure
~~~~
~~~~
OWASP Docker Security CheatSheet
https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html
Image built from the last official release
https://hub.docker.com/r/kalilinux/kali/tags
Official Kali Linux image (weekly snapshot of kali-rolling)
https://hub.docker.com/r/kalilinux/kali-rolling
Image built from the last official release
https://hub.docker.com/r/kalilinux/kali
~~~~
