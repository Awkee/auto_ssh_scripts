# auto_ssh_scripts
通过参数自动执行SSH访问Linux/Unix主机脚本，执行简单方便，对于没有配置免密码认证主机登录需要依赖expect命令工具辅助。

## mrun命令

### 使用说明:

	`mrun [-e -u username -p password] <ip-list|ip|ip_list,ip_list> <cmd>`

### 功能说明:
> 此工具帮助我们在多主机批量执行同一任务工作，比如批量开机/关机/批量查看主机参数信息等

### 参数说明：

	`[-e -u username -p password]`	:可选参数，默认使用当前登录用户并且可以免认证访问所有主机，如果设置此选项标识需要使用expect命令在指定username下批量执行<cmd>命令,如果username不指定就取默认testuser用户名

	`<ip|ip-list>`	: 此参数可以是确定的IP地址如192.168.1.2，也可以是指定的IP范围,示例:10.163.105.2-44,在默认情况下使用的是testuser用户访问指定IP地址列表,支持逗号指定离散IP列表，也支持减号指定连续IP列表,192.168.2.2-5,192.168.3.2-10,192.168.4.5-20,192.168.5.6

	`<cmd>`	: 此参数为需要在指定的IP地址下执行的命令语句。

### 示例:

	`mrun 192.168.2.2-5,192.168.3.2-10,192.168.4.5-20,192.168.5.6 'hostname;ls -l ~/.ssh/*.pub'` : 使用默认的testuser用户进行免密码认证执行命令

	`mrun -u myuser 192.168.1.2-23 'hostname;ls -l ~/.ssh/*.pub'`  : 使用myuser用户进行免密码认证执行命令

	`mrun -e -u testuser -p testpassword 192.168.1.2-23 'hostname;ls -l ~/.ssh/*.pub'` : 使用testuser用户进行自动输入密码方式执行命令

### 命令使用扩展
	`mkeygen` : 自动生成并配置RSA公钥，实现免密码认证访问命令脚本.

## mscp命令



### 使用说明:

	`mscp [-r] [-e] [-u username] [-p password] <ip-list> <local_path> <remote_path>`

### 功能说明:
> 此工具帮助我们执行从本地向多台主机进行版本发布任务,可以传输文件也可以传输目录

### 参数说明：

	`[-r]`	: 此参数为可选参数，仅当传输的<local_path>为目录时设置此参数，默认情况下只传输文件

	`[-e -u username -p password]`	: 可选参数，默认使用当前登录用户并且可以免认证访问所有主机，如果设置此选项标识需要使用expect命令在指定username下批量执行<cmd>命令,如果username不指定就取默认当前登录用户名

	`<ip|ip-list>`	: 此参数可以是确定的IP地址如192.168.1.2，也可以是指定的IP范围,示例:10.163.105.2-44,在默认情况下使用的是当前用户访问指定IP地址列表,支持逗号指定离散IP列表，也支持减号指定连续IP列表,192.168.2.2-5,192.168.3.2-10,192.168.4.5-20,192.168.5.6

	`<local_path>`	: 此参数为本地路径，可以是文件或者目录的绝对路径和相对路径

	`<remote_path>`	: 此参数为远端路径，可以是文件或者目录的绝对路径和相对路径

### 示例:

	`mscp 192.168.1.2-23 /tmp/my_upload_file /work/myuserpath/data/ `: 使用默认用户进行免密码认证传输本地文件到所有的远端主机目录下

	`mscp -r 192.168.1.2-23 /data01/tempdir/ /work/myuserpath/data/` : 用默认用户进行免密码认证传输本地目录到所有的远端主机目录下

	`mscp -r -u myuser 192.168.1.2-23 /data01/tempdir/ /work/myuserpath/data/` : 用myuser用户进行免密码认证传输本地目录到所有的远端主机目录下

	`mscp -e -u testuser -p testpassword 192.168.1.2-23 /tmp/my_upload_file /work/myuserpath/data/` : 使用testuser用户进行自动输入密码方式传输本地文件到所有的远端主机目录下

# 如何使用

1. 下载源码, 将脚本赋权可执行权限.
2. 将源码放到 $HOME/bin 或者 PATH 环境变量可以访问的路径下

现在你就可以使用mrun,mscp命令了.

---
