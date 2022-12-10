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

