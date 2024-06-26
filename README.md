# Спящий режим жесткого диска на Synology

### Описание

После обновления до DSM7 это может быть связано с неполным согласованием загрузки, большим объемом фонового создания журнала и частой записью на диск, что делает невозможным спящий режим. После долгой борьбы проблема с журналом не была решена. метод переноса всего журнала в память для решения проблемы. Журнал не может быть переведен в спящий режим.

### Планирование сценария в планировщике задач Synology

**Создаем две задачи**

Чтобы запланировать запуск сценария на Synology при загрузке или завершении работы, выполните следующие действия:

**Примечание.:** Вы можете настроить задачу расписания и оставить ее отключенной, чтобы она запускалась только тогда, когда вы выбираете задачу в планировщике задач и нажимаете кнопку «Выполнить».

1. Перейдите в «Панель управления» > «Планировщик заданий» > нажмите «Создать» > и выберите «Запускаемая задача» .
2. Выберите Пользовательский сценарий.
3. Введите имя задачи.
4. Выберите root в качестве пользователя (сценарий должен запускаться от имени root).
5. Выберите «Загрузка» в качестве события, запускающего задачу.
6. Оставьте галочку «Включить ».
7. Нажмите «Настройки задачи» .
8. При желании вы можете установить флажок «Отправлять сведения о запуске по электронной почте» и «Отправлять сведения о запуске только в случае аварийного завершения сценария», а затем ввести свой адрес электронной почты.
9. В поле Пользовательский скрипт введите сам срипт
- **Первая задача:** загрузочный скрипт для восстановления журнала в памяти при загрузке.
```
On Boot/root
if [ ! -d "/var/logbak" ]; then
     mkdir /var/logbak
     cp -a -f /var/log/.  /var/logbak/
fi
cp -a -f /var/logbak/.  /tmp/log/
mount -B /tmp/log  /var/log)
```
- **Вторая задача:** завершения работы для записи журнала из памяти в каталог резервного копирования при завершении работы.
```
On Shutdown/root
cp -a -f /tmp/log/.     /var/logbak
```
<p align="leftr"><img src="images/schedule1.PNG"></p>
