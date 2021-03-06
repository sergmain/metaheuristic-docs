---
layout: default
---

# Концепция, архитектура, планы развития

## Содержание

- [На главную](/p-ru/index)

## Концепция
Метаэвристика это приложение, которое является (будет являться) полной Тюринг машиной. 
Главное предназначение Метаэвристики это управление созданием и выполнением распределенных задач. 
В настоящее время Метаэвристика предоставляет 2 бизнес-сценария использования для организации 
конвейерной обработки информации:
 - Оптимизация гипер-параметров для нейронных сетей. \
   Каждая оптимизация представлена в виде Эксперимента. Эксперимент состоит их задач. 
   Задачи создаются Диспетчером и передаются на Процессоры для выполнения. 
   Для оценки моделей собираются метрики, 
   которые позднее могут быть проанализированы с использованием Метаэвристики.
 
 - Пакетная обработка. \
   Сценарии пакетной обработки включают в себя - распределенная обработка документов, расчет и агрегация статистики. 


## Архитектура
Метаэвристика состоит из 2-ух главных модулей - Диспетчер и Процессор.
Диспетчер предназначен для управления задачами, которые будут обрабатываться Процессорами.
Процессор это модуль, который отвечает за получение задачи от Диспетчера, подготовку данных, 
запуск на выполнение задачи, и отправку результатов обработки Диспетчеру.  


Типичной конфигурацией Метаэвристики является один Диспетчер и N Процессоров. Каждый Процессор может быть 
сконфигурирован для получения задач от неограниченного количества Диспетчеров. 


Сущностью, которая конфигурится на Диспетчере и выполняется на Процессоре, является Функция.
Функцией может быть любое системное приложение, python notebook, java файл 
или любой другой программный код, который может быть запущен из командной строки. 
Инстансем Функции является Задача. \
Полное описание Функций находится здесь - [documentation on Functions](/p/function)  

С точки зрения реализации доставки Функций на Процессоры, 
Функция может быть получена как с Диспетчера, так и из git репозитория. 


Описание конвейера (pipeline) реализовано в виде сущности Исходный код. Исходный код это yaml файл, 
который описывает последовательность вызовов Функций. Для отслеживания зависимостей Задач друг от друга используется
динамический ориентированный ациклический граф (DAG). 

Функции могут иметь неограниченное количество, как входных переменных, так и выходных. 
Наиболее простой вариант для оптимизации гипер-параметров включает в себя такие переменные, как датасет, модель, метрики.
Полное описание Переменных находится здесь - [documentation on Variables](/p/variable)

