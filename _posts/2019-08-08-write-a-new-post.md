---
title: 写一篇新文章（机翻）
author:
  name: Cotes Chung
  link: https://github.com/cotes2020
date: 2022-02-07 14:10:00 +0800
categories: [博客]
tags: [博客,Jekyll,写作]
render_with_liquid: false
layout: post
---


将指导你如何写一篇关于 *Chirpy *主题的文章。 即使你以前使用过 Jekyll，这篇文章也值得一读，因为很多特性都需要设置特定的变量。

> 这是翻译某GitHub帖子的，若要查阅请点击作者ID。

## 命名和路径

创建一个名为的新文件 `YYYY-MM-DD-TITLE.EXTENSION`并将其放入 `_posts`的根目录。  请注意， `EXTENSION`必须是其中之一 `md`和 `markdown`.  如果您想节省创建文件的时间，请考虑使用插件 [`Jekyll-Compose`](https://github.com/jekyll/jekyll-compose)来实现这一点。

## md头部的编写

基本上，需要在新的md文件顶部添加一段：

```yaml
---
title: 标题
date: YYYY-MM-DD HH:MM:SS +/-TTTT
categories: [父类, 子类]
tags: [标签]     # TAG names should always be lowercase
---
```

> The posts' _layout_ has been set to `post` by default, so there is no need to add the variable _layout_ in the Front Matter block.
> {: .prompt-note }

### 日期的时区

为了准确记录帖子的发布日期，您不仅应该设置 `timezone`的 `_config.yml`但也可以在变量中提供帖子的时区 `date`它的 Front Matter 块。  格式： `+/-TTTT`，例如 `+0800`.

### 类别和标签

 每个帖子的设计最多包含两个 `categories`， `tags`可以是零到无穷大，例如：

```yaml
---
categories: [父类, 子类]
tags: [标签1，标签2，标签3...]
---
```

### 作者信息

帖子头部允许不写作者信息，Jekyll会从 `_config.yml`自动获取 `social.name`和 `social.links`作为作者信息，可以在 `_config.yml`添加一段代码：

```yaml
---yml
social:
  name: Full Name
  link: https://example.com
---
```

## 目录

默认情况下，目录(**TOC**)显示在帖子的右侧面板 如果要全局关闭它，请转到 `_config.yml`并设置变量的值 `toc:false`. 如果要关闭特定帖子的 TOC，请将以下内容添加到帖子的**头部**：

```yaml
---
toc: false
---
```

## 评论

注释的全局切换由变量定义 `comments.active`在文件中 `_config.yml`.  为该变量选择评论系统后，将为所有帖子打开评论。

如果您想关闭特定帖子的评论，请将以下内容添加到帖子的**头部**：

```yaml
---
comments: false
---
```

## 数学

出于网站性能原因，默认情况下不会加载数学功能。  但它可以通过以下方式启用：

```yaml
---
math: true
---
```

## Mermaid

[**Mermaid **](https://github.com/mermaid-js/mermaid)是一个很棒的图表生成工具。 要在您的帖子中启用它，请将以下内容添加到 YAML 块中：

```yaml
---
mermaid: true
---
```

然后你可以像其他markdown语言一样使用它：用 ````mermaid`和 `````.

## 图片

### 标题

将斜体添加到图像的下一行，它将成为标题并出现在图像的底部：

```markdown
# 本地图片
![图片标题](/path/to/image)
# 网络图片
![图片标题](https://xxx.xxx/xx.jpg)
_Image Caption_
```

{: .nolineno}

### 尺寸

为了防止加载图片时页面内容布局发生偏移，我们应该为每张图片设置宽度和高度：

```markdown
![图片标题](/path/to/image){: width="700" height="400" }
```

{: .nolineno}

从  *Chirpy v5.0.0* ， `height`和 `width`支持缩写（ `height` →  `h`,  `width` →  `w`）。  下面的例子和上面的效果一样：

```markdown
![图片标题](/path/to/image){: w="700" h="400" }
```

{: .nolineno}

### 位置

默认情况下，图像居中，但您可以使用其中一个类来指定位置 `normal`,  `left`， 和 `right`.

> 指定位置后，不应添加图像标题。
>
> {: .prompt-warning }

- **正常位置 **

  图像将在下面的示例中左对齐：

  ```markdown
  ![Desktop View](/assets/img/sample/mockup.png){: .normal }
  ```

  {: .nolineno}
- **向左浮动 **

  ```markdown
  ![Desktop View](/assets/img/sample/mockup.png){: .left }
  ```

  {: .nolineno}
- **向右浮动 **

  ```markdown
  ![Desktop View](/assets/img/sample/mockup.png){: .right }
  ```

  {: .nolineno}

### 阴影

程序窗口的截图可以考虑显示阴影效果，阴影会在 `light`模式：

```markdown
![Desktop View](/assets/img/sample/mockup.png){: .shadow }
```

{: .nolineno}

### CDN 网址

如果将图像托管在 CDN 上，则可以通过分配变量来节省重复写入 CDN URL 的时间 `img_cdn`的 `_config.yml`文件：

```yaml
img_cdn: https://cdn.com
# 下次图片引用等同于
# ![图片标题](/path/to/image/xx.png)
# ![图片标题](https://cdn.com/xxx/path/to/image/xx.png)
```

{: file='_config.yml' .nolineno}

一次 `img_cdn`已分配，CDN URL 将添加到所有图像（站点头像和帖子的图像）的路径中，以 `/`.

例如，当使用图像时：

```markdown
![The flower](/path/to/flower.png)
```

{: .nolineno}

解析结果会自动添加CDN前缀 `https://cdn.com`在图像路径之前：

```html
<img src="https://cdn.com/path/to/flower.png" alt="The flower">
```

{: .nolineno}

### 图像路径

当一个帖子包含许多图像时，重复定义图像的路径将是一项耗时的任务。  为了解决这个问题，我们可以在帖子的 YAML 块中定义这个路径：

```yml
---
img_path: /img/path/
---
```

{: .nolineno }

然后，Markdown 的图片源就可以直接写文件名了：

```md
![The flower](flower.png)
```

{: .nolineno }

输出将是：

```html
<img src="/img/path/flower.png" alt="The flower">
```

{: .nolineno }

### 预览图像

如果要在帖子内容的顶部添加图像，请指定属性 `src`,  `width`,  `height`， 和 `alt`对于图像：

```yaml
---
image:
  src: /path/to/image/file
  width: 1000   # in pixels
  height: 400   # in pixels
  alt: image alternative text
---
```

除了 `alt`，所有其他选项都是必要的，尤其是 `width`和 `height`，这与用户体验和网页加载性能有关。 上面的“ [大小 ](http://127.0.0.1:4000/Study-Blog/posts/write-a-new-post/#size)”部分也提到了这一点。

从  *Chirpy v5.0.0 * ，属性 `height`和 `width`可以缩写： `height` →  `h`,  `width` →  `w`.  除此之外 [`img_path`](http://127.0.0.1:4000/Study-Blog/posts/write-a-new-post/#image-path)也可以传给预览图，也就是已经设置的时候，属性 `src`只需要图像文件名。

## 已标记的帖子

您可以将一个或多个帖子置顶到首页顶部，固定的帖子按照发布日期倒序排列。  通过以下方式启用：

```yaml
---
pin: true
---
```

## 提示

提示分为三种， `note`,  `warning`和 `danger`.  写的时候加上class `prompt-{type}`到块引用：

- **笔记**

  ```md
  > Example line for prompt.
  {: .prompt-note }
  ```
- **警告**

  ```md
  > Example line for prompt.
  {: .prompt-warning }
  ```
- **危险**

  ```md
  > Example line for prompt.
  {: .prompt-danger }
  ```

## 句法

### 内联代码

```md
`inline code part`
```

{: .nolineno }

### 文件路径突出显示

```md
`/path/to/a/file.extend`{: .filepath}
```

{: .nolineno }

### 代码块

降价符号 `````可以轻松创建代码块，如下所示：

````md
```
This is a plaintext code snippet.
```
````

#### 指定语言

使用 ````{language}`你会得到一个带有语法高亮的代码块：

````markdown
```yaml
key: value
```
````

> Jekyll 标签 `{% highlight %}`与此主题不兼容。
> {: .prompt-danger }

#### 电话号码

默认情况下，除 `plaintext`,  `console`， 和 `terminal`将显示行号。  当你想隐藏代码块的行号时，添加类 `nolineno`对它：

````markdown
```shell
echo 'No more line numbers!'
```
{: .nolineno }
````

#### 指定文件名

您可能已经注意到代码语言将显示在代码块的顶部。  如果要替换为文件名，可以添加属性 `file`为达到这个：

````markdown
```shell
# content
```
{: file="path/to/file" }
````

#### 突出代码

如果要显示 **Liquid **代码段，请在 Liquid 代码周围加上 `{% raw %}`和 `{% endraw %}`:

````markdown
{% raw %}
```liquid
{% if product.title contains 'Pack' %}
  This product's title contains the word Pack.
{% endif %}
```
{% endraw %}
````

或添加 `render_with_liquid: false`（需要 Jekyll 4.0 或更高版本）到帖子的 YAML 块。

## 学到更多

有关 Jekyll 帖子的更多知识，请访问 [Jekyll 文档：帖子 ](https://jekyllrb.com/docs/posts/)。
