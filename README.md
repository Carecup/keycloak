# keycloak
Keycloak in docker with cron backup 
Колесник Артем, Б9120-09.03.04(2)
Для поднятия keycloak использовался docker + docker compose, контейнеры описаны в docker-compose.yaml
После запуска контейнеров с помощью команды docker-compose up -d получаем следующую структуру:
<img width="1242" alt="image" src="https://github.com/Carecup/keycloak/assets/25736912/892f5fcc-4b30-4cd7-ba0a-63dd9d532719">
<img width="880" alt="image" src="https://github.com/Carecup/keycloak/assets/25736912/2b5083d6-50e5-481c-8fd0-9a349630263f">

Два контейнера, keycloak доступен  по порту 8080:
<img width="1066" alt="image" src="https://github.com/Carecup/keycloak/assets/25736912/74517048-e273-4fe2-aa1b-9c066448326b">

Для создания бэкапа используем следующую команду, проверяем что дамп БД создается:
docker exec -t downloads-postgres-1 pg_dumpall -c -U keycloak > dump_`date +%d-%m-%Y"_"%H_%M_%S`.sql
<img width="381" alt="image" src="https://github.com/Carecup/keycloak/assets/25736912/9587ae97-cc1a-4a3a-81cf-a5aec963e193">
Для того чтобы автоматически создавать бэкапы создаем крон с помощью утилиты crontab, который будет выполнять эту команду раз в сутки:
crontab -e
0 0 * * * /usr/local/bin/docker exec -t downloads-postgres-1 pg_dumpall -c -U keycloak > /Users/artemkolesnik/Downloads/dump_`date +\%d-\%m-\%Y"_"\%H_\%M_\%S`.sql
После этого бэкапы БД keycloak будут создаваться ежедневно вне контейнера 


Далее создаем клиента в keycloak:
![image](https://github.com/Carecup/keycloak/assets/25736912/9cf21a28-605b-4200-9f1b-cddd017cc1f7)

Создаем client scope: 
![image](https://github.com/Carecup/keycloak/assets/25736912/fde1beac-c628-4e77-b1d6-54b854ca946d)

Создаем группу:
![image](https://github.com/Carecup/keycloak/assets/25736912/f0ab45a8-cf1a-46f4-bae1-092d62bdc1a3)

Добавляем пользователя:
![image](https://github.com/Carecup/keycloak/assets/25736912/65196e49-9499-4a1b-850a-0577e59773e4)

Добавляем role в client:

![image](https://github.com/Carecup/keycloak/assets/25736912/495b9ebf-c06e-49d6-8d4b-4fcb5280a1e5)

Выполняем role mapping в группе:

![image](https://github.com/Carecup/keycloak/assets/25736912/446b43aa-c790-489b-ac55-c0b3ba683ba3)

Тест выполнения http запросов

Получение токена по паролю:

<img width="1325" alt="image" src="https://github.com/Carecup/keycloak/assets/25736912/15a97739-04bb-4867-8f1a-3b24fea3f037">

Получение информации о пользователе:

<img width="1325" alt="image" src="https://github.com/Carecup/keycloak/assets/25736912/49e1fad5-72ee-43d4-b3fb-9e4d5ff28cea">

Получение токена по refresh токену:

<img width="1325" alt="image" src="https://github.com/Carecup/keycloak/assets/25736912/0c26ce93-6f07-4b7e-88ba-b36f616cefbf">

Получение информации про realm:

<img width="1325" alt="image" src="https://github.com/Carecup/keycloak/assets/25736912/a6a0d396-f6bd-4e66-8c48-576c9c330865">
