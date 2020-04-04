## composer基本使用

1、仅更新单个或者多个库 不想更新它的所有依赖
	
	composer update foo/bar mononlog/monolog

2、不编辑composer.json安装库  

	composer require "foo/bar:1.0.0"

3、国内镜像加速
	
	composer config --global repo.packagist composer https://packagist.phpcomposer.com

4、更新所有依赖
	
	composer update

5、移除 remove

	composer remove monolog/monolog

6、查找 search
	
	composer search monolog

7、show 
	
	composer show    #所有的包

	composer show monolog/monolog  #单个

8、安装

	composer install