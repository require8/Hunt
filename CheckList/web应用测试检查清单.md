# 漏洞检查清单

> 这份清单或许能帮助你制定一个良好的漏洞赏金狩猎方法。
> 完成一项操作后，别忘了检查一下 ;)
> 祝你狩猎愉快！



## 目录

- [Recon on wildcard domain](https://github.com/KathanP19/HowToHunt/blob/master/CheckList/Web-Application-Pentesting-checklist.md#"Recon_on_wildcard_domain")
- [Single domain](https://github.com/KathanP19/HowToHunt/blob/master/CheckList/Web-Application-Pentesting-checklist.md#Single_domain)
- [Information Gathering](https://github.com/KathanP19/HowToHunt/blob/master/CheckList/Web-Application-Pentesting-checklist.md#Information)
- [Configuration Management](https://github.com/KathanP19/HowToHunt/blob/master/CheckList/Web-Application-Pentesting-checklist.md#Configuration)
- [Secure Transmission](https://github.com/KathanP19/HowToHunt/blob/master/CheckList/Web-Application-Pentesting-checklist.md#Transmission)
- [Authentication](https://github.com/KathanP19/HowToHunt/blob/master/CheckList/Web-Application-Pentesting-checklist.md#Authentication)
- [Session Management](https://github.com/KathanP19/HowToHunt/blob/master/CheckList/Web-Application-Pentesting-checklist.md#Session)
- [Authorization](https://github.com/KathanP19/HowToHunt/blob/master/CheckList/Web-Application-Pentesting-checklist.md#Authorization)
- [Data Validation](https://github.com/KathanP19/HowToHunt/blob/master/CheckList/Web-Application-Pentesting-checklist.md#Validation)
- [Denial of Service](https://github.com/KathanP19/HowToHunt/blob/master/CheckList/Web-Application-Pentesting-checklist.md#Denial)
- [Business Logic](https://github.com/KathanP19/HowToHunt/blob/master/CheckList/Web-Application-Pentesting-checklist.md#Business)
- [Cryptography](https://github.com/KathanP19/HowToHunt/blob/master/CheckList/Web-Application-Pentesting-checklist.md#Cryptography)
- [Risky Functionality - File Uploads](https://github.com/KathanP19/HowToHunt/blob/master/CheckList/Web-Application-Pentesting-checklist.md#File)
- [Risky Functionality - Card Payment](https://github.com/KathanP19/HowToHunt/blob/master/CheckList/Web-Application-Pentesting-checklist.md#Card)
- [HTML 5](https://github.com/KathanP19/HowToHunt/blob/master/CheckList/Web-Application-Pentesting-checklist.md#HTML)



## 侦察通配符域名



- Run Amass   子域名收集工具

-  Run Subfinder   被动式子域名收集工具
-  Run Rapid7 FDNS  类似与空间测绘工具
-  Use commonspeak2 list  子域名爆破字典
-  Run massdns   dns解析工具(类似与masscan)  
-  Run altdns      扩展猜测子域名工具



## 单独的域名

### 扫描

-  Arachni Scan
-  Owasp ZAp Scan
-  Burp Spider
-  Burp Scanning
-  Wayback machine
-  Linkfinder
-  Url with Android application



### 手动收集

-  Shodan
-  Censys
-  Google dorks   google黑客语法
-  Pastebin
-  Github
-  OSINT



### 信息收集



-  Manually explore the site   手动探测站点
-  Spider/crawl for missed or hidden content    爬取忽略的或者隐藏的内容
-  Check for files that expose content, such as robots.txt, sitemap.xml, .DS_Store  检查暴露文件
-  Check the caches of major search engines for publicly accessible sites  使用搜索引擎检查可公开访问的网站
-  Check for differences in content based on User Agent (eg, Mobile sites, access as a Search engine Crawler)  根据用户代理检查内容差异
-  Perform Web Application Fingerprinting  执行web应用指纹识别
-  Identify technologies used  识别所使用的技术
-  Identify user roles  识别用户角色
-  Identify application entry points  识别应用程序的入口点
-  Identify client-side code  识别客户端代码
-  Identify multiple versions/channels (e.g. web, mobile web, mobile app, web services)  识别不同的版本
-  Identify co-hosted and related applications  识别共同托管和相关应用程序
-  Identify all hostnames and ports  识别所有的主机和端口
-  Identify third-party hosted content  识别第三方托管内容
-  Identify Debug parameters  识别调试参数 类似与test



### 配置管理

检查是否有错误配置

-  Check for commonly used application and administrative URLs   检查常用的应用程序和管理url 例如admin
-  Check for old, backup and unreferenced files  检查旧文件，备份文件和未引用的文件
-  Check HTTP methods supported and Cross Site Tracing (XST)  检查支持的HTTP方法和跨站追踪
-  Test file extensions handling  测试文件扩展名处理
-  Test for security HTTP headers (e.g. CSP, X-Frame-Options, HSTS)  测试安全的HTTP头
-  Test for policies (e.g. Flash, Silverlight, robots)  测试策略
-  Test for non-production data in live environment, and vice-versa  在实际环境中测试非生产数据
-  Check for sensitive data in client-side code (e.g. API keys, credentials) 检查客户端代码中的敏感数据



### 安全传输

-  Check SSL Version, Algorithms, Key length
-  Check for Digital Certificate Validity (Duration, Signature and CN)
-  Check credentials only delivered over HTTPS
-  Check that the login form is delivered over HTTPS
-  Check session tokens only delivered over HTTPS
-  Check if HTTP Strict Transport Security (HSTS) in use



### 认证

-  Test for user enumeration   测试是否可以用户名枚举
-  Test for authentication bypass  测试身份认证绕过
-  Test for bruteforce protection  测试爆破保护
-  Test password quality rules  测试密码规则
-  Test remember me functionality 测试记住我功能
-  Test for autocomplete on password forms/input  测试自动填充在密码输入处的功能
-  Test password reset and/or recovery  测试密码重置
-  Test password change process  测试密码更改流程
-  Test CAPTCHA  测试验证码
-  Test multi factor authentication  测试多因素身份验证
-  Test for logout functionality presence 测试注销功能
-  Test for cache management on HTTP (eg Pragma, Expires, Max-age)  测试HTTP缓存管理
-  Test for default logins  测试默认登录
-  Test for user-accessible authenticati on history  测试用户可访问的身份验证历史记录
-  Test for out-of channel notification of account lockouts and successful password changes  测试账户锁定和密码更改成功的渠道外通知
-  Test for consistent authentication across applications with shared authentication schema / SSO  测试共享身份验证架构跨应用程序的一致性身份验证

### 会话管理

-  Establish how session management is handled in the application (eg, tokens in cookies, token in URL)  确定应用程序中会话管理的处理方式
-  Check session tokens for cookie flags (httpOnly and secure)  检查会话令牌中的cookie标志
-  Check session cookie scope (path and domain) 检查会话cookie范围
-  Check session cookie duration (expires and max-age) 检查会话cookie的持续时间
-  Check session termination after a maximum lifetime 检查会话达到最大生存期后是否终止
-  Check session termination after relative timeout  检查会话在相对超市后是否终止
-  Check session termination after logout 检查会话在注销后是否终止
-  Test to see if users can have multiple simultaneous sessions  测试用户是否可以同时进行多个会话
-  Test session cookies for randomness  检查会话的随机性  
-  Confirm that new session tokens are issued on login, role change and logout   确认在登录、角色更改和注销时都会发出新的会话令牌
-  Test for consistent session management across applications with shared session management   测试使用共享会话管理的应用程序之间的会话管理一致性
-  Test for session puzzling  测试会话混淆
-  Test for CSRF and clickjacking  测试 CSRF 和点击劫持



### 鉴权

-  Test for path traversal  测试路径遍历
-  Test for bypassing authorization schema   测试绕过授权方案
-  Test for vertical Access control problems (a.k.a. Privilege Escalation)  测试垂直访问控制问题
-  Test for horizontal Access control problems (between two users at the same privilege level) 测试水平访问控制问题
-  Test for missing authorization  测试授权缺失



### 数据合法性



-  Test for Reflected Cross Site Scripting   反射型XSS测试
-  Test for Stored Cross Site Scripting  存储型XSS测试
-  Test for DOM based Cross Site Scripting  DOOM型XSS测试
-  Test for Cross Site Flashing   XSF测试----已经过时了，其核心依赖技术flash已经被所有浏览器移除
-  Test for HTML Injection  HTML注入测试
-  Test for SQL Injection  SQL注入测试
-  Test for LDAP Injection LDAP注入测试
-  Test for ORM Injection  ORM注入测试
-  Test for XML Injection  XML注入测试
-  Test for XXE Injection  XXE注入测试
-  Test for SSI Injection SSI注入测试
-  Test for XPath Injection XPath注入测试
-  Test for XQuery Injection XQuery注入测试
-  Test for IMAP/SMTP Injection IMAP/SMTP注入测试
-  Test for Code Injection  代码注入测试
-  Test for Expression Language Injection  扩展语言注入测试
-  Test for Command Injection 命令注入测试
-  Test for Overflow (Stack, Heap and Integer)  溢出测试
-  Test for Format String 格式化字符测试
-  Test for incubated vulnerabilities 常规漏洞测试
-  Test for HTTP Splitting/Smuggling HTTP走私测试
-  Test for HTTP Verb Tampering HTTP请求方法改变测试
-  Test for Open Redirection  开发重定向测试
-  Test for Local File Inclusion 本地文件包含
-  Test for Remote File Inclusion 远程文件包含
-  Compare client-side and server-side validation rules  客户端和服务端验证比较测试
-  Test for NoSQL injection  NoSQL注入测试
-  Test for HTTP parameter pollution  HTTP参数污染测试
-  Test for auto-binding 自动绑定测试
-  Test for Mass Assignment  批量赋值测试
-  Test for NULL/Invalid Session Cookie 测试空会话





### 拒绝服务(DDOS)

- 反自动化测试
- 账户锁定测试
- HTTP 协议 DoS 测试
- SQL 通配符 DoS 测试



## 商业逻辑

- 测试功能滥用
- 测试不可否认性缺失
- 测试信任关系
- 测试数据完整性
- 测试职责分离



### 密码学

- 检查应加密的数据是否未加密
- 检查是否根据上下文使用了错误的算法
- 检查是否使用了弱算法
- 检查是否正确使用了加盐
- 检查随机函数



### 危险功能--文件上传

- 测试可接受的文件类型是否已列入白名单
- 测试文件大小限制、上传频率和文件总数是否已定义并强制执行
- 测试文件内容是否与定义的文件类型匹配
- 测试所有上传文件是否已进行防病毒扫描。
- 测试不安全的文件名是否已清理
- 测试上传的文件是否无法在 Web 根目录下直接访问
- 测试上传的文件是否不在同一主机名/端口上提供服务
- 测试文件和其他媒体是否已集成身份验证和授权方案





### 危险功能-支付功能

- 测试 Web 服务器和 Web 应用上的已知漏洞和配置问题
- 测试默认密码或可猜测密码
- 测试实际环境中的非生产数据，反之亦然
- 测试注入漏洞
- 测试缓冲区溢出
- 测试不安全的加密存储
- 测试传输层保护不足
- 测试不当的错误处理
- 测试所有 CVSS v2 评分 > 4.0 的漏洞
- 测试身份验证和授权问题
- 测试 CSRF



### HTML5

- 测试 Web 消息传递
- 测试 Web 存储 SQL 注入
- 检查 CORS 实现
- 检查离线 Web 应用程序












