# Подготовка сервера (RX PreparedServer Component)

#### Описание:

Подготовка сервера для сборки пакета. Создание недостающих директорий, очистка существующих директорий, обновление конфигурации DDS согласно указанным переменным, подтягивание указанных веток гита, создание xml для создания пакета.

##### Логика:

Указывает теги для джобов из $[RunnerTags](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component#runnertags)

Находит или создает папку для группы проекта по адресу: $[ProjectsFolderPath](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component/-/tree/main?ref_type=heads#projectsfolderpath)\\$[CI\_PROJECT\_NAMESPACE](https://docs.gitlab.com/ci/variables/predefined_variables/#:~:text=project%2D1.-,CI_PROJECT_NAMESPACE,-Pre%2Dpipeline)

Находит папку для DDS нужной версии в группе по адресу: $[ProjectsFolderPath](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component/-/tree/main?ref_type=heads#projectsfolderpath)\\$[CI\_PROJECT\_NAMESPACE](https://docs.gitlab.com/ci/variables/predefined_variables/#:~:text=project%2D1.-,CI_PROJECT_NAMESPACE,-Pre%2Dpipeline)\\DDS\\[RXVERSION](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component/-/tree/main?ref_type=heads#rxversion)  
Если такой папки нет, то создает её и копирует DDS нужной версии из [общей папки с DDS](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component/-/tree/main?ref_type=heads#ddsfolderpath)

Находит папку для DT нужной версии в группе по адресу: $[DTConfigPath](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component/-/tree/main?ref_type=heads#dtconfigpath)  
Если такой папки нет, то создает её и копирует DT нужной версии из [общей папки с DT](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component/-/tree/main?ref_type=heads#dtfolderpath)

Удаляет старый конфигурационный файл для DDS, если он есть, по адресу: $[DDSConfigPath](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component/-/tree/main?ref_type=heads#ddsconfigpath)\\\_ConfigSettings.xml

Находит или создает папку для гита проекта по адресу: $[GitProjectFolder](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component/-/tree/main?ref_type=heads#gitprojectfolder)

Находит или создает папку для логов по адресу: $[LogsPath](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component/-/tree/main?ref_type=heads#logspath) и чистит её

Чистит папку гита проекта

Клонирует репозитории, указанные в параметре Repositories в папку гита проекта

Создает конфигурационный файл DDS из примера по адресу $[DDSConfigPath](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component/-/tree/main?ref_type=heads#ddsconfigpath)\\\_ConfigSettings.xml.example или $[DDSFolderPath](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component/-/tree/main?ref_type=heads#ddsfolderpath)\\\_ConfigSettings.xml.example  
Заполняет его данными, указанными в параметре Repositories

Создает конфигурационный файл для DT из примера по адресу $[DTConfigPath](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component/-/tree/main?ref_type=heads#dtconfigpath)\\\_ConfigSettings.xml.example или $[DTFolderPath](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component/-/tree/main?ref_type=heads#dtfolderpath)\\\_ConfigSettings.xml.example  
И заполняет его данными, указанными в параметрах ([DeployDestination](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component/-/tree/main?ref_type=heads#deploydestination))DeployIpServer, [LogsPath](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component/-/tree/main?ref_type=heads#logspath), [WebProtocol](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component/-/tree/main?ref_type=heads#webprotocol), [ServerHttpsPort](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component/-/tree/main?ref_type=heads#serverhttpsport), [ServerHttpPort](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component/-/tree/main?ref_type=heads#serverhttpport)

#### Переменные:

##### Пользовательские настройки

##### Repositories

**Описание:** Список репозиториев в формате: Слой | Ссылка на репозиторий (ssh или http) | ветка , (если не последний)  
**Примечание**: Эту переменную нужно указывать в [основном компоненте](https://git.starkovgrp.ru/ci-cd-components/Completed-RXDTDeploy-Component)  
**Обязательность:** Да  
**Пример:**  Work | ssh://git@git.starkovgrp.ru:2224/anetskih/aai-edu-25.2.git | master,  
 Base | ssh://git@git.starkovgrp.ru:2224/anetskih/aai-edu-25.2-base.git | main

##### DeployIpServer

**Описание:** Адрес сервера, на который будет производится публикация  
**Обязательность:** Да  
**Примечание**: Не указывать напрямую, а [создать переменную в проекте](https://docs.gitlab.com/ci/variables/#define-a-cicd-variable-in-the-ui)  
**Пример:** 10.9.3.62