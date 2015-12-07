---
layout: post
title:  "LeetCode 41: First Missing Positive"
date:   2015-10-06 20:41:00
categories: coding
---

##LeetCode 41: First Missing Positive

Given an unsorted integer array, find the first missing positive integer.  
For example,  
Given [1,2,0] return 3,   
and [3,4,-1,1] return 2.  
Your algorithm should run in O(n) time and uses constant space.


<br>

* * * 

<br>


这题给定一个数组，要求在固定空间内完成，思路就是利用原数组进行记录。

1. 遍历数组，如果当前元素小于1或大于length，可以忽略不计
2. 将当前元素放到对应位置，并把那个位置的元素交换回来。
3. 这里有个陷阱，需要判断交换的两个元素是否相同；如果相同则不需要再进行比较，否则下一次循环检查当前位置元素。（如果没有判断，会出现死循环的情况。）

<br>

* * *

<br>

**Java Code**
{% highlight Java %}
public class Solution {
    // First Missing Positive
    public int firstMissingPositive(int[] nums) {
        for(int i = 0; i < nums.length; i++){
            if(nums[i] <= 0 || nums[i] > nums.length){
                continue;
            }
            if(nums[i] == i+1){
                continue;
            }
            int currentNum = nums[i];
            int temp = nums[currentNum - 1];
            nums[currentNum - 1] = currentNum;
            nums[i] = temp;
            if(currentNum != temp){
                i--;
            }
        }	    	
        for(int i = 0; i < nums.length; i++){
            if(nums[i] != i+1){
                return i+1;
            }
        }
        return nums.length + 1;
    }
}
{% endhighlight %}

