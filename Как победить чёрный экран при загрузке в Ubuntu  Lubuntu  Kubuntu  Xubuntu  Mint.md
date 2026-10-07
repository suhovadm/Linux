Как победить чёрный экран при загрузке в Ubuntu / Lubuntu / Kubuntu / Xubuntu / Mint ?  

1. Нажимаем Ctrl + Alt + F2, либо F3. Попадаем в терминал.  

2. Вводим свой логопас.  

3. Дальше вводим: sudo nano /etc/default/grub  
Если ещё раз попросит пароль - вводим.  

4. Ищем строку:  

GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"  

и меняем её на  

GRUB_CMDLINE_LINUX_DEFAULT="nomodeset"  

5. Сохраняемся, подтверждаем, выходим.  
Ctrl + X, Y, Enter.  

6. Теперь нужно перечитать и перезаписать загрузчик.  
sudo update-grub  

7. Всё, перезагружаемся.  
sudo reboot  

8. Как загрузиться с установочного диска?  
В моменте, где происходит выбор параметров:  

Try Xubuntu without installing  
Install Xubuntu  
OEM install (for manufacturers)  
Check disc for defects  

Нажимаем клавишу "E" на клавиатуре.  
И здесь точно также меняем quiet splash на nomodeset  
Дальше, чтобы загрузиться с уже отредактированной строкой, нажимаем Ctrl + X, или F10.  

9. Почему вообще так происходит?  
Это происходит на старом оборудовании, типа медицинских анализаторов,  
станков с ЧПУ и прочем, или на старом компьютерном железе.  
