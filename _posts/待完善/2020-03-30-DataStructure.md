# 数据结构

## 栈&队列

> 栈是一个元素集合，支持两个基本操作：push用于将元素压入栈，pop用于删除栈顶元素。

*   **后进先出**的数据结构（Last In First Out, LIFO）
*   时间复杂度
    *   索引：O(n)
    *   查找：O(n)
    *   插入：O(1)
    *   删除：O(1)

> 队列是一个元素集合，支持两种基本操作：enqueue 用于添加一个元素到队列，dequeue 用于删除队列中的一个元素。

*   **先进先出**的数据结构（First In First Out, FIFO）。
*   时间复杂度
    *   索引：O(n)
    *   查找：O(n)
    *   插入：O(1)
    *   删除：O(1)
### 经典问题

#### Implement Stack using Queues

>Implement the following operations of a stack using queues.
>
>*   push(x) -- Push element x onto stack.
>*   pop() -- Removes the element on top of the stack.
>*   top() -- Get the top element.
>*   empty() -- Return whether the stack is empty.
>    **Notes:**
>*   You must use _only_ standard operations of a queue -- which means only `push to back`, `peek/pop from front`, `size`, and `is empty` operations are valid.
>*   Depending on your language, queue may not be supported natively. You may simulate a queue by using a list or deque (double-ended queue), as long as you use only standard operations of a queue.
>*   You may assume that all operations are valid (for example, no pop or top operations will be called on an empty stack).


- **方法一：**

![](D:\Blog\LyricYang.github.io\img\datastructure\pic1.jpg)

```java
class MyStack {
    private LinkedList<Integer> q1 = new LinkedList<>();
    /** Initialize your data structure here. */
    public MyStack() {

    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        LinkedList<Integer> temp = new LinkedList<>();
        temp.add(x);
        while(q1.size()>0){
            temp.add(q1.pop());
        }
        while(temp.size()>0){
            q1.add(temp.pop());
        } 
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return q1.remove();
    }
    
    /** Get the top element. */
    public int top() {
        return q1.peek();
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return q1.isEmpty();
    }
}
```

- **方法二：**

```java
class MyStack {
    private LinkedList<Integer> q1 = new LinkedList<>();
    /** Initialize your data structure here. */
    public MyStack() {

    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        q1.add(x);
        int sz = q1.size();
        while (sz > 1) {
            q1.add(q1.remove());
            sz--;
        }
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return q1.remove();
    }
    
    /** Get the top element. */
    public int top() {
        return q1.peek();
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return q1.isEmpty();
    }
}
```

#### Implement Queue using Stacks

>Implement the following operations of a queue using stacks.
>
>*   push(x) -- Push element x to the back of queue.
>*   pop() -- Removes the element from in front of queue.
>*   peek() -- Get the front element.
>*   empty() -- Return whether the queue is empty.
>    **Notes:**
>*   You must use _only_ standard operations of a stack -- which means only `push to top`, `peek/pop from top`, `size`, and `is empty`operations are valid.
>*   Depending on your language, stack may not be supported natively. You may simulate a stack by using a list or deque (double-ended queue), as long as you use only standard operations of a stack.
>*   You may assume that all operations are valid (for example, no pop or peek operations will be called on an empty queue).

![](D:\Blog\LyricYang.github.io\img\datastructure\pic2.jpg)

```java
public class MyQueue {
    Stack<Integer> stack1 = new Stack<>();
    Stack<Integer> stack2 = new Stack<>();
    public MyQueue(){

    }

    public void push(Integer item){
        while(!stack1.empty()){
            stack2.push(stack1.pop());
        }
        stack2.push(item);
        while(!stack2.empty()){
            stack1.push(stack2.pop());
        }
    }

    public Integer pop(){
        return stack1.pop();
    }

    public Integer peek(){
        return stack1.peek();
    }

    public boolean isEmpty(){
        return stack1.isEmpty();
    }

    public static void main(String[] args){
        MyQueue myQueue = new MyQueue();
        for(int i=0; i<10; i++){
            myQueue.push(i);
        }
        while(!myQueue.isEmpty()){
            System.out.println(myQueue.pop());
        }
    }
}
```

#### Min Stack

>Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
>
>*   push(x) -- Push element x onto stack.
>*   pop() -- Removes the element on top of the stack.
>*   top() -- Get the top element.
>*   getMin() -- Retrieve the minimum element in the stack.

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.
```

![](D:\Blog\LyricYang.github.io\img\Algorithm\StackQueueHeap\pic3.jpg)

```java
public class MinStack {
    private Stack<Integer> mainStack = new Stack<>();
    private Stack<Integer> minStack = new Stack<>();

    public MinStack(){

    }

    public void push(Integer item){
        mainStack.push(item);
        if(minStack.isEmpty()){
            minStack.push(item);
        }else{
            minStack.push(minStack.peek()>item?item:minStack.peek());
        }
    }

    public Integer pop(){
        minStack.pop();
        return mainStack.pop();
    }

    public Integer top(){
        return mainStack.peek();
    }

    public Integer getMin(){
        return minStack.peek();
    }

    public static void main(String[] args){
        MinStack minStack = new MinStack();
        minStack.push(-2);
        minStack.push(0);
        minStack.push(-3);
        System.out.println(minStack.getMin());
        minStack.pop();
        minStack.top();
        System.out.println(minStack.getMin());
    }
}
```

#### 出栈顺序是否合法

>  **给定一个入栈序列，给定一个出栈序列，判断该出栈序列是否合法。** 

![](D:\Blog\LyricYang.github.io\img\datastructure\pic4.jpg)

```java
public class ValidOrder {

    public boolean checkIsValidOrder(Queue<Integer> order, Queue<Integer> items){
        if((order==null || order.size()==0) && (items==null || items.size()==0)){
            return true;
        }
        if(order != null && items!= null){
            if(order.size() != items.size()){
                return false;
            }else{
                Stack<Integer> stack = new Stack<>();
                while(!items.isEmpty()){
                    stack.push(items.poll());
                    while(!stack.isEmpty() && !order.isEmpty() && stack.peek().equals(order.peek())){
                        stack.pop();
                        order.poll();
                    }
                }
                if(order.size()==0){
                    return true;
                }
            }
        }
        return false;
    }

    public static void main(String[] args){
        Queue<Integer> order = new LinkedList<>();
        order.add(3);
        order.add(2);
        order.add(5);
        order.add(4);
        order.add(1);
        Queue<Integer> items = new LinkedList<>();
        items.add(1);
        items.add(2);
        items.add(3);
        items.add(4);
        items.add(5);
        ValidOrder validOrder = new ValidOrder();
        System.out.println(validOrder.checkIsValidOrder(order,items));
    }
}
```

#### Basic Calculator

>Implement a basic calculator to evaluate a simple expression string.
>The expression string may contain open ( and closing parentheses ), the plus + or minus sign -, non-negative integers and empty spaces .
>You may assume that the given expression is always valid.
>**Some examples:**

```
"1 + 1" = 2
" 2-1 + 2 " = 3
"(1+(4+5+2)-3)+(6+8)" = 23
```


```java
public class BasicCalculator {

    public List<String> parseExpression(String s){
        List<String> expression = new ArrayList<>();
        int len = s.length();
        int left = 0;
        int right = 0;
        // 分割表达式
        while(right <= len && left < len){
            if(right < len && s.charAt(right)<= '9' && s.charAt(right) >= '0'){
               right++;
            }
            else{
                if(left == right){
                    right++;
                }
                String sub = s.substring(left,right);
                if(" ".equals(sub)){
                    left=right;
                }else{
                    expression.add(sub);
                    left=right;
                }
            }
        }
        return expression;
    }

    private Integer calculate(List<String> expression) {
        Stack<Integer> numStack = new Stack<>();
        Stack<String> operationStack = new Stack<>();
        int len = expression.size();
        for(int i=0; i<len; i++){
            if("(".equals(expression.get(i))){
                operationStack.push("(");
            }else if("+".equals(expression.get(i)) || "-".equals(expression.get(i))){
                if("(".equals(expression.get(i+1))){
                    operationStack.push(expression.get(i));
                }else{
                    // 执行计算
                    operationStack.push(expression.get(i));
                    numStack.push(Integer.valueOf(expression.get(++i)));
                    tempCalculate(numStack,operationStack,false);
                }
            }else if(")".equals(expression.get(i))){
                // 执行计算
                tempCalculate(numStack,operationStack,true);
            }else{
                numStack.push(Integer.valueOf(expression.get(i)));
            }
        }
        return numStack.pop();
    }

