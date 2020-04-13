[TOC]


## Assign Cookies
>Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie. Each child i has a greed factor gi, which is the minimum size of a cookie that the child will be content with; and each cookie j has a size sj. If sj >= gi, we can assign the cookie j to the child i, and the child i will be content. Your goal is to maximize the number of your content children and output the maximum number.
>**Note:**
- You may assume the greed factor is always positive. 
- You cannot assign more than one cookie to one child.

```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int child=0;
        int cookie=0;
        while(child<g.length&&cookie<s.length){
            if(g[child]<=s[cookie]){
                child++;
            }
            cookie++;
        }
        return child;
    }
}
```

## Wiggle Subsequence
>A sequence with fewer than two elements is trivially a wiggle sequence.
>For example, `[1,7,4,9,2,5]` is a wiggle sequence because the differences (6,-3,5,-7,3) are alternately positive and negative. In contrast, `[1,4,7,2,5]` and `[1,7,4,5,5]` are not wiggle sequences, the first because its first two differences are positive and the second because its last difference is zero.
>Given a sequence of integers, return the length of the longest subsequence that is a wiggle sequence. A subsequence is obtained by deleting some number of elements (eventually, also zero) from the original sequence, leaving the remaining elements in their original order.
>**Examples:**
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">**Input:** [1,7,4,9,2,5]
**Output:** 6
The entire sequence is a wiggle sequence.
**Input:** [1,17,5,10,13,15,10,5,16,8]
**Output:** 7
There are several subsequences that achieve this length. One is [1,17,10,13,10,16,8].
**Input:** [1,2,3,4,5,6,7,8,9]
**Output:** 2
</pre>
**Follow up:**
Can you do it in O(_n_) time?

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\贪心算法\9ab594f7-6df8-43ab-a3f5-770b24d6d5a3.jpg)
```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if(nums.length<2){
            return nums.length;
        }
        final int BEGIN = 0;
        final int UP = 1;
        final int DOWN = 2;
        int STATE = BEGIN;
        int max_length = 1;
        for(int i=1;i<nums.length;i++){
            switch(STATE){
                case BEGIN:
                    if(nums[i-1]<nums[i]){
                        STATE = UP;
                        max_length++;
                    }
                    else if(nums[i-1]>nums[i]){
                        STATE = DOWN;
                        max_length++;
                    }
                    break;
                case UP:
                    if(nums[i-1]>nums[i]){
                        STATE = DOWN;
                        max_length++;
                    }
                    break;
                case DOWN:
                    if(nums[i-1]<nums[i]){
                        STATE = UP;
                        max_length++;
                    }
                    break;
            }
        }
        return max_length;
    }
}
```

## Remove K Digits
>Given a non-negative integer _num_ represented as a string, remove _k_ digits from the number so that the new number is the smallest possible.
>**Note:**
>*   The length of _num_ is less than 10002 and will be ≥ _k_.
>*   The given _num_ does not contain any leading zero.
>**Example 1:**
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">Input: num = "1432219", k = 3
Output: "1219"
Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.
</pre>
Example 2:
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">Input: num = "10200", k = 1
Output: "200"
Explanation: Remove the leading 1 and the number is 200\. Note that the output must not contain leading zeroes.
</pre>
Example 3:
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">Input: num = "10", k = 2
Output: "0"
Explanation: Remove all the digits from the number and it is left with nothing which is 0.</pre>

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\贪心算法\11af614b-5996-440b-bbdb-5592a6e40d9a.jpg)

- 思考如下问题：
 1. 当所有数字都扫描完成后，k仍然>0,应该做怎样的处理
 2. 当数字中有0出现时，应该有怎样的特殊处理
 3. 如何将最后结果存储为字符串返回

```java
class Solution {
    public String removeKdigits(String num, int k) {
        Stack<Integer> s = new Stack<>();
        String result = "";
        for(int i=0;i<num.length();i++){
            int number = num.charAt(i)-'0';
            while(s.size()!=0 && s.peek()>number && k>0 ){
                s.pop();
                k--;
            }
            if(!s.empty()||number!=0){
                s.push(number);
            }
        }
        while(s.size() != 0 && k>0){
            s.pop();
            k--;
        }
        while(!s.empty()){
            result = s.pop()+result;
        }
        if(result=="")
            return "0";
        return result;
    }
}
```

## Jump Game
>Given an array of non-negative integers, you are initially positioned at the first index of the array.
>Each element in the array represents your maximum jump length at that position.
>Determine if you are able to reach the last index.
>**For example:**
>A = `[2,3,1,1,4]`, return `true`.
>A = `[3,2,1,0,4]`, return `false`.

```java
class Solution {
    public boolean canJump(int[] nums) {
        int[] index = new int[nums.length];
        for(int i=0;i<nums.length;i++){
            index[i] = i+nums[i];
        }
        int jump = 0;
        int max_index = index[0];
        while(jump<=max_index&&jump<nums.length){
            if(max_index<index[jump]){
                max_index=index[jump];
            }
            jump++;
        }
        if(max_index>=nums.length-1){
            return true;
        }
        return false;
    }
}
```

