# Домашнее задание «TeamCity»

Отчёт в работе. Ниже перечислены проверенные результаты.

## Подготовка инфраструктуры

| Роль | ВМ | vCPU / RAM | Зона | Статус |
| --- | --- | --- | --- | --- |
| TeamCity Server | `compute-vm-4-4-30-hdd-1795703258627` | 4 / 4 ГиБ | `ru-central1-a` | Running |
| TeamCity Agent | `netology-teamcity-agent` | 2 / 4 ГиБ | `ru-central1-d` | Running |
| Nexus Repository | `netology-nexus` | 2 / 4 ГиБ | `ru-central1-e` | Running |

На сервере проверен Docker Engine 29.1.3 и установлен Docker Compose 2.40.3. Контейнер `jetbrains/teamcity-server:2026.2.2` запущен. Страница первичной настройки `/mnt` ответила HTTP 200.

Для отдельной ВМ агента задан официальный образ `jetbrains/teamcity-agent:2026.2.2` и переменная `SERVER_URL`. Для Nexus подготовлен Ansible playbook под Ubuntu с образом `sonatype/nexus3:3.98.3`.

[Исходный репозиторий задания](https://github.com/aragastmatb/example-teamcity)

## Проверки в работе

Первичная настройка TeamCity, подключение агента и запуск Nexus, настройки Maven и TeamCity, сборки `master` и `feature/add_reply`, публикация JAR и проверка конфигурации в Git.

## Скриншоты

Облачные ВМ в статусе `Running`.

<img width="390" height="184" alt="clipboard" src="https://github.com/user-attachments/assets/6a5c858c-9c56-4c3e-a908-38840ef31d16" />


Запущенный контейнер TeamCity Server.

<img width="1310" height="38" alt="clipboard" src="https://github.com/user-attachments/assets/37ea2b1e-a51f-47f8-8293-11fe9cd867b8" />
