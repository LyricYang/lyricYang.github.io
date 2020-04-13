[TOC]


# 二分查找
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\查找算法\6b5175ee-8069-43f8-87dc-e2d276f775c6.jpg)

### Search Insert Position
>Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.
>You may assume no duplicates in the array.
>**Example 1:**
>Input: [1,3,5,6], 5
>Output: 2
>**Example 2:**
>Input: [1,3,5,6], 2
>Output: 1
>**Example 3:**
>Input: [1,3,5,6], 7
>Output: 4
>**Example 4:**
>Input: [1,3,5,6], 0
>Output: 0

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int begin = 0;
        int end = nums.length-1;
        int mid = -1;
        while(begin<=end){
            mid=(begin+end)/2;
            if(nums[mid]==target){
                return mid;
            }
            else if(target<nums[mid]){
                if(mid==0||target>nums[mid-1]){
                    return mid;
                }
                end = mid-1 ;
            }else if(target>nums[mid]){
                if(mid==nums.length-1||target<nums[mid+1]){
                    return mid+1;
                }
                begin = mid+1;
            }
        }
        return mid;
    }
}
```


### Search for a Range

>Given an array of integers sorted in ascending order, find the starting and ending position of a given target value.
>Your algorithm's runtime complexity must be in the order of _O_(log _n_).
>If the target is not found in the array, return `[-1, -1]`.
>**For example,**
>Given `[5, 7, 7, 8, 8, 10]` and target value 8,
>return `[3, 4]`.

- 查找左端点
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\查找算法\29f4ad5a-a808-4542-b95a-a1617243101e.jpg)
- 查找右端点
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\查找算法\f98a4c29-fb8b-404b-b564-8aa9c298b109.jpg)

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int[] res = new int[2];
        res[0] = left_bound(nums,target);
        res[1] = right_bound(nums,target);
        return res;
    }
    
    private int left_bound(int[] nums,int target){
        int begin = 0;
        int end = nums.length-1;
        while(begin<=end){
            int mid = (begin+end)/2;
            if(target==nums[mid]){
                if(mid==0||target>nums[mid-1]){
                    return mid;
                }
                end = mid-1;
            }else if(target<nums[mid]){
                end = mid-1;
            }else if(target>nums[mid]){
                begin = mid+1;
            }
        }
        
        return -1;
    }
    
    private int right_bound(int[] nums,int target){
        int begin = 0;
        int end = nums.length-1;
        while(begin<=end){
            int mid = (begin+end)/2;
            if(target==nums[mid]){
                if(mid==nums.length-1||target<nums[mid+1]){
                    return mid;
                }
                begin = mid+1;
            }else if(target<nums[mid]){
                end = mid-1;
            }else if(target>nums[mid]){
                begin = mid+1;
            }
        }
        
        return -1;
    }
}
```

### Search in Rotated Sorted Array

>Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.
>(i.e., `0 1 2 4 5 6 7` might become `4 5 6 7 0 1 2`).
>You are given a target value to search. If found in the array return its index, otherwise return -1.
>You may assume no duplicate exists in the array.

```java
class Solution {
    public int search(int[] nums, int target) {
        int begin = 0;
        int end = nums.length-1;
        while(begin<=end){
            int mid = (begin+end)/2;
            if(nums[mid]==target){
                return mid;
            }else if(target<nums[mid]){
                if(nums[begin]<nums[mid]){
                    if(target>=nums[begin])
                        end = mid-1;
                    else
                        begin = mid+1;
                }else if(nums[begin]>nums[mid]){
                    end = mid-1;
                }else if(nums[begin]==nums[mid]){
                    begin = mid+1;
                }
            }else if(target>nums[mid]){
                if(nums[begin]<nums[mid]){
                    begin = mid+1;
                }else if(nums[begin]>nums[mid]){
                    if(target>=nums[begin])
                        end = mid-1;
                    else
                        begin = mid+1;
                }else if(nums[begin]==nums[mid]){
                    begin = mid+1;
                }
            }
        }
        return -1;
    }
}
```

# 搜索算法
### Number of Islands
>Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.
>Example 1:
>11110
>11010
>11000
>00000
>Answer: 1
>Example 2:
>11000
>11000
>00100
>00011
>Answer: 3

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\查找算法\9dd48379-a266-462c-8b8e-567324d37073.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\查找算法\e180d5ed-9ee4-41cd-b1bd-f82aed8b2671.jpg)

