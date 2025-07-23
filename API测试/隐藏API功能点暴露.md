# 参考

[原文](https://github.com/KathanP19/HowToHunt/blob/master/API_Testing/Hidden_API_Functionality_Exposure.md)

# 隐藏API功能点暴露

应用程序编程接口 (API) 已成为几乎所有企业的关键组成部分。

API 负责在公司内部系统之间或与外部公司之间传输信息。

例如，当您登录 Google 或 Facebook 等网站时，API 会处理您的登录凭据以验证其正确性。

1. Swagger UI 文档
2. 字典攻击 | 暴力破解
3. API 枚举的常用词汇表：
   - https://wordlists.assetnote.io/
   - https://github.com/Net-hunter121/API-Wordlist

# 攻击实现步骤

````
步骤 1：将请求捕获到 Burp 中，并将其发送到repeater和intruder选项卡。
步骤 2：将端点添加到intruder选项卡，并从wordlist中添加有效载荷。
步骤 3：首先在端点上使用 SecLists (https://github.com/danielmiessler/SecLists) 进行字典攻击。
步骤 4：您可以使用自定义列表，也可以使用我在上面步骤中提供的列表。
步骤 5：然后启动攻击，并检查 200 状态。
步骤 7：一旦 HTTP 200 OK 状态出现，就开始在同一端点上进行递归扫描，以获取有用的信息，例如 Swagger 文档等。
步骤 8：另一种方法是更改 API 版本并尝试对同一端点进行暴力破解。
例如：Redacted.com/api/v1/{Endpoint} ----- Redacted.com/api/v2/{Endpoint}
````



> [!note]
>
> 注意：每个请求都会有最低限制，无需 API 密钥即可分配，因此请确保尽可能使用手动方法，然后其余部分可以使用自动化工具自动扫描 API 中的漏洞。






