1) запустить контейнер с ubuntu, используя механизм LXC
2) ограничить контейнер 256 Мб ОЗУ и проверить, что ограничение работает
Задание по желанию ..
4)добавить автозапуск контейнеру, перезагрузить ОС и убедиться, что контейнер действительно запустился самостоятельно
5) ''при создании указать файл, куда записывать логи
6) ''после перезагрузки проанализировать логи


sudo apt update
sudo apt install lxc-utils
sudo apt install lxc lxc-templates


lxs storage list 

lxc-create -n test123 -t ubuntu --создаем контейнер
lxc-start -n test123 -- запускаем
lxc-attach -n test123 -- войдем в него
free -m —проверяем пямаять
exit   -- выходим
lxc-stop -n test123 - -закрываем


nano /var/lib/lxc/test123/config-открываем 



# Network configuration — Конфегурация сети
.
.
.
lxc.cgroup2.memory.max = 256M -- ограничиваем(добавляем запись)
сохраняем и выходим

lxc-start -n test123 -- запускаем
lxc-attach -n test123 -- войдем в него
free -m -- проверяем пямаять

Далее просмотрим список имеющихся в нашей системе контейнеров:
sudo lxc-ls -f
У нас имеется один контейнер test123, он запущен и в автозапуске стоит 0 (что означает, что автозапуск контейнера отключен)
Добавляем в конец текста файла конфигурации следующую строку в файл конфигурации nano /var/lib/lxc/test123/config :

lxc.start.auto = 1

При запуске контейнера можно в параметрах команды указать, где хранить логи, например:
lxc-start -n test123 --logfile log.log

Для удаления контейнера необходмио воспользоваться следующей командой:
lxc-destroy -n test123
