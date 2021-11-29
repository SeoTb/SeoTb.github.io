### 创建fastdfs文件目录
    在fileserver/files目录下面创建目录M00
    mkdir M00
### 打开迁移配置项
    将配置文件conf/cfg.json中enable_migrate参数值改为true，启用迁移（默认为false)
    重启配置：curl http://xxx.xxx.xxx.xxx:31507/group1/reload?action=reload
### 不同服务器上文件复制
###### 在fastdfs服务器上执行
    sudo scp -r /home/dfs/data/* xxx.xxx.xxx.xxx:/opt/fileserver/files/M00
### 同一个服务器上文件复制
    cp -r /home/dfs/data /opt/fileserver/files/M00
### 修复文件信息
    curl http://xxx.xxx.xxx.xxx:31507/group1/repair_fileinfo
### 修复文件状态
    curl http://xxx.xxx.xxx.xxx:31507/group1/repair_stat?date=20211129
### 备份md5文件
    curl http://xxx.xxx.xxx.xxx:31507/group1/backup?date=20211129
### 关闭迁移配置项
    将配置文件conf/cfg.json中enable_migrate参数值改为false
    重启配置：curl http://xxx.xxx.xxx.xxx:31507/group1/reload?action=reload
### 拷贝md5文件
    在fileserver/data文件夹下找到刚刚备份文件设置的date文件夹(如：20211129),将改文件夹下的files.md5拷贝出来
### 执行数据库脚本

### 其他：配置文件设置
    "管理ip列表": "用于管理集的ip白名单,如果放开所有内网则可以用 0.0.0.0,在白名单的不需要使用md5下载删除，不在白名单的需要使用md5下载删除
    "admin_ips": ["127.0.0.1"]

    "是否启用迁移": "默认不启用",迁移文件时打开,使用完之后关闭
    "enable_migrate": false

    "文件是否去重": "默认去重(true)",根据md5去重,相同的文件不会存两份
    "enable_distinct_file": false