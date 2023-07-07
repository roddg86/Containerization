## Родионов Дмитрий | 3252 | Урок 2

#### Задание
Запустить контейнер с ubuntu, используя механизм LXC.  
Ограничить контейнер 256 Мб ОЗУ и проверить, что ограничение работает.  
Добавить автозапуск контейнеру, перезагрузить ОС и убедиться, что контейнер действительно запустился самостоятельно.  

#### Текст обьяснение

Создание контейнера.  
Процесс скачивания запускается и длится несколько минут.

>lxc-create -n test123 -t ubuntu

Запуск контейнера.

>lxc-start -d -n test123

Подключение к контейнеру.

>lxc-attach -n test123

Выйти из контейнера.

>exit

Посмотреть на созданный контейнер в файловой системе.

>ll /sys/fs/cgroup/  

Отредактировать конфиг.  
Сделать ограничение оперативной памяти.  
Сделать автозапуск.  

>nano /var/lib/lxc/test123/config

>lxc.cgroup2.memory.max = 256M  
>lxc.start.auto = 1

Посмотреть автозапуск контейнера.

>lxc-ls -f

Посмотреть занятую память.  

>free -m

Остановить контейнер.

>lxc-stop -n test123

#### Логи

root@dmitry-vb:/home/roddg# lxc-create -n test123 -t ubuntu  
lxc-create: test123: tools/lxc_create.c: main: 264 Container already exists   
root@dmitry-vb:/home/roddg# nano /var/lib/lxc/test123/config  
root@dmitry-vb:/home/roddg# lxc-ls -f  
NAME    STATE   AUTOSTART GROUPS IPV4      IPV6 UNPRIVILEGED  
test123 RUNNING 1         -      10.0.3.42 -    false  
root@dmitry-vb:/home/roddg# lxc-start -n test123  
lxc-start: test123: tools/lxc_start.c: main: 256 Container is already running  
root@dmitry-vb:/home/roddg# lxc-attach -n test123  
root@test123:/# free -m  
total        used        free      shared  buff/cache   available  
Память:        256          26         228           0           0         229  
Подкачка:          0           0           0  
root@test123:/# exit  
exit  
root@dmitry-vb:/home/roddg# free -m  
total        used        free      shared  buff/cache   available  
Память:      15989        1461       11066          41        3461       14203  
Подкачка:       2047           0        2047  
root@dmitry-vb:/home/roddg# lxc-stop -n test123  
root@dmitry-vb:/home/roddg# lxc-ls -f  
NAME    STATE   AUTOSTART GROUPS IPV4 IPV6 UNPRIVILEGED  
test123 STOPPED 1         -      -    -    false  
root@dmitry-vb:/home/roddg#  

#### История(history)

167  lxc-create -n test123 -t ubuntu  
168  lxc-start -n test123  
169  ll /sys/fs/cgroup/  
170  lxc-attach -n test123  
171  nano /var/lib/lxc/test123/config  
172  lxc-ls -f  
173  lxc-stop -n test123  
174  lxc-start -n test123  
175  lxc-ls -f  
176  nano /var/lib/lxc/test123/config  
177  lxc-create -n test123 -t ubuntu  
178  nano /var/lib/lxc/test123/config  
179  lxc-ls -f  
180  lxc-start -n test123  
181  lxc-attach -n test123  
182  free -m  
183  lxc-stop -n test123  
184  lxc-ls -f  
185  history  

#### Сриншот

![configLxc.jpg](img%2FconfigLxc.jpg)

![screenShotLxc.jpg](img%2FscreenShotLxc.jpg)
