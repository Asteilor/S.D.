1.Скачиваем бокс с официального облака Hashicorp:
$  vagrant init generic/ubuntu2204 --box-version 4.3.12
Полученный Vagrantfile приводим к следующему виду:
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2204"
  config.vm.box_version = "4.3.12"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = "2"
  end
end

2.Запускаем виртуальную машину и подключаемся к ней по ssh:
$ vagrant up
$ vagrant ssh

3.Приступаем к обновлению ядра.
1 - Обновление ядра через update&upgrade.
После включения виртуальной машины проверяем информацию о системе:
 $ uname -a
Linux ubuntu2204.localdomain 5.15.0-91-generic #101-Ubuntu SMP Tue Nov 14 13:30:08 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux
Проводим стандартную процедуру обновления информации о доступных пакетах обновлений и само обновление:
 $ sudo apt update && sudo apt upgrade -y
После обновления перезагружаем платформу и проверяем версию ядра:
$ uname -a
Linux ubuntu2204.localdomain 5.15.0-131-generic #141-Ubuntu SMP Fri Jan 10 21:18:28 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux
Таким образом, произошло обновление с версии 5.15.0-91-generic на 5.15.0-131-generic. 

2 - Обновление ядра через apt install linux-image
Проверим, какие версии ядер поновее есть в доступе через пакетный менеджер apt (например, 5.19.0):
$ apt search linux-image-5.19.0
Sorting... Done
Full Text Search... Done
linux-image-5.19.0-1010-nvidia/jammy-updates,jammy-security 5.19.0-1010.10 amd64
  Signed kernel image nvidia

linux-image-5.19.0-1010-nvidia-lowlatency/jammy-updates,jammy-security 5.19.0-1010.10 amd64
  Signed kernel image nvidia-lowlatency

........

linux-image-5.19.0-46-generic/jammy-updates,jammy-security 5.19.0-46.47~22.04.1 amd64
  Signed kernel image generic

linux-image-5.19.0-50-generic/jammy-updates,jammy-security 5.19.0-50.50 amd64
  Signed kernel image generic
Из данного списка (сокращен для удобства представления примера) выбираем понравившуюся доступную версию и устанавливаем её:
 $ sudo apt install linux-image-5.19.0-50-generic
Перезагружаем систему и проверяем версию ядра:
$ uname -a
Linux ubuntu2204.localdomain 5.19.0-50-generic #50-Ubuntu SMP PREEMPT_DYNAMIC Mon Jul 10 18:24:29 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux
Итого обновились с 5.15.0-131-generic на 5.19.0-50-generic.

3 - Обновление ядра через скачивание dep-пакетов и их установку.
На сайте https://kernel.ubuntu.com/mainline выбираем версию ядра для установки (например, 6.1).
Скачиваем 4 deb-файла:
wget https://kernel.ubuntu.com/mainline/v6.1/amd64/linux-headers-6.1.0-060100-generic_6.1.0-060100.202303090726_amd64.deb
wget https://kernel.ubuntu.com/mainline/v6.1/amd64/linux-headers-6.1.0-060100_6.1.0-060100.202303090726_all.deb
wget https://kernel.ubuntu.com/mainline/v6.1/amd64/linux-image-unsigned-6.1.0-060100-generic_6.1.0-060100.202303090726_amd64.deb
wget https://kernel.ubuntu.com/mainline/v6.1/amd64/linux-modules-6.1.0-060100-generic_6.1.0-060100.202303090726_amd64.deb
Выполняем установку через dpkg:
$ sudo dpkg -i linux-headers* linux-image* linux-modules*
Обновляем grub:
$ sudo update-grub
Перезагружаем систему и проверяем версию ядра:
$ uname -a
Linux ubuntu2204.localdomain 6.1.0-060100-generic #202303090726 SMP PREEMPT_DYNAMIC Thu Mar  9 12:33:28 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux
Обновились с 5.19.0-50-generic на 6.1.0-060100-generic.
