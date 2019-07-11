---
layout: post
title:  "MasterCard 清算檔剔退原因"
date:   2019-07-06 13:42:00 +0800
categories: Business
tags: MasterCard
---

## main

{% highlight html %}
<h2>0670</h2>
<main>
<div>
PDS0158S4 INTERCHANGE RATE DESIGNATOR DOES NOT MEET TIMELINESS CRITERIA AND/OR DE12S1 DATE
CONTAINS INVALID DATA

*P0158 S04     73  交易帶的 IRD = 73 過期
</div>

<h2>2514</h2>
<div>
SERVICE CODE MUST BE PRESENT AND EQUAL TO 100-199,500-599,700-799. 

*D0040         201 => DE40 2開頭為晶片，D0050 要有 Binary Data
</div>

</main>
{% endhighlight html %}
