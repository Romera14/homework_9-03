# Домашнее задание к занятию "`9.2 Zabbix-2`" - `Паромов Роман`

## Задание 1
![](https://github.com/Romera14/homework_9-03/blob/main/Снимок%20экрана%202022-11-12%20в%2017.20.16.png)

## Задание 2,3
![](https://github.com/Romera14/homework_9-03/blob/main/Снимок%20экрана%202022-11-12%20в%2017.34.17.png)

## Задание 4
![](https://github.com/Romera14/homework_9-03/blob/main/Снимок%20экрана%202022-11-12%20в%2018.06.36.png)

## Задание 5
![](https://github.com/Romera14/homework_9-03/blob/main/Снимок%20экрана%202022-11-14%20в%2020.10.51.png)
![](https://github.com/Romera14/homework_9-03/blob/main/Снимок%20экрана%202022-11-14%20в%2020.31.35.png)

## Задание 6
```
UserParameter=sh_test[*], if [ $1 eq "1" ]; then echo 'Paromov Roman'; elif [ $1 eq "2" ]; then date; fi
```
![](https://github.com/Romera14/homework_9-03/blob/main/Снимок%20экрана%202022-11-15%20в%2018.26.08.png)

## Задание 7
```python
import sys
import os
import re
import datetime


date = datetime.date.today()

if (sys.argv[1] == '-ping'): # Если -ping
        result=os.popen("ping -c 1 " + sys.argv[2]).read() # Делаем пинг по заданному адресу
        result=re.findall(r"time=(.*) ms", result) # Выдёргиваем из результата время
        print(result[0]) # Выводим результат в консоль
elif (sys.argv[1] == '-simple_print'): # Если simple_print 
        print(sys.argv[2]) # Выводим в консоль содержимое sys.arvg[2]
elif (sys.argv[1] == "1" ):
        print('Paromov Roman')
elif (sys.argv[1] == "2" ):
        print(date)
```
![](https://github.com/Romera14/homework_9-03/blob/main/Снимок%20экрана%202022-11-15%20в%2018.26.08.png)

## Задание 8
### Сделал все как в лекции но хосты не появлялись, даже переустанвливал и машины и заббикс
![](https://github.com/Romera14/homework_9-03/blob/main/Снимок%20экрана%202022-11-15%20в%2018.49.20.png)
![](https://github.com/Romera14/homework_9-03/blob/main/Снимок%20экрана%202022-11-15%20в%2018.53.25.png)
![](https://github.com/Romera14/homework_9-03/blob/main/Снимок%20экрана%202022-11-15%20в%2018.55.34.png)

## Задание 9
### делал через терраформ и ансибл.
```
- name: install_zbx_agent
  apt:
    name:
    - zabbix-agent
    - python3
    state: present
    update_cache: yes

- name: shell_comands_conf
  shell: |
    sed -i 's/Server=127.0.0.1/Server=192.168.10.27/g' /etc/zabbix/zabbix_agentd.conf
    sed -i 's/ServerActive=127.0.0.1/ServerActive=192.168.10.27/g' /etc/zabbix/zabbix_agentd.conf

- name: user_parametr1
  template:
    src=/home/paromov/AnsTerZbx/roles/zbxroles/templates/test_python.py dest=/etc/zabbix/test_python.py

- name: user_parametr2
  template:
    src=/home/paromov/AnsTerZbx/roles/zbxroles/templates/sh-test.conf dest=/etc/zabbix/zabbix_agentd.conf.d/sh-test.conf

- name: user_parametr3
  template:
    src=/home/paromov/AnsTerZbx/roles/zbxroles/templates/python-test.conf dest=/etc/zabbix/zabbix_agentd.conf.d/python-test.conf

- name: Start service zabbix-agent, if not started
  ansible.builtin.service:
    name: zabbix-agent
    state: started
    enabled: yes
```
