
---
<center>dask分布式建模平台</center>
===

<div style="text-align:right">
<a  href="10.22.64.81:8888">10.22.64.81:8888</a>
</div>


---
目标
==
-  提高建模的效率
-  处理大数据量

---
#### 容器集群技术 docker swarm

相对于k8s，mesos等方案，docker swarm在我们的场景中有以下优势：

- docker Swarm和Docker环境很好的结合在一起
- Swarm 比较轻量，并且提供了很多的驱动，能够和其他的集群解决方案一起工作
- 易于使用

缺点： 不够成熟

---
#### docker swarm mode架构

<div align="center">
<img width="100%" src="http://ww1.sinaimg.cn/large/b433eefdgy1fhcwc18ezmj20j308vmyk.jpg"/>
</div>

---

集群结构：[http://10.22.64.81:8089/](http://10.22.64.81:8089/)
<div align="center">
<image  width="50%" src="http://ww1.sinaimg.cn/large/b433eefdgy1fhiiri9sqtj20k30qxwgw.jpg"/></div>

---
#### 建模平台结构图


<div align="center">
<img width="100%" src="http://ww1.sinaimg.cn/large/b433eefdgy1fhihv676naj20hr090dg8.jpg"/>
</div>

---
####  dask searchcv demo
```python
from dask.distributed import Client
client = Client('10.0.0.3:8786')

from sklearn.datasets import load_digits
from sklearn.svm import SVC

# Fit with dask-searchcv
from dask_searchcv import GridSearchCV

## Fit with scikit-learn
#from sklearn.model_selection import GridSearchCV

param_space = {'C': [1e-4, 1, 1e4],
               'gamma': [1e-3, 1, 1e3],
               'class_weight': [None, 'balanced']}

model = SVC(kernel='rbf')
digits = load_digits()

search = GridSearchCV(model, param_space, cv=3)
search.fit(digits.data, digits.target)
search.visualize("search_graph", "png")
```
---
####  [`searchcv execute graph`](https://10.22.64.85:8888/files/mydask/search_graph.svg)

<div align="center">
<img width="75%" src="http://ww1.sinaimg.cn/large/b433eefdgy1fhijhn8it3j21b717v456.jpg"/>
</div>

---




---


---
test
===

# ![](images/marp.png)

##### Markdown presentation writer, powered by [Electron](http://electron.atom.io/)

###### Created by Yuki Hattori ( [@yhatt](https://github.com/yhatt) )

---

# Features

- **Slides are written in Markdown.**
- Cross-platform. Supports Windows, Mac, and Linux
- Live Preview with 3 modes
- Slide themes (`default`, `gaia`) and custom background images
- Supports emoji :heart:
- Render maths in your slides
- Export your slides to PDF

---

# How to write slides?

Split slides by horizontal ruler `---`. It's very simple.

```md
# Slide 1

foobar

---

# Slide 2

foobar
```

> *Notice: Ruler (`<hr>`) is not displayed in Marp.*

---

# Directives

Marp's Markdown has extended directives to affect slides.

Insert HTML comment as below:
```html
<!-- {directive_name}: {value} -->
```

```html
<!--
{first_directive_name}:  {value}
{second_directive_name}: {value}
...
-->
```
---

## Global Directives

### `$theme`

Changes the theme of all the slides in the deck. You can also change from `View -> Theme` menu.

```
<!-- $theme: gaia -->
```

|Theme name|Value|Directive|
|:-:|:-:|:-|
|***Default***|default|`<!-- $theme: default -->`
|**Gaia**|gaia|`<!-- $theme: gaia -->`


---

### `$width` / `$height`

Changes width and height of all the slides.

You can use units: `px` (default), `cm`, `mm`, `in`, `pt`, and `pc`.

```html
<!-- $width: 12in -->
```

### `$size`

Changes slide size by presets.

Presets: `4:3`, `16:9`, `A0`-`A8`, `B0`-`B8` and suffix of `-portrait`.

```html
<!-- $size: 16:9 -->
```

<!--
$size: a4

Example is here. Global Directive is enabled in anywhere.
It apply the latest value if you write multiple same Global Directives.
-->

---

## Page Directives

The page directive would apply to the  **current page and the following pages**.
You should insert it *at the top* to apply it to all slides.

### `page_number`

Set `true` to show page number on slides. *See lower right!*

```html
<!-- page_number: true -->
```

<!--
page_number: true

Example is here. Pagination starts from this page.
If you use multi-line comment, directives should write to each new lines.
-->

---

### `template`

Set to use template of theme.

The `template` directive just enables that using theme supports templates.

```html
<!--
$theme: gaia
template: invert
-->

Example: Set "invert" template of Gaia theme.
```

---

### `footer`

Add a footer to the current slide and all of the following slides

```html
<!-- footer: This is a footer -->
```

Example: Adds "This is a footer" in the bottom of each slide

---

### `prerender`

Pre-renders a slide, which can prevent issues with very large background images.

```html
<!-- prerender: true -->
```

---

## Pro Tips

#### Apply page directive to current slide only

Page directive can be selectively applied to the current slide by prefixing the page directive with `*`.

```
<!-- *page_number: false -->
<!-- *template: invert -->
```

<!--
*page_number: false

Example is here.
Page number is not shown in current page, but it's shown on later pages.
-->

---

#### Slide background Images

You can set an image as a slide background.

```html
![bg](mybackground.png)
```

Options can be provided after `bg`, for example `![bg original](path)`.

Options include:

- `original` to include the image without any effects
- `x%` to include the  image at `x` percent of the slide size

Include multiple`![bg](path)` tags to stack background images horizontally.

![bg](images/background.png)

---

#### Maths Typesetting

Mathematics is typeset using the `KaTeX` package. Use `$` for inline maths, such as $ax^2+bc+c$, and `$$` for block maths:

$$I_{xx}=\int\int_Ry^2f(x,y)\cdot{}dydx$$

```html
This is inline: $ax^2+bx+c$, and this is block:

$$I_{xx}=\int\int_Ry^2f(x,y)\cdot{}dydx$$

```

---

## Enjoy writing slides! :+1:

### https://github.com/yhatt/marp

Copyright &copy; 2016 [Yuki Hattori](https://github.com/yhatt)
This software released under the [MIT License](https://github.com/yhatt/marp/blob/master/LICENSE).