---
layout: post
title:  "LeetCode: Single Number系列"
date:   2015-10-26 21:30:00
categories: coding
---

LeetCode中有三道Single Number系列题目，都挺巧妙地使用到位运算的相关知识。

<br>

* * *

<br>

###LeetCode 136 Single Number

>Given an array of integers, every element appears twice except for one. Find that single one.
>
>Note:  
>Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

假设有一个数组，除了一个元素之外，所有元素都出现两次，如何在线性时间和常数空间条件下找出那个单独的数？

对于Single Number这道题，我们只需要掌握两个位运算的性质即可得到答案：  
对于任何数x，x xor x = 0，x xor 0 = x  

这样数组中所有数做异或运算后，所有成双成对的数都将变成0，只剩下那个单独的数。  
代码如下：

**Java Code**
{% highlight Java %}
public class Solution {
    // Single Number
    public int singleNumber(int[] nums) {
        // x ^ x = 0, 0 ^ x = x
        int result = 0;
        for(int i = 0; i < nums.length; i++){
            result ^= nums[i];
        }
        return result;
    }
}
{% endhighlight %}


<br>

* * * 

<br>

###LeetCode 137 Single Number II

>Given an array of integers, every element appears three times except for one. Find that single one.  
>  
>Note:  
>Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

假设有一个数组，除了一个元素之外，所有元素都出现三次，如何在线性时间和常数空间条件下找出那个单独的数？

对于Single Number II这道题，我们需要这样考虑。除了single number外，其它数都出现三次，这样除了single number 外的所有数的和，二进制表示每一位上都应该是模3得0。  

我们可以把所有数加起来，加的过程中统计每一位上数字的和，并不断模3，最终得到的和一定就是那个单独的数的表示。

代码如下：

**Java Code**
{% highlight Java %}
public class Solution {
    // Single Number II
    public int singleNumber(int[] nums) {
        
        int[] bitCounter = new int[32];
        
        for(int i = 0; i < nums.length; i++){
            for(int j = 0; j < 32; j++){
                bitCounter[j] += (nums[i] >> j) & 1;
                bitCounter[j] %= 3;
            }
        }
        
        int result = 0;
        for(int j = 0; j < 32; j++){
            result |= (bitCounter[j] << j);
        }
        
        return result;
        
    }
}
{% endhighlight %}

<br>

* * *

<br>


###LeetCode 260 Single Number III

>Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.  
>
>For example:  
>
>Given nums = [1, 2, 1, 3, 2, 5], return [3, 5].  
> 
>Note:  
>The order of the result is not important. So in the above example, [5, 3] is also correct.  
>Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?

假设有一个数组，除了两个元素之外，所有元素都出现两次，如何在线性时间和常数空间条件下找出那两个单独的数？

真会开脑洞啊，这题怎么办呢？要这样想：  
假设这两个目标数为x和y，使用Single Number I的方法，可以得到r = x xor y的结果。   
根据题意，x和y肯定是不相同的，也就是说x xor y != 0（否则与x xor x == 0矛盾）。  
这时候寻找r最低位为1的位置，假设为第k位，可以知道在这第k位上，x与y一定是不同的。  

然后根据这第k位的性质，可以把所有数划分为两组，二进制表示在第k位为1的、在第k位为0的。  

此时分别对两组数求异或操作，就可以分别得到x和y的值了。  

代码如下：

**Java Code**
{% highlight Java %}
public class Solution {
    // Single Number III
    public int[] singleNumber(int[] nums) {
        
        int xorResult = 0;
        for(int i = 0; i < nums.length; i++){
            xorResult ^= nums[i];
        }
        
        int leastDiffPos = -1;
        for(int j = 0; j < 32; j++){
            if(((xorResult >> j) & 1) == 1){
                leastDiffPos = j;
                break;
            }
        }
        
        int xorResultA = 0;
        int xorResultB = 0;
        for(int i = 0; i < nums.length; i++){
            if(((nums[i] >> leastDiffPos) & 1) == 1){
                xorResultA ^= nums[i];
            }
            else{
                xorResultB ^= nums[i];
            }
        }
        
        int[] result = new int[2];
        result[0] = xorResultA;
        result[1] = xorResultB;
        return result;
    }
}
{% endhighlight %}

