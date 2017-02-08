## AntNet ##
    分布式配置管理系统
#### 安装zookeeper ####
    1. 下载安装zookeeper安装包，地址：http://zookeeper.apache.org/releases.html
    2. 解压安装包到安装目录，例如：/Users/lishangmin/apps/zookeeper-3.3.6
    3. 单机模式，进入zookeeper安装目录下conf子目录，创建zoo.cfg，添加以下内容
            tickTime=2000
            dataDir=/Users/lishangmin/data/zookeeper/data
            dataLogDir=/Users/lishangmin/data/zookeeper/logs
            clientPort=4180
       字段说明
            * tickTime : zookeeper中使用的基本事件单位，毫秒。
            * dataDir : 数据目录，可以是任意目录。
            * dataLogDir : 日志目录，可以是任务目录，如果该参数未设置，则默认为dataDir目录。
            * clientPort : 监听client链接的端口号。
    4. 启动zookeeper服务
            bin/zkServer.sh start
    5. 启动client链接server
            bin/zkCli.sh -server localhost:4180
    6. 集群模式，假设有三台机器
            zookeeper0 192.168.0.1
            zookeeper1 192.168.0.2
            zookeeper2 192.168.0.3
    7. 进入zookeeper0安装目录下conf子目录，创建zoo.cfg，添加以下内容
            tickTime=2000
            initLimit=5
            syncLimit=2
            dataDir=/Users/lishangmin/data/zookeeper/data
            dataLogDir=/Users/lishangmin/data/zookeeper/logs
            clientPort=4180
            server.0=192.168.0.1:8880:7770
            server.1=192.168.0.2:8880:7770
            server.2=192.168.0.3:8880:7770
       字段说明
            * initLimit : zookeeper集群中包含多个server，其中一台为leader，集群中其余server为follower。
                          initLimit参数配置初始化连接时，follower和leader之间的最长心跳时间。此时该参数设置为5，
                          说明时间限制为5倍tickTime，即5 * 2000 = 10000ms = 10s。
            * syncLimit : 该参数配置leader和follower之间发送消息，请求和应答的最大时间长度。此时该参数设置为2，
                          说明时间限制为2倍tickTime，即2 * 2000 = 4000ms = 4s。
            * server.X=A:B:C : 其中X是一个数字，标志这是第几号server，A是该server的ip地址。B是该server和集群中的leader
                               交换消息所使用的端口。C是选举leader时所使用的端口
    8. 参照zookeeper0配置，配置zookeeper1，zookeeper2，只需修改dataDir，dataLogDir，clentPort参数即可
    9. 在之前设置的dataDir的路径中新建myid文件，写入一个数字，该数字表示这是第几号server。该数字必须和zoo.cfg文件中的
       server.X中的X一一对应。zookeeper0 myid 中写入 0，zookeeper1 myid 中写入1，zookeeper2 myid 中写入2。
