# 如何在 1panel 静态网站中启用 IP 显示

由于静态网站（纯 HTML/JS）没有后端处理能力，无法直接获取访问者的 IP 地址。为了实现这一功能，我们需要利用 Nginx 服务器的能力来直接返回客户端 IP。

请按照以下步骤在 1panel 中配置：

1. **登录 1panel 面板**。
2. 进入 **网站** 菜单，找到您的测速网站。
3. 点击 **配置** -> **配置文件**（这将打开 Nginx 配置文件编辑器）。
4. 在 `server { ... }` 块内部，找到 `location` 相关的配置区域（通常在 `root` 或 `index` 指令下方），添加以下代码块：

```nginx
    # 用于返回访问者 IP
    location /getIP {
        default_type text/plain;
        return 200 $remote_addr;
    }
```

5. 点击 **保存** 并 **重载** Nginx 配置。
6. 回到测速页面刷新，现在应该能正确显示访问者的 IP 了（无论是内网还是公网 IP）。

## 原理说明
这段配置利用了 Nginx 的内置变量 `$remote_addr`，当浏览器请求 `/getIP` 路径时，Nginx 会直接以文本形式返回客户端的 IP 地址，而不需要 PHP 或其他后端语言的支持。
