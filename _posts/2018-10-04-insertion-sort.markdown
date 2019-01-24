---
layout: post
title:  "Insertion Sort"
date:   2019-01-24 00:43:13 +0800
categories: Computer-Science
tags: Algorithm

---

Insertion sort pseudo code looks like this

{% highlight C# %}
//mark first element as sorted

//for each unsorted element X

//  'extract' the element X

//  for j = lastSortedIndex down to 0

//    if current element j > X

//      move sorted element to the right by 1

//    break loop and insert X here
{% endhighlight %}


{% highlight C# %}
        public static void InsertionSort(int[] input)
        {
            for( int i = 1; i < input.Length; i++ )
            {
                while( i > 0 && input[i] < input[i-1] )
                {
                    var temp = input[i];
                    input[i] = input[i - 1];
                    input[i - 1] = temp;
                    i--;
                }
            }
        }
{% endhighlight %}

