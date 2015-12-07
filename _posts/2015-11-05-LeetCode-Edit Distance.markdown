---
layout: post
title:  "LeetCode 72: Edit Distance"
date:   2015-11-05 22:12:00
categories: coding
tags: Dynamic Programming
---

经典的动态规划问题：编辑距离。


>Given two words word1 and word2, find the minimum number of steps required to convert word1 to word2. (each operation is counted as 1 step.)  
>  
>You have the following 3 operations permitted on a word:
>  
>a) Insert a character  
>b) Delete a character  
>c) Replace a character  


这是动态规划问题的经典案例。     

可以这样考虑，用edit[m, n]表示word1前m个字符到word2前n个字符的编辑距离，则可以得到递推公式：

&nbsp; &nbsp; &nbsp; &nbsp; *edit[m,n] = min{ edit[m-1,n]+1, edit[m,n-1]+1, edit[m-1,n-1]+flag }*    

其中flag==0当且仅当word1[m]==word2[n]，否则flag==1。

初始条件为：  
&nbsp; &nbsp; &nbsp; &nbsp; *edit[m][0]=m*，*edit[0][n]=n*，*edit[0][0]=0*

根据这部分推导，可以写出以下代码：

**Java Code**
{% highlight Java %}
public class Solution {
    public int minDistance(String word1, String word2) {
        // edit[m, n] = min{edit[m-1, n] + 1, edit[m, n-1]+1, edit[m-1,n-1] + 1（word1[m]!=word2[n]或0}
        // 初始条件：edit[m, 0] = m; 
        
        int[][] edit = new int[word1.length() + 1][word2.length() + 1];
        
        // initial condition
        edit[0][0] = 0;
        for(int i = 1; i <= word1.length(); i++){
            edit[i][0] = i;
        }
        for(int j = 1; j <= word2.length(); j++){
            edit[0][j] = j;        
        }
        
        // dynamic programming
        for(int i = 1; i <= word1.length(); i++){
            for(int j = 1; j <= word2.length(); j++){
                int flag = 1;
                if(word1.charAt(i-1) == word2.charAt(j-1)){
                    flag = 0;
                }
                edit[i][j] = Math.min(edit[i-1][j] + 1, edit[i][j-1] + 1);
                edit[i][j] = Math.min(edit[i][j], edit[i-1][j-1] + flag);
            }
        }   

        return edit[word1.length()][word2.length()];
    }
}
{% endhighlight %}
