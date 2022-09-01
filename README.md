### Домашнее задание №5 NFS/FUSE
#### 1. Vagrantfile
* Создал Vagrantfile с 2 виртуальными машинами __server__ и __client__;
* Основа для гостевой ОС - дистрибутив __almalinux/8__;
* IP address server: 192.168.56.240/24, IP Address client: 192.168.56.241/24;
____

#### 2. Сервер:
* Создал на сервере директории для расшаривания:
```console
[root@server otus_share]# mkdir /var/otus_share/  
[root@server otus_share]# mkdir /var/otus_share/upload  
```
* Прописал данные директории в конфигурационный файл __/etc/exports__:
```console
[root@server otus_share]# cat /etc/exports  
/var/otus_share *(rw)  
/var/otus_share/upload *(rw)  
```
* Включил использование протокола __UDP__ в конфигурационном файле (параметр __udp=y__ в блоке __[nfsd]__): [/etc/nfs.conf](https://github.com/uNkindy/Otus_Unit_5_NFS/blob/main/nfs.conf);
* Поднял __firewalld__
```console
[root@server otus_share]# systemctl status firewalld  
● firewalld.service - firewalld - dynamic firewall daemon  
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor >  
   Active: active (running) since Sun 2022-08-28 16:33:09 UTC; 42min ago  
     Docs: man:firewalld(1)  
 Main PID: 4500 (firewalld)  
    Tasks: 2 (limit: 5952)  
   Memory: 25.1M  
   CGroup: /system.slice/firewalld.service  
           └─4500 /usr/libexec/platform-python -s /usr/sbin/firewalld --nofork   
```
           
* Установил дефолтную зону __trusted__;
```console
[root@server otus_share]# firewall-cmd  --set-default-zone=trusted  

[root@server otus_share]# firewall-cmd --get-active-zones  
trusted  
  interfaces: enp0s3 enp0s8  
```  
* Добавил в исключение IP адрес клиента:
```console
[root@server otus_share]# firewall-cmd --zone=trusted --permanent --add-rich-rule='rule family="ipv4" source address="192.168.56.241" accept'

[root@server otus_share]# firewall-cmd --zone=trusted --list-all  
trusted (active)  
  target: ACCEPT  
  icmp-block-inversion: no  
  interfaces: enp0s3 enp0s8  
  sources:   
  services:   
  ports:   
  protocols:   
  forward: no  
  masquerade: no  
  forward-ports:   
  source-ports:   
  icmp-blocks:   
  rich rules:   
	rule family="ipv4" source address="192.168.56.241" accept  
```
____

#### Клиент:
* Создал точку монтирования в /mnt/otus_share.
* Примонтировал расшаренную папку __otus_share__, используя следующие настройки: подключение __udp__, версия протокола __nfs - 3__;
```console
[root@client vagrant]# mount -t nfs __-o vers=3__ __-o udp__  192.168.56.240:/var/otus_share /mnt/otus_share

192.168.56.240:/var/otus_share on /mnt/otus_share type nfs   (rw,relatime,vers=3,rsize=32768,wsize=32768,namlen=255,hard,proto=udp, timeo=11,retrans=3,sec=sys,mountaddr=192.168.56.240,mountvers=3,mountport=20048,mountproto=udp,local_lock=none,addr=192.168.56.240)  
```
* Прописал автоматическое монтирование расшаренной директории в __/etc/fstab__:
```console
[root@client vagrant]# cat /etc/fstab   
192.168.56.240:/var/otus_share /mnt/otus_share nfs proto=udp 0 0  
```
____




