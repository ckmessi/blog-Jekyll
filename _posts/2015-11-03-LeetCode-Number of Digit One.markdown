---
layout: post
title:  "LeetCode 233: Number of Digit One"
date:   2015-11-03 21:19:00
categories: LeetCode
tags: [LeetCode, Math]
---

计算0~n的数中，一共出现了多少个1。  

>Given an integer n, count the total number of digit 1 appearing in all non-negative integers less than or equal to n.  
>  
>For example:  
>Given n = 13,  
>Return 6, because digit 1 occurred in the following numbers: 1, 10, 11, 12, 13.  

这道题目其实很有trick，关键要掌握核心。  
这道题目其实也是Crack Code Interview中的一道题，书中有非常巧妙的解答思路如下：

我们知道  

* 对于最后一位，每10个数字将重复一次  
* 对于最后两位，每100个数字将重复一次  
* 对于最后三位，每1000个数字将重复一次  

我们以2为例，

* 如果在0-99之间有x个2
* 那在0-199之是一定有2*x个2
* 在0-299之间，肯定有3*x个2，以前100个来自第一位的2。    

换句话讲，我们看下面这个数，可以这样划分：  
&nbsp; &nbsp; &nbsp; &nbsp; *f(513) = 5\*f(99) + f(13) + 100*   

划分一下：

* 后两位的序列重复了5次，因此有5*f(99)
* 我们需要数一下500-513之间的后两位，所以加上f(13)
* 我们还需要算第一位，在200-299之间，因此要加上100  

如果n是279，我们需要有一些小小的改变  
&nbsp; &nbsp; &nbsp; &nbsp; *f(279) = 2\*f(99) + f(79) + 79 + 1*  

同样划分一下：

* 后两位的序列重复了2次，因此有2*f(99)
* 我们需要数一下200-279之间的后两位，所以加上f(79)
* 我们还需要算第一位，在200-279之间，因此要加上79 + 1

因此可以直观地写出递归的代码如下：

**Java Code**
{% highlight Java %}
public class Solution {
    public int countDigitOne(int n) {
      
        if (n == 0){
            return 0;
        }
        
        int power = 1;
        while(10 * power <= n){
            power *= 10;
        }
        int first = n / power;
        int remainder = n % power;
        
        // count 1 from first digit
        int n1sFirst = 0;
        if(first > 1){
            n1sFirst += power;
        }
        else if (first == 1){
            n1sFirst += remainder + 1;
        }
        else{
            n1sFirst += 0;
        }
        
        // count 1 from others
        int n1sOther = first * countDigitOne(power - 1) + countDigitOne(remainder);
        return n1sFirst + n1sOther;

    }
}
{% endhighlight %}

然而递归的代码容易栈溢出，比如在LeetCode上如果使用递归算法，就会导致RuntimeError，错误为StackOverflow。   

我们还可以有迭代的算法如下：

**Java Code**
{% highlight Java %}
public class Solution {
    public int countDigitOne(int n) {
      
        if(n == 0){
            return 0;
        }
        int count = 0;
        int digit = 0;  // 记录当前最低位
        int j = n;
        int seendigits = 0; // 记录当前余数，相当于remainder
        int position = 0;
        int pow10_pos = 1;  // 当前digit所在位的基数
        
        while(j > 0){
            digit = j % 10;
            int pow10_posMinus1 = pow10_pos / 10;
            count += digit * position * pow10_posMinus1;
            // 妈蛋，这句坑爹的，观察了一下发现后面的 position* pow10_posMinus1 = 0,1,20,300,400,
            // 这1，20，300，400，就是f(9)，f(99)，f(999)的通项公式啊！
            
            if(digit == 1){
                count += seendigits + 1;
            }
            else if (digit > 1){
                count += pow10_pos;
            }
            
            seendigits = seendigits + pow10_pos * digit;
            pow10_pos *= 10;    // 进一位，*10
            position++;
            j = j / 10;
        }
        return count;
    }
}
{% endhighlight %}

