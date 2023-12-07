# Контекст решения

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

Person(admin, "Администратор")
Person(moderator, "Модератор")
Person(user, "Пользователь")

System(conference_site, "Сайт конференций", "Веб-сайт для конференций")

Rel(admin, conference_site, "Просмотр, добавление и изменение пользователей, докладов и конференций")
Rel(moderator, conference_site, "Модерация докладов и включение их в конференции")
Rel(user, conference_site, "Регистрация в системе, добавление, просмотр и изменение докладов")
@enduml
```

## Назначение систем
|Система| Описание|
|-------|---------|
| Сайт конференций | Веб-интерфейс, обеспечивающий доступ к конференциям и докладам к ним. Бэкенд сервиса реализован в виде микросервисной архитектуры. |
