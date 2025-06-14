
<b>Настраиваем бэкапы.</b>

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

Vagrantfile - generic/ubuntu2204
Предварительно создаем файл hosts.

2.Запуск стенда. Запуск локального скрипта и playbook.

Запуск:

```
$ vargant up && ansible-playbook -i hosts backup.yml
```

3.Описание проверки.

Ожидаем минут 10 и проверяем логи:

```
# cat /var/log/borg-backup.log
==== Borg Backup started at Thu May  1 05:31:23 PM UTC 2025 ====
------------------------------------------------------------------------------
Repository: ssh://borg@192.168.56.10/var/backup
Archive name: etc-2025-05-01_17:31:24
Archive fingerprint: ab05d6a97ec1c058568b4134064e57fce100255b1a6b971e59b35147992f0fdf
Time (start): Thu, 2025-05-01 17:31:24
Time (end):   Thu, 2025-05-01 17:31:24
Duration: 0.10 seconds
Number of files: 708
Utilization of max. archive size: 0%
------------------------------------------------------------------------------
                       Original size      Compressed size    Deduplicated size
This archive:                2.12 MB            932.88 kB                643 B
All archives:                6.28 MB              2.78 MB              1.05 MB

                       Unique chunks         Total chunks
Chunk index:                     684                 2076
------------------------------------------------------------------------------
Keeping archive (rule: daily #1):        etc-2025-05-01_17:31:24              Thu, 2025-05-01 17:31:24 [ab05d6a97ec1c058568b4134064e57fce100255b1a6b971e59b35147992f0fdf]
Pruning archive (1/1):                   etc-2025-05-01_17:29:04              Thu, 2025-05-01 17:29:04 [5b105bfd0a86468bae96f27953ac94f348a5b907c0559d024ea48b54fab71a4d]
Keeping archive (rule: daily[oldest] #2): etc-2025-05-01_16:26:44              Thu, 2025-05-01 16:26:44 [dcd0517de1cf95747b312bf35ae36971b7b81f2b24ede7c504afcb6446c1ee90]
==== Borg Backup finished at Thu May  1 05:31:26 PM UTC 2025 ====
==== Borg Backup started at Thu May  1 05:33:31 PM UTC 2025 ====
------------------------------------------------------------------------------
Repository: ssh://borg@192.168.56.10/var/backup
Archive name: etc-2025-05-01_17:33:32
Archive fingerprint: b77f861be3888b97ff0a21816b515267724189786d5d8364a794898ee7035492
Time (start): Thu, 2025-05-01 17:33:32
Time (end):   Thu, 2025-05-01 17:33:32
Duration: 0.09 seconds
Number of files: 708
Utilization of max. archive size: 0%
------------------------------------------------------------------------------
                       Original size      Compressed size    Deduplicated size
This archive:                2.12 MB            932.88 kB                643 B
All archives:                6.28 MB              2.78 MB              1.05 MB

                       Unique chunks         Total chunks
Chunk index:                     684                 2076
------------------------------------------------------------------------------
Keeping archive (rule: daily #1):        etc-2025-05-01_17:33:32              Thu, 2025-05-01 17:33:32 [b77f861be3888b97ff0a21816b515267724189786d5d8364a794898ee7035492]
Pruning archive (1/1):                   etc-2025-05-01_17:31:24              Thu, 2025-05-01 17:31:24 [ab05d6a97ec1c058568b4134064e57fce100255b1a6b971e59b35147992f0fdf]
Keeping archive (rule: daily[oldest] #2): etc-2025-05-01_16:26:44              Thu, 2025-05-01 16:26:44 [dcd0517de1cf95747b312bf35ae36971b7b81f2b24ede7c504afcb6446c1ee90]
==== Borg Backup finished at Thu May  1 05:33:34 PM UTC 2025 ====
```

Проверяем список бэкапов (первый и последний созданный):

```
# BORG_PASSPHRASE=Otus1234 borg list borg@192.168.56.10:/var/backup/
etc-2025-05-01_16:26:44              Thu, 2025-05-01 16:26:44 [dcd0517de1cf95747b312bf35ae36971b7b81f2b24ede7c504afcb6446c1ee90]
etc-2025-05-01_17:36:24              Thu, 2025-05-01 17:36:24 [af21fec0dec549faddc5922fdaad1a63be1031de16ea99e941a039b6024740c0]
```

Остановим бэкап:

```
# systemctl stop borg-backup.timer
```

Восстановим директорию /etc из бекапа:

```
# BORG_PASSPHRASE=Otus1234 borg extract borg@192.168.56.10:/var/backup/::etc-2025-05-01_17:36:24 etc
# ls
etc  snap  truncate
# rm -rf /etc/
# ls /
bin   dev   lib    lib64   lost+found  mnt  proc  run   snap  swap.img  tmp  var
boot  home  lib32  libx32  media       opt  root  sbin  srv   sys       usr
# ls /etc/ | wc -l
ls: cannot access '/etc/': No such file or directory
0
# mkdir /etc
root@client:~# cp -Rf etc/* /etc/
root@client:~# ls /
bin   dev  home  lib32  libx32      media  opt   root  sbin  srv       sys  usr
boot  etc  lib   lib64  lost+found  mnt    proc  run   snap  swap.img  tmp  var
root@client:~# ls /etc/ | wc -l
189
```
