# conversion-analysis
<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/45a9840c-c35c-4671-be87-d57c13a4dbe6" />


Funnel conversion analysis using SQL and Excel dashboard
Проект - мобильное приложение. Проблема Конверсия снизилась с 10% до 7%.
Основная просадка произошла на этапе app_open → add_to_cart (–40%).

user_id	event_name	event_time		country
1	      app_open	    ...		         UZ
1	      add_to_cart  	...		         UZ
1	      purchase	    ...		         UZ

Анализ:
Проверка data quality
Расчет ключевых метрик
Построение воронки
Формулировка гипотез

SQL рассчеты:

Воронка пользователей:
SELECT COUNT(DISTINCT CASE WHEN event_name = 'app_open' THEN user_id END) AS app_users, 
COUNT(DISTINCT CASE WHEN event_name = 'add_to_cart' THEN user_id END) AS add_to_users, 
COUNT(DISTINCT CASE WHEN event_name = 'purchase' THEN user_id END) AS purchase_users FROM events;
#подсчитываю количество пользователей на каждом этапе

Считаю конверсии:
SELECT app_users, add_to_users, purchase_users, 
add_to_users * 1.0 / app_users AS conversion_add_to_cart, 
purchase_users * 1.0 / app_users AS conversion_total, 
purchase_users * 1.0 / add_to_users AS conversion_purchase 
FROM ( SELECT COUNT(DISTINCT CASE WHEN event_name = 'app_open' THEN user_id END) AS app_users, COUNT(DISTINCT CASE WHEN event_name = 'add_to_cart' THEN user_id END) AS add_to_users, COUNT(DISTINCT CASE WHEN event_name = 'purchase' THEN user_id END) AS purchase_users FROM events ) t;
# добавляю *1.0 чтобы изменить типа данных с int на float. использую подзапрос с алиасами для возвращения конверсии пользователей добавляющих товар в корзину, покупателей и общей конверсии

#Результаты
Этап	      Пользователи
app_open	=     1000
add_to_cart =  	600
purchase     =   300

конверсия добавления в корзину - 60%
тотал конверсия - 30%
конверсия в покупку после добавления в корзину - 50%

причины просадки:
1. возможные баги при покупке товаров из корзину
2. Ухудшение интерфейса (стал менее дружелюбен к новым пользователям)
3. снижение целевой аудитории
4. влиаяние внешних факторов (сезонность, каникулы и т.д.)

Для визуализации были использованы 
воронка пользователей и конверсия
Используемые инструменты - sql, excel 




