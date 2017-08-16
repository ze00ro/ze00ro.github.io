---
layout: post
title: "关于服务器备份"
date: 2013-04-09 01:01:01
categories: program
tags: 

---

### A. 保存到本地 ##




----

### B. 保存到dropbox ##

更新于2013-09-16, 参考自尘埃落定的 [自动备份网站并同步到 Dropbox][lovelucy]

以前的备份方案只是备份到单机, 如果机器出问题, 资料就也出问题. 于是Google到了上传到dropbox的方案, 遂记录一下:

#### 1. 登录到dropbox, 建立个dropbox api app, 获得 `app-key` 和 `app-secret`.
#### 2. 下载一个开源的dropbox api脚本 [Dropbox-Uploader][github-dropbox-uploader], 这样可以在linux shell里直接用命令操作了:

```bash
$ wget https://raw.github.com/andreafabrizi/Dropbox-Uploader/master/dropbox_uploader.sh
$ chmod +x dropbox_uploader.sh
$ ./dropbox_uploader.sh
```
运行脚本, 根据提示设置自己的 Dropbox 应用 API, 然后按照步骤设置, 就可以使用其命令上传和下载文件了.
第一次上传东西的时候会提示让你到指定地址授权一下, 点过去再回来确定下就可以了.

#### 3. 建立一个备份脚本, 脚本里调用dropbox的操作就可以了.

```bash
#!/bin/bash
# 一些配置
DROPBOX_DIR=/$(date +%Y-%m-%d) # Dropbox 目录，根目录 / 是你已经创建的 app 目录
MYSQL_USER="dbuser"
MYSQL_PASS="dbpass"
MYSQL_DB=('dbname1' 'dbname2')
BACK_DATA=/root/backup-data # 备份文件保存在本地的目录
DATA=/var/www # 需要备份的网站文件
 
# 定义备份文件名
DataBakName=Database_$(date +"%Y-%m-%d").tar.gz
WebBakName=Web_$(date +%Y-%m-%d).tar.gz
OldData=Database_$(date -d -6day +"%Y-%m-%d").tar.gz
OldWeb=Web_$(date -d -6day +"%Y-%m-%d").tar.gz
# Dropbox 里 30 天以上的旧数据可以清除
Old_DROPBOX_DIR=/$(date -d -30day +%Y-%m-%d) 
# 清理本地保存了 6 天的备份
echo -ne "Delete local data of 6 days old..."
rm -rf $BACK_DATA/$OldData $BACK_DATA/$OldWeb
echo -e "Done"

cd $BACK_DATA
# 导出 MySQL 数据库，并压缩
echo -ne "Dump mysql..."
for db in ${MYSQL_DB[@]}; do
    (/usr/bin/mysqldump -u$MYSQL_USER -p$MYSQL_PASS ${db} ${db}.sql)
done
tar zcf $BACK_DATA/$DataBakName *.sql
rm -rf $BACK_DATA/*.sql
echo -e "Done"

# 备份网站文件
echo -ne "Backup web files..."
cd $DATA
tar zcf $BACK_DATA/$WebBakName *
echo -e "Done"

cd $BACK_DATA
# 开始上传到 Dropbox
echo -e "Start uploading..."
-------/dropbox_uploader.sh upload $BACK_DATA/$DataBakName $DROPBOX_DIR/$DataBakName
-------/dropbox_uploader.sh upload $BACK_DATA/$WebBakName $DROPBOX_DIR/$WebBakName

# 清理 Dropbox 里 30 天前的旧数据
-------/dropbox_uploader.sh delete $Old_DROPBOX_DIR/
 
echo -e "Thank you! All done."
```

把脚本里的路径自己改一下, 然后在crontab里增加计划, 就可以了. 运行完后看看dropbox里App/AppName/下就会有以日期为名称的数据了.
![上传后的截图][1]


  [lovelucy]: http://www.lovelucy.info/backup-website-and-sync-to-dropbox.html
  [github-dropbox-uploader]: https://github.com/andreafabrizi/Dropbox-Uploader
  [1]: {{ site.uploads }}img/dropbox-create-app.jpg "创建"
