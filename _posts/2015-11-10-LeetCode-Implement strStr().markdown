---
layout: post
title:  "LeetCode 28: Implement strStr()"
date:   2015-11-10 21:16:00
categories: LeetCode
tags: [LeetCode, String]
---

在一个字符串中，查找另外一个字符串的位置。

>Implement strStr().  
>
>Returns the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.   

其实就是标准的KMP算法。可是四年过去了，我依然不会用。

先上代码，再分析。

**Java Code**
{% highlight Java %}
public class Solution {
    public int strStr(String haystack, String needle) {
        
        if(needle.length() == 0){
            return 0;
        }
        if(haystack.length() == 0){
            return -1;
        }
        
        int[] next = this.getNext(needle);
        int result = this.kmp(haystack, needle, next);
        return result;
    }
    
    public int kmp(String s, String p, int[] next){
        int i = 0;
        int j = 0;
        while(i != s.length() && j != p.length()){
            if(j == -1 || s.charAt(i) == p.charAt(j)){
                i++;
                j++;
            }
            else{
                j = next[j];
            }
        }
        if(j == p.length()){
            return i - j;
        }
        else{
            return -1;
        }
    }
    public int[] getNext(String s){
        int[] next = new int[s.length()];
        next[0] = -1;
        int i = 0;
        int j = -1;
        while(i != s.length() - 1){
            if(j == -1 || s.charAt(i) == s.charAt(j)){
                i++;
                j++;
                next[i] = j;
            }
            else{
                j = next[j];
            }
        }
        return next;
    }
    
}
{% endhighlight %}

<br>

* * *

<br>

其实代码主要可以划分为两部分，两个概念：  
1. 获取next数组   
2. 匹配失败时，根据next数组进行移动

（1）next数组
next[j] =   

+ -1，当j=0时   
+ Max{k|0<k<j且'P<sub>0</sub>P<sub>1</sub>...P<sub>k-1</sub>' = 'P<sub>j-k</sub>P<sub>j-k+1</sub>...P<sub>j-1</sub>'}     
+ 0，其它情况  

第二个条件中，k的含义可以理解为子串的长度。即，从当前字符之前（不包括当前字符），从头截取k个字符，和从尾截取k个字符，能够相等的最大子串长度。  

这个算法精妙之处在于，计算next数组的本身，也用到了next数组的作用。同时结合动态规划的思想。  
初始条件next[0] = -1，假设next[j] = k，  
则对于next[j+1]，如果p[k] = p[j]，那么next[j+1] = k+1；否则，需要重新匹配，而这个匹配过程，可以用到next之前的计算结果。p向右滑动，滑动位置由next[k]决定；如果p[next[k]] = p[j]则next[j+1] = next[k] + 1；否则继续进行。
实现代码如下：

{% highlight Java%}
public int[] getNext(String s){
    int[] next = new int[s.length()];
    next[0] = -1;
    int i = 0;
    int j = -1;
    while(i != s.length() - 1){
        if(j == -1 || s.charAt(i) == s.charAt(j)){
            i++;
            j++;
            next[i] = j;
        }
        else{
            j = next[j];
        }
    }
    return next;
}
{% endhighlight%}

（2）匹配失败时的移动
头尾子串相同的条件，保证了当字符串匹配到当前字符p[j]如果失败的话（能匹配到p[j]，说明此前字符都匹配成功）；    
如果p[j]匹配失败，如果next[j] == k，说明此前p[0,k-1] === p[j-k,j-1]，并且p[k] != p[j]（否则k与最大值定义矛盾）；  
这样的话，按照原来的匹配方案，假设字符串为s，模式串为p，当前是s[i] == p[j]，  
如果s[i]  != p[j]，传统的字符串匹配，要重新开始匹配了，即i = i-j+1, j =   0重新再查找；现在呢，结合next数组的定义，可以直接令i = i，j = next[j]，知道为什么吗？  
如果存在x（x < 当前i）而匹配的情况，就与next[j]的定义矛盾了，因为有一个更长的串能够满足匹配。  
匹配的代码如下：  


{% highlight Java%}
public int kmp(String s, String p, int[] next){
    int i = 0;
    int j = 0;
    while(i != s.length() && j != p.length()){
        if(j == -1 || s.charAt(i) == p.charAt(j)){
            i++;
            j++;
        }
        else{
            j = next[j];
        }
    }
    if(j == p.length()){
        return i - j;
    }
    else{
        return -1;
    }
}
{% endhighlight%}


