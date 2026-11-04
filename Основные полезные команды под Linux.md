Как в Линуксах посмотреть сколько места занимает система и какое количество ресурсов она потребляет?  
_________________________________________________________________________________________________  

Как узнать сколько места занимает сама система и конкретные папки?  

1) df -h // Сколько места система занимает на диске.  
2) Как узнать, какие папки занимают больше всего места:  
 
sudo du -h --max-depth=1 / 2>/dev/null  

3) Отсортировать по размеру:  

sudo du -h --max-depth=1 2>/dev/null | sort -hr  

4) Посмотреть размер конкретной папки:  

du -sh /var  
du -sh /usr  
du -sh /home  
du -sh /* // Размер основных папок.  
_________________________________________________________________________________________________  

Сколько ОЗУ потребляет система? Использование RAM.  
 
free -h  
_________________________________________________________________________________________________  

Сколько процессора потребляет система?  

top, или htop (htop нужно будет сначала установить).  

Ubuntu / Debian / Mint / Kali / Astra: sudo apt install htop  

ALT Linux: sudo apt-get install htop  

RED OS / Fedora / CentOS / Rocky Linux: sudo dnf install htop, либо sudo -E dnf install htop  

Arch Linux / Manjaro: sudo pacman -S htop  

Alpine Linux: sudo apk add htop  

openSUSE: sudo zypper install htop  

Проверить, что htop есть в системе можно командой: htop --version (везде должно быть одинаково).  
_________________________________________________________________________________________________  

Инфа о дисках.  

lsblk // Инфа о дисках (hdd).  

iostat -xz 1 // Нагрузка на диски (hdd). iostat тоже нужно будет установить.  

Ubuntu / Debian / Astra / Kali / Mint: sudo apt install sysstat  

ALT Linux: sudo apt-get install sysstat  

Fedora / CentOS / Rocky Linux: sudo dnf install sysstat  

Arch Linux / Manjaro: sudo pacman -S sysstat  

Alpine: sudo apk add sysstat  

openSUSE: sudo zypper install sysstat  

Проверить, что iostat в системе можно командой: iostat -V  
_________________________________________________________________________________________________  

Всякие другие полезные команды.  

vmstat 1 // Мониторинг системных ресурсов каждую секунду.  

uname -a // Общая инфа о системе.  

uname -r // Версия ядра Линукс.  

lscpu // Инфа о процессоре.  

sudo dmidecode -t memory // Инфа о памяти.  

uptime // Время работы системы и средняя нагрузка.  

cat /etc/os-release // Версия Линукса и какой вообще Линукс установлен - Дебиан, Убунта и т.д.  
Это нужно, если непонятно, к какому серверу мы подключаемся.  
_________________________________________________________________________________________________  

Если нужно посмотреть всё одной командой?  

inxi -C // Процессор.  
inxi -M // Материнка.  
inxi -m // Оперативка.  
inxi -D // Диски.  
inxi -G // Видяха.  
inxi -N // Сетевухи.  
inxi -S // ОС.  
inxi -Fxxx // Полная инфа о системе, либо так:  
inxi -F  

Она покажет информацию о:  

* процессоре;  
* оперативной памяти;  
* видеокарте;  
* дисках;  
* файловых системах;  
* операционной системе;  
* ядре Linux;  
* сетевых интерфейсах.  

inxi тоже нужно будет установить.  

Ubuntu / Debian / Kali / Astra / Mint: sudo apt install inxi  

ALT Linux: sudo apt-get install inxi  

RED OS / Fedora / CentOS / Rocky Linux: sudo dnf install inxi  

Arch / Manjaro: sudo pacman -S inxi  

Alpine: sudo apk add inxi  

openSUSE: sudo zypper install inxi  

Проверить установку можно командой: inxi --version  
_________________________________________________________________________________________________  

Менеджеры пакетов:  

Debian / Ubuntu / Astra / Kali / Mint: APT  

ALT: APT-RPM (APT-GET)  

RED OS / Fedora / RHEL: DNF  

Arch / Manjaro: PACMAN  

Alpine: APK  

openSUSE: ZYPPER  
_________________________________________________________________________________________________  
