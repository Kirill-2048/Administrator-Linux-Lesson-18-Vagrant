## Vagrant-стенд для обновления ядра и создания образа системы

## Цель
Научиться обновлять ядро в ОС Linux. Получение навыков работы с Vagrant.

## Задание
1. Запустить ВМ с помощью Vagrant.
2. Обновить ядро ОС из репозитория ELRepo.
3. Оформить отчет в README-файле в GitHub-репозитории.

## Выполнение

### Предварительные требования
* Хостовая система: Ubuntu 22.04 Desktop
* Vagrant: 2.4.8
* VirtualBox: <Введите `VBoxManage --version`>

### Процесс выполнения
1. Создан Vagrantfile с конфигурацией ВМ:
   * Имя ВМ: `kernel-update`
   * Box: `generic/centos8s` (версия: 4.3.4)
   * CPU: 2
   * RAM: 1024 Mb

Vagrantfile:

#Описываем Виртуальные машины

MACHINES = {

  #Указываем имя ВМ "kernel update"
  
  "kernel-update" => {
  
                  #Какой vm box будем использовать
                  :box_name => "generic/centos8s",
                  #Указываем box_version
                  :box_version => "4.3.4",
                  #Указываем количество ядер ВМ
                  :cpus => 2,
                  #Указываем количество ОЗУ в мегабайтах
                  :memory => 1024,
                }
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|
  
    # Отключаем проброс общей папки в ВМ
    
    config.vm.synced_folder ".", "/vagrant", disabled: true

    # Применяем конфигурацию ВМ
    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.box_version = boxconfig[:box_version]
      box.vm.host_name = boxname.to_s
      box.vm.provider "virtualbox" do |v|
        v.memory = boxconfig[:memory]
        v.cpus = boxconfig[:cpus]
        
      end
      
    end
    
  end
  
end

2. ВМ запущена командой `vagrant up`.

3. Выполнено подключение к ВМ командой `vagrant ssh`.

4. Проверена версия ядра **до** обновления:
[vagrant@kernel-update ~]$ uname -r
<Вставьте сюда старую версию ядра, например, 4.18.0-277.el8.x86_64>
text

5. Подключен репозиторий ELRepo:
```bash
sudo yum install -y https://www.elrepo.org/elrepo-release-8.el8.elrepo.noarch.rpm
6.	Установлено новое ядро из репозитория elrepo-kernel:
bash
sudo yum --enablerepo elrepo-kernel install kernel-ml -y
7.	Обновлена конфигурация загрузчика и установлено новое ядро по умолчанию:
bash
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
sudo grub2-set-default 0
8.	ВМ перезагружена командой sudo reboot.
9.	Проверена версия ядра после обновления:
text
[vagrant@kernel-update ~]$ uname -r
<Вставьте сюда новую версию ядра, например, 6.6.10-1.el8.elrepo.x86_64>

Результат:
Ядро ОС на виртуальной машине было успешно обновлено. Новая версия ядра: <Новая версия ядра>.
