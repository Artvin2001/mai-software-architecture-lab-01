# Компонентная архитектура

## Компонентная диаграмма

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

AddElementTag("microService", $shape=EightSidedShape(), $bgColor="CornflowerBlue", $fontColor="white", $legendText="microservice")
AddElementTag("storage", $shape=RoundedBoxShape(), $bgColor="lightSkyBlue", $fontColor="white")

Person(admin, "Администратор")
Person(moderator, "Модератор")
Person(user, "Пользователь")

System_Ext(web_site, "Клиентский веб-сайт", "HTML, CSS, JavaScript, React", "Веб-интерфейс")

System_Boundary(conference_site, "Сайт конференций") {
   Container(user_service, "Сервис авторизации", "Python", "Сервис управления пользователями", $tags = "microService")    
   Container(report_service, "Сервис докладов", "Python", "Сервис управления докладами", $tags = "microService") 
   Container(conference_service, "Сервис конференций", "Python", "Сервис управления конференциями", $tags = "microService")   
   ContainerDb(db, "База данных", "MySQL", "Хранение данных о пользователях, докладах и конференциях", $tags = "storage")
}

Rel(admin, web_site, "Просмотр, добавление и редактирование информации о пользователях, конференциях и докладах")
Rel(moderator, web_site, "Модерация контента и пользователей")
Rel(user, web_site, "Регистрация, просмотр информации о конференциях и докладах и запись на них")

Rel(web_site, user_service, "Работа с пользователями", "/user")
Rel(user_service, db, "INSERT/SELECT/UPDATE", "SQL")

Rel(web_site, report_service, "Работа с докладами", "/report")
Rel(report_service, db, "INSERT/SELECT/UPDATE", "SQL")

Rel(web_site, conference_service, "Работа с конференциями", "/conference")
Rel(conference_service, db, "INSERT/SELECT/UPDATE", "SQL")
@enduml
```

## Список компонентов  

### Сервис авторизации
**API**:
- Создание нового пользователя
  - входные параметры: электронная почта, имя, фамилия, отчество, права доступа
  - выходные параметры: идентификатор, электронная почта, имя, фамилия, отчество, права доступа
- Получение пользователя по идентификатору
  - входные параметры: идентификатор
  - выходные параметры: идентификатор, электронная почта, имя, фамилия, отчество, права доступа
- Получение пользователя по электронной почте
  - входные параметры: электронная почта
  - выходные параметры: идентификатор, электронная почта, имя, фамилия, отчество, права доступа
- Редактирование пользователя
  - входные параметры: идентификатор, электронная почта, имя, фамилия, права доступа
  - выходные параметры: идентификатор, электронная почта, имя, фамилия, права доступа
- Удаление пользователя
  - входные параметры: идентификатор
  - выходные параметры: отсутствуют

### Сервис докладов
**API**:
- Создание доклада
  - входные параметры: название, аннотация, текст, идентификатор пользователя
  - выходные параметры: идентификатор, название, аннотация, текст, дата создания, дата обновления, флаг модерации, идентификатор пользователя, идентификатор конференции
- Получение доклада по идентификатору
  - входные параметры: идентификатор
  - выходные параметры: идентификатор, название, аннотация, текст, дата создания, дата обновления, флаг модерации, идентификатор пользователя, идентификатор конференции
- Получение доклада по маске названия (поиск по регулярному выражению)
  - входные данные: регулярное выражение
  - выходные данные: список[идентификатор, название, аннотация, текст, дата создания, дата обновления, флаг модерации, идентификатор пользователя, идентификатор конференции]
- Редактирование доклада
  - входные параметры: идентификатор, название, аннотация, текст, флаг модерации, идентификатор пользователя, идентификатор конференции
  - выходные параметры: идентификатор, название, аннотация, текст, дата создания, дата обновления, флаг модерации, идентификатор пользователя, идентификатор конференции
- Удаление доклада
  - входные параметры: идентификатор
  - выходные параметры: отсутствуют

### Сервис конференций
**API**:
- Создание конференции
  - входные параметры: название, дата
  - выходные параметры: идентификатор, название, дата
- Получение конференции по идентификатору
  - входные данные: идентификатор
  - выходные данные: идентификатор, название, дата
- Получение всех конференций
  - входные данные: отсутствуют
  - выходные данные: список[идентификатор, название, дата]
- Редактирование конференции
  - входные параметры: идентификатор, название, дата
  - выходные параметры: идентификатор, название, дата
- Удаление конференции
  - входные параметры: идентификатор
  - выходные параметры: отсутствуют

### Модель данных
```puml
@startuml

class User {
  id
  email
  first_name
  last_name
  patronymic
  access_rights
}

class Report {
  id
  title
  annotation
  text
  creation_date
  update_date
  moderation_flag
  user_id
  conference_id
}

class Conference {
  id
  title
  date
}

User <- Report
Conference <- Report

@enduml
```
