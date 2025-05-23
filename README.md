# Домашнее Задание Евгения Сартисона #

## (1) Создание проекта ##
***Создайте виртуальную машину в Яндекс.Облаке // любых других облаках // у себя на компьютере с параметрами: 1-2 ядра, 2-4 ГБ памяти, операционная система Ubuntu.. Назовите проект в формате bananaflow-<yyyymmdd>, где yyyymmdd — год, месяц и день вашего рождения (например, bananaflow-19900101).***

Для экономии времени, взял уже существующую у меня виртуальную машину и выполнил всю работу там. 


## (2) Настройка SSH-доступа ## 
***- Добавьте свой SSH-ключ к виртуальной машине.***
***- Подключитесь к машине через SSH (первая сессия). Не забудьте использовать ssh-add для добавления ключа в агент.***
***- *Почему это важно:* Алексей всегда использует SSH-ключи для безопасного доступа к серверам.***

Сделано


## (3) Установка PostgreSQL ##
***- Установите PostgreSQL из пакетов с помощью команды apt install.***
***- *Почему это важно:* Алексей выбрал этот метод, чтобы быстро развернуть базу данных без лишних сложностей.***

выполнил стандартную установку Postgres кластера
![image](https://github.com/user-attachments/assets/a27f7e74-17bc-460a-81c5-ad99cc17f262)

## (4) Подключение к PostgreSQL ##
 ***Откройте вторую SSH-сессию и подключитесь к PostgreSQL через psql от имени пользователя postgres.***
 ****Почему это важно:* Алексей использовал две сессии, чтобы имитировать работу нескольких пользователей.***

сделано

![image](https://github.com/user-attachments/assets/9cb97ca5-fbe5-4a08-9584-87a4ff9bd9e0)


## (5) Работа с транзакциями ##

***Выключите автоматический коммит (auto commit) в обеих сессиях.***
![image](https://github.com/user-attachments/assets/2830482b-9597-4a6d-9573-bda6b85711d5)
  
***В первой сессии создайте таблицу shipments (перевозки) и добавьте в неё данные:***

Выполнено
![image](https://github.com/user-attachments/assets/c4c42eaf-7f71-4cd7-8a6f-65cb9b962900)


## (6) Изучение уровней изоляции ##

***Проверьте текущий уровень изоляции с помощью команды:***
![image](https://github.com/user-attachments/assets/ce977a47-c685-45f5-8a9a-0929d5ac2361)


***Начните новую транзакцию в обеих сессиях с уровнем изоляции по умолчанию***
***В первой сессии добавьте новую запись***
![image](https://github.com/user-attachments/assets/4b21eea1-1554-432b-887d-d3d6c7f9b517)

***Во второй сессии выполните:***

![image](https://github.com/user-attachments/assets/c0d312e5-d423-481a-bf67-c44ca42a1883)

***- Видите ли вы новую запись? Объясните, почему да или нет.***

Во второй сессии новую запись с продуктом sugar НЕ ВИЖУ, так-как дынные не зафиксированы(commit)

***- Завершите первую транзакцию с помощью commit; и снова выполните select \* from shipments во второй сессии. Видите ли вы новую запись теперь? Объясните.***
![image](https://github.com/user-attachments/assets/863ca9e0-21a1-43c8-ab10-d1a5de39ac31)

После фиксации изменения ВИДНЫ во второй сессии. 


## (7) Эксперименты с уровнем изоляции Repeatable Read ##

***Начните новые транзакции в обеих сессиях с уровнем изоляции repeatable read:***

Открыл 2 новые сессии
![image](https://github.com/user-attachments/assets/deb76154-fdb5-4caa-98c3-62f2acc166cf)
  

***В первой сессии добавьте новую запись:***
![image](https://github.com/user-attachments/assets/0fbf67e9-4e22-4167-92a3-f148aa1bfc7b)

***Во второй сессии выполните:***
![image](https://github.com/user-attachments/assets/13cbbed2-4259-44a6-874a-b877792d8ee2)


***- Видите ли вы новую запись? Объясните, почему да или нет.***

Данные во второй сессии НЕ ВИДНЫ, так-как измененеия не зафиксированы(commit) и из-за уровня изоляции

***Завершите первую транзакцию с помощью commit; и снова выполните select \* from shipments во второй сессии. Видите ли вы новую запись теперь? Объясните.***
![image](https://github.com/user-attachments/assets/bf1598b1-4111-4aed-8f9e-65609afacd11)

Картина та же, изменения не видны во второй сессии после фиксации(commit) в первой. В режиме **Repeatable Read** видны только те данные, которые были зафиксированы до начала транзакции, но не видны незафиксированные данные и изменения,произведённые другими транзакциями в процессе выполнения данной транзакции.

***Завершите вторую транзакцию и снова выполните select \* from shipments. Видите ли вы новую запись? Объясните.***
![image](https://github.com/user-attachments/assets/18ad2bd8-e26a-4122-b488-a30f6816709f)

Запись во втрой сессии ВИДНА после фиксации. Причина в работе  **Repeatable Read**.

