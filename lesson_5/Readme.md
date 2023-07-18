## Родионов Дмитрий | 3252 | Урок 5

#### Задание

<details> 
  <summary>Задача</summary>
Создать сервис, состоящий из 2 различных контейнеров: 
1 - веб, 2 - БД (compose)
</details>

#### Текст обьяснение:

Для реализации задачи создадим манифест файл 
с названием compose.yaml

>vim compose.yaml

И наполним его содержимым, соблюдая отступы  

Где:  

первый сервис с названием db скачает образ mariadb с docker hub  
и создаст контейнер с этим образом  
будет всегда перезапускать этот контейнер если он остановлен
и имеет одну переменную с паролем к базе данных

второй сервис с названием adminer скачает образ adminer с docker hub
и создаст контейнер с этим образом  
будет всегда перезапускать этот контейнер если он остановлен
и откроет порт 6080 для связи с первым контейнером

```yaml
version: '3.9'

services:

  db:
    image: mariadb:10.10.2
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: 12345

  adminer:
    image: adminer:4.8.1
    restart: always
    ports:
      - 6080:8080
```

Запустим наш проект в режиме демона  

>docker-compose up -d

Сначала произойдет скачивание образов.  
Потом запуск контейнеров.  

Посмотрим статус контейнеров
>docker-compose ps

Посмотрим наши образы
>docker-compose images

И проверим через curl доступен ли наш adminer
> curl http://localhost:6080/

Просмотрим логи наших сервисов
> docker-compose logs -f db  

> docker-compose logs -f adminer

#### Логи:

root@dmitry-vb:/home/roddg/compo# vim compose.yaml  

[1]+  Остановлен    vim compose.yaml  
root@dmitry-vb:/home/roddg/compo#  
root@dmitry-vb:/home/roddg/compo# docker-compose up -d  
[+] Running 17/17    
✔ db 8 layers [⣿⣿⣿⣿⣿⣿⣿⣿]      0B/0B      Pulled           43.2s  
✔ 10ac4908093d Pull complete                               10.9s  
✔ 44779101e748 Pull complete                               11.0s  
✔ a721db3e3f3d Pull complete                               11.8s  
✔ 1850a929b84a Pull complete                               11.9s  
✔ 397a918c7da3 Pull complete                               12.0s  
✔ 806be17e856d Pull complete                               37.9s  
✔ 634de6c90876 Pull complete                               38.0s  
✔ cd00854cfb1a Pull complete                               38.0s  
✔ adminer 7 layers [⣿⣿⣿⣿⣿⣿⣿]      0B/0B      Pulled       42.7s  
✔ 34df401c391c Pull complete                               30.5s  
✔ 942860e9b081 Pull complete                               36.4s  
✔ f571177b537e Pull complete                               36.6s  
✔ 78d7a59571f8 Pull complete                               36.8s  
✔ 530e7e02f755 Pull complete                               37.0s  
✔ 03ee8734c62c Pull complete                               37.4s  
✔ ed7a0cc37cf2 Pull complete                              37.5s  
[+] Running 2/2  
✔ Container compo-db-1       Started  
2.0s  
✔ Container compo-adminer-1  Started  
2.0s  

root@dmitry-vb:/home/roddg/compo# docker-compose ps  
NAME                IMAGE               COMMAND                  SERVICE             CREATED             STATUS              PORTS  
compo-adminer-1     adminer:4.8.1       "entrypoint.sh php -…"   adminer             4 minutes ago       Up 4 minutes        0.0.0.0:6080->8080/tcp, :::6080->8080/tcp  
compo-db-1          mariadb:10.10.2     "docker-entrypoint.s…"   db                  4 minutes ago       Up 4 minutes        3306/tcp  

root@dmitry-vb:/home/roddg/compo# docker-compose images  
CONTAINER           REPOSITORY          TAG                 IMAGE ID            SIZE  
compo-adminer-1     adminer             4.8.1               34ad39a6f503        250MB  
compo-db-1          mariadb             10.10.2             039bd724508b        410MB  

root@dmitry-vb:/home/roddg/compo# curl http://localhost:6080/  
<!DOCTYPE html>
<html lang="en" dir="ltr">
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<meta name="robots" content="noindex">
<title>Login - Adminer</title>
<link rel="stylesheet" type="text/css" href="?file=default.css&amp;version=4.8.1">
<script src='?file=functions.js&amp;version=4.8.1' nonce="Mjc3YTg5MmRhZWZiYWQ0MTRhNjU3ZDNjY2NkN2RjYmU="></script>
<link rel="shortcut icon" type="image/x-icon" href="?file=favicon.ico&amp;version=4.8.1">
<link rel="apple-touch-icon" href="?file=favicon.ico&amp;version=4.8.1">

