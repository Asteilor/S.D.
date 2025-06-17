#### **Настройка PXE сервера для автоматической установки**  

***
##### Подготовка стенда.
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

Предварительно создаем файл *hosts*. 

***
##### Запуск playbook.
После запуска виртуальной машины запускаем плэйбук (он настроит PXE сервер):
```
$ ansible-playbook -i hosts pxe.yml
```
***
##### Проверка загрузки клиента.
**Legacy:**

![Screen:](https://github.com/Asteilor/S.D./blob/main/20.PXE/screens/legacy-1.png)
![Screen:](https://github.com/Asteilor/S.D./blob/main/20.PXE/screens/legacy-2.png)
![Screen:](https://github.com/Asteilor/S.D./blob/main/20.PXE/screens/legacy-3.png)


**UEFI:**
Добавлены конфиги в /srv/tftp/amd64/grub/grub.cfg. 

Пришлось увеличивать объем ОЗУ до 8ГБ, иначе не зависает на разных этапах.

![Screen:](https://github.com/Asteilor/S.D./blob/main/20.PXE/screens/uefi-1.png)
![Screen:](https://github.com/Asteilor/S.D./blob/main/20.PXE/screens/uefi-2.png)
![Screen:](https://github.com/Asteilor/S.D./blob/main/20.PXE/screens/uefi-3.png)
