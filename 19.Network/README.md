<b>Разворачиваем сетевую лабораторию</b>

1.Теоретическая часть

Дано:

Сеть office1:

- 192.168.2.0/26 - dev
- 192.168.2.64/26 - test servers
- 192.168.2.128/26 - managers
- 192.168.2.192/26 - office hardware

Сеть office2:

- 192.168.1.0/25 - dev
- 192.168.1.128/26 - test servers
- 192.168.1.192/26 - office hardware

Сеть central:

- 192.168.0.0/28 - directors
- 192.168.0.32/28 - office hardware
- 192.168.0.64/26 - wifi

Схема:

![Схема:](https://github.com/Asteilor/S.D./blob/main/19.Network/top1.png)

Считаем сети: 

![Схема:](https://github.com/Asteilor/S.D./blob/main/19.Network/networks.png)

Свободные подсети:

192.168.0.16/28
192.168.0.48/28
192.168.0.128/25
192.168.255.64/26
192.168.255.32/27
192.168.255.16/28
192.168.255.8/29
192.168.255.4/30

2.Практическая часть.

Анализируем схему:

![Схема:](https://github.com/Asteilor/S.D./blob/main/19.Network/top2.png)

2.1.Подготовка стенда.

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

2.2.Запуск стенда. Запуск локального скрипта и playbook.

```$ vagrant status
Current machine states:

inetRouter                running (virtualbox)
centralRouter             running (virtualbox)
centralServer             running (virtualbox)
office1Router             running (virtualbox)
office1Server             running (virtualbox)
office2Router             running (virtualbox)
office2Server             running (virtualbox)

This environment represents multiple VMs. The VMs are all listed
above with their current state. For more information about a specific
VM, run `vagrant status NAME`.
```

Запускаем плэйбук:

```
$ ansible-playbook -i hosts network.yml
```

2.3.Проверка.

Пингуем с centralServer другие серверы office1Server и office2Server и проверяем доступ в интернет:

```
vagrant@centralServer:~$ ping 192.168.2.130
PING 192.168.2.130 (192.168.2.130) 56(84) bytes of data.
64 bytes from 192.168.2.130: icmp_seq=1 ttl=62 time=2.13 ms
64 bytes from 192.168.2.130: icmp_seq=2 ttl=62 time=2.92 ms
64 bytes from 192.168.2.130: icmp_seq=3 ttl=62 time=4.16 ms
^C
--- 192.168.2.130 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2001ms
rtt min/avg/max/mdev = 2.125/3.067/4.162/0.838 ms

vagrant@centralServer:~$ ping 192.168.1.2
PING 192.168.1.2 (192.168.1.2) 56(84) bytes of data.
64 bytes from 192.168.1.2: icmp_seq=1 ttl=62 time=3.53 ms
64 bytes from 192.168.1.2: icmp_seq=2 ttl=62 time=3.25 ms
64 bytes from 192.168.1.2: icmp_seq=3 ttl=62 time=3.75 ms
^C
--- 192.168.1.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 3.247/3.508/3.746/0.204 ms

vagrant@centralServer:~$ traceroute 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  _gateway (192.168.0.1)  0.904 ms  0.572 ms  0.640 ms
 2  192.168.255.1 (192.168.255.1)  1.346 ms  1.298 ms  1.255 ms
 3  10.0.2.2 (10.0.2.2)  2.013 ms  1.932 ms  1.899 ms
 4  * * *
 5  * * *
```

Трассируемся во внешку с office1Server:

```
vagrant@office1Server:~$ traceroute 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  _gateway (192.168.2.129)  1.044 ms  0.648 ms  0.605 ms
 2  192.168.255.9 (192.168.255.9)  1.217 ms  1.177 ms  2.015 ms
 3  192.168.255.1 (192.168.255.1)  1.975 ms  1.939 ms  1.985 ms
 4  10.0.2.2 (10.0.2.2)  2.478 ms  2.323 ms  2.281 ms
 5  * * *
```

Трассируемся во внешку с office2Server:

```
vagrant@office2Server:~$ traceroute 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  _gateway (192.168.1.1)  0.647 ms  0.487 ms  0.889 ms
 2  192.168.255.5 (192.168.255.5)  1.250 ms  1.223 ms  1.066 ms
 3  192.168.255.1 (192.168.255.1)  1.460 ms  1.370 ms  1.710 ms
 4  10.0.2.2 (10.0.2.2)  2.880 ms  2.849 ms  2.751 ms
 5  * * *
 6  * * *
 7  * * *
```

