Enter password: ********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 20
Server version: 8.0.33 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> CREATE DATABASE MoviesDB;
Query OK, 1 row affected (0.01 sec)

mysql> USE MoviesDB;
Database changed
mysql> CREATE TABLE Movies (
    ->   ID INT PRIMARY KEY AUTO_INCREMENT,
    ->   Title VARCHAR(255),
    ->   Runtime INT,
    ->   Genre VARCHAR(255),
    ->   IMDBScore DECIMAL(3,1),
    ->   Rating VARCHAR(10)
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql> INSERT INTO Movies (Title, Runtime, Genre, IMDBScore, Rating)
    -> VALUES
    ->   ('Howard the Duck', 110, 'Sci-Fi', 4.6, 'PG'),
    ->   ('Lavalantula', 83, 'Horror', 4.7, 'TV-14'),
    ->   ('Starship Troopers', 129, 'Sci-Fi', 7.2, 'PG-13'),
    ->   ('Waltz With Bashir', 90, 'Documentary', 8.0, 'R'),
    ->   ('Spaceballs', 96, 'Comedy', 7.1, 'PG'),
    ->   ('Monsters Inc.', 92, 'Animation', 8.1, 'G');
Query OK, 6 rows affected (0.00 sec)
Records: 6  Duplicates: 0  Warnings: 0

mysql> INSERT INTO Movies (Title, Runtime, Genre, IMDBScore, Rating)
    -> VALUES
    ->   ('The Incredibles', 115, 'Animation', 8.0, 'PG'),
    ->   ('Jurassic Park', 127, 'Sci-Fi', 8.1, 'PG-13');
Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM Movies;
+----+-------------------+---------+-------------+-----------+--------+
| ID | Title             | Runtime | Genre       | IMDBScore | Rating |
+----+-------------------+---------+-------------+-----------+--------+
|  1 | Howard the Duck   |     110 | Sci-Fi      |       4.6 | PG     |
|  2 | Lavalantula       |      83 | Horror      |       4.7 | TV-14  |
|  3 | Starship Troopers |     129 | Sci-Fi      |       7.2 | PG-13  |
|  4 | Waltz With Bashir |      90 | Documentary |       8.0 | R      |
|  5 | Spaceballs        |      96 | Comedy      |       7.1 | PG     |
|  6 | Monsters Inc.     |      92 | Animation   |       8.1 | G      |
|  7 | The Incredibles   |     115 | Animation   |       8.0 | PG     |
|  8 | Jurassic Park     |     127 | Sci-Fi      |       8.1 | PG-13  |
+----+-------------------+---------+-------------+-----------+--------+
8 rows in set (0.00 sec)

mysql> SELECT *
    -> FROM Movies
    -> WHERE (Rating = 'G' OR Rating = 'PG') AND Runtime < 100;
+----+---------------+---------+-----------+-----------+--------+
| ID | Title         | Runtime | Genre     | IMDBScore | Rating |
+----+---------------+---------+-----------+-----------+--------+
|  5 | Spaceballs    |      96 | Comedy    |       7.1 | PG     |
|  6 | Monsters Inc. |      92 | Animation |       8.1 | G      |
+----+---------------+---------+-----------+-----------+--------+
2 rows in set (0.00 sec)

mysql> SELECT Genre, AVG(Runtime) AS AverageRuntime
    -> FROM Movies
    -> WHERE IMDBScore < 7.5
    -> GROUP BY Genre;
+--------+----------------+
| Genre  | AverageRuntime |
+--------+----------------+
| Sci-Fi |       119.5000 |
| Horror |        83.0000 |
| Comedy |        96.0000 |
+--------+----------------+
3 rows in set (0.00 sec)

mysql> UPDATE Movies
    -> SET Rating = 'R'
    -> WHERE Title = 'Starship Troopers';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql>  SELECT * FROM Movies;
+----+-------------------+---------+-------------+-----------+--------+
| ID | Title             | Runtime | Genre       | IMDBScore | Rating |
+----+-------------------+---------+-------------+-----------+--------+
|  1 | Howard the Duck   |     110 | Sci-Fi      |       4.6 | PG     |
|  2 | Lavalantula       |      83 | Horror      |       4.7 | TV-14  |
|  3 | Starship Troopers |     129 | Sci-Fi      |       7.2 | R      |
|  4 | Waltz With Bashir |      90 | Documentary |       8.0 | R      |
|  5 | Spaceballs        |      96 | Comedy      |       7.1 | PG     |
|  6 | Monsters Inc.     |      92 | Animation   |       8.1 | G      |
|  7 | The Incredibles   |     115 | Animation   |       8.0 | PG     |
|  8 | Jurassic Park     |     127 | Sci-Fi      |       8.1 | PG-13  |
+----+-------------------+---------+-------------+-----------+--------+
8 rows in set (0.00 sec)

mysql> UPDATE Movies
    -> SET Rating = 'R'
    -> WHERE Title = 'Starship Troopers';
Query OK, 0 rows affected (0.00 sec)
Rows matched: 1  Changed: 0  Warnings: 0

mysql> SELECT ID, Rating
    -> FROM Movies
    -> WHERE Genre IN ('Horror', 'Documentary');
+----+--------+
| ID | Rating |
+----+--------+
|  2 | TV-14  |
|  4 | R      |
+----+--------+
2 rows in set (0.00 sec)

mysql> SELECT Rating, AVG(IMDBScore) AS AverageScore, MAX(IMDBScore) AS MaxScore, MIN(IMDBScore) AS MinScore
    -> FROM Movies
    -> GROUP BY Rating;
+--------+--------------+----------+----------+
| Rating | AverageScore | MaxScore | MinScore |
+--------+--------------+----------+----------+
| PG     |      6.56667 |      8.0 |      4.6 |
| TV-14  |      4.70000 |      4.7 |      4.7 |
| R      |      7.60000 |      8.0 |      7.2 |
| G      |      8.10000 |      8.1 |      8.1 |
| PG-13  |      8.10000 |      8.1 |      8.1 |
+--------+--------------+----------+----------+
5 rows in set (0.00 sec)

mysql> df
    -> SELECT Rating, AVG(IMDBScore) AS AverageScore, MAX(IMDBScore) AS MaxScore, MIN(IMDBScore) AS MinScore
    -> FROM Movies
    -> GROUP BY Rating
    -> HAVING COUNT(*) > 1;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'df
SELECT Rating, AVG(IMDBScore) AS AverageScore, MAX(IMDBScore) AS MaxScore, MI' at line 1
mysql> SELECT Rating, AVG(IMDBScore) AS AverageScore, MAX(IMDBScore) AS MaxScore, MIN(IMDBScore) AS MinScore
    -> FROM Movies
    -> GROUP BY Rating
    -> HAVING COUNT(*) > 1;
+--------+--------------+----------+----------+
| Rating | AverageScore | MaxScore | MinScore |
+--------+--------------+----------+----------+
| PG     |      6.56667 |      8.0 |      4.6 |
| R      |      7.60000 |      8.0 |      7.2 |
+--------+--------------+----------+----------+
2 rows in set (0.00 sec)

mysql> DELETE FROM Movies
    -> WHERE Rating = 'R';
Query OK, 2 rows affected (0.00 sec)

mysql> Mohammeda amine Ztait
