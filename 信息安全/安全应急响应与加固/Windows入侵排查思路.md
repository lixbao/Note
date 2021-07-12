# Windows入侵排查思路

![image-20210702184920873](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210712225523.png)

## 1.检查系统账号安全

Ø检查系统是否存在弱口令、可疑账号、新增账号

•1.询问管理员



•2.查看本地用户和组（cmdàlusrmgr.msc），禁用或者删除新增或者可疑账号

•3.查看隐藏账号和克隆账号

◆查看注册表HKEY_LOCAL_MACHINE\SAM\SA

◆D盾查杀M\Domains\Account\Users

## 2.检测异常端口

Ø检查端口连接情况，是否有远程连接、可疑连接。

•netstat -ano 查看目前的网络连接，定位可疑的ESTABLISHED

•根据netstat 定位出的pid，再通过tasklist命令进行进程定位 tasklist | findstr “PID”

Ø检查进程

•开始--运行--输入msinfo32，依次点击“软件环境→正在运行任务”就可以查看到进程的详细信息，比如进程路径、进程ID、文件创建日期、启动时间等。

