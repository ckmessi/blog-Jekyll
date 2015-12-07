---
layout: post
title:  "LeetCode 187: Repeated DNA Sequences"
date:   2015-10-27 19:36:00
categories: coding
---

这道Repeated DNA Sequences挺有意思，其实很简单，但是少数几道以MLE为限制的题目。  
这道题目中用到了编码技巧，这是很少遇到的如此简单却实用的编码应用。

>All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.  
>
>Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.  
>  
>For example,  
>
>>Given s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT",  
>> 
>>Return:  
>>["AAAAACCCCC", "CCCCCAAAAA"].  
>  


给定一个序列，要求查找长度为10的，出现2次以上的子序列。

思路上很简单，就是遍历一次字符串，使用哈希表记录所有出现过的长度为10的字符串。但问题在于用String类型保存键值有点浪费了，因为只可能出现'A''C''G''T'四种类型。  
由于DNA序列的特殊性质，每个字符只有4种可能，可以用2位来表示，因此长度为10的字符串只需要20位就可以表达，即2^10大小的整数即可表示。现成的Int32肯定是能满足的。

代码如下：


**Java Code**
{% highlight Java %}
public class Solution {
    // Repeated DNA Sequences
    public List<String> findRepeatedDnaSequences(String s) {
        
        List<String> sequencesList = new ArrayList<String>();
        HashMap<Integer, Integer> strRecordMap = new HashMap<Integer, Integer>();
        for(int i = 0; i < s.length() - 9; i++){
            String curStr = s.substring(i, i + 10);
            int hashValue = this.getHashValueOfStr(curStr);
            if(strRecordMap.containsKey(hashValue) == true){
                if(strRecordMap.get(hashValue) == 1){
                    sequencesList.add(curStr);
                    strRecordMap.put(hashValue, 2);
                }
            }
            else{
                strRecordMap.put(hashValue, 1);
            }
        }
        return sequencesList;
    }    
    int getHashValueOfStr(String str){
        int n = 0;
        for(int i = 0; i < str.length(); i++){
            n = n << 2;
            if(str.charAt(i) == 'A'){
                n += 0;
            }
            else if (str.charAt(i) == 'C'){
                n += 1;
            }
            else if (str.charAt(i) == 'G'){
                n += 2;
            }
            else{
                n += 3;
            }
        }
        return n;
    }
}
{% endhighlight %}