```java
class Solution {
    public int numIslands(char[][] grid) {
        if(grid==null||grid.length<1) return 0;
        int island_num=0;
        int[][] mark=new int[grid.length][grid[0].length];
        for(int i=0;i<grid.length;i++){
            for(int j=0;j<grid[i].length;j++){
                if(mark[i][j]==0&&grid[i][j]=='1'){
                    BFS(mark,grid,i,j);
                    island_num++;
                }
            }
        }
        return island_num;
    }
    
    private void DFS(int[][] mark,char[][] grid,int x,int y){
        mark[x][y]=1;
        int[] dx={-1,1,0,0};
        int[] dy={0,0,-1,1};
        for(int i=0;i<4;i++){
            int newx=dx[i]+x;
            int newy=dy[i]+y;
            if(newx<0||newx>=mark.length||newy<0||newy>=mark[newx].length){
                continue;
            }
            if(mark[newx][newy]==0&&grid[newx][newy]=='1'){
                DFS(mark,grid,newx,newy);
            }
        }
    }
    
    private void BFS(int[][] mark,char[][] grid,int x,int y){
        int[] dx={-1,1,0,0};
        int[] dy={0,0,-1,1};
        Queue<Pair> Q = new LinkedList<>();
        Q.add(new Pair(x,y));
        mark[x][y]=1;
        while(!Q.isEmpty()){
            int tmpx=Q.peek().x;
            int tmpy=Q.peek().y;
            Q.remove();
            for(int i=0;i<4;i++){
                int newx = dx[i]+tmpx;
                int newy = dy[i]+tmpy;
                if(newx<0||newx>=mark.length||newy<0||newy>=mark[newx].length){
                    continue;
                }
                if(mark[newx][newy]==0&&grid[newx][newy]=='1'){
                    Q.add(new Pair(newx,newy));
                    mark[newx][newy]=1;
                }
            }
        }
    }
    
    class Pair{
        private int x;
        private int y;
        public Pair(int x,int y){
            this.x=x;
            this.y=y;
        }
    }
}
```


### Word Ladder
>Given two words (_beginWord_ and _endWord_), and a dictionary's word list, find the length of shortest transformation sequence from _beginWord_ to _endWord_, such that:
>1.  Only one letter can be changed at a time.
>2.  Each transformed word must exist in the word list. Note that _beginWord_ is _not_ a transformed word.
>For example,
>Given:
>_beginWord_ = `"hit"`
>_endWord_ = `"cog"`
>_wordList_ = `["hot","dot","dog","lot","log","cog"]`
>As one shortest transformation is `"hit" -> "hot" -> "dot" -> "dog" -> "cog"`,
>return its length `5`.
>**Note:**
>*   Return 0 if there is no such transformation sequence.
>*   All words have the same length.
>*   All words contain only lowercase alphabetic characters.
>*   You may assume no duplicates in the word list.
>*   You may assume _beginWord_ and _endWord_ are non-empty and are not the same.


