<b>Vagrant-стенд c PAM</b>

1.Подготовка стенда.

```$ ansible --version
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

2.Запуск стенда. Проверка.

Ansible-playbook запускается в провижине при включении виртуальной машины.
Результат выполнения в файле prov_res.txt

Пытаемся безуспешно залогиниться по ssh под пользователем otus:

```
ssh otus@192.168.56.10
The authenticity of host '192.168.56.10 (192.168.56.10)' can't be established.
ED25519 key fingerprint is SHA256:1a4pVoevE+WytxMne3KFiQ3Fz1vsBEfc3eEgf8ovGQY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '192.168.56.10' (ED25519) to the list of known hosts.
otus@192.168.56.10's password: 
Permission denied, please try again.
otus@192.168.56.10's password: 
Permission denied, please try again.
otus@192.168.56.10's password: 
otus@192.168.56.10: Permission denied (publickey,password).
```

Успешная авторизация под otusadm:

```
ssh otusadm@192.168.56.10
otusadm@192.168.56.10's password: 

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

otusadm@pam:~$ 
```

Безуспешно пробуем перезапустить докер и выполнить команды через docker-cli через otusadm:

```
otusadm@pam:~$ docker container ls
permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.49/containers/json": dial unix /var/run/docker.sock: connect: permission denied
otusadm@pam:~$
```

Теперь успешно через пользователя otus:

```
otus@pam:/home/otusadm$ docker container ls
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
otus@pam:/home/otusadm$
```
