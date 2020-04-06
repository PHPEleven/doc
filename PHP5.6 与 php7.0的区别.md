##  PHP5.6 与 php7.0的区别

PHP7.0 比PHP5.6性能提升了几倍  

PHP7.0 全面一致支持 64 位

PHP7.0 之前出现的致命错误，都改成了抛出异常。

增加了空结合操作符（??）。效果相当于三元运算符。

	php5.6 isset($_GET['dd']) ? $_GET['dd']:'default';  

	php7.0 $_GET['dd'] ?? 'default';

PHP7.0 标量类型 主要分为两种模式 强制性模式和严格模式
	
	declare(strict_types=1);  #1表示严格类型效验 0 表示弱类型校验模式

define 可以定义常量数组。
	
	define("LIST",['test1','test2','test3']);

	访问 LIST[1];

函数返回类型说明 PHP 7允许程序员根据期望的返回值声明函数的返回类型。  
这肯定会使代码健壮和准确。有四种不同的返回类型可用-bool，int，string和float
	
	function arraysSum(array ...$arrays): array{

		return array_map(function(array $array): int {

			return array_sum($array);

		}, $arrays);

	}

太空船操作符  (组合比较符）

	太空船操作符用于比较两个表达式。当$a 小于、等于或大于$b 时它分别返回-1、0 或1

	1 <=> 1  输出0

	1 <=> 2   -1

	2 <=> 1   1

use 语句
	
	namespace;

	php5.6  use \Api\model\classA;  use \Api\model\classB;  

	php7.0  use \Api\model\{ClassA, ClassB, ClassC as C};  

	
	php5  use const some\namespace\ConstA; use const some\namespace\ConstB;

	php7  use const some\namespace\{ConstA, ConstB, ConstC};

实例化一个匿名类
	
		interface Logger {

		   public function log(string $msg);  

		}  

		class Application {  
		   	private $logger;  
		   	public function getLogger(): Logger {  
		      	return $this->logger;   
		   	}     
		   	public function setLogger(Logger $logger) {  
		      	$this->logger = $logger;  
		   	}   
		}  

		$app = new Application;  
		// 使用 new class 创建匿名类  
		$app->setLogger(new class implements Logger {   
		   	public function log(string $msg) {    
		      	print($msg);  		  
		   	}  					
		});   
		$app->getLogger()->log("write log");  


int random_int(int $min,int $max);  返回一个指定范围内的int型数字




