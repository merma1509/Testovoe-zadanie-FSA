# BPMN Process Model - BPMN Code Block

## 📊 BPMN Diagram in Code Block

```bpmn
<?xml version="1.0" encoding="UTF-8"?>
<bpmn:definitions xmlns:bpmn="http://www.omg.org/spec/BPMN/20100524/MODEL" xmlns:bpmndi="http://www.omg.org/spec/BPMN/20100524/DI" xmlns:dc="http://www.omg.org/spec/DD/20100524/DC" xmlns:di="http://www.omg.org/spec/DD/20100524/DI" id="Definitions_1" targetNamespace="http://bpmn.io/schema/bpmn">
  <bpmn:collaboration id="Collaboration_1">
    <bpmn:participant id="Participant_CompanyX" name="Компания X" processRef="Process_CompanyX" />
    <bpmn:participant id="Participant_Supplier" name="Поставщик" processRef="Process_Supplier" />
    <bpmn:messageFlow id="MessageFlow_1" sourceRef="Task_OrderToSupplier" targetRef="Event_ReceiveOrder" />
    <bpmn:messageFlow id="MessageFlow_2" sourceRef="Event_DeliverEquipment" targetRef="Task_ReceiveFromSupplier" />
  </bpmn:collaboration>
  
  <bpmn:process id="Process_CompanyX" name="Процесс выдачи IT-оборудования" isExecutable="false">
    <bpmn:laneSet id="LaneSet_1">
      <bpmn:lane id="Lane_User" name="Пользователь">
        <bpmn:flowNodeRef>StartEvent_UserRequest</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_ReceiveEquipment</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_RateService</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>EndEvent_ProcessComplete</bpmn:flowNodeRef>
      </bpmn:lane>
      <bpmn:lane id="Lane_Support" name="1 линия техподдержки">
        <bpmn:flowNodeRef>Task_ReceiveUserRequest</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_CreateRequestSystem2</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_IssueToUser</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_CloseRequestSystem1</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_CloseRequestSystem2</bpmn:flowNodeRef>
      </bpmn:lane>
      <bpmn:lane id="Lane_Supply" name="Отдел снабжения">
        <bpmn:flowNodeRef>Task_AcceptRequestSystem2</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_CheckAvailabilitySystem3</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Gateway_EquipmentAvailable</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_CallLogistics</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_OrderToSupplier</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Event_WaitForSupplier</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_ReceiveFromSupplier</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_AcceptEquipmentSystem3</bpmn:flowNodeRef>
      </bpmn:lane>
      <bpmn:lane id="Lane_Logistics" name="Логистика">
        <bpmn:flowNodeRef>Task_DeliverToSupport</bpmn:flowNodeRef>
      </bpmn:lane>
    </bpmn:laneSet>

    <bpmn:startEvent id="StartEvent_UserRequest" name="Пользователю нужно оборудование">
      <bpmn:outgoing>Flow_1</bpmn:outgoing>
    </bpmn:startEvent>

    <bpmn:endEvent id="EndEvent_ProcessComplete" name="Процесс завершен">
      <bpmn:incoming>Flow_15</bpmn:incoming>
    </bpmn:endEvent>

    <bpmn:task id="Task_ReceiveUserRequest" name="Принять заявку от пользователя">
      <bpmn:incoming>Flow_1</bpmn:incoming>
      <bpmn:outgoing>Flow_2</bpmn:outgoing>
    </bpmn:task>

    <bpmn:task id="Task_CreateRequestSystem2" name="Создать заявку в системе №2">
      <bpmn:incoming>Flow_2</bpmn:incoming>
      <bpmn:outgoing>Flow_3</bpmn:outgoing>
    </bpmn:task>

    <bpmn:task id="Task_AcceptRequestSystem2" name="Принять заявку в системе №2">
      <bpmn:incoming>Flow_3</bpmn:incoming>
      <bpmn:outgoing>Flow_4</bpmn:outgoing>
    </bpmn:task>

    <bpmn:task id="Task_CheckAvailabilitySystem3" name="Проверить наличие в системе №3">
      <bpmn:incoming>Flow_4</bpmn:incoming>
      <bpmn:outgoing>Flow_5</bpmn:outgoing>
    </bpmn:task>

    <bpmn:exclusiveGateway id="Gateway_EquipmentAvailable" name="Оборудование доступно?">
      <bpmn:incoming>Flow_5</bpmn:incoming>
      <bpmn:outgoing>Flow_6</bpmn:outgoing>
      <bpmn:outgoing>Flow_7</bpmn:outgoing>
    </bpmn:exclusiveGateway>

    <bpmn:task id="Task_CallLogistics" name="Вызвать логистику">
      <bpmn:incoming>Flow_6</bpmn:incoming>
      <bpmn:outgoing>Flow_8</bpmn:outgoing>
    </bpmn:task>

    <bpmn:task id="Task_OrderToSupplier" name="Сделать заявку поставщику">
      <bpmn:incoming>Flow_7</bpmn:incoming>
      <bpmn:outgoing>Flow_9</bpmn:outgoing>
    </bpmn:task>

    <bpmn:intermediateCatchEvent id="Event_WaitForSupplier" name="Ожидание поставки">
      <bpmn:incoming>Flow_9</bpmn:incoming>
      <bpmn:outgoing>Flow_10</bpmn:outgoing>
    </bpmn:intermediateCatchEvent>

    <bpmn:task id="Task_ReceiveFromSupplier" name="Получить оборудование от поставщика">
      <bpmn:incoming>Flow_10</bpmn:incoming>
      <bpmn:outgoing>Flow_11</bpmn:outgoing>
    </bpmn:task>

    <bpmn:task id="Task_AcceptEquipmentSystem3" name="Провести приемку в системе №3">
      <bpmn:incoming>Flow_11</bpmn:incoming>
      <bpmn:outgoing>Flow_12</bpmn:outgoing>
    </bpmn:task>

    <bpmn:task id="Task_DeliverToSupport" name="Доставить оборудование техподдержке">
      <bpmn:incoming>Flow_8</bpmn:incoming>
      <bpmn:incoming>Flow_12</bpmn:incoming>
      <bpmn:outgoing>Flow_13</bpmn:outgoing>
    </bpmn:task>

    <bpmn:task id="Task_IssueToUser" name="Выдать оборудование пользователю">
      <bpmn:incoming>Flow_13</bpmn:incoming>
      <bpmn:outgoing>Flow_14</bpmn:outgoing>
    </bpmn:task>

    <bpmn:task id="Task_CloseRequestSystem1" name="Закрыть заявку в системе №1">
      <bpmn:incoming>Flow_14</bpmn:incoming>
      <bpmn:outgoing>Flow_16</bpmn:outgoing>
    </bpmn:task>

    <bpmn:task id="Task_CloseRequestSystem2" name="Закрыть заявку в системе №2">
      <bpmn:incoming>Flow_16</bpmn:incoming>
      <bpmn:outgoing>Flow_17</bpmn:outgoing>
    </bpmn:task>

    <bpmn:task id="Task_ReceiveEquipment" name="Получить оборудование">
      <bpmn:incoming>Flow_17</bpmn:incoming>
      <bpmn:outgoing>Flow_15</bpmn:outgoing>
    </bpmn:task>

    <bpmn:task id="Task_RateService" name="Оценить сервис техподдержки">
      <bpmn:incoming>Flow_15</bpmn:incoming>
      <bpmn:outgoing>Flow_18</bpmn:outgoing>
    </bpmn:task>

    <bpmn:sequenceFlow id="Flow_1" sourceRef="StartEvent_UserRequest" targetRef="Task_ReceiveUserRequest" />
    <bpmn:sequenceFlow id="Flow_2" sourceRef="Task_ReceiveUserRequest" targetRef="Task_CreateRequestSystem2" />
    <bpmn:sequenceFlow id="Flow_3" sourceRef="Task_CreateRequestSystem2" targetRef="Task_AcceptRequestSystem2" />
    <bpmn:sequenceFlow id="Flow_4" sourceRef="Task_AcceptRequestSystem2" targetRef="Task_CheckAvailabilitySystem3" />
    <bpmn:sequenceFlow id="Flow_5" sourceRef="Task_CheckAvailabilitySystem3" targetRef="Gateway_EquipmentAvailable" />
    <bpmn:sequenceFlow id="Flow_6" name="Да" sourceRef="Gateway_EquipmentAvailable" targetRef="Task_CallLogistics" />
    <bpmn:sequenceFlow id="Flow_7" name="Нет" sourceRef="Gateway_EquipmentAvailable" targetRef="Task_OrderToSupplier" />
    <bpmn:sequenceFlow id="Flow_8" sourceRef="Task_CallLogistics" targetRef="Task_DeliverToSupport" />
    <bpmn:sequenceFlow id="Flow_9" sourceRef="Task_OrderToSupplier" targetRef="Event_WaitForSupplier" />
    <bpmn:sequenceFlow id="Flow_10" sourceRef="Event_WaitForSupplier" targetRef="Task_ReceiveFromSupplier" />
    <bpmn:sequenceFlow id="Flow_11" sourceRef="Task_ReceiveFromSupplier" targetRef="Task_AcceptEquipmentSystem3" />
    <bpmn:sequenceFlow id="Flow_12" sourceRef="Task_AcceptEquipmentSystem3" targetRef="Task_DeliverToSupport" />
    <bpmn:sequenceFlow id="Flow_13" sourceRef="Task_DeliverToSupport" targetRef="Task_IssueToUser" />
    <bpmn:sequenceFlow id="Flow_14" sourceRef="Task_IssueToUser" targetRef="Task_CloseRequestSystem1" />
    <bpmn:sequenceFlow id="Flow_16" sourceRef="Task_CloseRequestSystem1" targetRef="Task_CloseRequestSystem2" />
    <bpmn:sequenceFlow id="Flow_17" sourceRef="Task_CloseRequestSystem2" targetRef="Task_ReceiveEquipment" />
    <bpmn:sequenceFlow id="Flow_15" sourceRef="Task_ReceiveEquipment" targetRef="Task_RateService" />
    <bpmn:sequenceFlow id="Flow_18" sourceRef="Task_RateService" targetRef="EndEvent_ProcessComplete" />
  </bpmn:process>

  <bpmn:process id="Process_Supplier" name="Процесс поставщика" isExecutable="false">
    <bpmn:laneSet id="LaneSet_2">
      <bpmn:lane id="Lane_Supplier_Process" name="Поставщик">
        <bpmn:flowNodeRef>Event_ReceiveOrder</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_ProcessOrder</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Event_DeliverEquipment</bpmn:flowNodeRef>
      </bpmn:lane>
    </bpmn:laneSet>

    <bpmn:startEvent id="Event_ReceiveOrder" name="Получить заказ">
      <bpmn:outgoing>Flow_S1</bpmn:outgoing>
    </bpmn:startEvent>

    <bpmn:task id="Task_ProcessOrder" name="Обработать заказ">
      <bpmn:incoming>Flow_S1</bpmn:incoming>
      <bpmn:outgoing>Flow_S2</bpmn:outgoing>
    </bpmn:task>

    <bpmn:endEvent id="Event_DeliverEquipment" name="Доставить оборудование">
      <bpmn:incoming>Flow_S2</bpmn:incoming>
    </bpmn:endEvent>

    <bpmn:sequenceFlow id="Flow_S1" sourceRef="Event_ReceiveOrder" targetRef="Task_ProcessOrder" />
    <bpmn:sequenceFlow id="Flow_S2" sourceRef="Task_ProcessOrder" targetRef="Event_DeliverEquipment" />
  </bpmn:process>
</bpmn:definitions>
```

## 📋 Process Elements Breakdown

### Pools and Participants
- **Компания X** - Основной процесс с 4 дорожками
- **Поставщик** - Внешний участник процесса

### Lanes (Дорожки)
1. **Пользователь** - Инициирует и завершает процесс
2. **1 линия техподдержки** - Обработка заявок и выдача оборудования
3. **Отдел снабжения** - Проверка наличия и заказ поставщику
4. **Логистика** - Доставка оборудования

### Key Elements
- **Start Event**: "Пользователю нужно оборудование"
- **End Event**: "Процесс завершен"
- **Exclusive Gateway**: "Оборудование доступно?" (Да/Нет)
- **Intermediate Event**: "Ожидание поставки"
- **Message Flows**: Взаимодействие с поставщиком

---

*This BPMN code block approach ensures the XML is properly formatted and can be easily copied or viewed in any IDE or text editor.*
