## 基于liunx实现自动备份数据库 shell脚本 
#号为备注  

#!/bin/bash   
dbname=databasename    

#定义备份数据库时使用的用户名和密码      
dbuser=username           
dbpasswd=userpwd  
dbport=11911    


#数据库备份的路径         
backuppath=/data/dbback/sqlzip/    
#数据库备份日志文件存储的路径     
logfile=/data/dbback/log/db_backup.log     
#以当前的时间作为备份的数据库命名。  
#获取当前日期     
Now=$(date +"%Y%m%d")       
#字符串组合生成文件名    
dumpfile=zb_new_db-$Now  

#备份数据库函数
back_db() {

	#将备份的时间、数据库名存入日志  
	echo "------"$(date +%Y-%m-%d%t%A%t%T)" Beginning database "${dbname}" backup--------" >>${logfile}  

	#备份数据库，如果有错误信息也记入日志。默认utf8编码   
	/alidata/server/mysql-5.6.21/bin/mysqldump -P${dbport}  -u${dbuser} -p${dbpasswd} --default-character-set=utf8 --quick --force --routines --add-drop-database --add-drop-table ${dbname} >${backuppath}${dumpfile}.sql 2>> ${logfile}
	echo "/alidata/server/mysql-5.6.21/bin/mysqldump --skip-opt -P${dbport} -u${dbuser} -p${dbpasswd} --default-character-set=utf8 --quick --force --routines --add-drop-database --add-drop-table ${dbname} >${backuppath}${dumpfile}.sql 2>> ${logfile}" 

	#开始压缩数据文件  
	echo $(date +%Y-%m-%d%t%A%t%T)" Beginning zip ${backuppath}${dumpfile}.sql" >>${logfile}   
	#将备份数据库文件库，并删除先前的SQL文件。如果有错误信息也记入日志。  
	tar zcvf ${dumpfile}.tar.gz ${dumpfile}.sql && rm ${dumpfile}.sql 2>> ${logfile}   

	#将压缩后的文件名存入日志。    
	echo "backup file name:"${dumpfile}".tar.gz" >>${logfile}    
	echo -e "-------"$(date +%Y-%m-%d%t%A%t%T)" Ending database "${dbname}" backup-------\n" >>${logfile}  
}  

#删除备份文件 

rm_oldfile() {  

	find /data/dbback/sqlzip/ -type f -mtime +30 -exec rm {} \;  
}

#切换到数据库备份的目录
cd ${backuppath}  

#运行备份数据函数  

	back_db  

#运行删除文件函数  

	rm_oldfile
