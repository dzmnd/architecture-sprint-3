```puml
@startuml

!includeurl https://raw.githubusercontent.com/RicardoNiepel/C4-PlantUML/master/C4_Component.puml

title Умный дом

top to bottom direction

Person(User, "User", "Пользователь умного дома")
Person(Engineer, "Engineer", "Инжинер по настройке устройств")
System(SmartHomeSystem, "Управление и Мониторинг Устройств")

Container_Boundary(SmartHomeSystem, "Smart Home System") {
  Component(APIGateway, "API Gateway", "API Request")

  Container_Boundary(ManageDevice, "Управление Устройствами") {
    Component(ManageDeviceService, "Управление Устройствами", "Java")
    Component(ManageDeviceDatabase, "ManageDeviceDatabase", "PostgreSQL")
  }

  Container_Boundary(MonitoringDevice, "Мониторинг Устройств") {
    Component(MonitoringDeviceService, "Мониторинг Устройств", "Java")
    Component(MonitoringDeviceDatabase, "MonitoringDeviceDatabase", "PostgreSQL")
  }

  Container_Boundary(Kafka, "Брокер сообщений", "Kafka") {
    Component(KafkaProducer, "Публикация сообщений", "Java")
    Component(KafkaConsumer, "Подписки на сообщения", "Java")
  }
}

System_Ext(Device, "Система отопления")

Rel(User, APIGateway, "Действия пользователя", User)
Rel(Engineer, APIGateway, "Действия инжинера", Engineer)
Rel(ManageDevice, KafkaProducer, "Отправка сообщения", Push)
Rel(MonitoringDevice, KafkaProducer, "Отправка сообщения", Push)
Rel(ManageDeviceService, ManageDeviceDatabase, "Чтение/Запись")
Rel(MonitoringDeviceService, MonitoringDeviceDatabase, "Чтение/Запись")
Rel(APIGateway, ManageDeviceService, "Управление Устройствами")
Rel(APIGateway, MonitoringDeviceService, "Мониторинг Устройств")
Rel(Device, KafkaProducer, "Показания датчиков", Push)
Rel(ManageDevice, Device, "Настройка систем")
Rel(KafkaConsumer, MonitoringDeviceService, "Показания датчиков")

@enduml
``` 