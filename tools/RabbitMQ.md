##  rabbitMQ资料

什么是rabbitmq? 

    RabbitMQ是一款开源的，Erlang编写的，基于AMQP协议的，消息中间件；

    可以用它来：解耦、异步、削峰
 
为什么用rabbitmq?  
  
    应用解耦
    
    消息分发
    
    消息缓冲

消息基于什么传输？
  
    由于TCP连接的创建和销毁开销较大，且并发数受系统资源限制，会造成性能瓶颈。  

    RabbitMQ使用信道的方式来传输数据。信道是建立在真实的TCP连接内的虚拟连接，且每条TCP连接上的信道数量没有限制。

RabbitMQ有什么优缺点？

    优点：解耦、异步、削峰；
  
    缺点：降低了系统的稳定性：本来系统运行好好的，现在你非要加入个消息队列进去，那消息队列挂了，你的系统不是呵呵了。因此，系统可用性会降低；  

    增加了系统的复杂性：加入了消息队列，要多考虑很多方面的问题，比如：一致性问题、如何保证消息不被重复消费、如何保证消息可靠性传输等。 
    
    因此，需要考虑的东西更多，复杂性增大


如何保证RabbitMQ不被重复消费？

    为什么会重复消费：正常情况下，消费者在消费消息的时候，消费完毕后，会发送一个确认消息给消息队列，消息队列就知道该消息被消费了，就会将该消息从消息队列中删除；  

    但是因为网络传输等等故障，确认信息没有传送到消息队列，导致消息队列不知道自己已经消费过该消息了，再次将消息分发给其他的消费者。   

    针对以上问题，一个解决思路是：保证消息的唯一性，就算是多次传输，不要让消息的多次消费带来影响；保证消息等幂性；  

    比如：在写入消息队列的数据做唯一标示，消费消息时，根据唯一标识判断是否消费过；  

如何保证RabbitMQ消息的可靠传输？

    生产者丢失消息：从生产者弄丢数据这个角度来看，RabbitMQ提供transaction和confirm模式来确保生产者不丢消息；

    transaction机制就是说：发送消息前，开启事务（channel.txSelect()）,然后发送消息，如果发送过程中出现什么异常，事务就会回滚（channel.txRollback()）,如果发送成功则提交事务（channel.txCommit()）。然而，这种方式有个缺点：吞吐量下降；

    消息队列丢数据：消息持久化。
    
    处理消息队列丢数据的情况，一般是开启持久化磁盘的配置。这个持久化配置可以和confirm机制配合使用，你可以在消息持久化磁盘后，再给生产者发送一个Ack信号。 

如何保证RabbitMQ消息的顺序性？
  
    单线程消费保证消息的顺序性；对消息进行编号，消费者处理消息是根据编号处理消息；


centos7安装rabbitmq

  1.安装erlang  

      wget https://bintray.com/rabbitmq-erlang/rpm/download_file?file_path=erlang%2F19%2Fel%2F7%2Fx86_64%2Ferlang-19.3.6.13-1.el7.centos.x86_64.rpm

      mv download_file\?file_path\=erlang%2F19%2Fel%2F7%2Fx86_64%2Ferlang-19.3.6.13-1.el7.centos.x86_64.rpm erlang-19.3.6.13-1.el7.centos.x86_64.rpm 

      rpm -ivh erlang-19.3.6.13-1.el7.centos.x86_64.rpm

      #查看erlang版本    

      erl -version    

  2.erlang安装成功，再安装rabbitmq-server：安装rabbitmq之前需要安装socat服务，依然下载二进制安装包

      wget http://repo.iotti.biz/CentOS/7/x86_64/socat-1.7.3.2-5.el7.lux.x86_64.rpm

      rpm -ivh socat-1.7.3.2-5.el7.lux.x86_64.rp

  3.开始安装rabbitmq-server服务

      wget https://github.com/rabbitmq/rabbitmq-server/releases/download/rabbitmq_v3_6_16/rabbitmq-server-3.6.16-1.el6.noarch.rpm

      rpm -ivh rabbitmq-server-3.6.16-1.el6.noarch.rpm

      rabbitmq-server

      systemctl status rabbitmq-server

      systemctl start rabbitmq-server

      systemctl stop rabbitmq-server

      systemctl enable rabbitmq-server

  4.开启web后台管理插件

      rabbitmq-plugins enable rabbitmq_management

      查看server监听的插件

      rabbitmqctl status | grep list

  5.开放端口

      添加tcp端口： firewall-cmd --zone=public --add-port=15672/tcp --permanent

      firewall-cmd --zone=public --add-port=5672/tcp --permanent

      firewall-cmd --reload

  6.浏览器中输入 http://服务器IP:15672  进入后台

  7.创建用户

      创建用户：rabbitmqctl add_user admin admin  

      分配权限：rabbitmqctl set_permissions -p / admin ".*" ".*" ".*"

      设置为管理员：rabbitmqctl set_user_tags admin administrator

      查看所有用户：rabbitmqctl list_users

  8.安装rabbitmq客户端

      wget https://github.com/alanxz/rabbitmq-c/archive/v0.9.0.tar.gz

      mv v0.9.0.tar.gz rabbitmq-c-v0.9.0.tar.gz

      tar -zxvf rabbitmq-c-v0.9.0.tar.gz

      cd rabbitmq-c-0.9.0/

      mkdir build && cd build

      cmake ..       #没有该命令 需要安装，具体怎么安装请百度

      cd .. 

      cmake -DCMAKE_INSTALL_PREFIX=/usr/local/rabbitmq-c-0.9.0

      cmake --build . --target install

  9.安装rabbitmq php扩展

      wget http://pecl.php.net/get/amqp-1.9.4.tgz

      tar -zxvf amqp-1.9.4.tgz

      cd amqp-1.9.4/

      /usr/local/php/bin/phpize

      ./configure --with-php-config=/usr/local/php/bin/php-config --with-amqp --with-librabbitmq-dir=/usr/local/rabbitmq-c-0.9.0

      ln -s /usr/local/rabbitmq-c-0.9.0/lib64 /usr/local/rabbitmq-c-0.9.0/lib 

      make && make install

      修改php.ini 添加 : extension=amqp.so

      重启fpm : systemctl restart php-fpm.service
  
      查看phpinfo()  搜索 amqp


10.php示例 生产与消费：

    消费者   

![image](https://github.com/PHPEleven/doc/blob/master/images/consumber.png?raw=true)
    
    生产者

![image](https://github.com/PHPEleven/doc/blob/master/images/publish.png?raw=true)


消费者 消费数据  
生产者  生产者一旦有数据进入 消费者会马上进行消费   
