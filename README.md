# Домашнее задание к занятию "`9.2 Zabbix-1`" - `Паромов Роман`

## Задание 1
### Устанавливал в облаке с помощью terraform и ansible

### код плейбука
```
---
- name: PB_zbx_agent
  become: true
  hosts: agents
  roles:
    - roles/zbxroles

- name : PB_zabbix_server
  become: true
  hosts: zbx-server
  roles:
    - roles/zbx-server-roles
```
###код ролей
```
# tasks file for roles/zbxroles

- name: install_zbx_agent
  apt:
    name:
    - zabbix-agent
    state: present
    update_cache: yes

- name: Start service zabbix-agent, if not started
  ansible.builtin.service:
    name: zabbix-agent
    state: started
    enabled: yes
```
```
---
# tasks file for roles/zbx-server-roles
- name: command1
  shell: |
    wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4%2Bdebian11_all.deb
    dpkg -i zabbix-release_6.0-4+debian11_all.deb
    apt update


- name: install zabbix-server, postgreSQL
  apt:
    name:
    - postgresql
    - zabbix-server-pgsql
    - zabbix-frontend-php
    - php7.4-pgsql
    - zabbix-apache-conf
    - zabbix-sql-scripts
    - nano
    - zabbix-agent
    state: present
    update_cache: yes

- name: Start service {{ item }} , if not started
  ansible.builtin.service:
    name: "{{ item }}"
    state: started
    enabled: yes
  with_items: 
  - zabbix-server
  - apache2
  - zabbix-agent

- name: shell_comands_conf
  shell: |
    su - postgres -c 'psql --command "CREATE USER zabbix WITH PASSWORD '\'123456789\'';"'
    su - postgres -c 'psql --command "CREATE DATABASE zabbix OWNER zabbix;"'
#    zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz  не стработало
#    su - zabbix psql zabbix не сработало
    sed -i 's/# DBPassword=/DBPassword=123456789/g' /etc/zabbix/zabbix_server.conf

- name: restart service {{ item }}
  ansible.builtin.service:
    name: "{{ item }}"
    state: restarted
  with_items: 
    - zabbix-server
    - apache2
    - zabbix-agent
```
### не сработала команда в shell, так что пришлось вручную вбивать
```
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
```
![](https://github.com/Romera14/homework_9-03/blob/main/Снимок%20экрана%202022-11-10%20в%2019.51.38.png)
---
## Задание 2
### настройки в zabbix_agentd.conf вбивал вручную
![](https://github.com/Romera14/homework_9-03/blob/main/Снимок%20экрана%202022-11-10%20в%2019.52.29.png)
![](https://github.com/Romera14/homework_9-03/blob/main/Снимок%20экрана%202022-11-10%20в%2019.56.21.png)
![](https://github.com/Romera14/homework_9-03/blob/main/Снимок%20экрана%202022-11-10%20в%2019.59.07.png)
