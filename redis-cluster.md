# Redis集群的搭建
1. 下载地址
   > wget http://download.redis.io/releases/redis-4.0.9.tar.gz
2. 配置准备
    * 增加配置文件目录用于区分不同端口
    
        mkdir /opt/redis_conf_63{79,80,81,82,83,84}
    * 解压到 redis
        
        tar xzf redis-4.0.9.tar.gz -C /opt/redis
    * 编译 
        
        make
    * 拷贝redis.conf配置文件到指定的配置目录
        
        echo /opt/redis_conf_63{79,80,81,82,83,84} | xargs -n 1 cp -v /opt/redis/src/redis.conf
    * 查看配置文件配置信息 
        
        grep -v '^#' redis.conf | sort -u
    * 添加redis-server 软连接
        
        ln -s /opt/redis/src/redis-server /usr/bin/redis-server
    * 检查ruby软件版本，根据redis的版本，安装合适的ruby
    * 下载 ruby
        
        wget https://cache.ruby-lang.org/pub/ruby/2.5/ruby-2.5.1.tar.gz
    * 解压 ruby
        
        tar zxf ruby-2.5.1.tar.gz
    * 编译配置 
        
        \./configure --prefix=/usr/local/ruby
    * 编译安装 
        
        make && make install
    * 添加ruby环境变量
        
        RUBY_HOME=/usr/local/ruby
        
        PATH=$RUBY_HOME/bin:$PATH
    * gem install redis 
        
        gem install redis
        
    * 分别启动redis不同端口
        
        redis-server /opt/redis_conf_6379/redis.conf
    * 添加集群
        
        /opt/redis/src/redis-trib.rb create --replicas 1 192.168.0.72:6379 192.168.0.72:6380 192.168.0.72:6381 192.168.0.72:6382 192.168.0.72:6383 192.168.0.72:6384
        