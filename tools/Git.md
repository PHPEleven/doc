Git简介     

    1、Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。    

	2、Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。    

    3、Git 与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。   

    4、完整的文档移步  https://git-scm.com/docs     

git安装  
     
    git 的工作需要调用 curl，zlib，openssl，expat，libiconv 等库的代码，所以需要先安装这些依赖工具  

    yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel      

    yum -y install git   


基础基础使用的步骤  

    1、 git clone https://github.com/username/blog.git   #克隆项目地址    

    2、 cd blog   #进入目录  

    3、 git add filename.php    #建立文件或目录  

    4、 git commit -m 'add filename.php to src' 　#提交，产生版本记录，注意代码依然在本地 单引号内 是提交说明  

    5、 git push origin　 #将修改提交到github上  

    6、 git pull   #拉代码    
	
	7、 git help   

用户信息 配置个人的用户名称和电子邮件地址  

	git config --global user.name "name"  
   
    git config --global user.email "yangsior@163.com"   
   
    git config --list  


git基本操作  
      
    git init     #使用当前目录作为git仓库 会生成一个 .git文件  

    git add *.php  #将想要快照的内容写入缓存区  

    git commit -m "init "  #将缓存区内容添加到仓库中  

    git clone https://github.com/PHPEleven/doc.git  #拉取项目  
 
    git status -s     #查看状态    

    git diff     #查看Git status 的结果的详细信息  比较差异  

    git reset HEAD  #取消git add的添加  

    git rm file.php  # 可选参数 -f 强制删除 --cached  把文件从暂存区移除保留在当前工作目录中  

git分支管理  
	
	分支管理言简意赅就是 用分支意味着你可以从开发主线上分离开来，然后在不影响主线的同时继续工作    

	1、创建分支  

	    git branch (branchname)

	2、切换分支

	    git checkout (branchname)  

	3、合并分支  

	    git merge branchname  

	4、列出分支  

	    git branch    

	5、删除分支    

	    git branch -d branchname  

	6、创建分支并立即切换分支   

	    git checkout -b (branchname)   

git日志    
    
    git log  #可选参数  --oneline 查看简洁版本  --graph 查看分支、合并  --reverse 逆向显示所有日志  
    
    git log --author=even   #指定用户日志        

    git log --oneline --before={3.weeks.ago} --after={2010-04-18} --no-merges #条件    


Git Gitee  
	
	你希望体验到 Git 飞一般的速度，可以使用国内的 Git 托管服务     

	地址 https://gitee.com/?utm_source=remote_blog_cnjc      




















