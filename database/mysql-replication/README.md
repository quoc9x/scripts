# Introduction
```
docker compose up -d
```
## Config on master
### Exec to master and login mysql
```
docker exec -it mysql-master bash
```
```
mysql -u root -p
```
### Create user
```
CREATE USER 'slave_user'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
```
### Grant permission for user
```
GRANT REPLICATION SLAVE ON *.* TO 'slave_user'@'%';
```
### FLUSH
```
FLUSH PRIVILEGES; 
```
### LOCK TABLE and recore FILE AND POS (For Case Master Doesn’t Have Any Existing Data to Migrate)
```
FLUSH TABLES WITH READ LOCK;
```
```
SHOW MASTER STATUS;
```
```
UNLOCK TABLES;
```
```
CREATE DATABASE db;
```
```
exit
```
## Config on slave
### Exec to slave and login mysql
```
docker exec -it mysql-slave bash
```
```
mysql -u root -p
```
### START
`SOURCE_LOG_FILE` - Replace by value first step     
`SOURCE_LOG_POS` - Replace by value first step     
```
CHANGE REPLICATION SOURCE TO
SOURCE_HOST='mysql-master',
SOURCE_USER='slave_user',
SOURCE_PASSWORD='123456',
SOURCE_LOG_FILE='mysql-bin.000003',
SOURCE_LOG_POS=839;
```
```
START REPLICA;
```
```
SHOW REPLICA STATUS\G;
```
## TEST
### On master (Write data)
```
mysql -u root -p
```
```
USE db;
```
```
CREATE TABLE example_table (example_column varchar(30));
```
```
INSERT INTO example_table VALUES
('This is the first row'),
('This is the second row'),
('This is the third row');
```
### On slave (Check data that sync with master)
```
mysql -u root -p
```
```
USE db;
```
```
SHOW TABLES;
```
```
SELECT * FROM example_table;
```