```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        Map<String,List<String>> graph = new HashMap<>();
        construct_graph(beginWord,wordList,graph);
        return BFS_graph(beginWord,endWord,graph);
    }
    
    public int BFS_graph(String beginWord, String endWord,Map<String,List<String>> graph){
        Queue<Pair> Q = new LinkedList<>();
        Set<String> visit = new HashSet<>();
        Q.add(new Pair(beginWord,1));
        visit.add(beginWord);
        while(!Q.isEmpty()){
            String node = Q.peek().word;
            int step = Q.peek().step;
            Q.remove();
            if(node.equals(endWord)){
                return step;
            }
            List<String> neighbors = graph.get(node);
            for(int i=0;i<neighbors.size();i++){
                if(!visit.contains(neighbors.get(i))){
                    Q.add(new Pair(neighbors.get(i),step+1));
                    visit.add(neighbors.get(i));
                }
            }
        }
        return 0;
    }
    public boolean connect(String word1,String word2){
        int cnt =0;
        for(int i=0;i<word1.length();i++){
            if(word1.charAt(i)!=word2.charAt(i)){
                cnt++;
            }
        }
        return cnt==1;
    }
    
    public void construct_graph(String beginWord,List<String> wordList,Map<String,List<String>> graph){
        wordList.add(beginWord);
        for(int i=0;i<wordList.size();i++){
            graph.put(wordList.get(i),new ArrayList<String>());
        }
        for(int i=0;i<wordList.size();i++){
            for(int j=i+1;j<wordList.size();j++){
                if(connect(wordList.get(i),wordList.get(j))){
                    graph.get(wordList.get(i)).add(wordList.get(j));
                    graph.get(wordList.get(j)).add(wordList.get(i));
                }
            }
        }
    }
    
    class Pair{
        private String word;
        private int step;
        public Pair(String word,int step){
            this.word = word;
            this.step = step;
        }
    }
}
```

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        Set<String> wordSet = new HashSet<>(wordList);
        wordSet.remove(beginWord);
        Set<String> begin = new HashSet<>();
        begin.add(beginWord);
        Set<String> end = new HashSet<>();
        end.add(endWord);
        if (beginWord.equals(endWord)) {
            return 1;
        }
        if (!wordSet.contains(endWord)) {
            return 0;
        }
        int len = 2;
        while (!begin.isEmpty()) {
            if (begin.size() > end.size()) {
                Set<String> tmp = begin;
                begin = end;
                end = tmp;
            }
            Set<String> set = new HashSet<>();
            for (String word : begin) {
                char[] ca = word.toCharArray();
                for (int i = 0; i < ca.length; i++) {
                    char prev = ca[i];
                    for (char c = 'a'; c <= 'z'; c++) {
                        ca[i] = c;
                        String str = new String(ca);
                        if (end.contains(str)) {
                            return len;
                        }
                        if (wordSet.contains(str)) {
                            wordSet.remove(str);
                            set.add(str);
                        }
                    }
                    ca[i] = prev;
                }
            }
            begin = set;
            len++;
        }
        return 0;
    }
}
```

### Matchsticks to Square
>Remember the story of Little Match Girl? By now, you know exactly what matchsticks the little match girl has, please find out a way you can make one square by using up all those matchsticks. You should not break any stick, but you can link them up, and each matchstick must be used **exactly** one time.
>Your input will be several matchsticks the girl has, represented with their stick length. Your output will either be true or false, to represent whether you could make one square using all the matchsticks the little match girl has.
>**Example 1:**
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">**Input:** [1,1,2,2,2]
**Output:** true
**Explanation:** You can form a square with length 2, one side of the square came two sticks with length 1.
</pre>
>**Example 2:**
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">**Input:** [3,3,3,3,4]
**Output:** false
**Explanation:** You cannot find a way to form a square with all the matchsticks.
</pre>
>**Note:**
>1.  The length sum of the given matchsticks is in the range of `0` to `10^9`.
>2.  The length of the given matchstick array will not exceed `15`.

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\查找算法\d7d765c4-c00a-4b27-8989-f1ece92929f5.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\查找算法\67f7805f-ea2d-4605-a9f2-400f581acbc4.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\查找算法\21b836aa-7eb2-49b1-846b-9ed15c481c99.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\查找算法\77c195a2-13d5-4c1d-9f25-444fda7bf29b.jpg)

### Trapping Rain Water II

>Given an `m x n` matrix of positive integers representing the height of each unit cell in a 2D elevation map, compute the volume of water it is able to trap after raining.
>**Note:**
>Both _m_ and _n_ are less than 110\. The height of each unit cell is greater than 0 and is less than 20,000.
>**Example:**
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">Given the following 3x6 height map:
[
  [1,4,3,1,3,2],
  [3,2,1,3,2,4],
  [2,3,3,2,3,1]
]
Return 4.
</pre>
![](https://leetcode.com/static/images/problemset/rainwater_empty.png)
The above image represents the elevation map `[[1,4,3,1,3,2],[3,2,1,3,2,4],[2,3,3,2,3,1]]` before the rain.
![](https://leetcode.com/static/images/problemset/rainwater_fill.png)
After the rain, water is trapped between the blocks. The total volume of water trapped is 4.

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\查找算法\375ba0f4-9c6c-4e1d-b804-05528462620a.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\查找算法\163b5706-7467-49f9-bbef-62cdce373074.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\查找算法\8f9b99df-f06d-4aea-950f-6b969195bb0c.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\查找算法\7001a8ee-d221-4bb2-94b9-6540012f8193.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\查找算法\b348aeea-903d-4439-a0e1-77714a46832f.jpg)