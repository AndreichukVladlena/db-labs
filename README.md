## Система онлайн-регистрации и управления мероприятиями

## Андрейчук Владлена Витальевна, 153501

## Сущности

* ```User``` 
  
    + id BIGSERIAL PRIMARY KEY NOT NULL,
    + name VARCHAR(50) NOT NULL,
    + email VARCHAR(50) UNIQUE NOT NULL,
    + password VARCHAR(255) NOT NULL,
    + role_id BIGINT REFERENCES role(id) NOT NULL
    +     Связи:
            Один пользователь может быть связан с множеством мероприятий как организатор или участник.
  
* ```Event```
  
    + id BIGSERIAL PRIMARY KEY NOT NULL,
    + name VARCHAR(100) NOT NULL,
    + description TEXT,
    + image_id BIGINT REFERENCES image(id) NOT NULL,
    + organizer_id BIGINT REFERENCES user(id) NOT NULL
    +     Связи:
            Множество мероприятий могут быть организованы одним пользователем (Организатором).
  
* ```Ticket```
  
    + id BIGSERIAL PRIMARY KEY NOT NULL,
    + event_id BIGINT REFERENCES event(id) NOT NULL,
    + user_id BIGINT REFERENCES user(id) NOT NULL,
    + ticket_code VARCHAR(50) NOT NULL
    +     Связи:
           Билет связан с одним мероприятием и одним участником.
  
* ```EventSchedule```
  
    + id BIGSERIAL PRIMARY KEY NOT NULL,
    + event_id BIGINT REFERENCES event(id) NOT NULL,
    + start_date DATE NOT NULL,
    + end_date DATE NOT NULL,
    + duration TIME NOT NULL,
    + location VARCHAR(100) NOT NULL,
    + description TEXT
    +     Связи:
           Несколько расписаний мероприятия может быть привязано к одному мероприятию.
  
  
* ```ActivityLog``` 
  
    + id BIGSERIAL PRIMARY KEY NOT NULL,
    + user_id BIGINT REFERENCES user(id) NOT NULL,
    + activity_id BIGINT REFERENCES event(id),
    + date DATE NOT NULL,
    + time TIME NOT NULL
    +     Связи:
            Журнал авторизации связан с пользователем и активностью.

* ```Activity``` 
  
    + id BIGSERIAL PRIMARY KEY NOT NULL,
    + name VARCHAR(50) UNIQUE NOT NULL
    +     Связи:
            Одна активность связана с одной записью в журнал актиностей.

  
* ```Role``` 
  
    + id BIGSERIAL PRIMARY KEY NOT NULL,
    + name VARCHAR(50) UNIQUE NOT NULL
    +     Связи:
            Роль может быть назначена множеству пользователей.
  
* ```EventCategory```
  
    + id BIGSERIAL PRIMARY KEY NOT NULL,
    + name VARCHAR(50) UNIQUE NOT NULL
    +     Связи:
            Мероприятие может принадлежать одной или нескольким категориям.
  
  
* ```Review```
  
    + id BIGSERIAL PRIMARY KEY NOT NULL,
    + user_id BIGINT REFERENCES user(id) NOT NULL,
    + event_id BIGINT REFERENCES event(id) NOT NULL,
    + content TEXT NOT NULL,
    + date DATE NOT NULL,
    + time TIME NOT NULL
    +     Связи:
            Отзыв связан с пользователем и мероприятием.
  
* ```EventImage``` 

    + id BIGSERIAL PRIMARY KEY NOT NULL,
    + event_id BIGINT REFERENCES event(id) NOT NULL,
    + image_path VARCHAR(255) UNIQUE NOT NULL,
    + description TEXT
    +     Связи:
            Фотография мероприятия привязана к одному мероприятию. К одному мероприятию могут быть привязаны несколько изображений.
  
* ```Payment```

    + id BIGSERIAL PRIMARY KEY NOT NULL,
    + user_id BIGINT REFERENCES user(id) NOT NULL,
    + ticket_id BIGINT REFERENCES ticket(id) NOT NULL,
    + sum DECIMAL NOT NULL,
    + date DATE NOT NULL
    +     Связи:
            Платеж связан с определенным пользователем и билетом.
  


## Функциональные требования к системе онлайн-регистрации и управления мероприятиями
* 1. **Авторизация и аутентификация**
    + 1.1 Вход в систему

            Система должна предоставлять возможность пользователям (организаторам мероприятий и участникам) войти в систему с использованием уникальных учетных данных (логин и пароль).

    + 1.2 Регистрация новых пользователей

            Система должна позволять новым пользователям создавать учетные записи путем предоставления необходимой информации, включая имя, адрес электронной почты и пароль.

    + 1.3 Восстановление пароля

            Пользователи должны иметь возможность сбросить свой пароль через отправку ссылки для восстановления на указанный адрес электронной почты.

* 2. **Управление мероприятиями**

    + 2.1 Создание мероприятий

            Организаторы мероприятий должны иметь возможность создавать новые мероприятия, указывая информацию, такую как название, дата, время, местоположение и описание.

    + 2.2 Редактирование мероприятий

            Организаторы мероприятий должны иметь возможность редактировать информацию о созданных мероприятиях.

    + 2.3 Удаление мероприятий

            Организаторы мероприятий должны иметь возможность удалять мероприятия, которые больше не актуальны.

* 3. **Управление расписанием**
  
    + 3.1 Добавление событий в расписание

            Организаторы мероприятий должны иметь возможность добавлять события (например, семинары, выступления) в расписание мероприятия, указывая дату, время и место проведения.

    + 3.2 Редактирование событий

            Организаторы мероприятий должны иметь возможность редактировать информацию о событиях в расписании.

    + 3.3 Удаление событий

            Организаторы мероприятий должны иметь возможность удалять события из расписания.

* 4. **Управление билетами**
    + 4.1 Генерация билетов

            После успешной регистрации на мероприятие, участникам должны генерироваться билеты в электронном виде с уникальными штрих-кодами или QR-кодами.

    + 4.2 Просмотр билетов

            Участники и организаторы мероприятий должны иметь возможность просматривать информацию о своих билетах, включая детали мероприятия и код билета.

* 5. **Система ролей**
  
    + 5.1 Роли пользователей

            Система должна поддерживать следующие роли пользователей: администратор, организатор мероприятий и участник.

    + 5.2 Права доступа

            Каждая роль должна иметь определенные права доступа, например, администратор имеет полный доступ к управлению системой, организатор может создавать и управлять мероприятиями, а участник может регистрироваться на мероприятия и просматривать свои билеты.

* 6. **Журналирование действий пользователя**
   
    + 6.1 Журналирование событий

            Система должна вести журнал действий пользователей, включая вход и выход из системы, создание и редактирование мероприятий, регистрацию на мероприятия и другие важные события.

    + 6.2 Хранение журнала

            Журнал должен храниться в безопасной и защищенной базе данных для последующего анализа и мониторинга.

## Ненормализованная схема БД
![Alt text](notnormalized.drawio(1).png)

## Схема БД
![Alt text](image.png)