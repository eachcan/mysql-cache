mysql-cache
===========

This is a plugin for mysql.

Mysql memery storage engine supports caching data in memory by table, and query cache stores data by sql statement. Mysql-Cache plugin stores data in memory by more complex conditions.

Usage
------------
Install plugin

mysql> INSTALL PLUGIN mysql-cache "mysql-cache.so"

Uninstall plugin

mysql> UNINSTALL PLUGIN mysql-cache

Create cache mode

The following is an example from an normal type game, the game contains users, properties, families and other assocating data. The data structure looks like:

<code>
Create Table `user` (
  user_id int not null primary key auto _increment,
  nickname varchar(20) not null,
  login_name varchar(20) not null,
  login_password varchar(32) not null,
  ...
) Engine = MyIsam;

Create table property (
  user_id int not null,
  prop_id int not null,
  ...
) Engine = InnoDb;

Create table family ( 
  family_id int not null primary key auto increment,
  family_name varchar(10) not null,
  ...
) Engine = memory;

Create table family_user (
  family_id int not null,
  user_id int not null) Engine = InnoDb;
</code>

Our purpose is caching all information about the user when an user login, so that:
mysql> create cache login_user(user _id)
mysql> BEGIN
mysql> cache SELECT * from user where user_id = user _id;
mysql> cache SELECT * FROM property WHERE user_id = user _id;
mysql> cache SELECT family_id FROM family _user where user _id = user _id;
mysql> END;

When an user logins the game, we call this cache-process.

mysql> call cache login_user(123);

and when the user  logout this game, we delete the data:

mysql> call deletecache login_user(123);

we can delete all the caches;

mysql> call cleancache where name="login_user";

or

mysql> call cleancache;
