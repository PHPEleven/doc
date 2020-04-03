Yapi安装及使用 demo地址传送  http://yapi.demo.qunar.com  



环境准备
　　　操作系统  centos  
　　　环境要求必须    
　　　　　nodejs  > 7.6  
　　　　　mongodb > 2.6  
　　　　　git   
　　　　　curl   

安装nodejs   
    1 获取资源  
        curl -sL https://rpm.nodesource.com/setup_8.x | bash -  
    2 安装  
        yum install -y nodejs  
    3 查看版本  
        node -v  
    4 查看npm版本  
        npm -v  

安装mongodb  
    1 更新yum源  非必须  
        yum -y update  
    2 添加 mongodb源文件     
        vim /etc/yum.repos.d/mongodb-org.repo    
    3 添加一下内容    
        [mongodb-org]    
        name=MongoDB Repository     
        baseurl=http://mirrors.aliyun.com/mongodb/yum/redhat/7Server/mongodb-org/3.2/x86_64/    
        gpgcheck=0      		
        enabled=1  
    ****  	
    4 安装mongodb	
        yum install -y mongodb-org		
    5 启动mongodb   			
        service mongod start   		
    6 设置开机启动  		
        chkconfig mongod on	 		
    7 修改mongod.conf配置文件	   		
        注释 bindIp  127.0.0.1   	
    8 重启mongod	  		
        service mongod restart		

安装git     	  		
    yum -y install git		
		
搭建Yapi环境	  	
    npm install -g yapi-cli --registry https://registry.npm.taobao.org  		
    	
运行	  	
    yapi server	  	
   	
初始化账号密码     		
    账号名  admin@admin.com       		 
    密码  passwd       		

进入安装目录     		
    cd /you_path/my-yapi       		
    		  
启动服务           		
    node vendors/server/app.js        		
   		
加入后台启动      		
    nobup node vendors/server/app.js &      
  
查看进程 是否启动    	 
    ps    
  	
  
  
加入守护进程  
安装pm2    
    npm install pm2@latest -g   
   
加入守护进程   
    pm2 start vendors/server/app.js --watch   