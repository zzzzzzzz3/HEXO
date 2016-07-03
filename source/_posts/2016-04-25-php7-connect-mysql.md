---
title: php7_connect_mysql
date: 2016-04-25 23:53:12
tags: php7
desc:
---
使用php7连接mysql时：
	
	$db_link = mysql_connect($db_hostname, $db_username, $db_password);
	
出现错误：

	Fatal error: Uncaught Error: Call to undefined function mysql_connect() 
	
<!-- more -->

查看[php官网针对MYSQL的扩展](http://php.net/manual/zh/mysqlinfo.api.choosing.php)
错误的原因是由于php7将"mysql.dll"移除，所以到php根目录下修改配置文件php.ini：

	extension_dir = "D:/Program/php7/ext"
	extension=php_curl.dll
	extension=php_gd2.dll
	extension=php_mbstring.dll 
	extension=php_mysqli.dll
	extension=php_pdo_mysql.dll
	extension=php_xmlrpc.dll
	
重启apache服务器。

使用mysqli的API连接数据库：

	$db_link = mysqli_connect($db_hostname, $db_username, $db_password, $db_database);
	
案例：

	<?php //mysqlcreate.php
		$db_hostname = 'localhost';
		$db_database = 'phpdb';
		$db_username = 'php';
		$db_password = 'zhang3';
		
		$db_link = mysqli_connect($db_hostname, $db_username, $db_password, $db_database);
		if($db_link) echo "success!";
		else echo "connect error";
		
		if(mysqli_connect_errno())echo "connect failed:" . mysqli_connect_error();
		
		$usernames = array(
			array('thepresident', 'iamprez', 'Barack', 'Obama'),
			array('elizabeth', 'oneisqueen', 'Elizabeth', 'Windsor'),
			array('superrich', 'windoz', 'Bill', 'Gates'),
			array('genius', 'e=mc2', 'Albert', 'Einstein' )
		);
		
		$query = "CREATE TABLE IF NOT EXISTS users(username VARCHAR(16),
				password CHAR(32),
				forename VARCHAR(32),
				surname VARCHAR(32),
				UNIQUE INDEX(username(6)),
				INDEX(forename(8)),
				INDEX(surname(8)))";
		
		if(mysqli_query($db_link,$query) === true) echo "creat success";
		else echo "\ncreat error:" . mysqli_error($db_link);
		
		for($i = 0; $i < 4; $i++){
			$u = $usernames[$i][0];
			$p = $usernames[$i][1];
			$f = $usernames[$i][2];
			$s = $usernames[$i][3];
			$query = "INSERT INTO users VALUES('$u', '$p', '$f', '$s')";
			
			mysqli_query($db_link,$query);
			
		}
		
		if($result = mysqli_query($db_link,"select * from users")){
			printf("Select returned %d rows.\n", mysqli_num_rows($result));
			mysqli_free_result($result);
		}
		
		
		mysqli_close($db_link);
	?>
	