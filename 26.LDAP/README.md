<b>LDAP</b>

1.Подготовка стенда.

```
$ ansible --version
ansible [core 2.16.3]
  config file = None
  configured module search path = ['/home/yup/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/yup/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.12.3 (main, Feb  4 2025, 14:48:35) [GCC 13.3.0] (/usr/bin/python3)
  jinja version = 3.1.2
  libyaml = True

$ vagrant -v
Vagrant 2.4.3
```

Vagrantfile - almalinux/9

Предварительно создаем файл hosts.

2.Запуск стенда. Запуск локального скрипта и playbook.

```
$ vagrant status
Current machine states:

ipa.otus.lan              running (virtualbox)
client1.otus.lan          running (virtualbox)
client2.otus.lan          running (virtualbox)

This environment represents multiple VMs. The VMs are all listed
above with their current state. For more information about a specific
VM, run `vagrant status NAME`.
```

Запускаем плэйбук:

```
$ ansible-playbook -i hosts ldap.yml
```

3.Проверка.

3.1.Установить FreeIPA:

![Скрин:](https://github.com/Asteilor/S.D./blob/main/26.LDAP/server.png)

![Скрин:](https://github.com/Asteilor/S.D./blob/main/26.LDAP/server2.png)

3.2.Написать Ansible-playbook для конфигурации клиента:

```
[root@client1 ~]# kinit admin
Password for admin@OTUS.LAN: 
[root@client1 ~]# klist
Ticket cache: KCM:0
Default principal: admin@OTUS.LAN

Valid starting       Expires              Service principal
05/29/2025 00:03:04  05/29/2025 23:18:13  krbtgt/OTUS.LAN@OTUS.LAN

[root@ipa ~]# kinit admin
Password for admin@OTUS.LAN: 
[root@ipa ~]# ipa user-add otus-user --first=Otus --last=User --password
Password: 
Enter Password again to verify: 
----------------------
Added user "otus-user"
----------------------
  User login: otus-user
  First name: Otus
  Last name: User
  Full name: Otus User
  Display name: Otus User
  Initials: OU
  Home directory: /home/otus-user
  GECOS: Otus User
  Login shell: /bin/sh
  Principal name: otus-user@OTUS.LAN
  Principal alias: otus-user@OTUS.LAN
  User password expiration: 20250528210617Z
  Email address: otus-user@otus.lan
  UID: 920000003
  GID: 920000003
  Password: True
  Member of groups: ipausers
  Kerberos keys available: True


[root@client1 ~]# kinit otus-user
Password for otus-user@OTUS.LAN: 
Password expired.  You must change it now.
Enter new password: 
Enter it again: 
```

![Скрин:](https://github.com/Asteilor/S.D./blob/main/26.LDAP/server3.png)
