---
layout: post
title: 给 Jekyll 添加评论插件
comments: true
---

捣鼓着给文章页加上了[多说](http://www.duoshuo.com/)评论，参考了部分教程，同时对 Jekyllbootstrap 关于页面功能调用和 Liquid 模板的使用方法有了一些了解。添加评论插件的方法不难，主要还是花时间了解页面调用的方法，以及观摩 [JekyllBootstrap](https://github.com/plusjade/jekyll-bootstrap/) 的源码去了。

## 获取代码

多说主页上有明显的『我要安装』按钮，点击后进入创建站点页面，填好相关信息后会看到生成的默认代码

    <!-- 多说评论框 start -->
        <div class="ds-thread" data-thread-key="请将此处替换成文章在你的站点中的ID" data-title="请替换成文章的标题" data-url="请替换成文章的网址"></div>
    <!-- 多说评论框 end -->
    <!-- 多说公共JS代码 start (一个网页只需插入一次) -->
    <script type="text/javascript">
    var duoshuoQuery = {short_name:"short_name"};
        (function() {
            var ds = document.createElement('script');
            ds.type = 'text/javascript';ds.async = true;
            ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
            ds.charset = 'UTF-8';
            (document.getElementsByTagName('head')[0] 
             || document.getElementsByTagName('body')[0]).appendChild(ds);
        })();
        </script>
    <!-- 多说公共JS代码 end -->
    
同个页面左侧能看到几个标签，其中 `设置` 标签里可以对评论插件的基本设置、自定义文本、评论默认头像和外观等做些调整。比较有用的一条是把默认显示的四种版权信息显示给去掉，在 `自定义CSS` 一栏内输入 `#ds-rest .ds-powered-by {display:none;}` 就行。这样做的好处是评论框还能省出一行文字的空间。

## 配置方法一

页面上对这段代码的使用有这么一句说明 `复制以下代码，并粘帖到您网页代码<body>与</body>间的任意位置。如果您的网站使用模板，请粘帖到模板代码中。` 最简单的方法就是把整段代码插入 `post.html` 里 `<body>` 与 `</body>` 间的任意位置，通常放在 `{{ content }}` 文章主体之后。

粘贴好代码后，需要把站点的文章标题、网址等参数替换掉默认内容，对 Jekyll 所使用的模板系统来说，`data-thread-key` 应填入 {% raw %}`{{ page.url }}`{% endraw %}，{% raw %}`data-title`{% endraw %} 填入 {% raw %}`{{ page.title }}`{% endraw %}，`data-url` 填入 {% raw %}`{{site.production_url}}{{ page.url }}`{% endraw %} 即可。最后把 {% raw %}`var duoshuoQuery = {short_name: " "};`{% endraw %} 一句中的 short_name 填入先前指定的内容。

站点更新后稍等片刻，文章底部就会出现多说的评论框了。

不过当文章页面有较多自定义控件，所有代码都直接贴进 `post.html` 的方法，会让更新、维护页面变成一件繁琐的事情。为此参考了另一篇在 Jekyllbootstrap 模板基础上添加多说评论的文章，顺便学习了一点 Liquid 模板的使用方法。

## 配置方法二

Jekyllbootstrap（下称 `JB` ）加载评论的思路是代码与页面分离，将评论看做组成页面的元素之一，而不写死在页面模板中，需要时调用相应内容即可。而由于提供了多种评论插件的示范用法，用户稍加改动就可以使用，而多种评论插件使用的不确定性，让 JB 多使用了一层逻辑判断。

### 修改 _config.yml

首先看 `_config.yml` ，需要在 `comments` 下加入评论插件标识

    comments :
        provider : disqus
        disqus :
          short_name : jekyllbootstrap
        livefyre :
          site_id : 123
        intensedebate :
          account : 123abc
        facebook :
          appid : 123
          num_posts: 5
          width: 580
          colorscheme: light

多说采用与 Disqus 同样的 `short_name` 来识别信息，照着 Disqus 那两行来写就可以，然后把 `provider` 一行的数值修改为 `duoshuo`

    comments :
        provider : duoshuo
        duoshuo :
          short_name : short_name
        disqus :
          short_name : short_name
          
这里使用多说及 Disqus 作为待选，并选择多说作为站点评论。

### 创建判断语句

接着在 `_includes` 文件夹下新建任意名称文件夹，这里使用的是 `custom`，并在文件夹里新建 `comments` 文档，然后在文档中添加一下判断语句

    {% raw %}
    {% case site.comments.provider %}

    {% when "duoshuo" %}
      {% include social/duoshuo.html %}
    {% when "disqus" %}
      {% include social/disqus.html %}

    {% endcase %}
    {% endraw %}    

这个文档将告诉 Jekyll 在读取 `_config.yml` 时判断 `comments > provider` 的信息，并执行相应的动作，在这里也就是插入 {% raw %}`{% include social/duoshuo.html %}`{% endraw %} 或者 {% raw %}`{% include social/disqus.html %}`{% endraw %}

### 创建评论代码文档

之后同样在 `_includes` 文件夹下新建 `social` 文件夹，并在 `social` 里各自新建 `duoshuo.html` 和 `disqus.html` 文档，分别填入评论代码。

这里的文件夹以及文档可以任意取名，但需要与 `comments` 文档里的语句对得上。

完成后 `_includes` 下的文档结构大概是下面这样

    .
    ├── _includes
    |   └── custom
    |       └── comments
    └── social
        ├── duoshuo.html
        └── disqus.html
        
### 将评论加入 post.html

最后是修改 `post.html` 文章页（或其它需要出现评论的页面），在文档恰当地方加入 {% raw %}`{% include custom/comments %}`{% endraw %} 来显示评论。

整个步骤完成后，评论代码的调用逻辑就变成了 `文章页调用判断文档 > 读取 _config.yml 对应信息 > 执行判断语句 > 调用代码文档`。

这样的处理可以在更新评论插件的内容时无需对 `post.html` 做出更改，让 `post.html` 仅作为规划页面内容的模板来使用，而不涉及具体内容的调整，同时还加强了调用的灵活性。

## 其它 & 参考

添加多说后，以类似的加载方法尝试着给文章添加了分享按钮和 Creative Common 信息，一切顺利。最后感觉页面略重，就把分享按钮从页面上去掉了，只留着源文档，之后需要时再链接文档即可。这样操作相比直接给 `post.html` 写入分享代码要清爽一些。

### 参考

-    [为 Jekyll 添加多说评论系统](http://havee.me/internet/2013-07/add-duoshuo-commemt-system-into-jekyll.html)
-    [Jekyll 使用多说评论系统](http://liberize.me/tech/jekyll-use-duoshuo-comment-system.html)
-    [为Jekyll添加多说评论系统](http://www.anaharb.com/2014/0215/Jekyll-duoshuo-comments/)