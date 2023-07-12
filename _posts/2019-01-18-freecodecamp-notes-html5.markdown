---
layout: post
title:  "HTML5 Notes"
date:   2019-01-18 14:04:13 +0800
categories: Front-End
tags: html
---

## main

HTML5 引進了 descriptive HTML tags. 包含 header, footer, nav, video, article, section 及其他標籤。
這些標籤同時增進了可讀性與SEO

{% highlight html %}
<h2>CatPhotoApp</h2>
<main>
<p>Something</p>

<p>Something
</main>
{% endhighlight html %}

## img
{% highlight html %}
<img src="https://bit.ly/fcc-relaxing-cat" alt="A cute orange cat lying on its back.">
{% endhighlight html %}

## anchor
{% highlight html %}
<a href="http://freecatphotoapp.com" target="_blank>cat photos</a>
{% endhighlight html %}

target="_blank 是用新分頁開啟連結
  
## internal anchor
{% highlight html %}
<a href="#contacts-header">Contacts</a>
...
<h2 id="contacts-header">Contacts</h2>
{% endhighlight html %}

## Turn an Image into a Link
{% highlight html %}
<a href="#"><img src="https://bit.ly/fcc-relaxing-cat" alt="A cute orange cat lying on its back."></a>
{% endhighlight html %}
