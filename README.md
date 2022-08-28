### Домашнее задание №5 NFS/FUSE
#### 1. Vagrantfile
* Создал Vagrantfile с 2 виртуальными машинами __server__ и __client__;
* Основа для гостевой ОС - дистрибутив __almalinux/8__;
* IP address server: 192.168.56.240/24, IP Address client: 192.168.56.241/24;
____

#### 2. Сервер:
* Создал на сервере директории для расшаривания:

[root@server otus_share]# mkdir /var/otus_share/  
[root@server otus_share]# mkdir /var/otus_share/upload  

* Прописал данные директории в конфигурационный файл __/etc/exports__:

[root@server otus_share]# cat /etc/exports  
/var/otus_share *(rw)  
/var/otus_share/upload *(rw)  

* 
____

#### Клиент:
* Создал точку монтирования в /mnt/otus_share.
* Примонтировал расшаренную папку __otus_share__, используя следующие настройки: подключение __udp__, версия протокола __nfs - 3__;

[root@client vagrant]# mount -t nfs __-o vers=3__ __-o udp__  192.168.56.240:/var/otus_share /mnt/otus_share

192.168.56.240:/var/otus_share on /mnt/otus_share type nfs   (rw,relatime,__vers=3__,rsize=32768,wsize=32768,namlen=255,hard,__proto=udp__,timeo=11,retrans=3,sec=sys,mountaddr=192.168.56.240,__mountvers=3__,mountport=20048,__mountproto=udp__,local_lock=none,addr=192.168.56.240)  

* Прописал автоматическое монтирование расшаренной директории в __/etc/fstab__:

[root@client vagrant]# cat /etc/fstab   
192.168.56.240:/var/otus_share /mnt/otus_share nfs defaults 0 0  

* 
____




