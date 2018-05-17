# Redis集群的搭建
1. 下载地址
   > http://download.redis.io/releases/redis-4.0.9.tar.gz
2. 配置准备
   ```
   mkdir /opt/redis_conf_63{79,80,81,82,83,84}
   tar xzf redis-4.0.9.tar.gz -C /opt/redis
   make
   #拷贝redis.conf配置文件到指定的配置目录
   echo /opt/redis_conf_63{79,80,81,82,83,84} | xargs -n 1 cp -v /opt/redis/src/redis.conf
   # 查看配置文件配置信息 
   grep -v '^#' redis.conf | sort -u
   # 添加redis-server 软连接
   ln -s /opt/redis/src/redis-server /usr/bin/redis-server
   # 检查ruby软件版本，根据redis的版本，安装合适的ruby
   wget https://cache.ruby-lang.org/pub/ruby/2.5/ruby-2.5.1.tar.gz
   tar zxf ruby-2.5.1.tar.gz
   ./configure --prefix=/usr/local/ruby
   make && make install
   #添加ruby环境变量
   RUBY_HOME=/usr/local/ruby
   PATH=$RUBY_HOME/bin:$PATH
   gem install redis
   #分别启动redis不同端口
   redis-server /opt/redis_conf_6379/redis.conf
   #添加集群
   /opt/redis/src/redis-trib.rb create --replicas 1 192.168.0.72:6379 192.168.0.72:6380 192.168.0.72:6381 192.168.0.72:6382 192.168.0.72:6383 192.168.0.72:6384
   ```