---
layout: post
title:  "MasterCard 清算被拒原因"
date:   2019-07-11 13:42:00 +0800
categories: Business
tags: MasterCard
---
<h3>0670</h3>
<main>
<div>
{% highlight html linenos %}  
? ERROR                                                                                               SOURCE                          
? CODE    DESCRIPTION                                                                                MESSAGE #   ELEMENT ID           
?                                                                                                                                     
? 0670    PDS0158S4 INTERCHANGE RATE DESIGNATOR DOES NOT MEET TIMELINESS CRITERIA AND/OR DE12S1 DATE  00000001    P0158 S04           
?          CONTAINS INVALID DATA                                                                                                      
?                                                                                                                                     
? MESSAGE DETAILS                                                                                                                     
?                                                                                                                                     
?                 MTI           1240                                                                                                  
?                 ...                                                                                              
?                *P0158 S04     73                                                                                                    
?                 ...
{% endhighlight html %}  
</div>
  
<div>
IRD 過期，解法 Rekey
</div>

<h2>2514</h2>
<div>
SERVICE CODE MUST BE PRESENT AND EQUAL TO 100-199,500-599,700-799. 

*D0040         201 => DE40 2開頭為晶片，D0050 要有 Binary Data
</div>

</main>

