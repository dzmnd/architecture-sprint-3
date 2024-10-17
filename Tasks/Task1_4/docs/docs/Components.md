```puml
@startuml

!includeurl https://raw.githubusercontent.com/RicardoNiepel/C4-PlantUML/master/C4_Component.puml

title Умный дом

top to bottom direction

Person(User, "User", "Пользователь умного дома")
Person(Engineer, "Engineer", "Инженер по настройке устройств")
System(SmartHomeSystem, "Управление и Мониторинг Устройств")

Container_Boundary(SmartHomeSystem, "Smart Home System") {
  Container_Boundary(WebApp, "Сайт/Мобильное приложение") {
      Component(WebSite, "Сайт", "React")
      Component(MobileApp, "Мобильное приложение", "Native React")
  }

  Component(APIGateway, "API Gateway", "API Request")

  Container_Boundary(ManageDevice, "Управление Устройствами") {
    Component(ManageDeviceController, "Контроллер", "Java")
    Component(ManageDeviceService, "Сервис", "Java")
    Component(ManageDeviceRepositoryLayer, "Слой доступа к данным", "Java")
    Component(ManageDeviceDatabase, "БД", "PostgreSQL")
  }

  Container_Boundary(MonitoringDevice, "Мониторинг Устройств") {
    Component(MonitoringDeviceController, "Контроллер", "Java")
    Component(MonitoringDeviceService, "Сервис", "Java")
    Component(MonitoringDeviceRepositoryLayer, "Слой доступа к данным", "Java")
    Component(MonitoringDeviceDatabase, "БД", "PostgreSQL")
  }

  Container_Boundary(Kafka, "Брокер сообщений", "Kafka") {
    Component(KafkaProducer, "Публикация сообщений", "Java")
    Component(KafkaConsumer, "Подписки на сообщения", "Java")
  }
}

System_Ext(Device, "Система отопления")

Rel(User, WebApp, "Действия пользователя", User)
Rel(Engineer, WebApp, "Действия инженера", Engineer)
Rel(WebApp, APIGateway, "Действия пользователя/Инженера", User)
Rel(ManageDevice, KafkaProducer, "Отправка сообщения", Push)
Rel(MonitoringDevice, KafkaProducer, "Отправка сообщения", Push)

Rel(ManageDeviceService, ManageDeviceRepositoryLayer, "")
Rel(MonitoringDeviceService, MonitoringDeviceRepositoryLayer, "")


Rel(ManageDeviceRepositoryLayer, ManageDeviceDatabase, "")
Rel(MonitoringDeviceRepositoryLayer, MonitoringDeviceDatabase, "")

Rel(APIGateway, ManageDevice, "Управление Устройствами")
Rel(ManageDeviceController, ManageDeviceService, "")
Rel(APIGateway, MonitoringDevice, "Мониторинг Устройств")
Rel(MonitoringDeviceController, MonitoringDeviceService, "")
Rel(Device, KafkaProducer, "Показания датчиков", Push)
Rel(ManageDevice, Device, "Настройка систем")
Rel(KafkaConsumer, MonitoringDevice, "Показания датчиков")

@enduml
``` 