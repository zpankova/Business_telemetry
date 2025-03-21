# Business_telemetry
Создание дашборда, описывающего и визуализирующего метрики и показатели интернет-магазина на [основе логов пользовательской активности.](https://storage.yandexcloud.net/bigdata-intensive-2023/dataset_telemetry.csv)

## Оглавление:   
[Этап 1 - EDA](#этап-1)  
[Этап 2 - СУБД + бизнес аспект](#этап-2)  
[Этап 3 - описание дашборда](#этап-3)  
[Итог - ссылки на дашборд и презентацию](#итог)  


### Этап 1 
Исходный датасет весит более 800мб, что создает трудности:
1) при проведении EDA привычными методами (pandas) -> решение: использовать pySpark
2) при работе в Datalense -> решение: подключить Clickhouse

Для начала был открыт датасет и на основе его колонок были выдвинуты предположения о том, на что можно посмотреть в данных

С помощью библиотек визуализации в python [файл EDA.ipynb](https://github.com/zpankova/Business_telemetry/blob/main/EDA.ipynb), построим графики для поиска зависимостей/ сезонности/ аномалий
Увидели: 
1. Сезонность в ARPU и ARPPU
   ![image](https://github.com/user-attachments/assets/0088fb50-4421-4cb0-bbcd-8fa17fdef978)

2. Товары для детей приносят больше всего выручки
   ![image](https://github.com/user-attachments/assets/95aa41e2-218a-4d44-89a0-b28e20eb016f)

3. Москва - топ-1 город по выручке
   ![image](https://github.com/user-attachments/assets/71e951f3-6e3c-4e32-afd9-77df4e9597f9)
  
Удалили дубли по userid и timestamp. Использовали Z-score для поиска аномалий (на столбце value).  

### Этап 2
После минимального EDA мы загрузили датасет на сервер в Clickhouse и далее в Datalens для построения графиков.  
Далее выстроили бизнес логику чартов:
![image](https://github.com/user-attachments/assets/7bcaacc8-f044-464b-b447-c57aff90bd61)

Мы построили базовые графики для дашборда и нашли бизнес-сферы (продажи, маркетинг, мониторинг аномалий), в которых бы хотели подробнее рассмотреть дашборд. Целью было посмотреть под разным углом на одни и те же данные, и за счет этого вынести максимум информации.   

### Этап 3
Вкладки дашборда:   
  
##### ПРОДАЖИ
![image](https://github.com/user-attachments/assets/978e18b7-879a-4a7c-a4f4-f125301319a1)

1. Представлены ключевые показатели эффективности, данные о продажах и аналитика поведения пользователей.  
![image](https://github.com/user-attachments/assets/24f1aa3f-87cc-47e9-a2b5-7625e2a93c23)

2. Использование аналитиком:  
- Итак, аналитик заходит на дашборд, он может посмотреть на динамику ключевых метрик, таких как сумма продаж, средний чек и конверсии, чтобы выявить тренды.  
- Также, аналитик может сравнивать показатели по категориям, полу и возрастным группам, что позволяет ему находить целевые аудитории и точки роста.
  ![image](https://github.com/user-attachments/assets/b9b5e5e5-7d2e-4c5f-9875-3b000c97ffbe)

- Используя временные фильтры, например скейл, отвечающий за группировку данных в датасете в трех вариантах - дни - недели - месяца , аналитик может детализировать данные за конкретные периоды.
  ![image](https://github.com/user-attachments/assets/f032c3ec-f18d-4dcc-9459-54c0df39f69c)

- Используя таблицу с метриками по городам можно находить точки продаж с лучшими результатами, а график - гео-карта позволяет визуализировать распределение продаж.
  ![image](https://github.com/user-attachments/assets/942002bc-161b-434a-950c-650ab0b261c0)
![image](https://github.com/user-attachments/assets/9fc8b2e8-cc7b-41e8-9cd1-9371fbd1fd8f)

3. Использование руководителем отдела продаж:  
- Руководитель может использовать вкладку для контроля общей эффективности отдела и выполнения KPI или чтобы оперативно увидеть, в каких категориях товаров или регионах наблюдается снижение продаж.  
- Гео-карта и таблица по городам помогают распределять ресурсы между точками продаж.  
- Руководитель анализирует динамику ARPU и ARPPU, что позволяет ему корректировать стратегию работы с клиентами и повышать доходность.  
- Конверсионная воронка и показатели CR дают возможность оценить этапы, на которых теряются клиенты, и внедрять улучшения в процесс.  
- Фильтры позволяют сосредоточиться на конкретной целевой аудитории или продуктовой категории.  
- В таблице с conversion rate есть иерархия - можно посмотреть детализацию по месяцам и далее спуститься на детализацию по неделям.
  ![image](https://github.com/user-attachments/assets/0eecc5a0-0711-4583-af29-eb2d9c8ef96f)

4. Другие сценарии использования:  
- Финансовый отдел: использует данные для построения прогнозов выручки и анализа доходности.  
- Региональные менеджеры: фокусируются на показателях по конкретным городам, чтобы корректировать локальные стратегии.  

##### МАРКЕТИНГ
1. Анализ влияния маркетинговыых кампаний на бизнес.  
2. Директора и топ-менеджеры  
- Директора могут использовать дашборд для принятия стратегических решений, основанных на данных. Например, они могут определить, какие города или возрастные группы приносят наибольшую выручку и направить ресурсы на эти сегменты.  
- Директора могут оценить эффективность текущих маркетинговых кампаний и стратегий продаж, а также выявить области, требующие улучшения.
  ![image](https://github.com/user-attachments/assets/99470937-d3e8-4372-8333-11bcb5c2e639)

3. Маркетинговые аналитики  
- Аналитики могут использовать дашборд для глубокого анализа поведения пользователей, выявления трендов и паттернов. Это поможет им разработать более целевые маркетинговые стратегии. Например, можно смотреть активность пользователей по часам.
  ![image](https://github.com/user-attachments/assets/96d87025-c0cb-471a-9e50-804fe9d25658)
 
- Аналитики могут сегментировать аудиторию на основе данных о пользователях, чтобы создать персонализированные маркетинговые кампании. Например, анализировать аудиторию со стороны возраста.
  ![image](https://github.com/user-attachments/assets/6bb85ae2-6b73-4667-8ed2-d37a2422597a)

- Можно анализировать поведения пользователей по сессиям и на основе информации о сессиях можно также проводить рекламные акции или различные A/B тестирования с наибольшей эффективностью.
  ![image](https://github.com/user-attachments/assets/59a0131b-ba4d-498c-9537-0ea7c36adede)

- Менеджеры по продукту могут использовать дашборд для анализа потребностей и предпочтений клиентов, что поможет им в разработке новых продуктов или улучшении существующих. Например, смотреть наиболее популярные категории у разных пользователей и тд.
  ![image](https://github.com/user-attachments/assets/fb0d3fa1-6381-414f-99c9-bca64432c991)


##### АНОМАЛИИ
1. Распределения по месяцам, для выявлении аномалий.  
2. На графике зависимости value по дням аналитик может определить аномалии в распределении по дням. Данный график также выделяет те дни и месяца, когда value больше, чем обычно.  
![image](https://github.com/user-attachments/assets/123d45bd-f7c3-4e0d-9dcf-5b5ba18e7b4b)

3. На графике распределения value по категориям можно увидеть различные аномалии, связанные с неравномерностью трат на различные категории  
![image](https://github.com/user-attachments/assets/02849700-9846-4642-899a-94eebdb5ab74)

4. На графике распределения value по гендеру можно увидеть на что больше тратят разные представители пола и, возможно, выявить какие-то аномалии  
![image](https://github.com/user-attachments/assets/44a51ca7-549f-4596-bc2f-cd8a369b50b7)


5. На графике распределения userid по часам, мы можем увидеть как изменяется количество покупателей в тот или иной час в разные года и, тем самым, показать различные аномалии  
![image](https://github.com/user-attachments/assets/532a50b3-f1df-40fc-ade7-64e0b108e7e5)


6. График распределения категорий по возрасту детей может показать на что больше уходит value и может помочь выявить различные аномалии,связанные  как с распределением value по категориям, так и с различием между годами.
![image](https://github.com/user-attachments/assets/6b949a4b-edbb-493a-a395-69f2ded0a83d)

### Итог
[Дашборд:](https://datalens.yandex/sd2yfrnb5462f)

[Презентация:](https://disk.yandex.ru/i/ceXGbnEGR_2r3g)


