# Домашнее задание к занятию 4 «Работа с roles» - `Мальцев Виктор`

---

Подготовка к выполнению
Необязательно. Познакомьтесь с LightHouse.
Создайте два пустых публичных репозитория в любом своём проекте: vector-role и lighthouse-role.
Добавьте публичную часть своего ключа к своему профилю на GitHub.

---

Основная часть
Ваша цель — разбить ваш playbook на отдельные roles.

Задача — сделать roles для ClickHouse, Vector и LightHouse и написать playbook для использования этих ролей.

Ожидаемый результат — существуют три ваших репозитория: два с roles и один с playbook.

Что нужно сделать

Создайте в старой версии playbook файл requirements.yml и заполните его содержимым:

```

---

  - src: git@github.com:AlexeySetevoi/ansible-clickhouse.git
    scm: git
    version: "1.13"
    name: clickhouse 

```

При помощи ansible-galaxy скачайте себе эту роль.

![alt text](https://github.com/vmmaltsev/screenshot/blob/main/Screenshot_25.png)

![alt text](https://github.com/vmmaltsev/screenshot/blob/main/Screenshot_26.png)

Создайте новый каталог с ролью при помощи ansible-galaxy role init vector-role.

На основе tasks из старого playbook заполните новую role. Разнесите переменные между vars и default.

Перенести нужные шаблоны конфигов в templates.

Опишите в README.md обе роли и их параметры. Пример качественной документации ansible role по ссылке.

---

Ansible Roles

Этот репозиторий содержит Ansible роли для установки и настройки Vector и LightHouse.
Роли
Vector

Эта роль устанавливает и настраивает Vector на вашем сервере.
Параметры

    vector_version: Версия Vector, которую вы хотите установить. По умолчанию 0.32.0.
    vector_rpm_url: URL для скачивания RPM пакета Vector. По умолчанию https://packages.timber.io/vector/{{ vector_version }}/vector-{{ vector_version }}-1.x86_64.rpm.
    vector_config_template: Путь к файлу шаблона конфигурации Vector. По умолчанию /root/08-ansible-02-playbook/playbook/templates/vector-config.j2.
    vector_config_destination: Путь, куда должен быть скопирован файл конфигурации Vector. По умолчанию /etc/vector/vector.toml.

Пример использования

```

- hosts: vector
  roles:
    - role: vector-role
      vars:
        vector_version: "0.32.0"
```

LightHouse

Эта роль устанавливает и настраивает LightHouse и Nginx на вашем сервере.
Параметры

    web_root: Корневой каталог для файлов веб-сайта. По умолчанию /var/www/html.

Пример использования

```

- hosts: lighthouse
  roles:
    - role: lighthouse-role
      vars:
        web_root: "/var/www/html"
```

Использование

    Установите требуемые роли, используя ansible-galaxy:
```
ansible-galaxy install -r requirements.yml
```
    Создайте файл инвентаря с вашими хостами. Например:

```
[vector]
vector_host ansible_ssh_host=your_vector_host_ip

[lighthouse]
lighthouse_host ansible_ssh_host=your_lighthouse_host_ip
```

Создайте playbook, который использует роли. Например:


```
---
- name: Install and Configure Vector and LightHouse
  hosts: 
    - vector
    - lighthouse
  roles:
    - role: vector-role
      when: "'vector' in group_names"
    - role: lighthouse-role
      when: "'lighthouse' in group_names"
```

Запустите ваш playbook:

```
ansible-playbook -i your_inventory_file your_playbook.yml
```

Замените your_inventory_file на путь к вашему файлу инвентаря и your_playbook.yml на путь к вашему playbook.

---

Повторите шаги 3–6 для LightHouse. Помните, что одна роль должна настраивать один продукт.

Выложите все roles в репозитории. Проставьте теги, используя семантическую нумерацию. Добавьте roles в requirements.yml в playbook.

Переработайте playbook на использование roles. Не забудьте про зависимости LightHouse и возможности совмещения roles с tasks.

Выложите playbook в репозиторий.

В ответе дайте ссылки на оба репозитория с roles и одну ссылку на репозиторий с playbook.
