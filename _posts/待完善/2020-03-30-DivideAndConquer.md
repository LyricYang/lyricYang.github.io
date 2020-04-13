[TOC]

---

## Subsets
>Given a set of **distinct** integers, _nums_, return all possible subsets (the power set).
>**Note:** The solution set must not contain duplicate subsets.
>For example,
>If **_nums_** = `[1,2,3]`, a solution is:
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]</pre>

### 方法一：
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\分治算法\ffaed612-5537-489d-9dcb-969dd8d26298.jpg)

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        List<Integer> item = new ArrayList<>();
        result.add(item);
        generate(0,nums,item,result);
        return result;
    }
    private void generate(int i,int[] nums,List<Integer> item,List<List<Integer>> result){
        if(i>=nums.length){
            return;
        }
        item.add(nums[i]);
        result.add(new ArrayList<Integer>(item));//注意点
        generate(i+1,nums,item,result);
        item.remove(item.size()-1);
        generate(i+1,nums,item,result);
    }
}
```

### 方法二：
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\分治算法\248d39c5-ba32-4efe-a8ad-331a4537e3cd.jpg)

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\分治算法\c1a1c593-ded4-49d9-bc34-bce0fca706e6.jpg)

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        int all_set = (int)Math.pow(2,nums.length);
        for(int i=0;i<all_set;i++){
            List<Integer> item = new ArrayList<>();
            for(int j=0;j<nums.length;j++){
                if(((i>>j)&1)==1){
                    item.add(nums[j]);
                }
            }
            result.add(item);
        }
        return result;
    }
}
```

## Subsets II
>Given a collection of integers that might contain duplicates, **_nums_**, return all possible subsets (the power set).
>**Note:** The solution set must not contain duplicate subsets.
>For example,
>If **_nums_** = `[1,2,2]`, a solution is:
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]</pre>

```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> item = new ArrayList<>();
        Set<List<Integer>> res_set = new HashSet<>();
        Arrays.sort(nums);
        result.add(item);
        generate(0,nums,result,item,res_set);
        return result;
    }
    
    private void generate(int i,int[] nums,List<List<Integer>> result,List<Integer> item,Set<List<Integer>> res_set){
        if(i>=nums.length){
            return;
        }
        item.add(nums[i]);
        List<Integer> temp = new ArrayList<>(item);
        if(!res_set.contains(temp)){
            result.add(temp);
            res_set.add(temp);
        }
        generate(i+1,nums,result,item,res_set);
        item.remove(item.size()-1);
        generate(i+1,nums,result,item,res_set);
    }
}
```

## Combination Sum II
>Given a collection of candidate numbers (**_C_**) and a target number (**_T_**), find all unique combinations in **_C_** where the candidate numbers sums to **_T_**.
>Each number in **_C_** may only be used **once** in the combination.
>**Note:**
>*   All numbers (including target) will be positive integers.
>*   The solution set must not contain duplicate combinations.
>For example, given candidate set `[10, 1, 2, 7, 6, 1, 5]` and target `8`, 
>A solution set is: 
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]</pre>

```java
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> item = new ArrayList<>();
        Set<List<Integer>> res_set = new HashSet<>();
        Arrays.sort(candidates);
        generate(0,candidates,result,item,res_set,0,target);
        return result;
    }
    
    public void generate(int i,int[] nums,List<List<Integer>> result,List<Integer> item,Set<List<Integer>> res_set,int sum,int target){
        if(i>=nums.length||sum>target){
            return;
        }
        sum = sum+nums[i];
        item.add(nums[i]);
        List<Integer> temp = new ArrayList<>(item);
        if(sum==target&&!res_set.contains(temp)){
            result.add(temp);
            res_set.add(temp);
        }
        generate(i+1,nums,result,item,res_set,sum,target);
        sum=sum-nums[i];
        item.remove(item.size()-1);
        generate(i+1,nums,result,item,res_set,sum,target);
    }
}
```

```java
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        //All numbers (including target) will be positive integers.
        //no dup
        List<List<Integer>> result = new ArrayList<>(); 
        if(candidates.length == 0){
            return result;
        }
        Arrays.sort(candidates);
        dfs(result, new ArrayList<>(), 0, target, candidates);
        return result;
    }
    private void dfs(List<List<Integer>> result, List<Integer> temp, int index, int target, int[] coins){
        //termination condition;
        if(target < 0){
            return ;
        }
        if(target == 0){
            result.add(new ArrayList<>(temp));
            return;
        }
        for(int i = index; i < coins.length && target >= coins[i]; i++){
            //when i is bigger than index & still duplicates we continue;
            if(i > index && coins[i] == coins[i-1]){
                continue;
            }
            temp.add(coins[i]);
            dfs(result, temp, i + 1, target - coins[i], coins); // we cannot reuse it;
            temp.remove(temp.size()-1);
        }
    }
}
```

