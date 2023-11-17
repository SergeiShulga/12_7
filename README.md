#### Домашнее задание к занятию «Репликация и масштабирование. Часть 2» - "Сергей Шульга"

### Задание 1
Опишите основные преимущества использования масштабирования методами:

#### активный master-сервер и пассивный репликационный slave-сервер;
С холодным резервом или активный/пассивный (master\slave). Активный узел выполняет запросы, а пассивный ждет его отказа и включается в работу, когда таковой произойдет. Пример – резервные сетевые соединения, в частности, алгоритм связующего дерева. Например, связка DRBD и HeartBeat /Corosync. Хотя многие говорят, что это не про отказоустойчивость, но это самый распространенный вариант. Если у вас все правильно настроено, именно master\slave-конфигурация позволяет с минимальным простоем продолжить обслуживание клиентов.
Slave сервер постоянно копирует все изменения с Master. С приложения на Slave-сервер отправляются запросы чтения данных (запросы SELECT). Таким образом Master-сервер отвечает за изменения данных, а Slave за чтение.
При выходе из строя Slave, достаточно просто переключить все приложение на работу с Master. После этого восстановить репликацию на Slave и снова его запустить. Если выходит из строя Master, нужно переключить все операции (и чтения и записи) на Slave. Таким образом он станет новым Master. После восстановления старого Master, настроить на нем реплику, и он станет новым Slave.
- при использовании масштабирования методом активный master-сервер и пассивный репликационный slave-сервер плюсами является, что можно уменьшить нагрузку (разделив функционал запись\чтение), при определенной настройке (где slave становится master) повышается отказоустойчивость;

#### master-сервер и несколько slave-серверов;

- повышение производительности чтения данных. С помощью репликации мы сможем поддерживать несколько копий сервера, и распределять между ними нагрузку.
- повышение отказоустойчивости. Репликация позволяет избавиться от единственной точки отказа, которой является одиночный сервер БД. В случае аварии на основном сервере, есть возможность быстро переключить нагрузку на резервный.
- распределение нагрузки. В случае, если БД обслуживает запросы разных типов (быстрые и легкие, медленные и тяжелые), может иметь смысл развести эти запросы по разным серверам, для увеличения эффективности работы каждого типа.
- тестирование новых конфигураций. С помощью репликации есть возможность проведения тестирования новых версий сервера БД, изменения параметров конфигурации, и даже изменения типов хранилища данных.

#### активный сервер со специальным механизмом репликации — distributed replicated block device (DRBD);

DRBD (Distributed Replicated Block Device — распределённое реплицируемое блочное устройство) представляет собой распределенное, гибкое и универсально реплицируемое решение хранения данных. Оно отражает содержимое блочных устройств, таких как жесткие диски, разделы, логические тома и т.д. между серверами. Оно создает копии данных на двух устройствах хранения для того, чтобы в случае сбоя одного из них можно было использовать данные на втором. 
Можно сказать, что это нечто вроде сетевой конфигурации RAID 1 с дисками, отражаемыми на разные сервера. Первоначально DRBD использовалось главным образом в компьютерных кластерах высокой доступности (HA — high availability), однако, начиная с девятой версии, оно может использоваться для развертывания решений облачного хранилища.

- плюсами является отказоустойчивость, принципы RAID1, бесперебойная работа системы, быстрая ресинхронизация в случае падения одного из узлов, преимущества HA-кластера;

#### SAN-кластер.
- при использовании масштабирования методом с помощью SAN-кластера плюсом является масштабируемость, модернизация (простота миграции и уменьшение времени простоя).



### Задание 2
Разработайте план для выполнения горизонтального и вертикального шаринга базы данных. База данных состоит из трёх таблиц:
пользователи,

книги,

магазины (столбцы произвольно).

![alt text](https://github.com/SergeiShulga/12_7/blob/main/img/001.png)


Опишите принципы построения системы и их разграничение или разбивку между базами данных.

Пришлите блоксхему, где и что будет располагаться. Опишите, в каких режимах будут работать сервера.





Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

Задание 3*
Выполните настройку выбранных методов шардинга из задания 2.

Пришлите конфиг Docker и SQL скрипт с командами для базы данных.
