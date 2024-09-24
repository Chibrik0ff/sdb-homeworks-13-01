# Домашнее задание к занятию "`Уязвимости и атаки на информационные системы`" - `Чибриков Станислав`

### Задание 1

Скачайте и установите виртуальную машину Metasploitable: https://sourceforge.net/projects/metasploitable/.

Это типовая ОС для экспериментов в области информационной безопасности, с которой следует начать при анализе уязвимостей.

Просканируйте эту виртуальную машину, используя **nmap**.

Попробуйте найти уязвимости, которым подвержена эта виртуальная машина.

Сами уязвимости можно поискать на сайте https://www.exploit-db.com/.

Для этого нужно в поиске ввести название сетевой службы, обнаруженной на атакуемой машине, и выбрать подходящие по версии уязвимости.

Ответьте на следующие вопросы:

- Какие сетевые службы в ней разрешены?
```
nmap -sv -O 192.168.0.11
```
977 портов закрыты - 23 открыты

**Службы** - ftp, ssh, telnet, smtp, domain, http, rpcbind, netbios-ssn, exec, login, tcpwrapped, java-rmi, bindshell, nfs, ftp, mysql, postgresql, vnc, X11, irc, ajp13, http

![screen](https://github.com/Chibrik0ff/sflt-homeworks3/blob/main/img/img1.png)
  
- Какие уязвимости были вами обнаружены? (список со ссылками: достаточно трёх уязвимостей)
```
https://www.exploit-db.com/exploits/17491
https://www.exploit-db.com/exploits/6122
https://www.exploit-db.com/exploits/30020
https://www.exploit-db.com/exploits/27407
https://www.exploit-db.com/exploits/31965
```

### Задание 2

Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.

Запишите сеансы сканирования в Wireshark.

Ответьте на следующие вопросы:

- Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?
- Как отвечает сервер?

**SYN** - Nmap посылает SYN-пакет, как бы намереваясь открыть настоящее соединение, и ожидает ответ. Наличие флагов SYN|ACK в ответе указывает на то, что порт удаленной машины открыт и прослушивается. Флаг RST в ответе означает обратное. Если Nmap принял пакет SYN|ACK, то в ответ немедленно отправляет RST-пакет для сброса еще не установленного соединения
```
nmap -sS <ip> - TCP SYN сканирование. SYN это используемый по умолчанию и наиболее популярный тип сканирования. 
```

![screen](https://github.com/Chibrik0ff/sflt-homeworks3/blob/main/img/img2.png)

**FIN** - Nmap посылает FIN-пакет, в TCP заголовок ставится флаг FIN. Согласно RFC 793, на прибывший FIN-пакет на закрытый порт сервер должен ответить пакетом RST. FIN-пакеты на открытые порты должны игнорироваться сервером. По этому различию становится возможным отличить закрытый порт от открытого. 
```
nmap -sF <ip> - FIN сканирование. Устанавливается только TCP FIN бит.
```

![screen](https://github.com/Chibrik0ff/sflt-homeworks3/blob/main/img/img3.png)

**Xmas** - Устанавливаются FIN, PSH и URG флаги. Если в результате FIN-сканирования мы получили список открытых портов, то это не Windows. Если же все эти методы выдали результат, что все порты закрыты, а SYN-сканирование обнаружило открытые порты, то мы скорей всего имеете дело с ОС Windows, Cisco, BSDI, IRIX, HP/UX и MVS. Все эти ОС не отправляют RST-пакеты.
```
nmap -sX <ip> - Xmas сканирование. Устанавливаются FIN, PSH и URG флаги.
```

![screen](https://github.com/Chibrik0ff/sflt-homeworks3/blob/main/img/img4.png)

**UDP** - На каждый порт сканируемой машины отправляется UDP-пакет без данных. Этот метод используется для определения, какие UDP-порты на сканируемом хосте являются открытыми.Если в ответ было получено ICMP-сообщение "порт недоступен", это означает, что порт закрыт. В противном случае предполагается, что сканируемый порт открыт.
```
nmap -sU <ip> - Различные типы UDP сканирования. Большой проблемой при UDP сканировании является его медленная скорость работы.
```

![screen](https://github.com/Chibrik0ff/sflt-homeworks3/blob/main/img/img5.png)
