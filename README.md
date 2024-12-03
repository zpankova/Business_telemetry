# Business_telemetry
Создание дашборда, описывающего и визуализирующего метрики и показатели интернет-магазина на [основе логов пользовательской активности.](https://storage.yandexcloud.net/bigdata-intensive-2023/dataset_telemetry.csv)

### Этап 1 
Исходный датасет весит более 800мб, чтобы создает трудности:
1) при проведении EDA привычными методами (pandas) -> решение: использовать pySpark
2) при работе в Datalense -> решение: создать кластер в Clickhouse

Для начала был открыт датасет и на основе его колонок были выдвинуты предположения о том, на что можно посмотреть в данных. 
![image](https://github.com/user-attachments/assets/546820f5-4893-499a-958c-6b4fba0a96b4)

С помощью библиотек визуализации в python [файл EDA.ipynb](https://github.com/zpankova/Business_telemetry/blob/main/EDA.ipynb), построим графики для поиска зависимостей/ сезонности/ аномалий
Увидели: 
1. Сезонность в ARPU и ARPPU
   ![image](https://github.com/user-attachments/assets/0088fb50-4421-4cb0-bbcd-8fa17fdef978)

2. Товары для детей приносят больше всего выручки
   ![image](https://github.com/user-attachments/assets/95aa41e2-218a-4d44-89a0-b28e20eb016f)

3. Москва - топ-1 город по выручке
   ![image](https://github.com/user-attachments/assets/71e951f3-6e3c-4e32-afd9-77df4e9597f9)


