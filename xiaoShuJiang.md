## markdown利器-小书匠


### 为什么要用markdown语法编写文档？

```text
编写文档的好处这里就不多说了。相信很多人都会在一些博客网站上发布自己的博客，那么怎么能使得自己的博客内容更加具有通用性呢？正如java和docker所说的，一次编译到处运行。博主这里就强烈给大家推荐使用markdwon标记语言，它旨在通过一些简单的标记语法，就能够使普通文本具有一定的格式，并且主流的博客或者开发平台都支持，如博客园，csdn，简书，语雀，github，gitee等。

```

### 为什么要使用小书匠？

![小书匠界面](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595645713396.png)

```text
上图就是博主在编写文档时截的小书匠的编辑界面。小书匠的功能十分丰富，下面我简单的介绍一下，在编写过程中我主要用到小书匠的一些功能。
```

#### 免费开源
```text
小书匠编辑器是一款免费，且开源的软件。
这里附上小书匠的github地址：https://github.com/suziwen/markdownxiaoshujiang/releases/tag/v8.2.2
百度云下载地址：https://pan.baidu.com/share/init?surl=Yf8m3PBtQYD4uMKSDAivZw#list/path=%2F 提取码：xyf3
```

####  提供第三方保存功能

![第三方保存](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595645563226.png)
```text
用markdown语法编写文档时，难免会在文档中插入一些截图。但是一般情况下，你在某个博客平台上面粘贴了截图，这时候你再将截图复制到其它平台的时候，你会发现图片不能显示。如果你要在每个平台上面都上传一下图片，那未免有些繁琐了。这里就可以使用小书匠的第三方同步功能。
```

怎么保存？请看[图片保存解决方案](#picSaveSolution)

#### 提供文章发布功能

![文章发布功能](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595645660368.png)
```text
小书匠贴心的准备了文章发布功能，这里使用的是用户名密码登录方式，虽然有点不安全，但是我们主要侧重的是功能的使用。
```
功能配置

```text
博主这里以博客园为例，配置完之后点击发布，就可以到博客园去查看自己的文章了。如博主的博客园地址是：https://www.cnblogs.com/chenhaoblog/
```

###  <span id="picSaveSolution">图片保存解决方案</span>
```text
小书匠提供了非常多的图床对接服务，博主主要试过了千牛云和gitee来作为图片的存储。千牛云个人认证之后，可以享受每月10G的存储容量，但是需要提供域名。因此，博主这里推荐使用gitee来作为图片存储。
```
#### 创建gitee项目
![创建gitee项目](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595646584609.png)
```text 
是否开源 -> 选择公开，其它正常填写即可。
```

#### 获取个人令牌
![私人令牌](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595646844182.png)
```text
在设置->私人令牌中生成个人私人令牌，保管好后右键复制，方便下文操作。
```

#### 在小书匠中填写gitee令牌

在小书匠编辑器中点击左上角 小书匠图标

![绑定界面](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595647174532.png)

进行gitee绑定

![添加绑定](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595646994643.png)

```text
这里需要注意的就是url前缀的填写，博主的前缀是https://gitee.com/chenhaogit/blogimages/raw/master，小伙伴们自行替换其中的用户{chenhaogit}和仓库{blogimages}。
```

#### enjoy it

```text
绑定成功之后，就到了激动人心的一刻了。小伙伴们可以使用截图工具，然后将其粘贴到小书匠编辑器上。成功的小伙伴们，可以看见图片的正常显示，并且复制内容到其它博客平台上也能够正常显示。对，就是那句话，一次编写，到处发布。
```