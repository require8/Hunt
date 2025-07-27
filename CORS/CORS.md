# 参考

[原文](https://github.com/KathanP19/HowToHunt/blob/master/CORS/CORS.md)



# CORS配置错误

>什么是CORS？
>
>CORS (Cross-Origin Resource Sharing)，即“跨源资源共享”，是一种基于 HTTP 头的机制，它允许服务器指示浏览器，除了自己的源（域、协议或端口）之外，还有哪些源可以加载资源



## 测试方法1

```
步骤->1. 捕获目标网站并使用 Burp 进行爬取或抓取整个网站。
步骤->2. 使用 Burp 搜索 Access-Control
步骤->3. 尝试添加 Origin 标头，例如 Origin:attacker.com、Origin:null、Origin:attacker.target.com 或 Origin:target.attacker.com
步骤->4 如果响应中显示来源，则表示目标存在 CORS 漏洞
```



## 测试方法2

```
步骤 1-> 查找域名，例如 subfinder -d target.com -o domains.txt
步骤 2-> 检查活跃域名：cat domains.txt | httpx | tee -a alive.txt
步骤 3-> 将每个活跃域名发送到 burp，例如 cat alive.txt | parallel -j 10 curl --proxy "http://127.0.0.1:8080" -sk 2>/dev/null
步骤 4-> 重复搜索方法 1
```



# 自动化测试

## 工具

- https://github.com/chenjj/CORScanner
- https://github.com/lc/theftfuzzer
- https://github.com/s0md3v/Corsy
- https://github.com/Shivangx01b/CorsMe



## 方法

```
步骤 1-> 查找域名，例如：subfinder -d domain.com -o target.txt
步骤 2-> grep alive：cat target.txt | httpx | tee -a alive.txt
步骤 3-> 使用 @tomnomnom 的 waybackurls 和 gau 工具 grep 所有 URL，例如：cat alive.txt | gau | tee -a urls.txt
步骤 4-> 在每个 URL 上运行以下任意工具
步骤 5-> 手动配置
```



# 额外的方法

## 工具

- https://github.com/tomnomnom/meg
- https://github.com/tomnomnom/gf
- https://github.com/projectdiscovery/subfinder
- https://github.com/tomnomnom/assetfinder
- https://github.com/Edu4rdSHL/findomain
- https://github.com/projectdiscovery/httpx



# 方法

```
1) 使用 subfinder、assetfinder 和 findomain 查找域名，例如：subfinder -d target.com | tee -a hosts1、findomain -t target.com | tee -a hosts1、assetfinder --subs-only target.com |tee -a hosts1。
2) 然后 cat hosts1 | sort -u | tee -a hosts2，然后 cat hosts2 | httpx | tee -a hosts。
3) 通过终端导航到 hosts 文件所在的位置，执行 echo "/" > 路径
4) 然后输入 meg -v
5) 完成后，输入 gf cors。
6) 所有带有 Access-Control-Allow 的 URL 都会显示出来。
```








