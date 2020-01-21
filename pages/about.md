---
layout: page
title: About
description: 前端小白鼠
keywords: Xiaoping Yi, 易小平
comments: true
menu: 关于
permalink: /about/
---

我是易小平，乐观人生，洒脱一世。

仰慕「一人、一脑、一世界」。

坚信办法总比问题多。

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
