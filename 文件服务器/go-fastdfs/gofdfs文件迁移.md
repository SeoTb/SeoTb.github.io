### 注意：文中的xxx.xxx.xxx.xxx都是go-fastdfs所在服务器的IP

### 创建fastdfs文件目录
    在fileserver/files目录下面创建目录M00

    mkdir M00
### 打开迁移配置项
    将配置文件conf/cfg.json中enable_migrate参数值改为true，启用迁移（默认为false)

    重启配置：curl http://xxx.xxx.xxx.xxx:31507/group1/reload?action=reload
### 文件复制
###### 不同服务器上文件复制:在fastdfs服务器上执行
    sudo scp -r /home/dfs/data/* xxx.xxx.xxx.xxx:/opt/fileserver/files/M00
###### 同一个服务器上文件复制
    cp -r /home/dfs/data /opt/fileserver/files/M00
### 修复并备份文件
    curl http://xxx.xxx.xxx.xxx:31507/group1/repair_fileinfo

    curl http://xxx.xxx.xxx.xxx:31507/group1/repair_stat?date=20211129

    curl http://xxx.xxx.xxx.xxx:31507/group1/backup?date=20211129
### 关闭迁移配置项
    将配置文件conf/cfg.json中enable_migrate参数值改为false

    重启配置：curl http://xxx.xxx.xxx.xxx:31507/group1/reload?action=reload
### 拷贝md5文件
    在fileserver/data文件夹下找到刚刚备份文件设置的date文件夹(如：20211129),将该文件夹下的files.md5拷贝出来
### 执行数据库脚本
    先在身份核验数据库中创建一张临时表 tmp_go_file(考虑在版本中预创建)

    将files.md5文件通过navicat工具导入到表tmp_go_file中

    执行数据库更新语句将相关表中文件路径与md5加密串更新
  更新库中md5脚本 [数据库脚本](https://172.16.3.8/svn/SCM_MICP/trunk/01.开发库/050.编码/01_数据库脚本/mysql/个性化及辅助检查脚本/文件服务器升级为gofast数据库操作脚本.sql)

1. 在表tmp_go_file上右键导入向导，选择文本文件(*.txt)，选择准备好的files.md5文件
2. 设置‘|’分隔符
   ![设置分隔符](https://static.dingtalk.com/media/lQLPDhrr-1PB21nNAfrNAqyw8BYl1Sxq3ZoBr-ATUQAZAA_684_506.png_720x720q90g.jpg?bizType=im)
3. 定位数据位置
   ![定位数据位置](https://static.dingtalk.com/media/lQLPDhrr-2F3YPPNAf_NAqywS7x96tn-tJkBr-Ap0UB0AQ_684_511.png_720x720q90g.jpg?bizType=im)
4. 对应字段
   ![对应字段](https://static.dingtalk.com/media/lQLPDhrr-44rdxzNAfzNAqywcFvbuaCYSrkBr-BygUB0AA_684_508.png_720x720q90g.jpg?bizType=im)
5. 一直下一步，知道导入完成

### 其他：配置文件设置
    "管理ip列表": "用于管理集的ip白名单,如果放开所有内网则可以用 0.0.0.0,在白名单的不需要使用md5下载删除，不在白名单的需要使用md5下载删除
    "admin_ips": ["127.0.0.1"]

    "是否启用迁移": "默认不启用",迁移文件时打开,使用完之后关闭
    "enable_migrate": false

    "文件是否去重": "默认去重(true)",根据md5去重,相同的文件不会存两份
    "enable_distinct_file": false