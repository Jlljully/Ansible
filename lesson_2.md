# Домашнее задание к занятию 2 «Работа с Playbook»

## Подготовка к выполнению

1. * Необязательно. Изучите, что такое [ClickHouse](https://www.youtube.com/watch?v=fjTNS2zkeBs) и [Vector](https://www.youtube.com/watch?v=CgEhyffisLY).
2. Создайте свой публичный репозиторий на GitHub с произвольным именем или используйте старый.
3. Скачайте [Playbook](./playbook/) из репозитория с домашним заданием и перенесите его в свой репозиторий.
4. Подготовьте хосты в соответствии с группами из предподготовленного playbook.

## Основная часть

1. Подготовьте свой inventory-файл `prod.yml`.
2. Допишите playbook: нужно сделать ещё один play, который устанавливает и настраивает [vector](https://vector.dev). Конфигурация vector должна деплоиться через template файл jinja2. От вас не требуется использовать все возможности шаблонизатора, просто вставьте стандартный конфиг в template файл. Информация по шаблонам по [ссылке](https://www.dmosk.ru/instruktions.php?object=ansible-nginx-install).
3. При создании tasks рекомендую использовать модули: `get_url`, `template`, `unarchive`, `file`.
4. Tasks должны: скачать дистрибутив нужной версии, выполнить распаковку в выбранную директорию, установить vector.
5. Запустите `ansible-lint site.yml` и исправьте ошибки, если они есть.
6. Попробуйте запустить playbook на этом окружении с флагом `--check`.
7. Запустите playbook на `prod.yml` окружении с флагом `--diff`. Убедитесь, что изменения на системе произведены.
8. Повторно запустите playbook с флагом `--diff` и убедитесь, что playbook идемпотентен.
9. Подготовьте README.md-файл по своему playbook. В нём должно быть описано: что делает playbook, какие у него есть параметры и теги. Пример качественной документации ansible playbook по [ссылке](https://github.com/opensearch-project/ansible-playbook).
10. Готовый playbook выложите в свой репозиторий, поставьте тег `08-ansible-02-playbook` на фиксирующий коммит, в ответ предоставьте ссылку на него.

---

# Ответ:

[Ссылка на репозиторий](https://github.com/Jlljully/ansible_push/tree/ansible02)

---



![Скрин](https://github.com/Jlljully/Ansible/blob/main/files/lesson_2/Untitled.png "1")
   
![Скрин](https://github.com/Jlljully/Ansible/blob/main/files/lesson_2/Untitled2.png "2")
  
![Скрин](https://github.com/Jlljully/Ansible/blob/main/files/lesson_2/Untitled3.png "3")  

![Скрин](https://github.com/Jlljully/Ansible/blob/main/files/lesson_2/Untitled4.png "4")  

![Скрин](https://github.com/Jlljully/Ansible/blob/main/files/lesson_2/Untitled5.png "5")

![Скрин](https://github.com/Jlljully/Ansible/blob/main/files/lesson_2/Untitled6.png "6")


---

# Документация:

---


![Скрин](https://github.com/Jlljully/Ansible/blob/main/files/lesson_2/Untitled7.png "7") ![Скрин](https://github.com/Jlljully/Ansible/blob/main/files/lesson_2/Untitled8.png "8")


# **Проект развертки хостов с Clickhouse и Vector при помощи Ansible-playbook**

## *Основные компоненты*

  # Структура проекта

 ```
  -/group_vars  
      -clickhouse  
         -vars.yml  
      -vector.yaml.j2  
  -/invetnory  
      -prod.yml  
  -site.yml  
  ```

  # Код проекта

 ```
---
- name: Install Clickhouse
  hosts: clickhouse
  handlers:
    - name: Start clickhouse service
      become: true
      ansible.builtin.service:
        name: clickhouse-server
        state: restarted
  tasks:
    - block:
        - name: Get clickhouse distrib
          ansible.builtin.get_url:
            url: "https://packages.clickhouse.com/rpm/stable/{{ item }}-{{ clickhouse_version }}.noarch.rpm"
            dest: "./{{ item }}-{{ clickhouse_version }}.rpm"
          with_items: "{{ clickhouse_packages }}"
      rescue:
        - name: Get clickhouse distrib
          ansible.builtin.get_url:
            url: "https://packages.clickhouse.com/rpm/stable/clickhouse-common-static-{{ clickhouse_version }}.x86_64.rpm"
            dest: "./clickhouse-common-static-{{ clickhouse_version }}.rpm"

    - name: Install clickhouse packages
      become: true
      ansible.builtin.yum:
        name:
          - clickhouse-common-static-{{ clickhouse_version }}.rpm
          - clickhouse-client-{{ clickhouse_version }}.rpm
          - clickhouse-server-{{ clickhouse_version }}.rpm
      notify: Start clickhouse service

    - name: Flush handlers
      meta: flush_handlers

    - name: Create database
      ansible.builtin.command: "clickhouse-client -q 'create database logs;'"
      register: create_db
      failed_when: create_db.rc != 0 and create_db.rc !=82
      changed_when: create_db.rc == 0

- name: Install Vector
  hosts: vector
  become: yes
  become_user: root
  tasks:
    - block:
        - name: Get vector distrib
          ansible.builtin.get_url:
            url: "https://packages.timber.io/vector/{{ vector_version }}/vector-{{ vector_version }}-1.{{ ansible_architecture }}.rpm"
            dest: "/home/centos/vector-{{ vector_version }}.rpm"

    - name: Install Vector packages
      become: true
      ansible.builtin.yum:
        name:
          - vector-{{ vector_version }}.rpm

    - name: Start Vector service
      become: true
      ansible.builtin.service:
        name: vector
        state: restarted

    - name: write using jinja2
      ansible.builtin.template:
         src: ./group_vars/vector.yaml.j2
         mode: 0644
         dest: /etc/vector/vector.yaml
         owner: vector
         group: vector
    - name: Restart Vector
      become: true
      ansible.builtin.command: "systemctl restart vector"

 ```
