# Домашнее задание к занятию "`Ansible. Часть 2`" - `Брусенцева Злата`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. В личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1 - Разработка базовых плейбуков
В рамках задания были написаны три плейбука для автоматизации базовых задач администрирования хостов. При написании использовались только модули Ansible, исключая применение командных интерпретаторов (command и shell).

1. Скачивание и распаковка архива (kafka_deploy.yml)
Используем встроенный модуль get_url для скачивания, file для папки и unarchive для распаковки.

```
---
- name: Download and unpack Apache Kafka
  hosts: all
  become: true
  tasks:
    - name: Ensure download directory exists
      ansible.builtin.file:
        path: /opt/kafka_dist
        state: directory
        mode: '0755'

    - name: Download Apache Kafka archive
      ansible.builtin.get_url:
        url: https://apache.org
        dest: /opt/kafka_dist/kafka.tgz
        mode: '0644'

    - name: Ensure extraction directory exists
      ansible.builtin.file:
        path: /opt/kafka
        state: directory
        mode: '0755'

    - name: Unpack Kafka archive
      ansible.builtin.unarchive:
        src: /opt/kafka_dist/kafka.tgz
        dest: /opt/kafka
        remote_src: true
```
`При необходимости прикрепитe сюда скриншоты
<img width="1426" height="1136" alt="image" src="https://github.com/user-attachments/assets/4a498af6-aad2-40bb-8f4a-93b55fe291b5" />

2. Установка и запуск демона tuned (tuned_setup.yml) 
Используем модуль apt и модуль service для автозапуска.

```
---
- name: Install and enable tuned daemon
  hosts: all
  become: true
  tasks:
    - name: Install tuned package
      ansible.builtin.apt:
        name: tuned
        state: present
        update_cache: true

    - name: Start and enable tuned service
      ansible.builtin.service:
        name: tuned
        state: started
        enabled: true
```
`При необходимости прикрепитe сюда скриншоты
<img width="1433" height="677" alt="image" src="https://github.com/user-attachments/assets/6c2725ba-3302-412d-8d3e-a862333aba75" />

3. Изменение приветствия системы через переменную (motd_variable.yml)
Используем модуль copy и переменную custom_motd:

```
---
- name: Change system MOTD greeting using variable
  hosts: all
  become: true
  vars:
    custom_motd: "Welcome to Netology DevOps DevOps environment!\n"
  tasks:
    - name: Update /etc/motd file
      ansible.builtin.copy:
        content: "{{ custom_motd }}"
        dest: /etc/motd
        mode: '0644'
```
`При необходимости прикрепитe сюда скриншоты
<img width="1413" height="637" alt="image" src="https://github.com/user-attachments/assets/05ba20e1-926e-40c6-8a8f-7ab8005de252" />

---

### Задание 2

`Модификация MOTD (Динамические факты)`
Здесь мы используем системные переменные Ansible facts (ansible_default_ipv4 и ansible_hostname).

```
Код...
---
- name: Configure dynamic MOTD with host facts
  hosts: all
  become: true
  tasks:
    - name: Generate dynamic MOTD greeting
      ansible.builtin.copy:
        content: |
          ==================================================
          Welcome to server: {{ ansible_hostname }}
          Server IP Address: {{ ansible_default_ipv4.address }}
          --------------------------------------------------
          Have a wonderful day, dear System Administrator!
          ==================================================
        dest: /etc/motd
        mode: '0644'

```

`При необходимости прикрепитe сюда скриншоты`
<img width="1434" height="676" alt="image" src="https://github.com/user-attachments/assets/657552ee-248c-422c-b8da-03a0bd1754c1" />

При входе пользователя в систему (SSH/консоль) выводится персонализированное приветствие с IP-адресом, именем хоста и пожеланием хорошего дня системному администратору.
---

### Задание 3

`Создание Ansible-роли для Apache`
Для комплексной автоматизации веб-инфраструктуры была разработана изолированная Ansible-роль apache_monitor.

Структура роли и проделанные действия:
1) Установка и автозапуск: Через модуль apt установлен веб-сервер apache2. Служба переведена в состояние started и добавлена в автозагрузку.
2) Динамический шаблон (templates/index.html.j2): Разработан шаблон Jinja2, выводящий ключевые аппаратные метрики хоста:
{{ ansible_memtotal_mb }} — объем оперативной памяти.
{{ ansible_processor_vcpus }} — количество ядер CPU.
{{ ansible_devices.sda.size }} — объем первого системного диска.

```
<!DOCTYPE html>
<html>
<head>
    <title>System Monitor</title>
</head>
<body>
    <h1>System Characteristics for {{ ansible_hostname }}</h1>
    <ul>
        <li><strong>IP Address:</strong> {{ ansible_default_ipv4.address }}</li>
        
        <li><strong>CPU Model:</strong> 
        {% if ansible_processor|length > 1 %}
            {{ ansible_processor[1] }}
        {% else %}
            {{ ansible_processor[0] | default('Generic CPU') }}
        {% endif %}
        ({{ ansible_processor_vcpus }} vCPUs)</li>
        
        <li><strong>Total RAM:</strong> {{ ansible_memtotal_mb }} MB</li>
        
        <li><strong>First HDD Size:</strong> 
        {% if ansible_devices.sda is defined %}
            {{ ansible_devices.sda.size }}
        {% elif ansible_devices.vda is defined %}
            {{ ansible_devices.vda.size }}
        {% else %}
            Unknown Size
        {% endif %}
        </li>
    </ul>
</body>
</html>
```
3) Обработчики (Handlers): В каталоге handlers/main.yml настроен триггер Restart apache service. Перезапуск службы apache2 происходит только в случае физического изменения контента или структуры файла index.html.
4) Безопасность и проверка: Настроен сетевой экран UFW (модуль community.general.ufw) на пропуск трафика по порту 80/tcp. Добавлена финальная самодиагностика доступности веб-страницы через модуль ansible.builtin.uri с ожиданием HTTP-статуса 200 OK.

`При необходимости прикрепитe сюда скриншоты
<img width="2119" height="1075" alt="image" src="https://github.com/user-attachments/assets/1f50a454-ec46-4270-8117-8017edd9df81" />

Вся разработанная структура плейбуков и ролей успешно протестирована на локальном хосте (Ubuntu). Сценарии завершились со статусом failed=0, конфигурация успешно применена.
