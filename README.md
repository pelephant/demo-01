# Тестовое задание.

## Техническое задание
Необходимо написать Vagrantfile который по команде vagrant up сделает следующее:
1) Запустит бокс Ubuntu 20.04 (https://app.vagrantup.com/generic/ ).
2) Внутри ВМ произойдет автоматическая установка docker и docker compose (используя ansible).
3) Внутри ВМ произойдет установка node exporter для Prometheus (используя ansible).
4) С помощью docker compose (docker-compose up, сам docker compose стоит дернуть так же через ansible) будет запущено 2 контейнера:
4.1) Первый контейнер с prometheus, который будет настроен на сбор метрик с node exporter из Ubuntu
4.2) Второй контейнер с grafana, которая будет предварительно преднастроена: в качестве datasource используется prometheus из соседнего контейнера, уже загружен dashboard 1860 (https://grafana.com/grafana/dashboards/1860), который визуализирует метрики полученные с node exporter
5) Контейнер с grafana должен экспозить 3000 порт контейнера в ВМ с убунту
6) Сама ВМ с убунту должна экспозить 3000 порт в хостовую машину
 
По итогу нужно, что бы после запуска ВМ и выполнения настройки можно было бы открыть в браузере localhost:3000 и обнаружить там Grafana, зайти в нее и увидеть внутри dashboard который визуализирует метрики запущенной ВМ

## Описание выполнения тестового задания
- Создаем ВМ в OS ESXI. Настраиваем OS ESXI для возможности запуска аппаратной виртуализации на тестовой ВМ
- Устанавливаем ОС Fedora-Workstation-Live-x86_64-35 
- Обновляем все базовые пакет ОС Fedora
- Добавляем репозиторий http://download.virtualbox.org/virtualbox/rpm/fedora/35/x86_64/
- Ставим virtualbox из подключенного репозитория и vagrant из базовых репозиториев ОС Fedora
- Качаем "generic/ubuntu2004 Vagrant box" для virtualbox с https://app.vagrantup.com/generic/boxes/ubuntu2004 в /u01/ubuntu2004.box (в репе файл нулевой длины)
- Создаем в /u01 Vagrantfile c нужным содержимом (увеличиваем ресурсы запускаемой ВМ, проброс портов и др.)
- Создаем Ansible роль для установки и настройки prometheus и grafana в контейнерах в ВМ ubuntu2004
- Добавляем этот box-файл ( #vagrant box add ubuntu2004 ubuntu2004.box )
- Запускаем ВМ ubuntu2004 ( #vagrant up )
- Проверяем ВМ ubuntu2004 ( #vagrant ssh)
- Выясняем IP адрес ubuntu2004 его вставляем в файл конфигурации prometheus.yml для прометея в нашей Ansible роли
- Запускаем нашу Ansible роль для настройки prometheus и grafana в контейнерах в ВМ ubuntu2004 ( #vagrant provision )
- в графическом интерфейсе ОС Fedora в браузере заходим в grafana для просмотра метрик node-exporter ОС ubuntu2004 собираемой prometheus.

## Результат
- артефакт в виде скриншота с метриками grafana ( FINAL.JPG )
  
## ИТОГ
- Имеем ВМ в ESXI ОС с ОС Fedora-Workstation-Live-x86_64-35 в ней внутри virtualbox запущена ВМ ОС ubuntu2004 в которой node-exporter отдает метрики.
- Внутри ОС ubuntu2004 запущены в докере prometheus и grafana. 
- Прометей собирает метрики с node-exporter ОС ubuntu2004.
- grafana через дашборд  node-exporter-full_rev29 показывает метрики прометея с ОС ubuntu2004.



