#### Домашнее задание к занятию «Репликация и масштабирование. Часть 2» - "Сергей Шульга"

#### Задание 1
Опишите основные преимущества использования масштабирования методами:

Пользователи

master-сервер и несколько slave-серверов;

активный сервер со специальным механизмом репликации — distributed replicated block device (DRBD);

SAN-кластер.


#### Задание 2
Разработайте план для выполнения горизонтального и вертикального шаринга базы данных. База данных состоит из трёх таблиц:
пользователи,
книги,
магазины (столбцы произвольно).
Опишите принципы построения системы и их разграничение или разбивку между базами данных.

Пришлите блоксхему, где и что будет располагаться. Опишите, в каких режимах будут работать сервера.

#### Вертикальный шардинг — это выделение таблицы или группы таблиц на отдельный сервер. Например, в приложении есть такие таблицы:
пользователи — данные пользователей
магазины (столбцы произвольно) — фотографии пользователей
книги — пользователей
Таблицу пользователи оставляем на одном сервере, а таблицы магазины и книги переносите на другой. В таком случае в приложении Вам необходимо будет использовать соответствующее соединение для работы с каждой таблицей

#### Горизонтальный шардинг
Горизонтальный шардинг — это разделение одной таблицы на разные сервера. Это необходимо использовать для огромных таблиц, которые не умещаются на одном сервере. Разделение таблицы на куски делается по такому принципу:

На нескольких серверах создается одна и та же таблица (только структура, без данных).
В приложении выбирается условие, по которому будет определяться нужное соединение (например, четные на один сервер, а нечетные — на другой).
Перед каждым обращением к таблице происходит выбор нужного соединения.
Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

Задание 3*
Выполните настройку выбранных методов шардинга из задания 2.

Пришлите конфиг Docker и SQL скрипт с командами для базы данных.
