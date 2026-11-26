# Домашнее задание «TeamCity»

Отчёт в работе. Ниже перечислены проверенные результаты.

## Подготовка инфраструктуры

| Роль | ВМ | vCPU / RAM | Зона | Статус |
| --- | --- | --- | --- | --- |
| TeamCity Server | `compute-vm-4-4-30-hdd-1795703258627` | 4 / 4 ГиБ | `ru-central1-a` | Running |
| TeamCity Agent | `netology-teamcity-agent` | 2 / 4 ГиБ | `ru-central1-d` | Running |
| Nexus Repository | `netology-nexus` | 2 / 4 ГиБ | `ru-central1-e` | Running |

На сервере проверен Docker Engine 29.1.3 и установлен Docker Compose 2.40.3. Контейнер `jetbrains/teamcity-server:2026.2.2` запущен. Страница первичной настройки `/mnt` ответила HTTP 200.

На ВМ агента `cloud-init` скачал `jetbrains/teamcity-agent:2026.2.2` и запустил контейнер с переменной `SERVER_URL`. Агент `netology-agent` подключён к TeamCity по внутренней сети, авторизован и добавлен в пул `Default`.

На ВМ Nexus выполнен [адаптированный Ansible playbook](infrastructure/nexus/site.yml). Он запускает `sonatype/nexus3:3.98.3` и проверяет HTTP 200 на локальном `/service/rest/v1/status`. Журнал последовательного порта подтвердил старт контейнера и завершение `cloud-init` без ошибки. Исходный playbook из задания рассчитан на CentOS и OpenJDK 8.

[Исходный репозиторий задания](https://github.com/aragastmatb/example-teamcity)

## TeamCity и первая сборка

TeamCity инициализирован со встроенной базой HSQLDB. Проект `Example Teamcity` создан из форка. Автоопределение нашло Maven и `pom.xml`, шаг сохранён с целями `clean test`. Сборка №1 на `master` запустилась на `netology-agent` и завершилась со статусом `Success`: прошли 5 тестов.

## Публикация в Nexus

| Ветка | Шаг Maven |
| --- | --- |
| `master` | `clean deploy` |
| Остальные ветки | `clean test` |

В Nexus создан Maven Hosted-репозиторий `netology-releases` с разрешённой повторной публикацией. В [pom.xml](pom.xml) задан путь к репозиторию, адрес Nexus передаётся параметром TeamCity. Шаг публикации использует [ci/settings.xml](ci/settings.xml) из рабочей копии. Файл получает имя пользователя и пароль из переменных окружения TeamCity. Учётная запись издателя имеет доступ только к учебному репозиторию.

Сборка №8 ветки `master` завершилась со статусом `Success`. Прошли 5 тестов. В Nexus появился Maven-компонент `org.netology:plaindoll:0.0.2`.

## Оставшиеся проверки

Перенос конфигурации TeamCity в Git, автоматическая сборка `feature/add_reply`, слияние в `master` и публикация JAR в разделе Artifacts сборки TeamCity.

## Скриншоты

Облачные ВМ в статусе `Running`.

<img width="390" height="184" alt="clipboard" src="https://github.com/user-attachments/assets/6a5c858c-9c56-4c3e-a908-38840ef31d16" />


Запущенный контейнер TeamCity Server.

<img width="1310" height="38" alt="clipboard" src="https://github.com/user-attachments/assets/37ea2b1e-a51f-47f8-8293-11fe9cd867b8" />


Первая успешная сборка `master` на `netology-agent`.

<img width="780" height="270" alt="clipboard" src="https://github.com/user-attachments/assets/d849c3d0-2544-44a0-9eed-cfffb67336bf" />
