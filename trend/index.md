# 








{{< admonition type=quote title="2021.01.27，跨站脚本攻击（Cross-Site Scripting, XSS）" open=false >}}

参考：[https://www.zhihu.com/search?type=content&q=跨站脚本攻击](https://www.zhihu.com/search?type=content&q=跨站脚本攻击)

### 跨站脚本攻击（Cross-Site Scripting, XSS）

在web页面插入恶意脚本，当用户浏览页面时，促使脚本执行，从而达到攻击目的。XSS的特点就是想尽一切办法在目标网站上执行第三方脚本。

**栗子**：原有的网站有个将数据库中的数据显示到页面上的功能，`document.write("data from server")`。但如果服务器没有验证数据类型，直接接受任何数据时，攻击者可以会将`<script src='http:bad-script.js'></script>`当做一个数据写入数据库。当其他用户请求这个数据时，网站原有的脚本就会执行`document.write("<script src='http:bad-script.js'></script>")`。这样，便会执行`bad-script.js`。如果攻击者在这段第三方的脚本中写入恶意脚本，那么普通用户便会受到攻击。

#### XSS的三种类型：

- 存储型/持久型XSS（Persistent XSS）：注入的脚本永久地存储在目标服务器上，每当受害者向服务器请求此数据时就会重新唤醒攻击脚本；

- 反射型XSS（Reflected XSS）：当用受害者被引诱点击一个恶意链接，提交一个伪造的表单，恶意代码便会和正常返回数据一起作为响应发送到受害者的浏览器，从而骗过了浏览器，使之误以为恶意脚本来自可信的服务器，以至于让恶意脚本得以执行。

- DOM型XSS（DOM XSS）：有点类似于存储型 XSS，但存储型 XSS 是将恶意脚本作为数据存储在服务器中，每个调用数据的用户都会受到攻击。但 DOM 型 XSS 则是一个本地的行为，更多是本地更新 DOM 时导致了恶意脚本执行。

#### 防御XSS攻击的方法：

- 从客户端和服务器端双重验证所有的输入数据，这一般能阻挡大部分注入的脚本

- 对所有的数据进行适当的编码
- 设置 HTTP Header： "X-XSS-Protection: 1"

{{< /admonition >}}



{{< admonition type=note title="2021.01.27，近期推文计划" open=true >}}

- [ ] EDA，一种文本数据增强方法
- [ ] 卡片校正算法
- [ ] 一个数独算法
- [ ] LMSI栏目下周起恢复推送

{{< /admonition >}}



{{< admonition type=info title="2021.01.26，施工进度自查" open=true >}}

- [x] 初始化，选用Hugo-CodeIT主题
- [x] 网站信息基础配置，个性化样式（字号、色彩风格等）
- [x] 添加几篇文章
- [x] 小站运行天数foot
- [x] 网站Logo  
- [x] 域名申请
- [x] Netlify部署
- [x] 新增『动态/Trends』栏目
- [x] 『关于』页面
- [ ] 几个栏目的Markdown模板
- [ ] CI/CD工作流
- [ ] 加载速度优化（CDN加速/图片压缩）
- [ ] 评论系统及相关组件测试
- [ ] 添加文章置顶标记🔝
- [ ] 新增Tiddlywiki
- [ ] 404页面
- [ ] 英文网页支持配置
- [ ] 新增『项目/Project』栏目
- [ ] 新增『相册/Photos』栏目
- [ ] 彩蛋与一个隐藏页面

{{< /admonition >}}










