---
layout: post
title:  "LeetCode 49: Group Anagrams"
date:   2015-10-28 20:54:00
categories: LeetCode
tags: [LeetCode, Hash]
---

这道Group Anagrams不难，但其中的一些技巧还是值得记录的。

>Given an array of strings, group anagrams together.  
>  
>For example, given: ["eat", "tea", "tan", "ate", "nat", "bat"],   
>Return:  
>[  
>  ["ate", "eat","tea"],  
>  ["nat","tan"],  
>  ["bat"]  
>]
>Note:  
>For the return value, each inner list's elements must follow the lexicographic order.  
>All inputs will be in lower-case.  

给一个字符串数组，将其中字符串按同字母异形分组，并返回列表。  
其中需要用到的技巧：  
1. 字符串升序排序，作为标识每个同字母异型的键值。  
2. 组内的字母顺序排序，直接调用Java的Collections的排序函数即可。  

代码如下：

**Java Code**
{% highlight Java %}
public class Solution {
    // Group Anagrams
    public List<List<String>> groupAnagrams(String[] strs) {
        
        HashMap<String, List<String>> anagramRecord = new HashMap<String, List<String>>();
        for(int i = 0; i < strs.length; i++){
            String curStr = strs[i];
            char[] charArray = curStr.toCharArray();
            Arrays.sort(charArray);
            String anagramStr = String.valueOf(charArray);
            if(anagramRecord.containsKey(anagramStr) == true){
                anagramRecord.get(anagramStr).add(curStr);
            }
            else{
                List<String> x = new ArrayList<String>();
                x.add(curStr);
                anagramRecord.put(anagramStr, x);
            }           
        }        
        Iterator iterator = anagramRecord.keySet().iterator();
        List<List<String>> anagramsGroupList = new ArrayList<List<String>>();
        while(iterator.hasNext()){
            String key = (String) iterator.next();
            List<String> x = anagramRecord.get(key);
            Collections.sort(x);
            anagramsGroupList.add(x);
        }        
        return anagramsGroupList;
    }
}
{% endhighlight %}
