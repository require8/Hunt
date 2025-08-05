
# 总结

当用户在 example.com 上传图片时，图片的 EXIF 地理位置数据不会被剥离。因此，任何人都可以获取 example.com 用户的敏感信息，例如地理位置、设备信息（例如设备名称、版本、软件及其所用软件版本等）。

## 测试步骤

1. 前往 Github (https://github.com/ianare/exif-samples/tree/master/jpg)
2. 有很多图片的分辨率（例如 1280 * 720），大小也各不相同。
3. 点击网站上的“上传”选项
4. 上传图片
5. 查看上传图片的路径（右键点击图片，然后复制图片地址，或者右键点击图片，检查图片，URL 会出现在检查窗口中，将其编辑为 HTML 格式）
6. 打开 (http://exif.regex.info/exif.cgi)
7. 查看是否还显示 exif 数据，如果还显示，请报告。

# 相关案例报告

- [IDOR with Geolocation data not stripped from images](https://hackerone.com/reports/906907)

