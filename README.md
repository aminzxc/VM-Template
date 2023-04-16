
Operating system update

       sudo apt update && sudo apt upgrade -y

Kernel update

       sudo apt dist-upgrade

Install the required tools

       sudo apt install -y dstat htop net-tools nmon fail2ban ncdu iptables-persistent vim

Change the Hostname

       sudo hostnamectl set-hostname template2204

Set the time server

       sudo dpkg-reconfigure tzdata          Asia/Tehran

Configuring the SSH Server

       sudo vim /etc/ssh/sshd_config
       change port to 7268
       # Port 7268
       restrict root user
       # PermitRootLogin no
       sudo systemctl restart sshd.service

Add user

       sudo adduser <username>

Add user to group sudoer

       sudo usermod -aG sudo <username>

Allow members of group sudo to execute any command

       sudo visudo
       <username> 	ALL=(ALL) NOPASSWD:ALL

Configure IPtables

       sudo vim /etc/iptables/rules.v4
      *filter
      :INPUT ACCEPT [0:0]
      :FORWARD DROP [0:0] 
      :OUTPUT ACCEPT [0:0]
      :CHECK_INPUT - [0:0]

      -A INPUT -j CHECK_INPUT
      -A INPUT -j DROP

      -A CHECK_INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
      -A CHECK_INPUT -i lo -j ACCEPT

      -A CHECK_INPUT -i ens33 -p tcp --dport 7268 -j ACCEPT
      -A CHECK_INPUT -i ens33 -p icmp -j ACCEPT


      COMMIT

Apply changs to IPtables

      sudo iptables-apply /etc/iptables/rules.v4
