---
layout: post
title:  "Selection Sort"
date:   2019-01-25 19:16:00 +0800
categories: Computer-Science
tags: Algorithm

---

Selection sort pseudo code looks like this

{% highlight C# %}
// repeat (numOfElements - 1) times

//  set the first unsorted element as the minimum

//  for each of the unsorted elements

//    if element < currentMinimum

//      set element as new minimum

//  swap minimum with first unsorted position
{% endhighlight %}


{% highlight C# %}
    // Initial version 01/25
    for (int i = 1; i < input.Length; i++)
    {
        int min = i-1;

        for ( int j = i; j < input.Length; j++)
        {
            if(input[min] > input[j])
            {
                min = j;                        
            }
        }

        if ( min != i-1 )
        {
            var temp = input[i-1];
            input[i-1] = input[min];
            input[min] = temp;
        }                
    }        
{% endhighlight %}
