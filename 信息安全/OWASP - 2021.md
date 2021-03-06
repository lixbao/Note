#	OWASP介绍

- 官网：http://www.owasp.org.cn/
- OWASP是一个开源的、非盈利的全球性安全组织，致力于应用软件的安全研究。我们的使命是使应用软件更加安全，使企业和组织能够对应用安全风险做出更清晰的决策。目前OWASP全球拥有250个分部近7万名会员，共同推动了安全标准、安全测试工具、安全指导手册等应用安全技术的发展。
- 近几年，OWASP峰会以及各国OWASP年会均取得了巨大的成功，推动了数以百万的IT从业人员对应用安全的关注以及理解，并为各类企业的应用安全提供了明确的指引。

# 2021 OWASP TOP 10

+ A01：失效的访问控制
+ A02：加密机制失效
+ A03：注入
+ A04：不安全设计
+ A05：安全配置错误
+ A06：自带缺陷和过时的组件
+ A07：身份识别和身份验证错误
+ A08：软件和数据完整性故障
+ A09：安全日志和监控故障
+ A010：服务端请求伪造



官方详细说明

| 排名 |                    名称                    |                             说明                             |
| :--: | :----------------------------------------: | :----------------------------------------------------------: |
| A01  |           Broken Access Control            | 从第5位上升成为Web应用程序安全风险最严重的类别；提供的数据表明，平均3.81%的测试应用程序具有一个或多个CWE，且此类风险中CWE总发生漏洞应用数超过31.8万次。在应用程序中出现的34个匹配为“失效的访问控制”的CWE次数比任何其他类别都多。 |
| A02  |           Cryptographic Failures           | 排名上升一位。其以前被称为“A3:2017-敏感信息泄漏（Sensitive Data Exposure）”。敏感信息泄漏是常见的症状，而非根本原因。更新后的名称侧重于与密码学相关的风险，即之前已经隐含的根本原因。此类风险通常会导致敏感数据泄露或系统被攻破。 |
| A03  |                 Injection                  | 排名下滑两位。94%的应用程序进行了某种形式的注入风险测试，发生安全事件的最大率为19%，平均率为3.37%，匹配到此类别的33个CWE共发生27.4万次，是出现第二多的风险类别。原“A07:2017-跨站脚本（XSS）”在2021年版中被纳入此风险类别。 |
| A04  |              Insecure Design               | 2021年版的一个新类别，其重点关注与设计缺陷相关的风险。如果我们真的想让整个行业“安全左移” ，我们需要更多的威胁建模、安全设计模式和原则，以及参考架构。不安全设计是无法通过完美的编码来修复的；因为根据定义，所需的安全控制从来没有被创建出来以抵御特定的安全攻击。 |
| A05  |         Security Misconfiguration          | 排名上升一位。90%的应用程序都进行了某种形式的配置错误测试，平均发生率为4.5%，超过20.8万次的CWE匹配到此风险类别。随着可高度配置的软件越来越多，这一类别的风险也开始上升。原“A04:2017-XML External Entities(XXE) XML外部实体”在2021年版中被纳入此风险类别。 |
| A06  |     Vulnerable and Outdated Components     | 排名上升三位。在社区调查中排名第2。同时，通过数据分析也有足够的数据进入前10名，是我们难以测试和评估风险的已知问题。它是唯一一个没有发生CVE漏洞的风险类别。因此，默认此类别的利用和影响权重值为5.0。原类别命名为“A09:2017-Using Components with Known Vulnerabilities 使用含有已知漏洞的组件”。 |
| A07  | Identification and Authentication Failures | 排名下滑五位。原标题“A02:2017-Broken Authentication失效的身份认证”。现在包括了更多与识别错误相关的CWE。这个类别仍然是Top 10的组成部分，但随着标准化框架使用的增加，此类风险有减少的趋势。 |
| A08  |    Software and Data Integrity Failures    | 2021年版的一个新类别，其重点是：在没有验证完整性的情况下做出与软件更新、关键数据和CI/CD管道相关的假设。此类别共有10个匹配的CWE类别，并且拥有最高的平均加权影响值。原“A08:2017-Insecure Deserialization不安全的反序列化”现在是本大类的一部分。 |
| A09  |      Logging and Monitoring Failures       | 排名上升一位。来源于社区调查（排名第3）。原名为“A10:2017-Insufficient Logging & Monitoring 不足的日志记录和监控”。此类别现扩大范围，包括了更多类型的、难以测试的故障。此类别在 CVE/CVSS 数据中没有得到很好的体现。但是，此类故障会直接影响可见性、事件告警和取证。 |
| A10  |        Server-Side Request Forgery         | 2021年版的一个新类别，来源于社区调查（排名第1）。数据显示发生率相对较低，测试覆盖率高于平均水平，并且利用和影响潜力的评级高于平均水平。加入此类别风险是说明：即使目前通过数据没有体现，但是安全社区成员告诉我们，这也是一个很重要的风险。 |


