# install-wordpress

# Step one

下载安装包

wget http://wordpress.org/latest.tar.gz

tar -xzvf latest.tar.gz

# Step two 

创建数据库

可能涉及到数据库ROOT密码修改

sudo /usr/bin/mysql_secure_installation

mysql> SET PASSWORD=PASSWORD('Root1234@');

Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> FLUSH PRIVILEGES;

Query OK, 0 rows affected (0.00 sec)

新的修改方式:

mysql> SET password='Root1234@';

或者可以这样写

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';

消除弱密码限制

mysql -u root -p

如果密码设置太简单，MySQL也会报错：ERROR 1819 (HY000): Your password does not satisfy the current policy requirements。因为MySQL5.7默认安装了validate_password插件

mysql> SHOW VARIABLES LIKE 'vali%';

+--------------------------------------+--------+

| Variable_name                        | Value  |

+--------------------------------------+--------+

| validate_password_dictionary_file    |        |

| validate_password_length             | 8      |

| validate_password_mixed_case_count   | 1      |

| validate_password_number_count       | 1      |

| validate_password_policy             | MEDIUM |

| validate_password_special_char_count | 1      |

+--------------------------------------+--------+

6 rows in set (0.00 sec)

修改：set global validate_password_length = 4;

。

。

。

想要关闭这个插件，则在配置文件中加入以下并重启mysqld即可：

validate_password=off

重启mysqld

mysql> CREATE DATABASE wordpress;

Query OK, 1 row affected (0.00 sec)

mysql> CREATE USER user@localhost;

Query OK, 0 rows affected (0.00 sec)

mysql> SET PASSWORD FOR user@localhost= PASSWORD("password");

Query OK, 0 rows affected (0.00 sec)

mysql> GRANT ALL PRIVILEGES ON wordpress.* TO user@localhost IDENTIFIED BY 'password';

Query OK, 0 rows affected (0.00 sec)

mysql> FLUSH PRIVILEGES;

Query OK, 0 rows affected (0.00 sec)

mysql> exit

# Step three set wordpress

cp ~/wordpress/wp-config-sample.php ~/wordpress/wp-config.php

vi ~/wordpress/wp-config.php

修改文件：

// ** MySQL settings - You can get this info from your web host ** //

/** The name of the database for WordPress */

define('DB_NAME', 'wordpress');

/** MySQL database username */

define('DB_USER', 'user');

/** MySQL database password */

define('DB_PASSWORD', 'password');

/** MySQL hostname */

# Step four Transfer Wordpres files to /var/www/html

cp -r ~/wordpress/* /var/www/html

service httpd restart

# Step five 检查

127.0.0.1/wp-admin/install.php
