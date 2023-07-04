## Родионов Дмитрий | 3252 | Урок 1 

### Изоляция процесса корневого каталога файловой системы

#### Текст обьяснения:

Войдем рутом.

>su  
Пароль:

Создадим каталоги в домашнем каталоге, которые нам понадобятся для изоляции компандного интерпритатора.

>mkdir GB  
mkdir GB/bin

Проверим что создались

>ls GB

Скопируем в него рабочую папку из основной файловой системы в изолированную.

>cp /bin/bash GB/bin

Проверим что скопировалось.

>ls GB/bin

Узнаем какие библиотеки нам еще нужны для работы.

>ldd /bin/bash

Создадим дополнительные каталоги в изолированной системе для этих библиотек.

>mkdir GB/lib  
mkdir GB/lib64

Проверим что создались

>ls GB

Скопируем туда эти библиотеки.

>cp /lib/x86_64-linux-gnu/libtinfo.so.6 GB/lib  
cp /lib/x86_64-linux-gnu/libc.so.6 GB/lib  
cp /lib64/ld-linux-x86-64.so.2 GB/lib64/

Запустим изоляцию интерпритатора.

>chroot GB /bin/bash

#### Логи:

❯ su  
Пароль:  
root@dmitry-vb:/home/roddg# mkdir GB  
root@dmitry-vb:/home/roddg# mkdir GB/bin  
root@dmitry-vb:/home/roddg# ls GB  
bin  
root@dmitry-vb:/home/roddg# cp /bin/bash GB/bin  
root@dmitry-vb:/home/roddg# ls GB/bin  
bash  
root@dmitry-vb:/home/roddg# ldd /bin/bash  
	linux-vdso.so.1 (0x00007ffee29db000)  
	libtinfo.so.6 => /lib/x86_64-linux-gnu/libtinfo.so.6 (0x00007f0d1947e000)  
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f0d19200000)  
	/lib64/ld-linux-x86-64.so.2 (0x00007f0d19622000)  
root@dmitry-vb:/home/roddg# mkdir GB/lib  
root@dmitry-vb:/home/roddg# mkdir GB/lib64  
root@dmitry-vb:/home/roddg# ls GB  
bin  lib  lib64  
root@dmitry-vb:/home/roddg# cp /lib/x86_64-linux-gnu/libtinfo.so.6 GB/lib  
root@dmitry-vb:/home/roddg# cp /lib/x86_64-linux-gnu/libc.so.6 GB/lib  
root@dmitry-vb:/home/roddg# cp /lib64/ld-linux-x86-64.so.2 GB/lib64/  
root@dmitry-vb:/home/roddg# chroot GB /bin/bash  
bash-5.1#  

#### История(history):

su  
mkdir GB  
mkdir GB/bin  
ls GB  
cp /bin/bash GB/bin  
ls GB/bin  
ldd /bin/bash  
mkdir GB/lib  
mkdir GB/lib64  
ls GB  
cp /lib/x86_64-linux-gnu/libtinfo.so.6 GB/lib  
cp /lib/x86_64-linux-gnu/libc.so.6 GB/lib  
cp /lib64/ld-linux-x86-64.so.2 GB/lib64/  
chroot GB /bin/bash  
history  

#### Сриншот:

![bashFiles.png](img%2FbashFiles.png)

---

### Изоляция сети

#### Текст обьяснения:

Войдем рутом.

>su  
Пароль:

Создадим сетевое пространство имен с названием testns:

>ip netns add testns

Проверим в какой мы сети:

>ip a

Проверим существование изолированного пространства сети:

>ip netns list

Запустим изоляцию командного интерпретатора в сетевом пространстве имен:

>ip netns exec testns bash

Проверим сеть:

>ip a

#### Логи:

❯ su  
Пароль:  
root@dmitry-vb:/home/roddg# ip netns add testns  
root@dmitry-vb:/home/roddg# ip a  
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000  
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00  
    inet 127.0.0.1/8 scope host lo  
       valid_lft forever preferred_lft forever  
    inet6 ::1/128 scope host  
       valid_lft forever preferred_lft forever  
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000  
    link/ether 08:00:27:90:f9:90 brd ff:ff:ff:ff:ff:ff  
    inet 192.168.1.213/24 brd 192.168.1.255 scope global dynamic noprefixroute enp0s3  
       valid_lft 81352sec preferred_lft 81352sec  
    inet6 fe80::f16f:202e:acb8:2017/64 scope link noprefixroute  
       valid_lft forever preferred_lft forever  
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default  
    link/ether 02:42:23:a1:25:96 brd ff:ff:ff:ff:ff:ff  
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0  
       valid_lft forever preferred_lft forever  
root@dmitry-vb:/home/roddg# ip netns list testns  
root@dmitry-vb:/home/roddg# ip netns exec testns bash  
root@dmitry-vb:/home/roddg# ip a  
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN group default qlen 1000  
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00  
root@dmitry-vb:/home/roddg#  

#### История(history):

su  
ip netns add testns  
ip a  
ip netns list   
ip netns exec testns bash  
ip a

#### Скриншот:

![bashNets.png](img%2FbashNets.png)

---

### Изоляция сетевого пронстранства имен, дерева процессов, памяти, процессов:

#### Текст обьяснения

Войдем рутом.

>su  
Пароль:

Запустим изоляцию командного интерпретатора:

>unshare --net --pid --fork --mount-proc /bin/bash

Проверим:

>ps aux

Проверим что имеем доступ к файловой системе:

>ls

#### Логи:

❯ su  
Пароль:  
root@dmitry-vb:/home/roddg# unshare --net --pid --fork --mount-proc /bin/bash  
root@dmitry-vb:/home/roddg# ps aux  
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND  
root           1  0.0  0.0  18888  4464 pts/2    S    17:46   0:00 /bin/bash  
root           7  0.0  0.0  21356  1580 pts/2    R+   17:46   0:00 ps aux  
root@dmitry-vb:/home/roddg# ls  
100.sh             Dockerfile    example3.c    example.sh   lines_count.txt   nginx_signing.key   test.txt   Документы     Музыка          Шаблоны  
deltempfiles.sh    example2.c    example3.sh   GB           name_user.sh      skript1.sh          text.txt   Загрузки      Общедоступные  
developer_folder   example2.sh   example.c     hello.py     nano              snap                Видео      Изображения  'Рабочий стол'  
root@dmitry-vb:/home/roddg#  

#### История(history)

su  
unshare –net –pid –fork –mount-proc /bin/bash  
unshare --net --pid --fork --mount-proc /bin/bash  
ps aux  
ls  
history

#### Скриншот

![bashProc.png](img%2FbashProc.png)