https://github.com/BaggerFast/ItManuals/blob/main/markdown/deploy_tg_bot.md


`ssh root@ip`
`yes`
вводим пароль
1. делаем обновление пакетов и всех программ

`sudo apt clean all && sudo apt update && sudo apt dist-upgrade`
`y`
`keep the local version` y
везде далбше ок

2. создаем еще одного пользователя системы

`adduser user_name`
далее пишем пароль и инф по желанию

3. дальше пользователь должен стать админом

`gpasswd -a nadya sudo`

sudo - SuperUserDO

4. переключаемся на пользователя nadya

`su - nadya`

5. настраиваем публичные ключи

6. создаем папку для публичного ключа

`mkdir .ssh`

7. ограничиваем права доступа к этой папке

`chmod 700 .ssh`

8. для входа на сервер используется стандартный 22 порт, нужно заменить этот порт на какой-либо другой (зачем?)

9. смотрим, используется ли этот порт, который мы хотим, в системе

`grep -w 80 /etc/services` - свободен ли 80-й порт. если красное - то занят

проверяем так же 5050, если ничего не появилось, значит, он свободен

10. теперь в папке .ssh нужно создать файл для публичного ключа под названием authorised keys

`nano .ssh/authorized_keys`

11. в появившемся окне нужно вставить свой публичный ключ.
12. добываем свой публичный ключ

- открыть новую консоль
- `ssh-keygen` - команда для генерации публичного ключа
- спросит в какой файл сохранить публичный ключ, просто можно нажать энтер

12. теперь нужно вывести на экран публичный ключ и скопировать его, пока что мы в консоли нашего компа

`cat ~/.ssh/id_rsa.pub` - выведет ключ
13. выделяем его и копируем и вставляем в nano. вставлять аккуратно, а то фигня какая-то получается
14. далее ctrl-x, yes, enter - выходим обратно в консоль
15. ограничивам права доступа к этому файлу

`chmod 600 .ssh/authorized_keys` - код 600 - это разрешение на чтение и запись
16. выходим из этого пользователя (автоматически переключит на пользователя root)

`exit`
17. открываем конфигурационный файл

`nano /etc/ssh/sshd_config`
- раскомментируем порт (port 22). Заменяем на 5050
- permitrootlogin no - запрещаем заходить под пользователем root и будем заходить под пользователем nadya
- password authentification no - запрещаем заходить по паролю
- ctrl+x, y, enter

18. перезагружаем ssh службу

`service ssh restart`

19. дальше чтобы система не спрашивала пароль на какие-либо испольняемые операции, заходим в этот файл и добавляем там строчку

`nano /etc/sudoers`
и там в самом низу пишем:
nadya ALL=(ALL) NOPASSWD: ALL - наде даем разрешение на все операции в системе
ctrl+x, y, enter

20. и пробуем зайти на сервер через публичный ключ
`ssh -p 5050 nadya@ip`
но у меня не зашел через этот порт
зашел так:
`ssh nadya@ip`
и кодовая фраза от ключа

21. устанавливаем программы

`sudo apt install nginx`

22. нужно посмотреть статус

`sudo systemctl status nginx`

23. нажимаем ctrl+c для выхода
24. теперь нужно сделать так, чтобы nginx загружался каждый раз при входе в систему

`sudo systemctl enable nginx`

25. создаем папку для приложений

`cd /home/nadya`

`mkdir my_apps`

`cd my_apps`

`mkdir test_bot`

26. создаем разрешение на чтение и запись для директории my_apps

`sudo chmod -R 777 /home/nadya/my_apps`
27. идем в папку и создаем тестовый файл index.html

`sudo nano index.html`
вставляем туда разметку

28. чтобы сайт обслуживался nginx, нужно добавить файл конфига

`sudo nano /etc/nginx/sites-available/test_bot`

29. и вставляем туда это:

server {
        listen 111.222.333:80;
        root /home/nadya/my_apps/test_bot;
        index index.html;
        location / {
                  try_files $uri.html $uri $uri/ =404;
                  }
       }
       
30. дальше нужно активировать этот файл, чтобы nginx знал о нем

`sudo ln /etc/nginx/sites-available/test_bot /etc/nginx/sites-enabled/test_bot`

31. проверка что файл успешно активировался

`sudo nginx -t`

32. перезапускаем службу nginx

`sudo systemctl restart nginx`

33. проверяем статус nginx

`sudo systemctl status nginx`

34. ctrl+c

УСТАНОВКА ФАЕРВОЛА

1. `sudo apt-get install ufw -y`
2. `sudo ufw default deny incoming` - запрет входящих подключений к серверу
3. `sudo ufw default allow outgoing`
4. `sudo ufw allow ssh` - добавляем в ислючения все нужные порты

`sudo ufw allow 'Nginx Full'`

`sudo ufw allow http`

`sudo ufw allow 5050`

`sudo ufw allow 22`

`sudo ufw allow 3000`

5. проверяем статус фаервола

`sudo ufw status verbose` - статус неактивен

6. делаем его активным

`sudo ufw enable`

7. смотрим все исключения

`sudo ufw status`

УСТАНОВКА СЕРВЕРНОГО ЯЗЫКА
PYTHON
`sudo apt-get install python3 python3-dev`

проверяем версию python
`python3 --version`

устанавливаем pip
`sudo apt install python3-pip`

проверяем версию pip
`pip3 --version`

устанавливаем git
`sudo apt-get install git`

проверяем версию git
`git --version`

устанавливаем sqlite3
`sudo apt install sqlite3`

для мониторинга всех процессов системы нам понадобится програм htop

`sudo apt install htop`

чтобы посмотреть все процессы пишем 
`htop`

чтобы выйти ctrl+c

