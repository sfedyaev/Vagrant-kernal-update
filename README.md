# Vagrant-kernal-update
 Обновить ядро в базовой системе

Домашнее задание
Обновить ядро в базовой системе

Цель:
Получить навыки работы с Git, Vagrant;
Обновлять ядро в ОС Linux.

Описание/Пошаговая инструкция выполнения домашнего задания:
Программа минимум: установить свежее ядро.


Для выполнения домашнего задания используйте методичку1. Выполните действия, описанные в методичке.

2. Полученный в ходе выполнения ДЗ Vagrantfile и отчет залейте в ваш git-репозиторий.

3. Пришлите ссылку на него в чате для проверки ДЗ. Обычно мы проверяем ДЗ в течение 48 часов.

Для выполнения ДЗ со * и ** вам потребуется сборка ядра и модулей из исходников.

Создание ВМ в Vagrant, команды:
                                                                                                                                                                                                                                                    
  1 vagrant.exe box add --name "centos7" .\CentOS-7-x86_64-Vagrant-2004_01.VirtualBox.box       # добавление vagrant box centos7                                             
  2 vagrant.exe box list                                                                        # просмотр добавленных боксов                                           
  3 vagrant.exe init                                                                            # инициализация вм                                             
  4 notepad.exe .\Vagrantfile                                                                   # изменение конфигурационного файла                                             
  5 vagrant.exe up																				# создание вм   
  6 vagrant.exe ssh																				# вход на вм по ssh
                                 
#заходим на вм по ssh
vagrant.exe ssh
#получаем root привелегии
[vagrant@centos7-kernal-update ~]$ sudo -s

#проверяем версию ядра
[root@centos7-kernal-update vagrant]# uname -r 
3.10.0-1127.el7.x86_64

#импортируем elrepo public key
[root@centos7-kernal-update vagrant]# rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org 

#установка elrepo репозиторий
[root@centos7-kernal-update vagrant]# yum install https://www.elrepo.org/elrepo-release-7.el7.elrepo.noarch.rpm 
Loaded plugins: fastestmirror
elrepo-release-7.el7.elrepo.noarch.rpm                                                                                  | 8.7 kB  00:00:00
Examining /var/tmp/yum-root-f_kAnM/elrepo-release-7.el7.elrepo.noarch.rpm: elrepo-release-7.0-6.el7.elrepo.noarch
Marking /var/tmp/yum-root-f_kAnM/elrepo-release-7.el7.elrepo.noarch.rpm to be installed
Resolving Dependencies
--> Running transaction check
---> Package elrepo-release.noarch 0:7.0-6.el7.elrepo will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===============================================================================================================================================
 Package                       Arch                  Version                          Repository                                          Size
===============================================================================================================================================
Installing:
 elrepo-release                noarch                7.0-6.el7.elrepo                 /elrepo-release-7.el7.elrepo.noarch                5.0 k

Transaction Summary
===============================================================================================================================================
Install  1 Package

Total size: 5.0 k
Installed size: 5.0 k
Is this ok [y/d/N]: y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : elrepo-release-7.0-6.el7.elrepo.noarch                                                                                      1/1
  Verifying  : elrepo-release-7.0-6.el7.elrepo.noarch

#установка ядра
[root@centos7-kernal-update vagrant]# yum --enablerepo elrepo-kernel install kernel-ml -y
Loaded plugins: fastestmirror
Determining fastest mirrors
 * base: mirror.sale-dedic.com
 * elrepo: mirror.koddos.net
 * elrepo-kernel: mirror.koddos.net
 * extras: mirror.hyperdedic.ru
 * updates: mirror.axelname.ru
base                                                                                                                    | 3.6 kB  00:00:00
elrepo                                                                                                                  | 3.0 kB  00:00:00
elrepo-kernel                                                                                                           | 3.0 kB  00:00:00
extras                                                                                                                  | 2.9 kB  00:00:00
updates                                                                                                                 | 2.9 kB  00:00:00
(1/6): base/7/x86_64/group_gz                                                                                           | 153 kB  00:00:00
(2/6): extras/7/x86_64/primary_db                                                                                       | 250 kB  00:00:00
(3/6): base/7/x86_64/primary_db                                                                                         | 6.1 MB  00:00:00
(4/6): elrepo/primary_db                                                                                                | 376 kB  00:00:01
(5/6): elrepo-kernel/primary_db                                                                                         | 2.1 MB  00:00:03
(6/6): updates/7/x86_64/primary_db                                                                                      |  25 MB  00:00:17
Resolving Dependencies
--> Running transaction check
---> Package kernel-ml.x86_64 0:6.7.9-1.el7.elrepo will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===============================================================================================================================================
 Package                        Arch                        Version                                   Repository                          Size
===============================================================================================================================================
Installing:
 kernel-ml                      x86_64                      6.7.9-1.el7.elrepo                        elrepo-kernel                       68 M

Transaction Summary
===============================================================================================================================================
Install  1 Package

Total download size: 68 M
Installed size: 349 M
Downloading packages:
kernel-ml-6.7.9-1.el7.elrepo.x86_64.rpm                                                                                 |  68 MB  00:00:07
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : kernel-ml-6.7.9-1.el7.elrepo.x86_64                                                                                         1/1
  Verifying  : kernel-ml-6.7.9-1.el7.elrepo.x86_64                                                                                         1/1

Installed:
  kernel-ml.x86_64 0:6.7.9-1.el7.elrepo

Complete!

# перезагрузка вм
[root@centos7-kernal-update vagrant]# shutdown -r now
Connection to 127.0.0.1 closed by remote host.
Connection to 127.0.0.1 closed.

#заходим на вм по ssh
PS X:\OTUS_lab\CentOS7> vagrant.exe ssh
Last login: Thu Mar 14 07:00:42 2024 from 10.0.2.2

#проверка версии ядра
[vagrant@centos7-kernal-update ~]$ uname -r
6.7.9-1.el7.elrepo.x86_64
