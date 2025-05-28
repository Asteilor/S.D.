<b>Сборка RPM-пакета и создание репозитория</b>

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
Предварительно создаем файл hosts. \


2.Запуск playbook'а.

ДЗ выполнено с помощью ansible. Результат его работы в файле ansible-res.txt

```
$ ansible-playbook -i hosts rpm.yml
```