    private void tempCalculate(Stack<Integer> numStack, Stack<String> operationStack, boolean popBrackets){
        while(!operationStack.isEmpty() && !"(".equals(operationStack.peek())){
            String operation = operationStack.pop();
            int num1 = numStack.pop();
            int num2 = numStack.pop();
            if("+".equals(operation)){
                numStack.push(num1+num2);
            }
            if("-".equals(operation)){
                numStack.push(num2-num1);
            }
        }
        if(popBrackets && "(".equals(operationStack.peek())){
            operationStack.pop();
            tempCalculate(numStack,operationStack,false);
        }
    }

    public static void main(String[] args){
        BasicCalculator basicCalculator = new BasicCalculator();
        List<String> expression = basicCalculator.parseExpression("(1+(4+5+2)-3)+(6+8)");
        int result = basicCalculator.calculate(expression);
        System.out.println(result);
    }
}
```

## 堆

> 堆是一种基于树的满足某些特性的数据结构：整个堆中的所有父子节点的键值都满足相同的排序条件。堆分为最大堆和最小堆。在最大堆中，父节点的键值永远大于等于所有子节点的键值，根节点的键值是最大的。最小堆中，父节点的键值永远小于等于所有子节点的键值，根节点的键值是最小的。

*   时间复杂度
    *   索引：O(log(n))
    *   查找：O(log(n))
    *   插入：O(log(n))
    *   删除：O(log(n))
    *   删除最大值/最小值：O(1)

### 经典问题

#### Kth Largest Element in an Array
>Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.
>**For example**
>Given [3,2,1,5,6,4] and k = 2, return 5.
>**Note: **
>You may assume k is always valid, 1 ≤ k ≤ array's length.

```java
public class Heap {

    public void buildMinHeap(List<Integer> list){
        int len = list.size();
        for(int i=len/2; i>=0; i--){
            int parent = i;
            int child = i*2+1;
            while(child < len){
                if(child<len && child+1 < len && list.get(child) > list.get(child+1)){
                    child ++;
                }
                if(list.get(parent) < list.get(child)){
                    break;
                }else{
                    change(list, parent, child);
                    parent=child;
                    child = child*2+1;
                }
            }
        }
    }

    public int findKValue(List<Integer> list, int k){
        List<Integer> kList = new ArrayList<>();
        for(int i=0; i<list.size(); i++){
            kList.add(list.get(i));
            if(i>=k){
                buildMinHeap(kList);
                kList.remove(0);
            }

        }
        return kList.get(0);
    }

    public void change(List<Integer> list, int left, int right){
        int temp = list.get(left);
        list.set(left,list.get(right));
        list.set(right,temp);
    }


