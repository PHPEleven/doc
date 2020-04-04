XHProf简介
	
	XHProf是一个分层PHP性能分析工具。
	
	可以报告函数级别的请求次数和各种指标，包括阻塞时间，CPU时间和内存使用情况。

	有一个简单的html的用户界面，基于浏览器的性能分析用户界面更容易查看。


下载地址  

	https://github.com/longxinH/xhprof.git

安装  

	cd xhprof/extension/  

	/usr/local/php7/bin/phpize  

	./configure --with-php-config=/usr/local/php7/bin/php-config --enable-xhprof  

	make && make install    

	修改PHP.ini文件

	[xhprof]  

	extension=xhprof.so   

	xhprof.output_dir=/data/www/xhprof/save_output_dir  ;存储文件的地方  
 
重启fpm   

	systemctl  restart php-fpm.servive

查看图形界面还需安装 graphviz
	
	yum install graphviz


拷贝这两个文件夹到你的项目目录 后面需要加载使用
	
	cp -r xhprof/xhprof_html  /you_path/

	cp -r xhprof/xhprof_lib /you_path/

如何使用

	xhprof_enable();  #开始分析
	
	code();  //你需要分析的代码

	$xhprof_data = xhprof_disable();

	include_once './xhprof_lib/utils/xhprof_lib.php';

	include_once './xhprof_lib/utils/xhprof_runs.php';

	$xhprof_runs = new XHProfRuns_Default();

	$run_id = $xhprof_runs->save_run($xhprof_data, "xhprof_test"); //将run_id保存起>来或者随代码一起输出

	echo $run_id;
	
得到run_id之后在浏览器打开
	
	http://ip:host/xhprof/ROOT_PATH/index.php?run={run_id}&source=xhprof_test

字段分析

	Calls / Call Count：函数被调用的次数 

	Incl. Wall Time / Wall Time：执行该函数（包括子函数）耗费的时间

	Incl. MemUse / Memory Usage：该函数（包括子函数）占用的内存  

	Incl. PeakMemUse / Peak Memory Usage：函数（包括子函数）占用内存的峰值  

	Incl. CPU / CPU：执行该函数（包括子函数）花费的CPU时间 

	Excl. Wall Time / Exclusive Wall Time：函数本身（不包括子函数）耗费的时间  

 	Excl. MemUse / Exclusive Memory Usage：函数本身（不包括子函数）占用的内存   

	Excl. PeakMemUse / Exclusive Peak Memory Usage：函数本身（不包括子函数）耗费内存的峰值  

	Exclusive CPU：函数本身（不包括子函数）花费的CPU时间
	

