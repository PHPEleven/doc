Api接口安全要求：
	
	防伪装攻击   (有意恶意的调用接口)

	防篡改攻击    （请求内容，请求头 更改内容）

	防数据信息泄露  （截获账号密码等）

设计原则：
	
	轻量级

	多语言易实现

	易于开发

	易于测试

	易于部署

	满足接口安全需求

适用范围：
	
	所有写操作接口

	非公开的读接口
 
实现思路：  
	
	约定变化秘钥 (使用方或者调用方，可以根据一定规则自动生成，周变化或者月变化)  
	
	固定秘钥 (防止泄露)  

		appkey = Yf7mBdnSR!eUW&bz7JQ@H@rHuth9N25@vHYVwGr5

	必要输入的参数[必须传]  

		_sign   一次性接口调用的签名值 防伪装/篡改/重复  

		_timestamp  时间戳  

	算法过程  

		1、需要加密的参数 如 Action=index, Model=test, Api=info, k1=1
 
		2、转成数组  array('Action'=>'index','Model'=>'test','Api'=>'info')
 
		3、按照键名对关联数组进行升序排序后变成 [Model=test,Api=info,Action=index]
 
		4、按照url传参模式拼成字符串最后加上密匙

			Model=test&Api=info&Action=index&appkey=Yf7mBdnSR!eUW&bz7JQ@H@rHuth9N25@vHYVwGr5
 
		5、最后进行 sha1加密 获得签名值

			sign  =  daa1f9ec578df43e9bae4e70215574def5040ae8
 
		6、进行请求接口 后台根据约定进行验证_sign的值，并将值存入缓存，验证时间有效期

			url = api/test/index

			data['Action'] = 'index';

			data['model'] = 'test';

			data['api'] = 'info';

			data['_sign'] = 'daa1f9ec578df43e9bae4e70215574def5040ae8';

			data['_timestamp'] = 155559999;

			http_request(url,data);