    public static void main(String[] args){
        List<Integer> list = Arrays.asList(5,8,6,9,4,1,2);
        Heap heap = new Heap();
        System.out.println(heap.findKValue(list, 3));
    }
}
```

#### Find Median from Data Stream
>Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.
>Examples: 
>[2,3,4] , the median is 3
>[2,3], the median is (2 + 3) / 2 = 2.5
>Design a data structure that supports the following two operations:
- void addNum(int num) - Add a integer number from the data stream to the data structure.
- double findMedian() - Return the median of all elements so far.

**For example:**

```
addNum(1)
addNum(2)
findMedian() -> 1.5
addNum(3) 
findMedian() -> 2
```

![](D:\Blog\LyricYang.github.io\img\datastructure\002feb22-abf2-471c-bd76-7cae55052364.jpg)
![](D:\Blog\LyricYang.github.io\img\datastructure\afa72e3f-289e-4b46-a737-67784633f39c.jpg)
![](D:\Blog\LyricYang.github.io\img\datastructure\9d44e411-52a3-42a7-8992-e556b1d9d9b3.jpg)

![](D:\Blog\LyricYang.github.io\img\datastructure\2a68f677-895b-4660-b9c5-f41a808c1547.jpg)

## Hash表

> 哈希表（散列表），是根据关键字（Key）直接进行访问的数据结构，它通过将关键字映射到表中的一个位置来直接访问，以加快查找值的速度。这个映射函数叫哈希函数，存放记录的数据叫哈希表。

### 经典问题

#### Longest Palindrome

>Given a string which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.
>This is case sensitive, for example `"Aa"` is not considered a palindrome here.
>**Note:**
>Assume the length of given string will not exceed 1,010.
>**Example:**
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">Input:
"abccccdd"
Output:
7
Explanation:
One longest palindrome that can be built is "dccaccd", whose length is 7.</pre>

```java
class Solution {
    public int longestPalindrome(String s) {
        int[] char_map= new int[128];
        int max_length = 0;
        int flag = 0;
        for(int i=0;i<s.length();i++){
            char_map[s.charAt(i)]++;
        }
        for(int i=0;i<128;i++){
            int tmp = char_map[i];
            if(tmp%2==0){
                max_length+=tmp;
            }else{
                flag=1;
                max_length+=tmp;
                max_length-=1;
            }
        }
        return max_length+flag;
    }
}
```

#### Word Pattern

>Given a `pattern` and a string `str`, find if `str` follows the same pattern.
>Here **follow** means a full match, such that there is a bijection between a letter in `pattern` and a **non-empty** word in `str`.
>**Examples:**
>
>1.  pattern = `"abba"`, str = `"dog cat cat dog"` should return true.
>2.  pattern = `"abba"`, str = `"dog cat cat fish"` should return false.
>3.  pattern = `"aaaa"`, str = `"dog cat cat dog"` should return false.
>4.  pattern = `"abba"`, str = `"dog dog dog dog"` should return false.
>**Notes:**
>You may assume `pattern` contains only lowercase letters, and `str` contains lowercase letters separated by a single space.
>**Credits:**
>Special thanks to [@minglotus6](https://leetcode.com/discuss/user/minglotus6) for adding this problem and creating all test cases.

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\哈希和字符串\ac3de417-fff7-4820-96d1-670332889a8b.jpg)

```java
class Solution {
    public boolean wordPattern(String pattern, String str) {
        String[] words = str.split(" ");
        if (words.length != pattern.length())
            return false;
        Map index = new HashMap();
        for (Integer i=0; i<words.length; ++i)
            if (index.put(pattern.charAt(i), i) != index.put(words[i], i))
                return false;
        return true;
    }
}
```

#### Group Anagrams

>Given an array of strings, group anagrams together.
>For example, given: `["eat", "tea", "tan", "ate", "nat", "bat"]`, 
>Return:
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">[
  ["ate", "eat","tea"],
  ["nat","tan"],
  ["bat"]
]</pre>
>**Note:** All inputs will be in lower-case.

```java
public class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs == null || strs.length == 0) return new ArrayList<List<String>>();
        Map<String, List<String>> map = new HashMap<String, List<String>>();
        for (String s : strs) {
            char[] ca = s.toCharArray();
            Arrays.sort(ca);
            String keyStr = String.valueOf(ca);
            if (!map.containsKey(keyStr)) map.put(keyStr, new ArrayList<String>());
            map.get(keyStr).add(s);
        }
        return new ArrayList<List<String>>(map.values());
    }
}
```

#### Longest Substring Without Repeating Characters

>Given a string, find the length of the **longest substring** without repeating characters.
>**Examples:**
>Given `"abcabcbb"`, the answer is `"abc"`, which the length is 3.
>Given `"bbbbb"`, the answer is `"b"`, with the length of 1.
>Given `"pwwkew"`, the answer is `"wke"`, with the length of 3\. Note that the answer must be a **substring**, `"pwke"` is a _subsequence_ and not a substring.

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\哈希和字符串\e482a409-e615-46a6-9da9-d1243d16bb70.jpg)
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int[] map = new int[256];
        Arrays.fill(map, -1);
        int max = 0;
        for(int l=0, r=0; r<s.length(); r++){
            if(map[s.charAt(r)] >= l){
                l=map[s.charAt(r)] + 1;
            }
            map[s.charAt(r)] = r;
            max = Math.max(max, r-l+1);
        }
        return max;
    }
}
```
#### Repeated DNA Sequences

>All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.
>Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.
>For example,
<pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">Given s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT",
Return:
["AAAAACCCCC", "CCCCCAAAAA"].</pre>