<body class="ltr nojs">
<script nonce="Mjc3YTg5MmRhZWZiYWQ0MTRhNjU3ZDNjY2NkN2RjYmU=">
mixin(document.body, {onkeydown: bodyKeydown, onclick: bodyClick, onload: partial(verifyVersion, '4.8.1', '?', '830559:240356')});
document.body.className = document.body.className.replace(/ nojs/, ' js');
var offlineMessage = 'You are offline.';
var thousandsSeparator = ',';
</script>

<div id="help" class="jush-sql jsonly hidden"></div>
<script nonce="Mjc3YTg5MmRhZWZiYWQ0MTRhNjU3ZDNjY2NkN2RjYmU=">mixin(qs('#help'), {onmouseover: function () { helpOpen = 1; }, onmouseout: helpMouseout});</script>

<div id="content">
<h2>Login</h2>
<div id='ajaxstatus' class='jsonly hidden'></div>
<form action='' method='post'>
<div></div>
<table cellspacing='0' class='layout'>
<tr><th>System<td><select name='auth[driver]'><option value="server" selected>MySQL<option value="sqlite">SQLite 3<option value="sqlite2">SQLite 2<option value="pgsql">PostgreSQL<option value="oracle">Oracle (beta)<option value="mssql">MS SQL (beta)<option value="mongo">MongoDB (alpha)<option value="elastic">Elasticsearch (beta)</select><script nonce="Mjc3YTg5MmRhZWZiYWQ0MTRhNjU3ZDNjY2NkN2RjYmU=">qsl('select').onchange = function () { loginDriver(this); };</script>
<tr><th>Server<td><input name="auth[server]" value="db" title="hostname[:port]" placeholder="localhost" autocapitalize="off">
<tr><th>Username<td><input name="auth[username]" id="username" value="" autocomplete="username" autocapitalize="off"><script nonce="Mjc3YTg5MmRhZWZiYWQ0MTRhNjU3ZDNjY2NkN2RjYmU=">focus(qs('#username')); qs('#username').form['auth[driver]'].onchange();</script>
<tr><th>Password<td><input type="password" name="auth[password]" autocomplete="current-password">
<tr><th>Database<td><input name="auth[db]" value="" autocapitalize="off">
</table>
<p><input type='submit' value='Login'>
<label><input type='checkbox' name='auth[permanent]' value='1'>Permanent login</label>
</form>
</div>

<form action='' method='post'>
<div id='lang'>Language: <select name='lang'><option value="en" selected>English<option value="ar">العربية<option value="bg">Български<option value="bn">বাংলা<option value="bs">Bosanski<option value="ca">Català<option value="cs">Čeština<option value="da">Dansk<option value="de">Deutsch<option value="el">Ελληνικά<option value="es">Español<option value="et">Eesti<option value="fa">فارسی<option value="fi">Suomi<option value="fr">Français<option value="gl">Galego<option value="he">עברית<option value="hu">Magyar<option value="id">Bahasa Indonesia<option value="it">Italiano<option value="ja">日本語<option value="ka">ქართული<option value="ko">한국어<option value="lt">Lietuvių<option value="ms">Bahasa Melayu<option value="nl">Nederlands<option value="no">Norsk<option value="pl">Polski<option value="pt">Português<option value="pt-br">Português (Brazil)<option value="ro">Limba Română<option value="ru">Русский<option value="sk">Slovenčina<option value="sl">Slovenski<option value="sr">Српски<option value="sv">Svenska<option value="ta">த‌மிழ்<option value="th">ภาษาไทย<option value="tr">Türkçe<option value="uk">Українська<option value="vi">Tiếng Việt<option value="zh">简体中文<option value="zh-tw">繁體中文</select><script nonce="Mjc3YTg5MmRhZWZiYWQ0MTRhNjU3ZDNjY2NkN2RjYmU=">qsl('select').onchange = function () { this.form.submit(); };</script> <input type='submit' value='Use' class='hidden'>
<input type='hidden' name='token' value='204649:793042'>
</div>
</form>
<div id="menu">
<h1>
<a href='https://www.adminer.org/' target="_blank" rel="noreferrer noopener" id='h1'>Adminer</a> <span class="version">4.8.1</span>
<a href="https://www.adminer.org/#download" target="_blank" rel="noreferrer noopener" id="version"></a>
</h1>
</div>
<script nonce="Mjc3YTg5MmRhZWZiYWQ0MTRhNjU3ZDNjY2NkN2RjYmU=">setupSubmitHighlight(document);</script>


