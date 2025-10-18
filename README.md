# Ansible
 Ansible (Configuration Management and Deployment) - Oracle VM VirtualBox - Linux (Ubuntu, CentOS), Nodes


Ansible
https://docs.ansible.com/ansible/latest/getting_started/index.html#extpack



Oracle VirtualBox
https://www.oracle.com/virtualization/technologies/vm/downloads/virtualbox-downloads.html#extpack

En son sürüm için bu siteyi de kullanabilirsiniz.
https://www.virtualbox.org/



putty
https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

mremoteng
https://mremoteng.org/


### MyControlNode makinesi
yum update -y 



PUBLIC_IP numaramısını göreceğiz.
ifconfig

su root
dhclient
onboot=yes
yum update -y 
reboot

su root
yum install epel-release -y
yum install net-tools 
ifconfig



yum update -y 
yum install openssh-server -y

systemctl status firewalld
systemctl stop firewalld
systemctl disable firewalld
systemctl status firewalld

su root
ssh-keygen

yum install ansible-core
ansible --version


Burada bu komutu çalıştırmadım ama ihtiaç olursa hostname vermek için kullanabilirsiniz.
hostnamectl set-hostname control-node
cat /etc/hostname
reboot


## Makine managed-node1
su root
yum update -y 

systemctl status firewalld
systemctl stop firewalld
systemctl disable firewalld
systemctl status firewalld

hostnamectl set-hostname managed-node1
cat /etc/hostname
reboot


su root
ssh-keygen



## Makine managed-node2
su root
yum update -y 

systemctl status firewalld
systemctl stop firewalld
systemctl disable firewalld
systemctl status firewalld

hostnamectl set-hostname managed-node2
cat /etc/hostname
reboot


su root
ssh-keygen




ControlNode makinesindeyiz.

ControlNode makinesinden ManagedNode1 makinesini kullanmak için 

İlk bağlantıyı bu komutla gerçekleştir.
ssh-copy-id root@ManagedNode1MakinesininPublicIPsi


Sonraki bağlantıları da bununla yap.
ssh         root@ManagedNode1MakinesininPublicIPsi





İlk bağlantıyı bu komutla gerçekleştir.
ssh-copy-id root@ManagedNode2MakinesininPublicIPsi


Sonraki bağlantıları da bununla yap.
ssh         root@ManagedNode2MakinesininPublicIPsi




SSH ile bu makineden bağlantı kurduğum makinelerin bilgileri burada tutulur.
cd /root/.ssh

cat known_hosts

cat id_ed25519.pub
Terminalde yazan satırı kopyalıyoruz.

ssh-ed25519 ABCABCABCABCABCABCABCABCABCABCABCABCABCABC root@localhost.localdomain



Node1 ve Node2 makinelerine Control makinesini tanıtacağız.

cd /root/.ssh

nano authorized_keys  YA DA    vi authorized_keys  komutuyla açacağız.

ControlNode makinesinden gelen bu bilgiyi authorized_keys dosyasının içine yapıştırıp kaydet.
ssh-ed25519 ABCABCABCABCABCABCABCABCABCABCABCABCABCABC root@localhost.localdomain




ControlNode makinesinde Ansible için ayar dosyasına gidiyoruz.

cat /etc/ansible/hosts

nano /etc/ansible/hosts   YA DA    vi /etc/ansible/hosts  



[mynodes]
192.168.11.32
192.168.11.82
192.168.11.99

[mynode1]
192.168.11.32

[mynode2]
192.168.11.82

[mynode3]
192.168.11.99



Ansible komutları

        [pattern] [module] [module options]
ansible            -m        ping all


Bütün managed node makinelerini yeniden başlattı. 
ansible all -a "/sbin/reboot"


Sadece node1 için ping atacağız.
ansible            -m        ping    mynode1

