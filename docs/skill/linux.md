# Linux 常用命令
注：表格中使用的 \ 为 `|`

|命令    |说明    |
| ------ |:------:|
| `nslookup www.baidu.com` | 查看域名解析 |
| `ps -ef\grep tomcat` | 查看 tomcat 进程 |
| `kill port` | 终止进程 |
| `kill -9 port` | 强制终止进程 |
| `nohup ./start.sh &` | 在后台不间断的运行 |
| `chomd +** **.sh` | 给文件增加权限 |
| `cat /dev/null >catalina.out` | 清空文件日志 |
| `echo hello > 1.txt` | 1.txt 文件中会记录 hello 信息|
| `tar -cvf /bak/1.tar /data.../1` | 压缩：将名称为 1 的文件夹压缩成 1.tar |
| `tar -xvf /bak/a.tar -C /data.../a` | 解压：将名称为 1.tar 的文件解压到 a 文件夹下  |
| `ssh ip -p port -v ` | 查看网络端口策略是否连通 |
| `scp -r /etc root@192.168.60.135:/opt` | 远程拷贝 |
