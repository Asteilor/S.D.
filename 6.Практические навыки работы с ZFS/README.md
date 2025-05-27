<b>Практические навыки работы с ZFS</b>

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

Предварительно создаем файл hosts. \

2.Запуск стенда. Запуск локального скрипта и playbook.

Скрипт bash запускается сразу при включении виртуальной машины.
Результат выполенния bash_prov-result.txt
Для запуска ansible-playbook предварительно, перед запуском виртуальной машины, закомментировать строку с bash и расскомментировать с ansible:

```
config.vm.provision "shell", path: "bash_zfs.sh"
#config.vm.provision "ansible" do |ansible|
#      ansible.playbook = "zfs.yml"
#end
```

3.Результат работы плейбука в приложенном файле ansible_prov-result.txt
