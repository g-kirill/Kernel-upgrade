# Kernel-update



1. Создаём Vagrantfile
 
2. Подключаемся по ssh к созданной виртуальной машине
 ubuntu@ubuntu22:~/Kernel_upgrade$ vagrant up
 ubuntu@ubuntu22:~/Kernel_upgrade$ vagrant ssh

==Текущая версия ядра==
vagrant@kernel-update:~$ uname -r
5.4.0-189-generic

3. Для загрузки исходного кода ядра Linux использую команду wget:
 wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.4.279.tar.xz

4. Извлекаю исходный код
 tar xvf linux-5.4.279.tar.xz

5. Устанавливаю необходимые пакеты
 sudo apt-get install git fakeroot build-essential ncurses-dev xz-utils libssl-dev bc flex libelf-dev bison

6. Настройка ядра
 vagrant@kernel-update:~$ cd linux-5.4.279
 vagrant@kernel-update:~/linux-5.4.279$ cp -v /boot/config-$(uname -r) .config

7. Cборка ядра
 Меняем строки в файле .config
 vagrant@kernel-update:~/linux-5.4.279$ scripts/config --disable SYSTEM_TRUSTED_KEYS
 Меняем CONFIG_DEBUG_INFO_BTF=y на CONFIG_DEBUG_INFO_BTF=n
 Далее запускаем команды:
 vagrant@kernel-update:~/linux-5.4.279$ make
 vagrant@kernel-update:~/linux-5.4.279$ sudo make modules_install
 vagrant@kernel-update:~/linux-5.4.279$ sudo make install

8. Перезагружаем машину
 vagrant@kernel-update:~/linux-5.4.279$ sudo reboot

9. Просмотр версии ядра
 vagrant@kernel-update:~/linux-5.4.279$ uname -mrs
 Linux 5.4.279 x86_64
