# 参考
[原文](https://github.com/KathanP19/HowToHunt/blob/master/API_Testing/Reverse_Engineer_an_API.md)
# 目的
为了逆向出一份清晰可用的API文档
# 使用的工具
1.FoxyProxy
2.mitmweb
3.mitmproxy2swagger
4.https://editor.swagger.io/
5.Postman

# 步骤
1.**Foxproxy**:开启本地8080端口代理.
2.**mitmproxy**：使用`sudo mitmweb` 来启动和安装，并且导入https的证书.
3.**访问网站**: 访问想要收集API端点的网站,尽可能多地点击功能点. mitmweb会捕获http数据流历史文件,通过点击"文件->全部保存"将捕获的数据下载为mitmweb中的流文件
4.**mitmproxy2swagger**: 使用命令`sudo mitmproxy2swagger -i flows -o spec.yml -p <website api> -f flow`,这会将流文件转换为yml文件,需要删除yml文件中的`ignore` 
                         使用命令`sudo mitmproxy2swagger -i flows -o spec.yml -p <website api> -f flow --examples`,`--examples`参数会会增强该端点文档
5.https://editor.swagger.io/: 现在可以导入干净的 spec.yml 文件并可视化不同的端点.
6.**Postman**：在 Postman 中导入 spec.yml，这将生成一个有序的api端点集合。
