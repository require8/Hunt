
# 搜索简单的CVE

## 工具

- `Google`
- `Twitter`
- `Nuclei`



## 步骤

1. 获取所有的子域名

```
subfinder -d domain.com -o subs.txt
```

2. 获取所有存活的子域名

```
httpx -l subs.txt -mc 200 -o alive.txt
```

3. 使用nuclei扫描不同种类的模板并存储在不同的文件中

```
nuclei -l alive.txt -t nuclei-templates/http/misconfiguration -o misconfigurations.txt
nuclei -l alive.txt -t nuclei-templates/http/exposed-panels -o exposed-panels.txt
nuclei -l alive.txt -t nuclei-templates/http/cves -o cves.txt
nuclei -l alive.txt -t nuclei-templates/http/technologies -o technologies.txt
```

4. 耐心仔细阅读每条输出。

5. 查找目标使用的有趣技术（例如 Jira、WordPress 等）。

6. 访问页面并检查所使用的版本。

7. 使用该版本在 Google 上搜索，例如：`jira <version> exploit`

8. 查找看起来有希望的 CVE ID。

9. 在 Twitter 上搜索 CVE`（CVE-XXXX-XXXX poc 或 CVE-XXXX-XXXXexploit）`

10. 在 Google 上搜索 CVE 或漏洞关键词，获取更准确的 PoC 或报告。

11 测试所有 CVE——如果成功，请报告！