```java
class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        Map<String,Integer> word_map = new HashMap<>();
        List<String> result = new ArrayList<>();
        if(s.length()<=10) return result;
        for(int i=0;i<s.length()-9;i++){
            String word = s.substring(i,i+10);
            if(word_map.containsKey(word)){
                word_map.put(word,word_map.get(word)+1);
            }else{
                word_map.put(word,1);
            }
        }
        Iterator<Map.Entry<String, Integer>> entries = word_map.entrySet().iterator();
        while (entries.hasNext()) {
            Map.Entry<String, Integer> entry = entries.next();
            if(entry.getValue()>1){
                result.add(entry.getKey());
            }
        }
        return result;
    }
}
```
- `Iterator<Map.Entry<String, Integer>> entries = word_map.entrySet().iterator();`

```java
class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        Set<Integer> words = new HashSet<>();
        Set<Integer> doubleWords = new HashSet<>();
        List<String> rv = new ArrayList<>();
        char[] map = new char[26];
        //map['A' - 'A'] = 0;
        map['C' - 'A'] = 1;
        map['G' - 'A'] = 2;
        map['T' - 'A'] = 3;

        for(int i = 0; i < s.length() - 9; i++) {
            int v = 0;
            for(int j = i; j < i + 10; j++) {
                v <<= 2;
                v |= map[s.charAt(j) - 'A'];
            }
            if(!words.add(v) && doubleWords.add(v)) {
                rv.add(s.substring(i, i + 10));
            }
        }
        return rv;
    }
}
```

#### Minimum Window Substring

>Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).
>For example,
>**S** = `"ADOBECODEBANC"`
>**T** = `"ABC"`
>Minimum window is `"BANC"`.
>**Note:**
>If there is no such window in S that covers all characters in T, return the empty string `""`.
>If there are multiple such windows, you are guaranteed that there will always be only one unique minimum window in S.

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\哈希和字符串\fdc90829-eba4-4dbb-8207-205600ae4b6f.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\哈希和字符串\bc846c66-4f40-43ac-8fac-a42ea5dc31f0.jpg)

```java
class Solution {
    public String minWindow(String s, String t) {
        int[] map_s = new int[128];
        int[] map_t = new int[128];
        List<Character> vec_t = new ArrayList<>();
        int window_begin=0;
        String result="";
        for(int i=0;i<t.length();i++){
            map_t[t.charAt(i)]++;
            vec_t.add(t.charAt(i));
        }
        for(int i=0;i<s.length();i++){
            map_s[s.charAt(i)]++;
            while(window_begin<i){
                char begin_ch=s.charAt(window_begin);
                if(map_t[begin_ch]==0){
                    window_begin++;
                }else if(map_s[begin_ch]>map_t[begin_ch]){
                    map_s[begin_ch]--;
                    window_begin++;
                }else{
                    break;
                }
            }
            if(is_window_ok(map_s,map_t,vec_t)){
                int new_window_len = i-window_begin+1;
                if(result==""||new_window_len<result.length()){
                    result = s.substring(window_begin,i+1);
                }
            }
        }
        return result;
    }
    
    private boolean is_window_ok(int[] map_s,int[] map_t,List<Character> vec_t){
        for(int i=0;i<vec_t.size();i++){
            if(map_s[vec_t.get(i)]<map_t[vec_t.get(i)]){
                return false;
            }                            
        }
        return true;
    }
}
```

```java
class Solution {
    public String minWindow(String s, String p) {
        if(s==""||s.length()==0||p==null||p.length()==0) return "";
        int min=Integer.MAX_VALUE,left=0,right=0,leftstart=-1,rightstart=-1,count=p.length();
        int[] hash = new int[256];
        for(char c:p.toCharArray()){
            hash[c]++;
        }
        while(right<s.length()){
            if(hash[s.charAt(right)]>0) count--;
            hash[s.charAt(right++)]--;
            while(count==0){
              if((right-left)<min){
                   leftstart = left;
                   rightstart = right;
                   min = right-left;
               }
                if(hash[s.charAt(left)] >=0 )
                count++;
                hash[s.charAt(left++)]++;
            }
        }
        if(min!=Integer.MAX_VALUE)
        return s.substring(leftstart,rightstart);
        return "";
    }
}
```