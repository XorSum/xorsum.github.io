---
title: webdav
date: 2019-06-22 21:17:54
tags:
---


安装软件
```
sudo apt-get install davfs2
```
如果是使用坚果云，修改配置文件/etc/davfs2/davfs2.conf,在文件尾加上'ignore_dav_header 1' [来源](https://zohead.com/archives/davfs2-nutstore/)

挂载
`sudo mount -t davfs https://dav.jianguoyun.com/dav/  /mnt/webdav/`
查看
'ls /mnt/webdav'

为了长期使用:

```
mkdir ~/.davfs2/
echo "https://webdav.example.com webdavuser webdavpassword" >> ~/.davfs2/secrets
chmod 0600 ~/.davfs2/secrets
```

将用户添加进davfs2用户组
```
sudo usermod -a -G davfs2 username
```

编辑/etc/fstab文件，在文件尾添加如下内容，注意用户名换成自己的
```
https://webdav.example.com /home/username/webdav davfs user,noauto,uid=username,file_mode=600,dir_mode=700 0 1
```

挂载
```
mount /home/username/webdav
```

取消挂载
```
unmount  /home/username/webdav
```

