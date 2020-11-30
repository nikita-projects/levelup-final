# LevelUP final work
## Никита Попов. Итоговое задание. Вариант 2 - типизированный

Дополнительные репозитории:
 - Приложение - https://github.com/nikita-projects/boxfuse-sample-java-war-hello/tree/final-work
 - Билд Docker https://github.com/nikita-projects/test-docker

Текущий адрес Jenkins - http://35.228.72.29/
Настроено две джобы.

Для деплоя на узел используется агент.

Тецущий адрес узла - http://35.228.18.105/
Узел может быть любой. Требования к узлу:
 - должен быть пользователь jenkins с sudo правами и публичным ключём jenkins.pub (доступен в этом репозитории)
 - на узел необходимо разрешить HTTP трафик (для доступа к развёрнутому проекту)

# Описание джоб

## Джоба Start build and trigger deploy

Принимает параметры:
 - ссылку на репозиторий Docker Hub
 - креды для доступа к Docker Hub
 - IP адрес узла, на котором необходимо развернуть образ в Docker Swarm.

Собирает контейнер и пушит его на Docker Hub в приватный репозиторий. Затем запускает джобу для деплоя.

## Джоба Prepare node and deploy

Принимает параметры:
 - сылку на репозиторий Docker Hub
 - креды для доступа к Docker Hub
 - IP адрес узла, на котором необходимо развернуть образ в Docker Swarm
 - номер билда (тег) образа

Подключается на узел, настраивает окружение и запускает контейнер в Docker Swarm.
