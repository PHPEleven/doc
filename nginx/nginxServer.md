php开源框架项目部署 实战可行    
  
  
tp5 nginx 配置        
    
    server {     
        listen       8001;  
        server_name  _;   
        root   /usr/share/nginx/html/tp5/public;   

        location / {  
            index index.php index.html;  
            #下面这句去掉index.php  
            if (!-e $request_filename) {  
                rewrite  ^(.*)$  /index.php?s=/$1  last;  
            }  
        }  

        location ~ \.php(.*)$ {  
            fastcgi_pass 127.0.0.1:9000;  
            fastcgi_split_path_info  ^((?U).+\.php)(/?.+)$;  
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;    
            fastcgi_param  PATH_INFO  $fastcgi_path_info;  
            fastcgi_param  PATH_TRANSLATED  $document_root$fastcgi_path_info;   
            include fastcgi_params;   
        }   
    }   

    
ci3.1配置    

    config.php 文件     
    $config['enable_query_strings'] = FALSE;//设置成false      

    server {  
  
        listen       8002;    
        server_name  _;    
        root /usr/share/nginx/html/server;  
        index index.php; 

        location / {   

            rewrite ^/$/index.php last; 

            rewrite ^/(?!index\.php|robots\.txt|images|js|styles)(.*)$ /index.php/$1 last;   
        }    

        location /index.php{   
            fastcgi_pass 127.0.0.1:9000;    
            fastcgi_index index.php;  
            fastcgi_param SCRIPT_FILENAME /usr/share/nginx/html/server/index.php;  
            fastcgi_param PATH_INFO $fastcgi_path_info;   
            fastcgi_split_path_info ^(.+\.php)(.*)$;   
            fastcgi_param PATH_TRANSLATED $document_root$fastcgi_path_info;   
            include fastcgi.conf;   
        } 

    }    

    
laravel nginx配置   

    server {    

        listen       8003;   
        server_name  _;   
        root /usr/share/nginx/html/laravel/public;   
        index index.php;   
  
        location / {
            try_files $uri $uri/ /index.php?$query_string;  
        }
      
        location /index.php{  
            fastcgi_pass 127.0.0.1:9000;  
            fastcgi_index index.php;  
            fastcgi_param PATH_INFO $fastcgi_path_info;  
            fastcgi_split_path_info ^(.+\.php)(.*)$;   
            fastcgi_param PATH_TRANSLATED $document_root$fastcgi_path_info;   
            include fastcgi.conf;   
        }   
    }  