root@dmitry-vb:/home/roddg/compo# docker-compose logs -f db  
compo-db-1  | 2023-07-18 04:07:30+00:00 [Note] [Entrypoint]: Entrypoint script for MariaDB Server 1:10.10.2+maria~ubu2204 started.  
compo-db-1  | 2023-07-18 04:07:31+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'  
compo-db-1  | 2023-07-18 04:07:31+00:00 [Note] [Entrypoint]: Entrypoint script for MariaDB Server 1:10.10.2+maria~ubu2204 started.  
compo-db-1  | 2023-07-18 04:07:32+00:00 [Note] [Entrypoint]: Initializing database files  
compo-db-1  |  
compo-db-1  |  
compo-db-1  |  
compo-db-1  |  
compo-db-1  | PLEASE REMEMBER TO SET A PASSWORD FOR THE MariaDB root USER !  
compo-db-1  | To do so, start the server, then issue the following command:  
compo-db-1  |  
compo-db-1  |  
compo-db-1  | '/usr/bin/mysql_secure_installation'  
compo-db-1  |  
compo-db-1  |  
compo-db-1  | which will also give you the option of removing the test  
compo-db-1  | databases and anonymous user created by default.  This is  
compo-db-1  | strongly recommended for production servers.  
compo-db-1  |  
compo-db-1  |  
compo-db-1  | See the MariaDB Knowledgebase at https://mariadb.com/kb  
compo-db-1  |  
compo-db-1  |  
compo-db-1  | Please report any problems at https://mariadb.org/jira  
compo-db-1  |  
compo-db-1  |  
compo-db-1  | The latest information about MariaDB is available at https://mariadb.org/.  
compo-db-1  |  
compo-db-1  |  
compo-db-1  | Consider joining MariaDB's strong and vibrant community:  
compo-db-1  | https://mariadb.org/get-involved/  
compo-db-1  |  
compo-db-1  |  
compo-db-1  | 2023-07-18 04:07:34+00:00 [Note] [Entrypoint]: Database files initialized  
compo-db-1  | 2023-07-18 04:07:34+00:00 [Note] [Entrypoint]: Starting temporary server  
compo-db-1  | 2023-07-18 04:07:34+00:00 [Note] [Entrypoint]: Waiting for server startup  
compo-db-1  | 2023-07-18  4:07:34 0 [Note] mariadbd (server 10.10.2-MariaDB-1:10.10.2+maria~ubu2204) starting as process 91 ...  
compo-db-1  | 2023-07-18  4:07:34 0 [Note] InnoDB: Compressed tables use zlib 1.2.11  
compo-db-1  | 2023-07-18  4:07:34 0 [Note] InnoDB: Number of transaction pools: 1  
compo-db-1  | 2023-07-18  4:07:34 0 [Note] InnoDB: Using crc32 + pclmulqdq instructions  
compo-db-1  | 2023-07-18  4:07:34 0 [Note] mariadbd: O_TMPFILE is not supported on /tmp (disabling future attempts)  
compo-db-1  | 2023-07-18  4:07:34 0 [Note] InnoDB: Using liburing  
compo-db-1  | 2023-07-18  4:07:34 0 [Note] InnoDB: Initializing buffer pool, total size = 128.000MiB, chunk size = 2.000MiB  
compo-db-1  | 2023-07-18  4:07:34 0 [Note] InnoDB: Completed initialization of buffer pool  
compo-db-1  | 2023-07-18  4:07:34 0 [Note] InnoDB: File system buffers for log disabled (block size=512 bytes)  
compo-db-1  | 2023-07-18  4:07:34 0 [Note] InnoDB: 128 rollback segments are active.  
compo-db-1  | 2023-07-18  4:07:34 0 [Note] InnoDB: Setting file './ibtmp1' size to 12.000MiB. Physically writing the file full; Please wait ...  
compo-db-1  | 2023-07-18  4:07:34 0 [Note] InnoDB: File './ibtmp1' size is now 12.000MiB.  
compo-db-1  | 2023-07-18  4:07:34 0 [Note] InnoDB: log sequence number 46590; transaction id 14  
compo-db-1  | 2023-07-18  4:07:34 0 [Note] Plugin 'FEEDBACK' is disabled.  
compo-db-1  | 2023-07-18  4:07:34 0 [Warning] 'user' entry 'root@e09ceb42f057' ignored in --skip-name-resolve mode.  
compo-db-1  | 2023-07-18  4:07:34 0 [Warning] 'proxies_priv' entry '@% root@e09ceb42f057' ignored in --skip-name-resolve mode.  
compo-db-1  | 2023-07-18  4:07:34 0 [Note] mariadbd: ready for connections.  
compo-db-1  | Version: '10.10.2-MariaDB-1:10.10.2+maria~ubu2204'  socket: '/run/mysqld/mysqld.sock'  port: 0  mariadb.org binary distribution  
compo-db-1  | 2023-07-18 04:07:35+00:00 [Note] [Entrypoint]: Temporary server started.  
compo-db-1  | 2023-07-18 04:07:37+00:00 [Note] [Entrypoint]: Securing system users (equivalent to running mysql_secure_installation)  
compo-db-1  |  
compo-db-1  |  
compo-db-1  | 2023-07-18 04:07:37+00:00 [Note] [Entrypoint]: Stopping temporary server  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] mariadbd (initiated by: unknown): Normal shutdown  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: FTS optimize thread exiting.  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: Starting shutdown...  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: Dumping buffer pool(s) to /var/lib/mysql/ib_buffer_pool  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: Buffer pool(s) dump completed at 230718  4:07:37  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: Removed temporary tablespace data file: "./ibtmp1"  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: Shutdown completed; log sequence number 46590; transaction id 15  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] mariadbd: Shutdown complete  
compo-db-1  |  
compo-db-1  |  
compo-db-1  | 2023-07-18 04:07:37+00:00 [Note] [Entrypoint]: Temporary server stopped  
compo-db-1  |  
compo-db-1  |  
compo-db-1  | 2023-07-18 04:07:37+00:00 [Note] [Entrypoint]: MariaDB init process done. Ready for start up.  
compo-db-1  |  
compo-db-1  |  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] mariadbd (server 10.10.2-MariaDB-1:10.10.2+maria~ubu2204) starting as process 1 ...  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: Compressed tables use zlib 1.2.11  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: Number of transaction pools: 1  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: Using crc32 + pclmulqdq instructions  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] mariadbd: O_TMPFILE is not supported on /tmp (disabling future attempts)  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: Using liburing  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: Initializing buffer pool, total size = 128.000MiB, chunk size = 2.000MiB  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: Completed initialization of buffer pool  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: File system buffers for log disabled (block size=512 bytes)  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: 128 rollback segments are active.  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: Setting file './ibtmp1' size to 12.000MiB. Physically writing the file full; Please wait ...  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: File './ibtmp1' size is now 12.000MiB.  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: log sequence number 46590; transaction id 14  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] Plugin 'FEEDBACK' is disabled.  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: Loading buffer pool(s) from /var/lib/mysql/ib_buffer_pool  
compo-db-1  | 2023-07-18  4:07:37 0 [Warning] You need to use --log-bin to make --expire-logs-days or --binlog-expire-logs-seconds work.  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] InnoDB: Buffer pool(s) load completed at 230718  4:07:37  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] Server socket created on IP: '0.0.0.0'.  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] Server socket created on IP: '::'.  
compo-db-1  | 2023-07-18  4:07:37 0 [Note] mariadbd: ready for connections.  
compo-db-1  | Version: '10.10.2-MariaDB-1:10.10.2+maria~ubu2204'  socket: '/run/mysqld/mysqld.sock'  port: 3306  mariadb.org binary distribution  

