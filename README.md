# Со скриншотами здесь https://docs.google.com/document/d/1N6vDKGbiF4Umr3DWcpkidnLdTjYvOC0zavdl3LLyU90/edit?usp=sharing

Задание 1
Напишите ответ в свободной форме, не больше одного абзаца текста.
Установите Docker Compose и опишите, для чего он нужен и как может улучшить лично вашу жизнь.


Docker Compose в одном манифесте может управлять большим количеством контейнеров. Такой подход можно назвать “Инфраструктура как код”.




Задание 2
Выполните действия и приложите текст конфига на этом этапе.
Создайте файл docker-compose.yml и внесите туда первичные настройки:
version;
services;
volumes;
networks.
При выполнении задания используйте подсеть 10.5.0.0/16. Ваша подсеть должна называться: <ваши фамилия и инициалы>-my-netology-hw. Все приложения из последующих заданий должны находиться в этой конфигурации.


version: '3'


services:


volumes:


networks:
 bednikdo-my-netology-hw:
   name: "bednikdo-my-netology-hw"
   driver: bridge
   ipam:
     config:
       - subnet: 10.5.0.0/16
         gateway: 10.5.0.1




Задание 3
Выполните действия:
Создайте конфигурацию docker-compose для Prometheus с именем контейнера <ваши фамилия и инициалы>-netology-prometheus.
Добавьте необходимые тома с данными и конфигурацией (конфигурация лежит в репозитории в директории 6-04/prometheus ).
Обеспечьте внешний доступ к порту 9090 c докер-сервера.

 prometheus:
   image: prom/prometheus:v2.47.2
   container_name: bednikdo-netology-prometheus
   command: --web.enable-lifecycle --config.file=/etc/prometheus/prometheus.yml
   ports:
     - 9090:9090
   volumes:
     - ./prometheus:/etc/prometheus
     - prometheus-data:/prometheus
   networks:
     - bednikdo-my-netology-hw
 volumes:
   prometheus-data:



Задание 4
Выполните действия:
Создайте конфигурацию docker-compose для Pushgateway с именем контейнера <ваши фамилия и инициалы>-netology-pushgateway.
Обеспечьте внешний доступ к порту 9091 c докер-сервера.

 pushgateway:
   image: prom/pushgateway:v1.6.2
   container_name: bednikdo-netology-pushgateway
   ports:
     - 9091:9091
   networks:
   - bednikdo-my-netology-hw

Задание 5
Выполните действия:
Создайте конфигурацию docker-compose для Grafana с именем контейнера <ваши фамилия и инициалы>-netology-grafana.
Добавьте необходимые тома с данными и конфигурацией (конфигурация лежит в репозитории в директории 6-04/grafana.
Добавьте переменную окружения с путем до файла с кастомными настройками (должен быть в томе), в самом файле пропишите логин=<ваши фамилия и инициалы> пароль=netology.
Обеспечьте внешний доступ к порту 3000 c порта 80 докер-сервера.

 grafana:
   image: grafana/grafana
   container_name: bednikdo-netology-grafana
   environment:
     GF_PATHS_CONFIG: /etc/grafana/custom.ini
   ports:
     - 80:3000
   volumes:
     - ./grafana:/etc/grafana
     - grafana-data:/var/lib/grafana
   networks:
     - bednikdo-my-netology-hw
 volumes:
   grafana-data:





Задание 6
Выполните действия.
Настройте поочередность запуска контейнеров.
Настройте режимы перезапуска для контейнеров.
Настройте использование контейнерами одной сети.
Запустите сценарий в detached режиме.

В каждую секцию сервисов добавляет depends_on и restart:

Для Prometheus:
   restart: always

Для Pushgateway:
   depends_on:
     - prometheus
   restart: always
Для Grafana:
   depends_on:
     - pushgateway
   restart: always


Задание 7
Выполните действия.
Выполните запрос в Pushgateway для помещения метрики <ваши фамилия и инициалы> со значением 5 в Prometheus: echo "<ваши фамилия и инициалы> 5" | curl --data-binary @- http://localhost:9091/metrics/job/netology.
Залогиньтесь в Grafana с помощью логина и пароля из предыдущего задания.
Cоздайте Data Source Prometheus (Home -> Connections -> Data sources -> Add data source -> Prometheus -> указать "Prometheus server URL = http://prometheus:9090" -> Save & Test).
Создайте график на основе добавленной в пункте 5 метрики (Build a dashboard -> Add visualization -> Prometheus -> Select metric -> Metric explorer -> <ваши фамилия и инициалы -> Apply.
В качестве решения приложите:
docker-compose.yml целиком;
скриншот команды docker ps после запуске docker-compose.yml;
скриншот графика, постоенного на основе вашей метрики.




















Файл docker-compose.yml


Cкриншот команды docker ps после запуске docker-compose.yml


Cкриншот графика, постоенного на основе метрики bednikdo:

Задание 8
Выполните действия:
Остановите и удалите все контейнеры одной командой.
В качестве решения приложите скриншот консоли с проделанными действиями.



Задание 9*
Выполните действия:
Создайте конфигурацию docker-compose для Alertmanager с именем контейнера <ваши фамилия и инициалы>-netology-alertmanager.
Добавьте необходимые тома с данными и конфигурацией, сеть, режим и очередность запуска.
Обновите конфигурацию Prometheus (необходимые изменения ищите в презентации или документации) и перезапустите его.
Обеспечьте внешний доступ к порту 9093 c докер-сервера.
В качестве решения приложите скриншот с событием из Alertmanager.


Задание 10*
Запустите свой сценарий на чистом железе без предзагруженных образов.
Ответьте на вопросы в свободной форме:
Опишите выполненный вами процесс развертывания сценария.
Как вы думаете зачем может понадобиться такой способ развертывания?

1. На новой машине склонировал репозиторий из github -> запустил docker compose файл, через одну минуту все контейнеры уже работали.
2. Для быстрого поднятия инфраструктыры.
