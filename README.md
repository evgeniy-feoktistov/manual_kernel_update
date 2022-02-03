### Домашнее задание - Обновить ядро в базовой системе

# 1. Установка Vagrant и Packer
```bash
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install vagrant
sudo apt-get install packer

```

# 2. Установка VirtualBox
```bash
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -
sudo apt-get update
sudo apt-get install virtualbox-6.1
```

# 3. Устанавливаем git, генерируем ключ, задаем глобальные переменные и клонируем репозиторий
```bash
sudo apt install git
cd ~/.ssh && ssh-keygen
git config --global user.name "evgeniy-feoktistov"
git config --global user.email "e.feoktistov@rts-tender.ru"
git clone git@github.com:evgeniy-feoktistov/manual_kernel_update.git

```

