---
layout: post
title:  "LeetCode 292: Nim Game"
date:   2015-10-29 21:07:00
categories: LeetCode
tags: [LeetCode, Simple]
---

我从未见过如此水的题目！

>You are playing the following Nim Game with your friend: There is a heap of stones on the table, each time one of you take turns to remove 1 to 3 stones. The one who removes the last stone will be the winner. You will take the first turn to remove the stones.  
>  
>Both of you are very clever and have optimal strategies for the game. Write a function to determine whether you can win the game given the number of stones in the heap.  
>  
>For example, if there are 4 stones in the heap, then you will never win the game: no matter 1, 2, or 3 stones you remove, the last stone will always be removed by your friend.  

一种游戏，桌上有一堆石子，两人轮流取，可以取1-3颗。取完最后一明码标价石子的玩家获胜，假设你先取，问是否有必胜的策略。  

显然，由于每次可取石子的范围为1-3，因此当桌上剩余石子为4时，如果让对方取，则自己必胜。   
推广而言，在每轮进行中，都可以根据对方取的数量，选择自己的数量，保证每过一轮石子数减少4。  
因此，在自己先取的情况下，只要能够保证取完之后桌面剩余石子数量为4的倍数，即可必胜。否则必败。

所以，如果初始石子数为4的倍数，则必败，否则必胜。  

代码如下：

**Java Code**
{% highlight Java %}
public class Solution {
    public boolean canWinNim(int n) {
        if(n % 4 == 0){
            return false;
        }
        else{
            return true;
        }
    }
}
{% endhighlight %}
