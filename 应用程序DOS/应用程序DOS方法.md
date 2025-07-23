# 说明

该笔记只作为教育和学习的目的，DDOS是要吃国家饭的

# 参考

[原文](https://github.com/KathanP19/HowToHunt/blob/master/Application_Level_DoS/ALD_Methods.md)

\- Email Bounce Issues

- https://medium.com/bugbountywriteup/an-unexpected-bounty-email-bounce-issues-b9f24a35eb68

\- Long Password DoS Attack

- https://www.acunetix.com/vulnerabilities/web/long-password-denial-of-service/
- https://hackerone.com/reports/738569
- https://hackerone.com/reports/167351

\- Long String DOS

- https://medium.com/@shahjerry33/long-string-dos-6ba8ceab3aa0
- https://hackerone.com/reports/764434

\- Permanent DOS to victim

- https://youtu.be/5drIMXCQuNw

# 1.邮件退回问题

```
检查应用程序是否具有邀请功能
尝试向无效的邮箱账户发送邀请
尝试查找邮箱服务提供商，例如 AWS SES、Hubspot、Campaign Monitor。注意：您可以通过检查邮件标头来查找邮箱服务提供商。
找到邮箱服务提供商后，请检查其硬退信限制。以下是部分服务提供商的限制：
1. Hubspot 硬退信：HubSpot 的硬退信限制为 5%。作为参考，许多 ISP 更倾向于将退信率控制在 2% 以下。
2. AWS SES：SES 的退信率最初在 2-5% 之间，之后在 5-10% 之间。
影响：一旦达到硬退信限制，邮箱服务提供商将屏蔽该公司，这意味着用户将无法收到任何邮件！
```

补充:

> **硬退信” (Hard Bounces)**: 攻击者利用这个邀请功能，故意向大量无效的、不存在的电子邮件地址发送邀请。当邮件服务器尝试向一个不存在的地址发送邮件时，会收到一个永久投递失败的错误，这被称为“硬退信”。



>为什么会触发？
>
>大多数公司不自己搭建邮件服务器，而是使用专业的第三方邮件服务提供商（ESP），如 **AWS SES**、**Hubspot** 等。为了维护自己服务器IP地址的信誉（不被当成垃圾邮件发送者），这些 ESP 对其客户的“硬退信率”有非常严格的限制（通常在 5%-10% 之间）



# 2.长密码DOS攻击

````
由于密码值经过哈希处理后存储在数据库中。如果密码长度没有限制，则会导致对长密码进行哈希处理时资源消耗。

如何测试？
使用长度约为 150-200 个单词的密码来检查是否存在长度限制。
如果没有限制，请选择一个较长的密码并关注响应时间。
检查应用程序是否会崩溃几秒钟。

在哪里测试？
注册密码字段通常受到限制，但“忘记密码”页面上的密码长度和“以经过身份验证的用户身份更改密码”功能通常缺失。
````



# 3.长字符串攻击

```
当你设置一个过长的字符串，服务器无法处理时，有时会引发 DOS 攻击。

如何测试
创建应用程序，并添加字段，例如用户名、地址，甚至头像名称参数（第二个引用），例如 1000 个字符的字符串。
从 B 的账户搜索 A 的账户，

它会持续搜索很长时间，

应用程序会崩溃（500 错误代码）
```



# 4.对受害者的永久DOS攻击

```
这不是应用程序级 DOS，而是对受害者的永久 DOS 攻击。在某些网站，用户尝试使用错误的凭据登录后会被封禁。我们将此漏洞利用为漏洞 :D。

如何检查：
前往 example.com 的登录页面。
现在输入有效的账户邮箱和错误的密码。
尝试使用这些信息登录几次（至少 10-20 次）。您可以使用 Burpsuite 中的repeater或intruder。
如果您的账户被封禁，请检查封禁时长。如果封禁时长超过 30 分钟，您可以报告。

注意事项：
确保登录时没有验证码，因为我们无法制作任何自动化工具来循环请求。
确保封禁后旧会话已过期。

此漏洞的优先级是多少？
如果用户在多次错误尝试后被永久封禁，则视为 P2 漏洞。
如果用户被暂时封禁，则被视为 P3/P4。
在报告中，尝试通过说明您可以通过以一定间隔循环发送此请求来永久封禁用户帐户，从而增加影响力。
```




