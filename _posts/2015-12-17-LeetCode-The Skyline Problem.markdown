---
layout: post
title:  "LeetCode 218: The Skyline Problem"
date:   2015-12-17 21:37:00
categories: LeetCode
tags: [LeetCode, Data Structure]
---

###天际线问题

>A city's skyline is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Now suppose you are given the locations and height of all the buildings as shown on a cityscape photo (Figure A), write a program to output the skyline formed by these buildings collectively (Figure B).
>
>![附图]({{site.url}}/assets/2015-12-17-LeetCode-The Skyline Problem/skyline.png)
>
>The geometric information of each building is represented by a triplet of integers [Li, Ri, Hi], where Li and Ri are the x coordinates of the left and right edge of the ith building, respectively, and Hi is its height. It is guaranteed that 0 ≤ Li, Ri ≤ INT_MAX, 0 < Hi ≤ INT_MAX, and Ri - Li > 0. You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height 0.  
>
>For instance, the dimensions of all buildings in Figure A are recorded as: [ [2 9 10], [3 7 15], [5 12 12], [15 20 10], [19 24 8] ] .  
>
>The output is a list of "key points" (red dots in Figure B) in the format of [ [x1,y1], [x2, y2], [x3, y3], ... ] that uniquely defines a skyline. A key point is the left endpoint of a horizontal line segment. Note that the last key point, where the rightmost building ends, is merely used to mark the termination of the skyline, and always has zero height. Also, the ground in between any two adjacent buildings should be considered part of the skyline contour.  
>
>For instance, the skyline in Figure B should be represented as:[ [2 10], [3 15], [7 12], [12 0], [15 10], [20 8], [24, 0] ].  

天际线问题，可以使用高级数据结构如线段树等进行求解，不过我们还是从直观的角度看一下这个问题。

我们所要求的关键点，实际上就是轮廓中最高边缘线的左侧点，因此可以将问题转化为求解在每个阶段的最高边缘高度的问题。由于边缘高度的变化只会发生在出现建筑的两侧，因此可以通过遍历建筑边缘的方式，查找每一段边缘高度。

总结以下几点：

* 当出现建筑的左侧边缘时，将其高度添加到当前“拥有”的高度列表中。
* 当出现建筑的右侧边缘时，将其高度从当前“拥有”的高度列表中删除。
* 每次出现建筑的左侧边缘或右侧边缘时，获取当前“拥有”的最大高度，观察与之前记录的最大高度是否相同，如果不同，说明出现拐点，即出现我们需要添加的关键点。

需要实现的功能点如下：

* 能够从左到右遍历所有边缘，因此需要将所有边缘按x轴坐标从小到大排列。
* 能够维护一个当前“拥有”的高度列表，并能够快速选取最大高度，快速删除某个值。

因此需要使用优先队列这样的数据结构来实现。  
在本题求解过程中熟悉了Java中Collections的排序功能及PriorityQueue的使用。另外有一个小技巧，为了区分建筑的左右边缘，我们将左边缘的高度值用负数来表示。


**Java Code**
{% highlight Java %}

public class Solution {
    public List<int[]> getSkyline(int[][] buildings) {
        
        List<int[]> heightList = new ArrayList<int[]>();
        for(int i = 0; i < buildings.length; i++){
            int li = buildings[i][0];
            int ri = buildings[i][1];
            int hi = buildings[i][2];
            int[] leftColumn = new int[2];
            leftColumn[0] = li;
            leftColumn[1] = -hi;
            heightList.add(leftColumn);
            int[] rightColumn = new int[2];
            rightColumn[0] = ri;
            rightColumn[1] = hi;
            heightList.add(rightColumn);
        }
        
        Collections.sort(heightList, new Solution().new myComparator());
        
        List<int[]> keyPoints = new ArrayList<int[]>();
        
        PriorityQueue<Integer> maxHeap = new PriorityQueue<Integer>(11, new Comparator<Integer>(){
            public int compare(Integer a, Integer b){
                return b - a;
            }
            });
        maxHeap.add(0);
        
        int cur = 0;
        int pre = 0;
        for(int i = 0; i < heightList.size(); i++){
            if(heightList.get(i)[1] < 0){
                maxHeap.add(-heightList.get(i)[1]);
                cur = maxHeap.peek();
            }
            else{
                maxHeap.remove(heightList.get(i)[1]);
                if(maxHeap.peek() == null){
                    cur = 0;
                }
                else{
                    cur = maxHeap.peek();
                }
            }
            if(cur != pre){
                int[] x = new int[2];
                x[0] = heightList.get(i)[0];
                x[1] = cur;
                keyPoints.add(x);
                pre = cur;
            }
        }
        
        return keyPoints;
        
    }
    class myComparator implements Comparator<int[]>{
        public int compare(int[] o1, int[] o2){
            if(o1[0] < o2[0]){
                return -1;
            }
            else if (o1[0] == o2[0]){
                return o1[1] - o2[1];
            }
            else{
                return 1;
            }
        }
    }
}
{% endhighlight %}
