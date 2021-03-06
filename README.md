# Homework-PostgreSQL
![music servise_02 drawio](https://user-images.githubusercontent.com/95634307/174990112-2c9a42e1-d426-4cdf-aea0-aaf247b63e8e.png)

# Домашнее задание к лекции «Работа с SQL. Создание БД»

## Задание

### Обязательная часть

Будем развивать схему для музыкального сервиса.

Ранее было ограничение, что каждый исполнитель поет строго в одном жанре - пора убрать это ограничение. Исполнители могут петь в разных жанрах, как и одному жанру могут принадлежать несколько исполнителей.

Аналогичное ограничение было и с альбомами у исполнителей (альбом мог выпустить только один исполнитель). Теперь альбом могут выпустить несколько исполнителей вместе. Как и исполнитель может принимать участие во множестве альбомов.

С треками ничего не меняем, все так же трек принадлежит строго одному альбому.

Но появилась новая сущность - сборник. Сборник имеет название и год выпуска. В него входят различные треки из разных альбомов.

_Обратите внимание_: один и тот же трек может присутствовать в разных сборниках.

Задание состоит из двух частей:

1. Спроектировать и нарисовать схему (как в [первой домашней работе](../01-introduction)). Прислать изображение со схемой.
2. Написать SQL-запросы, создающие спроектированную БД. Прислать ссылку на файл, содержащий SQL-запросы.

_Примечание:_ можно прислать сначала схему, получить подтверждение, что схема верная и после этого браться за написание запросов.

```sql
---жанр-
create table if not exists Genres (
	id SERIAL primary key,
	name VARCHAR (60) unique not null
);

--Артист
create table if not exists Artists(
	id SERIAL primary key,
	name VARCHAR (80) unique not null
);

--Альбом
create table if not exists Albums(
	id SERIAL primary key,
	name VARCHAR (80) not null,
	year DATE 
);

--Треки
create table if not exists Tracks(
	id SERIAL primary key,
	name VARCHAR (80),
	duration numeric (3,2),
	album_id INTEGER not null references Albums(id)
);

--Коллекции(Сборники)
create table if not exists Collections(
	id SERIAL primary key,
	name VARCHAR (80),
	year DATE
);

create table if not exists Genre_Artists(
	genres_id INTEGER not null references Genres(id),
	artists_id INTEGER not null references Artists(id),
	constraint pk1 primary key (genres_id,artists_id)
);

create table if not exists Album_Artists(
	artists_id INTEGER not null references Artists(id),
	albums_id INTEGER not null references Albums(id),
	constraint pk2 primary key (artists_id,albums_id)
);

create table if not exists Collections_Tracks(
	collections_id INTEGER not null references Collections (id),
	tracks_id INTEGER not null references Tracks(id),
	constraint pk3 primary key (collections_id, tracks_id)
);

```


