
# <center>查看linux系统相关信息命令汇总</center>
#### <center>作者：华仔</center>
#### <center>2023-3-10 10:39:00</center>

linux 查看系统信息命令是linux初学者必备的基础知识。通常在进行开展开发工作之前，需要了解系统信息，因此有必要了解linux中的相关命令信息。
下面根据不同功能进行分类提供了各linux发行版比较常用的系统信息查询的命令, 大家可以参考。

## 一、内核相关命令

### （1）查看Linux内核版本命令
1. cat /proc/version
2. uname -a 

### （2）列出加载的内核模块 
lsmod 


## 二、系统版本相关命令

### （1）查看Linux 系统版本命令
1. lsb_release -a
2. cat /etc/redhat-release （注：只适合Redhat系的Linux）、
3. cat /etc/issue

### （2）查看操作系统版本
 head -n 1 /etc/issue
 ### （3）查看CPU信息
 cat /proc/cpuinfo
 ### （4）查看系统运行时间、用户数、负载
 uptime 
 
 ## 三、查看进程和服务相关命令 
### （1）查看所有进程 
ps -ef 
### （2）列出所有系统服务 
chkconfig –list 
### （3）列出所有启动的系统服务程序 
chkconfig –list | grep on 
### （4）实时显示进程状态用户 
top 
### （5）根据PID查询进程信息
ps -f -p <进程PID>
### （6）查看端口占用进程
lsof -i:<PORT_ID>
 
 
## 四、查看系统用户相关命令
  ### （1）查看指定用户信息 
id <用户名> 
### （2）查看用户登录日志
 last  
### （3）查看系统所有用户 
cut -d: -f1 /etc/passwd 
### （4）查看系统所有组 
cut -d: -f1 /etc/group 


## 五、查询外界设备命令

### （1）列出所有PCI设备
lspci -tv 

### （2）列出所有USB设备 
lsusb -tv 
### （3）查看启动时IDE设备检测状况网络
 dmesg | grep IDE 
### （4）查看计算机名 
 hostname  
 ### （5）查看硬件信息
 1. lshw
 2. lshw -short
 3. dmidecode


## 六、系统磁盘相关查询命令
### （1）查看内存使用量和交换区使用量 
 free -m 
### （2）查看各分区使用情况 
 df -h 
### （3）查看指定目录的大小 
du -sh <目录名> 
### （4）查看内存总量 
grep MemTotal /proc/meminfo 
### （5）查看空闲内存量 
 grep MemFree /proc/meminfo 
### （6）查看系统负载磁盘和分区 
cat /proc/loadavg 
### （7）查看挂接的分区状态 
 mount | column -t 
### （8）查看所有分区 
 fdisk -l 
### （9）查看所有交换分区 
 swapon -s 
### （10）查看磁盘参数(仅适用于IDE设备) 
hdparm -i /dev/hda 

## 七、查询网络相关信息

### （1）查看所有网络接口的属性
ifconfig  
### （2）查看防火墙设置 
 iptables -L 
###  （3）查看路由表 
 route -n 
### （4）查看所有监听端口 
 netstat -lntp 
### （5）查看所有已经建立的连接 
netstat -antp 
###  （6）查看网络统计信息进程
netstat -s  

## 八、使用scp进行不同机器之间的文件拷贝

### （1）将单个文件从本地远程拷贝到目标主机
scp local_file remote_username@remote_ip:remote_folder 
### （2）将文件夹从本地远程拷贝到目标主机
scp -r local_folder remote_username@remote_ip:remote_folder 
###  (3) 将单个文件从远程主机拷贝到本地
scp remote_username@remote_ip:remote_folder/remote_file local_folder
###  (4) 将文件夹从远程主机拷贝到本地
scp -r remote_username@remote_ip:remote_folder local_folder