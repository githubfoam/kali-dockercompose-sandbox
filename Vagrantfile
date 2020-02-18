# -*- mode: ruby -*-
# vi: set ft=ruby :
$centos_docker_compose_script = <<SCRIPT
# Get Docker Engine - Community for CentOS
# https://docs.docker.com/install/linux/docker-ce/centos/
sudo yum remove docker \
docker-client \
docker-client-latest \
docker-common \
docker-latest \
docker-latest-logrotate \
docker-logrotate \
docker-engine
sudo yum install -y yum-utils \
device-mapper-persistent-data \
lvm2
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install -y docker-ce \
docker-ce-cli \
containerd.io
sudo systemctl enable docker
sudo systemctl start docker && sudo docker --version
sudo usermod -aG docker vagrant # add user to the docker group
echo "===================================================================================="
docker --version
echo "===================================================================================="
# Install Docker Compose
# https://docs.docker.com/compose/install/
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose && sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
echo "===================================================================================="
sudo docker-compose --version
echo "===================================================================================="
SCRIPT
$build_kali_script = <<SCRIPT
echo "==================BUILDING KALI IMAGE=================================================================="
sudo docker build -<<EOF
FROM kalilinux/kali-rolling
ENV BASE_TOOLZ="aircrack-ng crackmapexec crunch curl dirb dirbuster dnsenum dnsrecon dnsutils dos2unix enum4linux exploitdb ftp git gobuster hashcat hping3 hydra impacket-scripts john joomscan masscan metasploit-framework mimikatz nasm ncat netcat-traditional nikto nmap patator php powersploit proxychains python-impacket python-pip python2 python3 recon-ng responder samba samdump2 smbclient smbmap snmp socat sqlmap sslscan sslstrip theharvester vim wafw00f weevely wfuzz whois wordlists wpscan ruby-dev gcc libffi-dev make"
RUN set -euxo  && \
apt-get update && apt-get install -y --no-install-recommends ${BASE_TOOLZ} && \
groupadd -r -g 998 builder && \
useradd -r -u 998 -g 998 -M -c "docker builder account" -d /tmp builder && \
gem install winrm winrm-fs colorize stringio evil-winrm && \
echo "alias l='ls -al'" >> /root/.bashrc && \
echo "alias nse='ls /usr/share/nmap/scripts | grep '" >> /root/.bashrc && \
echo "alias scan-range='nmap -T5 -n -sn'" >> /root/.bashrc && \
echo "alias http-server='python3 -m http.server 8080'" >> /root/.bashrc && \
echo "alias php-server='php -S 127.0.0.1:8080 -t .'" >> /root/.bashrc && \
echo "alias ftp-server='python -m pyftpdlib -u \"admin\" -P \"S3cur3d_Ftp_3rv3r\" -p 2121'" >> /root/.bashrc
# switch login user
USER builder
# Set working directory to user builder
WORKDIR /tmp
# Open shell
CMD ["/bin/bash"]
EOF
echo "==============BUILT KALI IMAGE======================================================================="
docker image ls
SCRIPT

Vagrant.configure(2) do |config|
  config.vm.box_check_update = false

  # vbox template for all vagranth instances
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "512"
    vb.cpus = 2
  end

  # customize vagrant instance
  config.vm.define "linuxsrv-01" do |dockercluster|
    dockercluster.vm.box = "bento/centos-7.7"
    dockercluster.vm.network "private_network", ip: "172.28.128.24"
    dockercluster.vm.network "forwarded_port", guest: 80, host: 83
    dockercluster.vm.provider "virtualbox" do |vb|
      vb.name = "linuxsrv-01"
      vb.memory = "1024"
     end
     dockercluster.vm.provision "ansible_local" do |ansible|
       ansible.playbook = "deploy.yml"
       ansible.become = true
       ansible.compatibility_mode = "2.0"
       ansible.version = "2.9.3"
     end
    dockercluster.vm.provision "shell", inline: $centos_docker_compose_script, privileged: false
    dockercluster.vm.provision "shell", inline: <<-SHELL
    echo "===================================================================================="
                              hostnamectl status
    echo "===================================================================================="
    echo "         \   ^__^                                                                  "
    echo "          \  (oo)\_______                                                          "
    echo "             (__)\       )\/\                                                      "
    echo "                 ||----w |                                                         "
    echo "                 ||     ||                                                         "
    SHELL
  end


end
