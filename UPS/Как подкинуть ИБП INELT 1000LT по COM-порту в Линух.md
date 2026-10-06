Как подключить ИБП inelt 1000LT2 по COM-порту к Линуксу и посмотреть его (ИБП-шника) спеки?  

____________________________________________________________________________________________  

1. Подключаемся по SSH к нужному серверу, в нашем случае это: 192.168.80.5, порт 22.  
Логин: root, пароль: password  
Как только подключились, сразу же переходим в режим root-a: sudo su  
Если попросит пароль - вводим ещё раз.  

2. Установим NUT на машину:  
sudo apt-get install nut  

3. Настроим правила.  
Создаем файл: sudo touch /lib/udev/rules.d/52_nut-serialups.rules  
Открываем файл: sudo nano /lib/udev/rules.d/52_nut-serialups.rules  

Внутри файлика напишем:  
# inelt  
KERNEL=="ttyS0",GROUP="nut"  

Здесь ttyS0 - это номер COM-порта к которму подключен ИБП, в нашем случае COM-1.  
Сохраняем, закрываем.  

4. Теперь, по идее, надо перезагрузить систему, или просто перезапустить службы.  
sudo udevadm control --reload_rules  
sudo udevadm control trigger  
Если эти две не сработают, вводим:  
systemctl restart nut*  
Она перезапустит всё, что связано с NUT.  

5. Теперь настроим сам NUT.  
sudo nano /etc/nut/nut.conf и смотрим, там должно быть: MODE=standalone  

6. Теперь вводим sudo nano /etc/nut/ups.conf  
Здесь должно быть вот так:  
[inelt]  
driver = blazer_ser  
port = /dev/ttyS0  
desc = "inelt monolith 1000LT" // это просто комментарий, какая модель ИБП и всё такое. Его можно не указывать на самом деле.  
cablepower = both  
// offdelay 6  
// ondelay 7  
// эти две можно добавлять, можно нет. Это переход в режим ожидания через столько-то минут.  

Листаем файлик в самый низ, до пункта [ups]. Если он закоментирован - раскоментируем его.  
[ups]  
driver = blazer_ser  
port = /dev/ttyS0  
// ignorelb - это эксплуатация ИБП в режиме игнорирования низкого заряда аккумулятора, то есть при команде override.battery.charge.low =  
// При данной команде, NUT запустит процедуру корректного выключения системы.  
ignorelb  
// Критический заряд батареи в процентах.  
override.battery.charge.low = 80  
// Опасный заряд батареи в процентах.  
override.battery.charge.warning = 70  
// Время в секундах, через которое отключается комп после низкого заряда батареи.  
overrdie.battery.runtime.low = 60  

7. Прописываем контроль доступа.  

sudo nano /etc/nut/upsd.conf и пишем внутрь файлика вот это:  

ACL all 0.0.0.0/0  
ACL localnet 192.168.1.0/24  
ACL localhost 127.0.0.1/32  
ACCEPT localhost localnet  
REJECT all  

Это можно вписать под закоментированный блок `#LISTEN <address> [<port>]`   

Всё, сохраняем, закрываем.  

8. Теперь заведем пользователей, которые могут контролировать ИБП.  

sudo nano /etc/upsd.users ( возможно здесь ошибка, возможно надо заходить по sudo nano /etc/nut/upsd.users )  

[suhovadm]  
passdword = здесь вводим суперсложный суперпароль  
allfrom = localnet  
upsmon master  
actions = SET  
instcmds = ALL  

// allfrom - это источник подключения.  
// upsmon master - параметр дающий права на управление ИБП.  

9. Последнее. Осталось настроить службу мониторинга.  

sudo nano /etc/nut/upsmon.conf  

RUN_AS_USER nut  
MONITOR inelt@localhost 1 suhovadm password master  
MINSUPPLIES 1  
POWERDOWNFLAG /etc/killpower  
SHUTDOWNCMD "sbin/shutdown -Ph +0"  
POLLFREQ 5  
POLLFREQALERT 5  
HOSTSYNC 15  
DEADTIME 15  
RBWARNTIME 43200  
NOCOMMWARNTIME 300  
FINALDELAY 5  

// SHUTDOWNCMD "sbin/shutdown -Ph +0" - команда на завершение работы компьютера.  

10. Всё перезагружаем удалённую машину командой: reboot ( sudo reboot )  
Если ввести poweroff (выключение), то включить сервак обратно можно будет только находясь физически рядом с ним :)  

11. После перезагрузки снова подключаемся к машине, вводим логин/пароль: suhovadm / password,  
переходим в root-a, и вводим: upsc inelt.  
inelt - это, понятное дело, имя нашего ИБП.  

************************************************************* ПОЛЕЗНЫЕ ССЫЛКИ. *************************************************************  

Ссылка на ту самую инструкцию:  
https://kubuntu.ru/node/10011?ysclid=lqoositmq9395105022  

Ссылка на список драйверов (что к чему подходит):  
https://networkupstools.org/stable-hcl.html  

Список сигналов бесперебойника:  
https://www.linux.org.ru/forum/admin/14495353?ysclid=lqowc87upf438397791  

И ещё одна полезная ссылочка на эту же тему (сигналы бесперебойника):  
https://www.linux.org.ru/forum/general/8478803?ysclid=lqox6ws96l391200300  

Просто ещё одна инструкция, чисто для ознакомления:  
https://vladimir-stupin.blogspot.com/2014/11/nut-eaton-powerware-5110.html  

Как посмотреть, что COM-порты включены и что на них висит:  
https://unix.stackexchange.com/questions/125183/how-to-find-which-serial-port-is-in-use  