## Generate Parentheses
>Given _n_ pairs of parentheses, write a function to generate all combinations of well-formed parentheses.
>For example, given _n_ = 3, a solution set is:
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]</pre>

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<>();
        generate("",n,n,result);
        return result;
    }
    private void generate(String item,int left,int right,List<String> result){
        if(left==0 && right==0){
            result.add(item);
            return;
        }
        if(left>0){
            generate(item+"(",left-1,right,result);
        }
        if(left<right){
            generate(item+")",left,right-1,result);
        }
    }
}
```

## N-Queens
>The _n_-queens puzzle is the problem of placing _n_ queens on an _n_×_n_ chessboard such that no two queens attack each other.
>![](https://leetcode.com/static/images/problemset/8-queens.png)
>Given an integer _n_, return all distinct solutions to the _n_-queens puzzle.
>Each solution contains a distinct board configuration of the _n_-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space respectively.
>For example,
>There exist two distinct solutions to the 4-queens puzzle:
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">[
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],
 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]</pre>

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\分治算法\9d337bb3-1376-49b6-ae8b-a9faa2e52eae.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\分治算法\83331d41-61f2-41d1-b6de-3ad024a8c574.jpg)

```java
class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> result = new ArrayList<>();
        int[][] mark = new int[n][n];
        List<String> location = new ArrayList<>();
        for(int i=0;i<n;i++){
            String loc = "";
            for(int j=0;j<n;j++){
                loc=loc+".";
                mark[i][j] = 0;
            }
            location.add(loc);
        }
        generate(0,n,location,result,mark);
        return result;
    }
    private void generate(int k,int n,List<String> location,List<List<String>> result,int[][] mark){
        if(k==n){
            List<String> res = new ArrayList<>(location); 
            result.add(res);
            return;
        }
        for(int i=0;i<n;i++){
            if(mark[k][i]==0){
                int[][] temp_mark = new int[n][n];
                for(int x=0;x<n;x++){
                    for(int y=0;y<n;y++){
                        temp_mark[x][y]=mark[x][y];
                    }
                }
                String s=location.get(k);
                StringBuilder temp_string = new StringBuilder(s);
                location.remove(k);
                temp_string.replace(i,i+1,"Q");
                location.add(k,temp_string.toString());
                put_down_the_queen(k,i,mark);
                generate(k+1,n,location,result,mark);
                mark = temp_mark;
                location.remove(k);
                location.add(k,s);
            }
        }
    }
    private void put_down_the_queen(int x,int y,int[][] mark){
        final int dx[]={-1,1,0,0,-1,-1,1,1};
        final int dy[]={0,0,-1,1,-1,1,-1,1};
        mark[x][y] = 1;
        for(int i=1;i<mark.length;i++){
            for(int j=1;j<8;j++){
                int new_x = x+i*dx[j];
                int new_y = y+i*dy[j];
                if(new_x>=0&&new_x<mark.length&&new_y>=0&&new_y<mark.length){
                    mark[new_x][new_y] = 1;
                }
            }
        }
    }
}
```

## Combination Sum
>Given a **set** of candidate numbers (**_C_**) **(without duplicates)** and a target number (**_T_**), find all unique combinations in **_C_** where the candidate numbers sums to **_T_**.
>The **same** repeated number may be chosen from **_C_** unlimited number of times.
>**Note:**
>*   All numbers (including target) will be positive integers.
>*   The solution set must not contain duplicate combinations.
>For example, given candidate set `[2, 3, 6, 7]` and target `7`, 
>A solution set is: 
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">[
  [7],
  [2, 2, 3]
]</pre>

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);
        List<List<Integer>> result = new ArrayList<>();
        getResult(result, new ArrayList<Integer>(),candidates,target,0);
        return result;
    }
    
    public void getResult(List<List<Integer>> result,List<Integer> cur,int [] candidates,int target,int start){
        if(target>0){
            for(int i=start;i<candidates.length&&target>=candidates[i];i++){
                cur.add(candidates[i]);
                getResult(result,cur,candidates,target-candidates[i],i);
                cur.remove(cur.size()-1);
            }
        }else if(target==0){
            result.add(new ArrayList<Integer>(cur));
        }
        
    }
}
```

## Permutations
>Given a collection of **distinct** numbers, return all possible permutations.
>For example,
>`[1,2,3]` have the following permutations:
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]</pre>

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        getResult(result,nums,new ArrayList<Integer>(),new HashSet<Integer>());
        return result;
    }
    
    public void getResult(List<List<Integer>> result,int[] nums,List<Integer> cur,Set<Integer> indexSet){
        if(indexSet.size()<nums.length){
            for(int i=0;i<nums.length;i++){
                if(indexSet.add(i)){
                    cur.add(nums[i]);
                    getResult(result,nums,cur,indexSet);
                    indexSet.remove(i);
                    cur.remove(cur.size()-1);
                }
                
            }
        }else{
            result.add(new ArrayList<Integer>(cur));
        }
    }
}
```

>Given two integers _n_ and _k_, return all possible combinations of _k_ numbers out of 1 ... _n_.
>For example,
>If _n_ = 4 and _k_ = 2, a solution is:
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]</pre>

```java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> result = new ArrayList<>();
        Set<Integer> map = new HashSet<Integer>();
        getCombine(result,map,new ArrayList<Integer>(),1,n,k);
        return result;
    }
    
    public void getCombine(List<List<Integer>> result,Set<Integer> map,List<Integer> cur,int start,int n,int k){
        if(cur.size()==k){
            result.add(new ArrayList<Integer>(cur));
        }else{
            for(int i=start;i<=n;i++){
                cur.add(i);
                getCombine(result,map,cur,i+1,n,k);
                cur.remove(cur.size()-1);
            }
        }
    }
}
```