## Jump Game II
>Given an array of non-negative integers, you are initially positioned at the first index of the array.
>Each element in the array represents your maximum jump length at that position.
>Your goal is to reach the last index in the minimum number of jumps.
>For example:
>Given array A = [2,3,1,1,4]
>The minimum number of jumps to reach the last index is 2. (Jump 1 step from index 0 to 1, then 3 steps to the last index.)
>Note:
>You can assume that you can always reach the last index.

```java
class Solution {
    public int jump(int[] nums) {
        if(nums.length<2){
            return 0;
        }
        int current_max_index = nums[0];
        int pre_max_max_index = nums[0];
        int jump_min = 1;
        for (int i=1;i<nums.length;i++){
            if(i>current_max_index){
                jump_min++;
                current_max_index = pre_max_max_index;
            }
            if(pre_max_max_index<nums[i]+i){
                pre_max_max_index=nums[i]+i;
            }
        }
        return jump_min;
    }
}
```

## Minimum Number of Arrows to Burst Balloons
>There are a number of spherical balloons spread in two-dimensional space. For each balloon, provided input is the start and end coordinates of the horizontal diameter. Since it's horizontal, y-coordinates don't matter and hence the x-coordinates of start and end of the diameter suffice. Start is always smaller than end. There will be at most 10<sup style="box-sizing: border-box; position: relative; font-size: 12px; line-height: 0; vertical-align: baseline; top: -0.5em;">4</sup> balloons.
>An arrow can be shot up exactly vertically from different points along the x-axis. A balloon with x<sub style="box-sizing: border-box; position: relative; font-size: 12px; line-height: 0; vertical-align: baseline; bottom: -0.25em;">start</sub> and x<sub style="box-sizing: border-box; position: relative; font-size: 12px; line-height: 0; vertical-align: baseline; bottom: -0.25em;">end</sub> bursts by an arrow shot at x if x<sub style="box-sizing: border-box; position: relative; font-size: 12px; line-height: 0; vertical-align: baseline; bottom: -0.25em;">start</sub> ≤ x ≤ x<sub style="box-sizing: border-box; position: relative; font-size: 12px; line-height: 0; vertical-align: baseline; bottom: -0.25em;">end</sub>. There is no limit to the number of arrows that can be shot. An arrow once shot keeps travelling up infinitely. The problem is to find the minimum number of arrows that must be shot to burst all balloons.
>**Example:**
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">**Input:**
[[10,16], [2,8], [1,6], [7,12]]
**Output:**
2
**Explanation:**
One way is to shoot one arrow for example at x = 6 (bursting the balloons [2,8] and [1,6]) and another arrow at x = 11 (bursting the other two balloons).</pre>

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\贪心算法\f8cc9fa6-185f-4aa1-bb37-17f82016158f.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\贪心算法\70065a16-c400-453c-bc14-e7ed7682f442.jpg)

```java
class Solution {
    public int findMinArrowShots(int[][] points) {
        if(points.length<=1) return points.length;
        Arrays.sort(points,new Comparator<Object>(){
            public int compare(Object o1, Object o2) {
                int[] one = (int[]) o1;
                int[] two = (int[]) o2;
                int k = 0;
                if (one[k] > two[k]) {
                    return 1;
                } else if (one[k] < two[k]) {
                    return -1;
                }
                return 0;
            }
        });
        int left=points[0][0];
        int right=points[0][1];
        int arrows=1;
        for(int i=1;i<points.length;i++){
            if(points[i][0]>right){
                arrows++;
                left = points[i][0];
                right = points[i][1];
            }else{
                left=maxIndex(left,points[i][0]);
                right=minIndex(right,points[i][1]);
            }
        }
        return arrows;
    }
    public int maxIndex(int a,int b){
        return Math.max(a,b);
    }
    public int minIndex(int a,int b){
        return Math.min(a,b);
    }
}
```

## Merge Intervals
>Given a collection of intervals, merge all overlapping intervals.
>For example,
>Given `[1,3],[2,6],[8,10],[15,18]`,
>return `[1,6],[8,10],[15,18]`.

```java
/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
 *     Interval() { start = 0; end = 0; }
 *     Interval(int s, int e) { start = s; end = e; }
 * }
 */
class Solution {
    public List<Interval> merge(List<Interval> intervals) {
        if(intervals.size()<2) return intervals;
        Collections.sort(intervals,new Comparator<Interval>(){
           public int compare(Interval I1,Interval I2){
               return I1.start-I2.start;
           }
        });
        List<Interval> result = new ArrayList<>();
        Interval bef = intervals.get(0);
        for(int i=1;i<intervals.size();i++){
            Interval beh = intervals.get(i);
            if(bef.end<beh.start){
                result.add(bef);
                bef = beh;
            }else{
                int start = bef.start;
                int end = Math.max(bef.end,beh.end);
                bef = new Interval(start,end);
            }
        }
        result.add(bef);
        return result;
    }
}
```

## 加油站问题

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\贪心算法\590937a5-e44c-4eaa-bbab-0e13f9583804.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\贪心算法\820f3277-6fe8-414c-ad45-ac38e83b866a.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\贪心算法\6fa911f7-ad1d-4706-b917-40a6d1623fa5.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\贪心算法\4ee2cabe-7b23-4962-b65d-102e25ddd2e8.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\贪心算法\6240521a-5bf2-4375-b1a9-422166ef625c.jpg)