---
title: hexo安装实用插件
date: 2016-11-08 19:53:53
tags: Tools
---


### 支持数学公式

<br>

参见 [hexo中插入数学公式](https://dashen.tech/2021/02/21/hexo%E4%B8%AD%E6%8F%92%E5%85%A5%E6%95%B0%E5%AD%A6%E5%85%AC%E5%BC%8F/)



<br>



### 使hexo中的Markdown支持脚注功能


<br>

卸载hexo默认有渲染器：

`npm un hexo-renderer-marked --save`

安装 hexo-renderer-markdown-it：

`npm i hexo-renderer-markdown-it --save`



(使用该插件并用如下配置，会使得`###`后面的内容在前面多一个段落起始符`¶`)

<br>

在根目录的 **_config.yml**中添加：

```yaml
# Markdown-it config
## Docs: https://github.com/celsomiranda/hexo-renderer-markdown-it/wiki
markdown:
  render:
    html: true
    xhtmlOut: false
    breaks: true
    linkify: true
    typographer: true
    quotes: '“”‘’'
  plugins:
    - markdown-it-abbr
    - markdown-it-footnote
    - markdown-it-ins
    - markdown-it-sub
    - markdown-it-sup
  anchors:
    level: 2
    collisionSuffix: 'v'
    permalink: true
    permalinkClass: header-anchor
    #permalinkSymbol: ¶
    permalinkSymbol: ¶

    
```


<details>
<summary><b>详解：</b></summary>


```yaml
# Markdown-it config
## Docs: https://github.com/celsomiranda/hexo-renderer-markdown-it/wiki
markdown:
  # 渲染设置
  render:
    # 置为true时，html内容保持不变；置为false时，html内容将被转义成普通字符串
    html: true
    # 是否生成与XHTML完全兼容的标签（虽然我不懂是什么意思）
    xhtmlOut: false
    # 置为true时，每个换行符都被渲染成一个<br>（即Hexo的默认表现）；置为false时，只有空行才会被渲染为<br>（GFM的默认表现）
    breaks: true
    # 是否自动识别链接并把它渲染成链接
    linkify: true
    # 是否自动识别印刷格式（意思是把(c)渲染为©这样的）
    typographer: true
    # 如果typographer被设置为true，则该选项用于设置将dumb quotes（""）自动替换为smart quotes
    quotes: '“”‘’'
  # 设置所需插件
  plugins:
    - markdown-it-abbr
    - markdown-it-footnote
    - markdown-it-ins
    - markdown-it-sub
    - markdown-it-sup
  # 锚点设置（因为我没有尝试相关内容，所以就不翻译相关说明了）
  anchors:
    level: 2
    collisionSuffix: 'v'
    permalink: true
    permalinkClass: header-anchor
    permalinkSymbol: ¶
```

</details>

<br>


#### 脚注 效果如下：

Footnotes[^1] have a label[^label] and a definition[^DEF].

[^1]: This is a footnote
[^label]: A footnote on "label"
[^DEF]: The definition of a footnote.


#### 下标 效果如下：




sup x^2^

#### 上标 效果如下：

ins Inserted

#### 下划线 效果如下：


https://github.com/hexojs/hexo-renderer-markdown-it



[为hexo添加上标、下标、脚注等功能](https://jovi.uxlib.net/add-superscript-subscript-footnote-and-other-functions-to-hexo.html/#%E7%AC%AC%E4%B8%80%E6%AD%A5%E5%AE%89%E8%A3%85%E6%8F%92%E4%BB%B6)

[为Hexo博客添加脚注插件](https://zhanghuimeng.github.io/post/add-footnote-plugin-for-hexo-blog/)