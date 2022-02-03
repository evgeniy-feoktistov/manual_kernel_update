## Домашнее задание - Обновить ядро в базовой системе

### 1. Установка Vagrant и Packer
```bash
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install vagrant
sudo apt-get install packer

```

### 2. Установка VirtualBox
```bash
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -
sudo apt-get update
sudo apt-get install virtualbox-6.1
```

### 3. Устанавливаем git, генерируем ключ, задаем глобальные переменные и клонируем репозиторий
```bash
sudo apt install git
cd ~/.ssh && ssh-keygen
git config --global user.name "evgeniy-feoktistov"
git config --global user.email "e.feoktistov@rts-tender.ru"
git clone git@github.com:evgeniy-feoktistov/manual_kernel_update.git

```

Далее в директории проекта в файле Vagrantfile изменил выделение оперативки на 512
И проверяем, что образ запускается и работает.
```bash
vagrant up
vagrant ssh
[vagrant@kernel-update ~]$ uname -r
3.10.0-957.12.2.el7.x86_64
```

### 4. Обновление ядра, делаем в соответсвии с инструкцией 
```bash
sudo yum install -y http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm
sudo yum --enablerepo elrepo-kernel install kernel-ml -y
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
sudo grub2-set-default 0
sudo reboot
```

### 5. Создаем свой образ с помощью Packer
в новой версии необходимо из конфига centos.json удалить строчку
"iso_checksum_type": "sha256",
так как данный параметр является устаревшим.

Запускаем packer
```bash
packer build centos.json
```
Импортируем образ 
```bash
vagrant box add --name centos-7-5 centos-7.7.1908-kernel-5-x86_64-Minimal.box
```
Создадим тестовую директорию для проверки созданного бокса, создаим виртуалку и запустим ее
```bash
mkdir test
cd test
vagrant init centos-7-5
vagrant up
vagrant ssh
vagrant halt
```

Загрузим в VagrantCloud
```bash
vagrant cloud publish --release efeoktistov/centos-7-5 1.0 virtualbox centos-7.7.1908-kernel-5-x86_64-Minimal.box
```
<span style="color:red"> *тут мой интернет меня подводит сильно, 50+ часов на загрузку и она обрывается*</span> 

