# Misc 100

**FBI**

**Author: Kseniya Kravtsova (ksvark) and Vlad Roskov (vos)**

Миссия найти |b}|{а

**1. Ищем полезную инфу на исходном сайте**

https://hy17-fbi.spb.ctf.su/

Особо ничего нет, но зато у нас есть имя - **Yzheslav “Yzh” Hwong-Leejin**

**2. Пройдемся по соцсетям**

Сразу натыкаемся в ФБ на следующего пользователя: https://www.facebook.com/theblzh2016

Брутфорс по геометкам его посещений ничего не дает, но зато у нас есть логин - **theblzh2016**. Пробуем дальше

**3. Инста**

Натыкаемся на инсту Ыжика - https://www.instagram.com/theblzh2016/

С первого взгляда ничего нет, но это только на первый взгляд. Приглядимся к одной фотографии:

![The|b}|{](https://scontent-ams3-1.cdninstagram.com/t51.2885-15/e35/22277988_133263900653933_5450767961998491648_n.jpg)

Вот и оно! BSSID(mac). Если вспомнить, то существуют базы расположения wifi-точек.

Сразу же нагугливается удобная карта - https://find-wifi.mylnikov.org/

Начинаем вбивать и понимаем, что Ыжик в Люксембурге. 

Методом триангуляции, пыринга и брутфорса наконец вводим Rue Louvigny и получаем флаг **hy17{ffb9fec8c69eb0e0eb34d1d22529194b}**
