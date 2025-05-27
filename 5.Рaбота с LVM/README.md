<b>Работа с LVM</b>

1.Подготовка стенда

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
Предварительно создаем файл hosts. \

2.Запуск стенда. Запуск локального скрипта и playbook.

Запускаем стенд:

```
$ vagrant up
$ vagrant status
Current machine states:

default                   running (virtualbox)

The VM is running. To stop this VM, you can run `vagrant halt` to
shut it down forcefully, or you can run `vagrant suspend` to simply
suspend the virtual machine. In either case, to restart it again,
simply run `vagrant up`.
```

3.Для запуска ansible-playbook используем следующую команду:

```
$ ansible-playbook -i hosts lvm.yml
```

4.Результат.

Результат работы ansible-playbook представлен в файле ansible-result.txt.
