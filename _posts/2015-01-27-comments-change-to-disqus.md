---
layout: post
title: Jekyll 设置评论是否显示及 Liquid 代码显示问题
comments: true
---

## 设置评论是否显示

昨天查找资料时看到一个能对页面单独设置是否显示评论的方法，需要设置 YAML 文件头。

方法很简单，在评论代码头尾各自加上 {% raw %}`{% if page.comments %}`{% endraw %} 和 {% raw %}`{% endif %}`{% endraw %} ，大概会是下面这个样子（以多说举例）

    {% raw %}
    {% if page.comments %}
    <!-- 多说评论框 start -->
        <div class="ds-thread" data-thread-key="{{ page.url }}" data-title="{{ page.title }}" data-url="{{site.production_url}}{{ page.url }}"></div>
    <!-- 多说评论框 end -->
    <!-- 多说公共JS代码 start (一个网页只需插入一次) -->
    <script type="text/javascript">
    var duoshuoQuery = {short_name:"liyishan"};
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
    {% endif %}
    {% endraw %}
    
然后在撰写每篇文档和或者页面的时候，如果需要显示评论，则在文件头加上 `comments: true` ，默认不加注或设置为空，则为不在本页显示评论。

    {% raw %}
    ---
    layout: post
    comments: true
    ---
    {% endraw %}

应用这个方法后控制评论框能显得更灵活一些。

## Liquid 代码在 Markdown 中的显示问题

昨晚写如何添加多说评论的文章里加入了较多的类似 {% raw %}`{% page.title %}`{% endraw %} 这样的 Liquid 模板语句，但直接用 Markdown 的行内代码标识时，会导致语句在页面生成时被执行，直接就调用了所生成的页面信息，而不显示源码。

### 方法一

使用 {{ "{%" }} raw %} 和 {{ "{%" }} endraw %} 包裹代码，任何在其之间的内容都会保留，这个方法在整块代码引用时会很方便

    {{ "{%" }} raw %}
    {{ "{%" }} if page.comments %}
    {{ "{%" }} endif %}
    {{ "{%" }} endraw %}
    
上面的写法会显示出下面的代码块

    {% raw %}
    {% if page.comments %}
    {% endif %}
    {% endraw %}
    
### 方法二

这个方法适合行内高亮。比如输入`{{ "{%" }} include %}`，则写成 {% raw %}`{{ "{%" }} include %}`{% endraw %}，如果是`{{ "{{" }} include }}`，就写成 {% raw %}`{{ "{{" }} include }}`{% endraw %}。

如果查看 `yishan.github.io` 上这篇文档的 `.md` 源档，你会发现这两种方法结合着用会顺手一些，不然页面显得太长，编辑时也看得不大清楚。

## 更换 Disqus

昨晚把 多说 评论改为 [Disqus](https://disqus.com/) 了。

## 参考

-    [How I Created a Beautiful and Minimal Blog Using Jekyll, Github Pages, and poole](http://joshualande.com/jekyll-github-pages-poole/)
-    [How to escape liquid template tags?](http://stackoverflow.com/questions/3426182/how-to-escape-liquid-template-tags)
-    [Escape Liquid template tags in Jekyll posts](http://sarathlal.com/escape-liquid-template-tags-in-jekyll-posts/)