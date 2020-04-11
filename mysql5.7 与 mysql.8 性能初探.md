# mysql5.7 与 mysql.8 性能初探

话不多说直接安装mysql8.0, 环境是liunx centos7.5

step 1 老规矩更新 yum 

	yum update

step 2 修改安装源 vim  /etc/yum.repos.d/mysql-community.repo
	
	[mysql80-community]

	name=MySQL 8.0 Community Server

	baseurl=http://repo.mysql.com/yum/mysql-8.0-community/el/7/$basearch/

	enabled=1

	gpgcheck=1

	gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

step 3 安装mysql8
	
	yum install -y mysql-community-server

step 4 启动mysql

	systemctl start mysqld.service      //启动MYSQL

	systemctl enable mysqld.service    //设置开机启动

	systemctl daemon-reload         //重新加载

	systemctl status mysqld.service     //查看MYSQL状态

step 5 登录mysql
	
	安装完会生成一个默认密码，要进行更改密码 否则也用不了 执行下面命令拿到密码 在显示的最后面

	grep 'temporary password' /var/log/mysqld.log

	修改密码 密码强度默认为中等，大小写字母、数字、特殊符号都要包含

	ALTER USER 'root'@'localhost' IDENTIFIED BY 'newPSD2019@163.com';

	systemctl restart mysqld  #重启服务

	mysql -u root -p newpwd

step 6 性能测试 [left join where] 如何测试？order_0表约2.3万； order_detail_0 约23万

	sql1 = SELECT o1.userId, o1.id , o1.`name` , o1.orderId , od1.total , od1.price FROM order_0 AS o1 LEFT JOIN order_detail_0 AS od1 ON o1.id = od1.orderId where o1.userId < 10000;


	sql2 = SELECT o1.userId, o1.id , o1.`name` , o1.orderId , od1.total , od1.price FROM order_0 AS o1 LEFT JOIN order_detail_0 AS od1 ON o1.id = od1.orderId where o1.userId < 10000;

	8.0版本上 第一次测试 [ userId 不加索引及详情表orderId也不加索引 ] 实际耗时 4.999 秒

	1 | 4.99991725 | SELECT o1.userId, o1.id , o1.`name` , o1.orderId , od1.total , od1.price FROM order_0 AS o1 LEFT JOIN order_detail_0 AS od1 ON o1.id = od1.orderId where o1.userId < 10000;


	5.7版本上 第一次测试 条件和上一样实测如下：实际耗时 14.347 秒

	1 | 14.34700900 | SELECT o1.userId, o1.id , o1.`name` , o1.orderId , od1.total , od1.price FROM order_0 AS o1 LEFT JOIN order_detail_0 AS od1 ON o1.id = od1.orderId where o1.userId < 10000;

	8.0版本 第二次测试 userId 和orderId 加索引 实测如下 实际耗时 0.01 秒

	1 | 0.01009800 | SELECT o1.userId, o1.id , o1.`name` , o1.orderId , od1.total , od1.price FROM order_0 AS o1 LEFT JOIN order_detail_0 AS od1 ON o1.id = od1.orderId where o1.userId < 10000;

	5.7版本 第二次测试 userId 和orderId 加索引 实测如下 实际耗时 0.035秒

	1 |  0.03518000 | SELECT o1.userId, o1.id , o1.`name` , o1.orderId , od1.total , od1.price FROM order_0 AS o1 LEFT JOIN order_detail_0 AS od1 ON o1.id = od1.orderId where o1.userId < 10000;

	8.0版本 第三次测试 条件 不加where条件 orderId 有索引 实际耗时 0.19秒

	3 | 0.19185775 | SELECT o1.userId, o1.id , o1.`name` , o1.orderId , od1.total , od1.price FROM order_0 AS o1 LEFT JOIN order_detail_0 AS od1 ON o1.id = od1.orderId;

	5.7版本 第三次测试 条件 不加where条件 orderId有索引 实际耗时 0.68秒

	3 |  0.68636000 | SELECT o1.userId, o1.id , o1.`name` , o1.orderId , od1.total , od1.price FROM order_0 AS o1 LEFT JOIN order_detail_0 AS od1 ON o1.id = od1.orderId;

step 8 总结
	
	以上测试总结： 5.7与8.0 在left join和 where条件上 加索引和不加索引 8.0要快三倍左右
