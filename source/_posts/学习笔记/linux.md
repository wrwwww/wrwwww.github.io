 git clone 拉取
ipconfig /all
ping  -t -1 128   ctri c 结束
查看局域网 arp -a
netstat -a -n 端口
dir 类似目录 ls
UID：启动这些进程的用户。
PID：进程的进程 ID。
PPID：父进程的进程号（如果该进程是由另一个进程启动的）。
C：进程生命周期中的 CPU 利用率。
STIME：进程启动时的系统时间。
TTY：进程启动时的终端设备。
TIME：运行进程需要的累计 CPU 时间。
CMD：启动的程序名称。

(1) 源码编译安装的qemu需要手动卸载：  
可执行文件默认放在/usr/local/bin  
库文件默认存放在/usr/local/libexec  
配置文件默认存放在/usr/local/etc  
共享文件默认存放在/usr/local/share
或者
 sudo apt-get remove --auto-remove qemu-system-x86