---
layout: post
title:  "LeetCode 204: Count Primes"
date:   2015-10-28 21:08:00
categories: coding
tags: Math
---

这是一道标准的与质数相关的题目，需要好好记住几种递进的方法。


>Description:  
>
>Count the number of prime numbers less than a non-negative number, n. 

给定一个非负整数n，返回比n小的质数的个数。

<br>  

LeetCode题目下的解释还是挺不错的，层层递进，记录如下：  

1. 从 *isPrime* 函数开始。为了判断一个数是否为质数，我们需要检查能否被任何比n小的数整除。因此 *isPrime* 函数的时间复杂度是 *O(n)* ，因此计算到n的质数数量总共需要 *O(n<sup>2</sup>)* 。  

2. 我们知道任何整数都不可能被 > * n / 2* 的数整除，我们可以直接将总迭代次数减半。  

3. 我们发现最多只需要考虑到 *n<sup>1/2</sup>* 的因素，因为如果 *n* 被某个数 *p* 整除，那么 *n = p \* q* 并且 *p <= q*，我们可以推出 *p <= n<sup>1/2</sup>* 。  
现在我们的运行时间已经提升到 *O(n<sup>1.5</sup>)* 了，还有更快的方法吗？  

4. Sieve of Eratosthenes 是最有效的找到n以内质数的方法。  
从一个包含n个数的表开始，先看第一个数，2。我们知道所有2的倍数都不是质数，因此我们将它们划为非质数。我们看下一个数，3。同样的，所有3的倍数也不是质数，我们也将它们划去。下一个数，4，已经被划掉了，这说明了什么？

5. 4不是质数，因为它是2的倍数，这也意味着所有4的倍数都能被2整除，并且都已经被划掉了。所以我们可以跳过4直接看下一个数，5。现在，所有5的倍数，比如5*2=10，5*3=15等都可以被划掉。这里有一个小优化，我们不需要从5*2开始，我们应该从哪开始呢？

6. 事实上，我们可以直接从5\*5=25开始，因为5\*2=10已经作为2的倍数被划掉了，同样5\*3=15已经作为3的倍数被划掉了。因此，如果当前数是 *p* ，我们可以直接从 *p* 的倍数 *p<sup>2</sup>* 开始，然后每次增加 *p* ，划去 *p<sup>2</sup>+p*，...，现在问题是什么时候应该结束？

7. 最容易的方法就是结束条件为 *p < n*，不过别忘了#3的情况  

8. 结束条件可以是 *p < n<sup>0.5</sup>*，因为所有 *>=n<sup>0.5</sup>* 的非质数一定已经被划掉了。当循环结束时，所有在表里的非质数都被划去了。  
Sieve of Eratosthenes方法使用 *O(n)* 空间，它的运行时间是 *O(n log log n)* 

<br>

代码如下：

**Java Code**
{% highlight Java %}
public class Solution {
    // Count Primes
    public int countPrimes(int n) {
    	boolean[] isPrime = new boolean[n];
    	   for (int i = 2; i < n; i++) {
    	      isPrime[i] = true;
    	   }
    	   // Loop's ending condition is i * i < n instead of i < sqrt(n)
    	   // to avoid repeatedly calling an expensive function sqrt().
    	   for (int i = 2; i * i < n; i++) {
    	      if (!isPrime[i])
    	    	  continue;
    	      for (int j = i * i; j < n; j += i) {
    	         isPrime[j] = false;
    	      }
    	   }
    	   int count = 0;
    	   for (int i = 2; i < n; i++) {
    	      if (isPrime[i]) count++;
    	   }
    	   return count;        
    }
}
{% endhighlight %}
