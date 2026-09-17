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

### Задание 1

1. Скачивание и распаковка архива (kafka_deploy.yml)
Используем встроенный модуль get_url для скачивания, file для папки и unarchive для распаковки.

Код...
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

`При необходимости прикрепитe сюда скриншоты
<img width="1426" height="1136" alt="image" src="https://github.com/user-attachments/assets/4a498af6-aad2-40bb-8f4a-93b55fe291b5" />

2. Установка и запуск демона tuned (tuned_setup.yml) 
Используем модуль apt и модуль service для автозапуска.

Код...
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

`При необходимости прикрепитe сюда скриншоты
<img width="1433" height="677" alt="image" src="https://github.com/user-attachments/assets/6c2725ba-3302-412d-8d3e-a862333aba75" />

3. Изменение приветствия системы через переменную (motd_variable.yml)
Используем модуль copy и переменную custom_motd:

Код...
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

`При необходимости прикрепитe сюда скриншоты
<img width="1413" height="637" alt="image" src="https://github.com/user-attachments/assets/05ba20e1-926e-40c6-8a8f-7ab8005de252" />

---

### Задание 2

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6. 

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота 2](ссылка на скриншот 2)`


---

### Задание 3

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6. 

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`

### Задание 4

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6. 

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`
