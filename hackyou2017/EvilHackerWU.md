# Forensic 200. EvilHacker

**Задание: We caught some evil hacker. What is his secret?**

**Author: Khanov Arthur (awengar)**

Скачиваем файл, это оказывается дамп памяти.

Используем программу Volatility.

**1. Раскукожим дамп и узнаем его метаданные.**

```shell
kerby@kerbydesktop ~/ctf> volatility imageinfo -f 20171009.mem 
Volatility Foundation Volatility Framework 2.6
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win7SP1x86_23418, Win7SP0x86, Win7SP1x86
                     AS Layer1 : IA32PagedMemoryPae (Kernel AS)
                     AS Layer2 : FileAddressSpace (/home/kerby/ctf/20171009.mem)
                      PAE type : PAE
                           DTB : 0x185000L
                          KDBG : 0x82741c68L
          Number of Processors : 1
     Image Type (Service Pack) : 1
                KPCR for CPU 0 : 0x82742d00L
             KUSER_SHARED_DATA : 0xffdf0000L
           Image date and time : 2017-10-08 21:05:33 UTC+0000
     Image local date and time : 2017-10-09 00:05:33 +0300
```

Круть, мы узнали его профиль. Переходим к следующему этапу.

**2. Узнаем больше информации, выведем запущенные процессы.**

kerby@kerbydesktop ~/ctf> volatility -f 20171009.mem --profile=Win7SP1x86_23418 pstree
   
```shell
Volatility Foundation Volatility Framework 2.6
Name                                                  Pid   PPid   Thds   Hnds Time
-------------------------------------------------- ------ ------ ------ ------ ----
 0x83d3acf0:System                                      4      0     87    553 2017-10-08 15:18:48 UTC+0000
. 0x8440dd28:smss.exe                                 256      4      2     29 2017-10-08 15:18:49 UTC+0000
 0x84b67518:wininit.exe                               384    320      3     74 2017-10-08 15:18:54 UTC+0000
(тут еще большой список)
 0x84b2e518:csrss.exe                                 328    320      9    442 2017-10-08 15:18:53 UTC+0000
 0x84b89d28:explorer.exe                             3716   3636     47   1200 2017-10-08 21:04:13 UTC+0000
. 0x851dd728:VBoxTray.exe                            2340   3716     13    159 2017-10-08 21:04:37 UTC+0000
. 0x85175308:RamCapture.exe                          2056   3716      6     69 2017-10-08 21:05:20 UTC+0000
. 0x8468ed28:regsvr32.exe                            3508   3716      0 ------ 2017-10-08 21:04:16 UTC+0000
. 0x84f7e030:7zG.exe                                 3832   3716      6     98 2017-10-08 21:05:30 UTC+0000
```

Уже интереснее - видим архив, больше ничего цепляющего, запомним его PID - 3832. 

**3. Сдампим имеющиеся файлы, которые относятся к процессу 3832.**

Описание метода - https://github.com/volatilityfoundation/volatility/wiki/Command-Reference#dumpfiles

```shell
kerby@kerbydesktop ~/ctf> volatility -f 20171009.mem --profile=Win7SP1x86_23418 dumpfiles -n -p 3832 -D dump/
Volatility Foundation Volatility Framework 2.6
DataSectionObject 0x850cea78   3832   \Device\HarddiskVolume2\Windows\Fonts\StaticCache.dat
SharedCacheMap 0x850cea78   3832   \Device\HarddiskVolume2\Windows\Fonts\StaticCache.dat
DataSectionObject 0x84e7dd30   3832   \Device\HarddiskVolume2\Users\hacker\Desktop\flag.txt.7z
SharedCacheMap 0x84e7dd30   3832   \Device\HarddiskVolume2\Users\hacker\Desktop\flag.txt.7z
----Далее куча DLL----
```
Сразу отмечаем архивы с расширением 7z, при открытии которых видим, что внутри завалялся flag.txt, но при 
попытке открыть просит пароль. Ну что поделать, гуглим.

**4. Натыкаемся на программу mimikatz И потом на плагин для volatility.**

https://xakep.ru/2012/11/22/extract-passwords-from-windows-memory/

Сам плагин: https://github.com/sans-dfir/sift-files/blob/master/volatility/mimikatz.py

Ставим его по этому туториалу https://www.computersecuritystudent.com/FORENSICS/LosBuntu/lesson4/index.html

Запускаем:
```shell
kerby@kerbydesktop ~/ctf> volatility --plugins=/usr/share/volatility/contrib/plugins/ -f 20171009.mem --profile=Win7SP1x86 mimikatz
                          
Volatility Foundation Volatility Framework 2.6
*** Failed to import volatility.plugins.malware.zeusscan (ImportError: No module named zeusscan)
*** Failed to import volatility.plugins.malware.poisonivy (ImportError: No module named poisonivy)
*** Failed to import volatility.plugins.malware.psempire (ImportError: No module named psempire)
Module   User             Domain           Password                                
-------- ---------------- ---------------- ----------------------------------------
wdigest  hacker           rev-1            eeXuRee4Eokahm5i                        
wdigest  REV-1$           WORKGROUP
```

Ну и собственно наш пароль - eeXuRee4Eokahm5i

Флаг - **L33t_L1k3_L00K_D33P**