root@dmitry-vb:/home/roddg/compo# docker-compose logs -f adminer  
compo-adminer-1  | [Tue Jul 18 04:07:30 2023] PHP 7.4.33 Development Server (http://[::]:8080) started  
compo-adminer-1  | [Tue Jul 18 04:08:26 2023] [::ffff:172.18.0.1]:44934 Accepted  
compo-adminer-1  | [Tue Jul 18 04:08:26 2023] [::ffff:172.18.0.1]:44934 [200]: GET /  
compo-adminer-1  | [Tue Jul 18 04:08:26 2023] [::ffff:172.18.0.1]:44934 Closing  
compo-adminer-1  | [Tue Jul 18 04:08:26 2023] [::ffff:172.18.0.1]:44950 Accepted  
compo-adminer-1  | [Tue Jul 18 04:08:26 2023] [::ffff:172.18.0.1]:44950 [200]: GET /?file=default.css&version=4.8.1  
compo-adminer-1  | [Tue Jul 18 04:08:26 2023] [::ffff:172.18.0.1]:44950 Closing  
compo-adminer-1  | [Tue Jul 18 04:15:54 2023] [::ffff:172.18.0.1]:55692 Accepted  
compo-adminer-1  | [Tue Jul 18 04:15:54 2023] [::ffff:172.18.0.1]:55692 [200]: GET /  
compo-adminer-1  | [Tue Jul 18 04:15:54 2023] [::ffff:172.18.0.1]:55692 Closing  


#### История(history):

623  vim compose.yaml  
624  docker-compose up -d  
625  docker-compose ps  
626  docker-compose images  
627  curl http://localhost:6080/  
628  docker-compose logs -f db  
629  docker-compose logs -f adminer  
630  history  

#### Скриншот:

![composeUp.jpg](img%2FcomposeUp.jpg)