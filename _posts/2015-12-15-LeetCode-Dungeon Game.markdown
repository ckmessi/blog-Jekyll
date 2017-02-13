---
layout: post
title:  "LeetCode 174: Dungeon Game"
date:   2015-12-15 21:19:00
categories: LeetCode
tags: [LeetCode, Dynamic Programming]
---

###地牢游戏

>The demons had captured the princess (P) and imprisoned her in the bottom-right corner of a dungeon. The dungeon consists of M x N rooms laid out in a 2D grid. Our valiant knight (K) was initially positioned in the top-left room and must fight his way through the dungeon to rescue the princess.  
>The knight has an initial health point represented by a positive integer. If at any point his health point drops to 0 or below, he dies immediately.  
>Some of the rooms are guarded by demons, so the knight loses health (negative integers) upon entering these rooms; other rooms are either empty (0's) or contain magic orbs that increase the knight's health (positive integers).  
>In order to reach the princess as quickly as possible, the knight decides to move only rightward or downward in each step.  
>Write a function to determine the knight's minimum initial health so that he is able to rescue the princess.

描述了一个骑士去地牢救公主的故事背影，实质上就是求初始所需要的最小气血值。

其实这道题目需要注意的一点就是，在通往终点的路径中，任何一个位置的气血值都要大于零。
除此之外，就是一个最大路径和的问题。
由于求的是初始时所需要的最小气血值，因此动态规划过程从后往前，逐个计算，最终得到原点处的最小气血值

**Java Code**
{% highlight Java %}
public class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        
        int m = dungeon.length;
        if(m == 0){
            return 0;
        }
        int n = dungeon[0].length;
        if(n == 0){
            return 0;
        }
        
        int[][] sum = new int[m][n];
        for(int i = m - 1; i >= 0; i--){
            for(int j = n - 1; j >= 0; j--){
                int currentDamage = dungeon[i][j];
                if(i == m-1 && j == n-1){
                    sum[i][j] = Math.max(1, 1 - currentDamage);
                }
                else if (i == m - 1){
                    sum[i][j] = Math.max(1, sum[i][j+1] - currentDamage);
                }
                else if (j == n - 1){
                    sum[i][j] = Math.max(1, sum[i+1][j] - currentDamage);
                }
                else{
                    sum[i][j] = Math.max(1, Math.min(sum[i+1][j], sum[i][j+1]) - currentDamage);
                }
            }
        }
        return sum[0][0];
    }
}
{% endhighlight %}
