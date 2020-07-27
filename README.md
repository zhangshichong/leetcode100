# leetcode100
LeetCode热题100 Java实现
# leetcode 热题100

[TOC]

## [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

难度中等4480

给出两个 **非空** 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 **逆序** 的方式存储的，并且它们的每个节点只能存储 **一位** 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例：**

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode root = new ListNode(0);
        ListNode cursor = root;
        int c = 0;
        while(l1!=null || l2!=null || c!=0){
            int val1 = l1==null? 0 : l1.val;
            int val2 = l2==null? 0 : l2.val;
            int sum = val1 + val2 + c;
            c = sum / 10;
            ListNode node = new ListNode(sum%10);
            cursor.next = node;
            cursor = node;
            if (l1 != null) l1=l1.next;
            if (l2 != null) l2=l2.next;
        }
        return root.next;

    }
}
~~~

## [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

难度中等3815

给定一个字符串，请你找出其中不含有重复字符的 **最长子串** 的长度。

**示例 1:**

```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

思路：

~~~java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n=s.length(), ans=0;
        Map<Character, Integer> map = new HashMap<>();
        for(int j=0, i=0; j<n; j++)
        {
            if (map.containsKey(s.charAt(j)))
            {
                i = Math.max(map.get(s.charAt(j)), i);
            }
            ans = Math.max(ans, j-i+1);
            map.put(s.charAt(j), j+1);
        }
        return ans;        
    }
}
~~~

使用滑动窗口：

~~~java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int res = 0;
        if(s==null || s.length()<1) return res;
        if(s.length()==1) return 1;
        for(int i=0; i<s.length()-1; i++){
            Set<Character> set = new HashSet<>();
            set.add(s.charAt(i));
            for(int j=i+1 ; j<s.length(); j++){
                if(!set.contains(s.charAt(j))){
                    set.add(s.charAt(j));
                }else{
                    
                    break;
                }
            }
            res = Math.max(res, set.size());
        }
        return res;
    }
}
~~~

## [4. 寻找两个正序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

难度困难2787

给定两个大小为 m 和 n 的正序（从小到大）数组 `nums1` 和 `nums2`。

请你找出这两个正序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 `nums1` 和 `nums2` 不会同时为空。 

**示例 1:**

```
nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0
```

**示例 2:**

```
nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5
```

~~~java
class Solution {
  public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        int left = (m + n + 1) / 2;//注意
        int right = (m + n + 2) / 2;
        return (findKth(nums1, 0, nums2, 0, left) + findKth(nums1, 0, nums2, 0, right)) / 2.0;
    }
    //i: nums1的起始位置 j: nums2的起始位置
    public int findKth(int[] nums1, int i, int[] nums2, int j, int k){//寻找第K小
        if( i >= nums1.length) return nums2[j + k - 1];//nums1为空数组
        if( j >= nums2.length) return nums1[i + k - 1];//nums2为空数组
        if(k == 1){
            return Math.min(nums1[i], nums2[j]);
        }
        int midVal1 = (i + k / 2 - 1 < nums1.length) ? nums1[i + k / 2 - 1] : Integer.MAX_VALUE;
        int midVal2 = (j + k / 2 - 1 < nums2.length) ? nums2[j + k / 2 - 1] : Integer.MAX_VALUE;
        if(midVal1 < midVal2){
            return findKth(nums1, i + k / 2, nums2, j , k - k / 2);
        }else{
            return findKth(nums1, i, nums2, j + k / 2 , k - k / 2);
        }        
    }
}
~~~

## [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

难度中等2333

给定一个字符串 `s`，找到 `s` 中最长的回文子串。你可以假设 `s` 的最大长度为 1000。

**示例 1：**

```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```

**示例 2：**

```
输入: "cbbd"
输出: "bb"
```

![image-20200724095427793](C:\Users\zhangshichong\AppData\Roaming\Typora\typora-user-images\image-20200724095427793.png)

~~~java
class Solution {
    public String longestPalindrome(String s) {
        if(s==null || s.length()==0)
            return "";
        int len = s.length();
        int[][] dp = new int[len][len];
        int start = 0;
        int longest = 1;
        for (int i=0; i<len; i++){
            dp[i][i] = 1;
            if(i<len-1){
                if (s.charAt(i)==s.charAt(i+1)){
                    dp[i][i+1] = 1;
                    start = i;
                    longest = 2;
                }
            }
        }
        for(int l=3; l<=len; l++){
            for (int i=0; i+l-1<len; i++){
                int j=i+l-1;
                if(s.charAt(i)==s.charAt(j) && dp[i+1][j-1]==1){
                    dp[i][j]  = 1;
                    start = i;
                    longest = l;
                }
            }
        }
        return s.substring(start, start+longest);
    }
}
~~~

~~~C++
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        vector<vector<int>> dp(n, vector<int>(n));
        string ans;
        for (int l = 0; l < n; ++l) {
            for (int i = 0; i + l < n; ++i) {
                int j = i + l;
                if (l == 0) {
                    dp[i][j] = 1;
                }
                else if (l == 1) {
                    dp[i][j] = (s[i] == s[j]);
                }
                else {
                    dp[i][j] = (s[i] == s[j] && dp[i + 1][j - 1]);
                }
                if (dp[i][j] && l + 1 > ans.size()) {
                    ans = s.substr(i, l + 1);
                }
            }
        }
        return ans;
    }
};
~~~

## [10. 正则表达式匹配](https://leetcode-cn.com/problems/regular-expression-matching/)

难度困难1316

给你一个字符串 `s` 和一个字符规律 `p`，请你来实现一个支持 `'.'` 和 `'*'` 的正则表达式匹配。

```
'.' 匹配任意单个字符
'*' 匹配零个或多个前面的那一个元素
```

所谓匹配，是要涵盖 **整个** 字符串 `s`的，而不是部分字符串。

**说明:**

- `s` 可能为空，且只包含从 `a-z` 的小写字母。
- `p` 可能为空，且只包含从 `a-z` 的小写字母，以及字符 `.` 和 `*`。

**示例 1:**

```
输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
```

**示例 2:**

```
输入:
s = "aa"
p = "a*"
输出: true
解释: 因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
```

**示例 3:**

```
输入:
s = "ab"
p = ".*"
输出: true
解释: ".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。
```

**示例 4:**

```
输入:
s = "aab"
p = "c*a*b"
输出: true
解释: 因为 '*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。
```

**示例 5:**

```
输入:
s = "mississippi"
p = "mis*is*p*."
输出: false
```

~~~java
class Solution {
    public boolean isMatch(String s, String p) {
        int slen = s.length();
        int plen = p.length();
        boolean[][] dp = new boolean[slen+1][plen+1];
        dp[0][0] = true;
        if(plen != 0) dp[0][1] = false;
        for(int i=0; i<= slen; i++){
            for(int j=1; j<=plen; j++){
                if(p.charAt(j-1)=='*'){
                    dp[i][j] = dp[i][j-2];
                    if(macthes(s, p, i, j-1)){
                        dp[i][j] = dp[i][j] || dp[i-1][j];
                    }
                }else{
                    if (macthes(s, p, i, j)){
                        dp[i][j] = dp[i-1][j-1];
                    }
                }
            }
        }
        return dp[slen][plen];
    }

    private boolean macthes(String s, String p, int i, int j){
        if(i==0) return false;
        if(p.charAt(j-1)=='.') return true;
        return s.charAt(i-1)==p.charAt(j-1);
    }
}
~~~

## [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

难度中等1548

给你 *n* 个非负整数 *a*1，*a*2，...，*a*n，每个数代表坐标中的一个点 (*i*, *ai*) 。在坐标内画 *n* 条垂直线，垂直线 *i* 的两个端点分别为 (*i*, *ai*) 和 (*i*, 0)。找出其中的两条线，使得它们与 *x* 轴共同构成的容器可以容纳最多的水。

**说明：**你不能倾斜容器，且 *n* 的值至少为 2。

 

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

 

**示例：**

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49
```

~~~java
class Solution {
    public int maxArea(int[] height) {
        if(height.length <= 1) return 0;
        int i=0, j=height.length-1, re=0;
        while(i<j){
            int h = Math.min(height[i], height[j]);
            re = Math.max(re, h*(j-i));
            if(height[i]<height[j]) i++;
            else j--;
        }
        return re;
    }
}
~~~

## [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

难度中等2300

给你一个包含 *n* 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 *a，b，c ，*使得 *a + b + c =* 0 ？请你找出所有满足条件且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。 

**示例：**

```
给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

~~~java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        int len = nums.length;
        if(len<3 || nums==null) return ans;
        Arrays.sort(nums);
        for(int i=0; i<len; i++){
            if(nums[i]>0) break;
            if(i>0  && nums[i]==nums[i-1]) continue;//去除i重复
            int l=i+1;
            int r=len-1;
            while(l<r){
                int sum = nums[i] + nums[l] + nums[r];
                if(sum==0){
                    ans.add(Arrays.asList(nums[i], nums[l], nums[r]));
                    while(l<r && nums[l]==nums[l+1]) l++;//去除l重复
                    while(l<r && nums[r]==nums[r-1]) r--;
                    l++;
                    r--; 
                }else if(sum<0) l++;//左移
                else if(sum>0) r--;//右移
            }
        }
        return ans; 
    }
}
~~~

## [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

难度中等760

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/original_images/17_telephone_keypad.png)

**示例:**

```
输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

**说明:**
尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。

~~~java
class Solution {
    Map<String, String> map = new HashMap<>(){{
        put("2", "abc");
        put("3", "def");
        put("4", "ghi");
        put("5", "jkl");
        put("6", "mno");
        put("7", "pqrs");
        put("8", "tuv");
        put("9", "wxyz");
    }};
    List<String> ans = new ArrayList<>();
    public void backtrack(String combination, String next_digits){
        if(next_digits.length()==0){
            ans.add(combination);
        }else{
            String digit = next_digits.substring(0, 1);
            String letters = map.get(digit);
            for (int i = 0; i < letters.length(); i++) {
                String letter = letters.substring(i, i+1);
                backtrack(combination+letter, next_digits.substring(1));
            }
        }
    }
    public List<String> letterCombinations(String digits) {
        if(digits.length() != 0){
            backtrack("", digits);
        }
        return ans;
    }
}
~~~

## [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

难度中等861

给定一个链表，删除链表的倒数第 *n* 个节点，并且返回链表的头结点。

**示例：**

```
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```

**说明：**

给定的 *n* 保证是有效的。

**进阶：**

你能尝试使用一趟扫描实现吗？

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode first = head;
        int len = 0;
        while(first!=null){
            len++;
            first=first.next;
        }
        len-=n;
        first = dummy;
        while(len>0){
            len--;
            first = first.next;
        }
        first.next = first.next.next;
        return dummy.next;
    }
}
~~~

一趟扫描：

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode fast = head;
        ListNode slow = dummy;
        for(int i=0; i<n; i++){
            fast = fast.next;
        }
        while(fast!=null){
            slow = slow.next;
            fast = fast.next;
        }
        slow.next = slow.next.next;
        return dummy.next;
    }
}
~~~

## [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

难度简单1627

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。

注意空字符串可被认为是有效字符串。

**示例 1:**

```
输入: "()"
输出: true
```

**示例 2:**

```
输入: "()[]{}"
输出: true
```

**示例 3:**

```
输入: "(]"
输出: false
```

**示例 4:**

```
输入: "([)]"
输出: false
```

**示例 5:**

```
输入: "{[]}"
输出: true
```

~~~java
class Solution {
    public boolean isValid(String s) {
        if(s.length()==0) return true;
        Stack<Character> stack = new Stack<>();
        for(int i=0; i<s.length(); i++){
            if(stack.isEmpty()){
                stack.push(s.charAt(i));
            }else{
                if(macth(stack.peek(), s.charAt(i))) stack.pop();
                else stack.push(s.charAt(i));
            }
        }
        return stack.isEmpty();
    }

    private boolean macth(char left, char right){
        if ((left=='(' && right==')') ) return true;
        if ((left=='[' && right==']') ) return true;
        if ((left=='{' && right=='}') ) return true;
        return  false;
    }
}
~~~

## [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

难度简单1115

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。  

**示例：**

```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1==null) return l2;
        if(l2==null) return l1;
        if(l1.val < l2.val){
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        }else{
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
}
~~~

## [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

难度中等1109

数字 *n* 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

 

**示例：**

```
输入：n = 3
输出：[
       "((()))",
       "(()())",
       "(())()",
       "()(())",
       "()()()"
     ]
```

~~~java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList<>();
        backtrack(ans, "", 0, 0, n);
        return ans;
    }

    public void backtrack(List<String> ans, String cur, int open, int close, int max){
       if(open==max && close==max){
           ans.add(cur);
           return;
       }
       if(open<close) return;
       if(open<max) backtrack(ans, cur+"(", open+1, close, max);
       if(close<max) backtrack(ans, cur+")", open, close+1, max);
    }
}
~~~

## [23. 合并K个排序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

难度困难732

合并 *k* 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

**示例:**

```
输入:
[
  1->4->5,
  1->3->4,
  2->6
]
输出: 1->1->2->3->4->4->5->6
```

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode sum = new ListNode(Integer.MIN_VALUE);
        for(int i=0; i<lists.length; i++){
            sum = mergeTwo(sum, lists[i]);
        }
        return sum.next;
    }

    public ListNode mergeTwo(ListNode root1, ListNode root2){
        if(root1==null) return root2;
        if(root2==null) return root1;
        if(root1.val<root2.val){
            root1.next = mergeTwo(root1.next, root2);
            return root1;
        }else{
            root2.next = mergeTwo(root1, root2.next);
            return root2;
        }
    }
}
~~~

6.21 update

~~~java 
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode dummy = new ListNode(Integer.MIN_VALUE);        
        for(ListNode node : lists){
            dummy = mergeTwo(dummy, node);
        }
        return dummy.next;
    }

    private ListNode mergeTwo(ListNode l1, ListNode l2){
        if(l1==null) return l2;
        if(l2==null) return l1;
        if(l1.val  < l2.val){
            l1.next = mergeTwo(l1.next, l2);
            return l1;
        }else{
            l2.next = mergeTwo(l1, l2.next);
            return l2;
        }
    } 
}
~~~

## [31. 下一个排列](https://leetcode-cn.com/problems/next-permutation/)

难度中等530

实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须**[原地](https://baike.baidu.com/item/原地算法)**修改，只允许使用额外常数空间。

以下是一些例子，输入位于左侧列，其相应输出位于右侧列。
`1,2,3` → `1,3,2`
`3,2,1` → `1,2,3`
`1,1,5` → `1,5,1`

~~~java
import java.util.Arrays;

//leetcode submit region begin(Prohibit modification and deletion)
class Solution {
    public void nextPermutation(int[] nums) {
        for (int i = nums.length-1; i >=0; i--) {
            for (int j = nums.length-1; j > i ; j--) {
                if(nums[i]<nums[j]){
                    swap(nums, i, j);
                    Arrays.sort(nums, i+1, nums.length);
                    return;
                }
            }
        }
        Arrays.sort(nums);
    }
    public void swap(int[] a, int i, int j){
        int t = a[i];
        a[i] = a[j];
        a[j] = t;
    }
}
//leetcode submit region end(Prohibit modification and deletion)
~~~

## [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)

难度困难699

给定一个只包含 `'('` 和 `')'` 的字符串，找出最长的包含有效括号的子串的长度。

**示例 1:**

```
输入: "(()"
输出: 2
解释: 最长有效括号子串为 "()"
```

**示例 2:**

```
输入: ")()())"
输出: 4
解释: 最长有效括号子串为 "()()"
```

~~~java
import java.util.Stack;

class Solution {
    public int longestValidParentheses(String s) {
        if(s.length()==0 || s==null) return 0;
        Stack<Integer> stack = new Stack<>();
        stack.add(-1);
        int max = 0;
        for (int i = 0; i < s.length(); i++) {
            if(s.charAt(i)=='('){
                stack.add(i);
            }
            else {
                stack.pop();
                if(stack.isEmpty()){
                    stack.push(i);
                }else {
                    max = max > (i-stack.peek()) ? max : i - stack.peek();
                }
            }
        }
        return max;
    }

}
~~~

x 动态规划版：

~~~java
class Solution {
    public int longestValidParentheses(String s) {
        int maxans=0;
        int[] dp = new int[s.length()];
        for(int i=1; i<s.length(); i++){
            if(s.charAt(i)==')'){
                if(s.charAt(i-1)=='('){
                    dp[i] = (i>=2 ? dp[i-2] : 0) + 2;
                }else if((i-dp[i-1])>0 && s.charAt(i-dp[i-1]-1)=='('){
                    //dp[i-1]是指前一个符号已经匹配的数量。
                    dp[i] = dp[i-1] + (i-dp[i-1]>=2 ? dp[i- dp[i-1] - 2] : 0) + 2;
                }
            }
            maxans = Math.max(maxans, dp[i]);
        }
        return maxans;
    }
}
~~~

## [33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

难度中等788

假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 `[0,1,2,4,5,6,7]` 可能变为 `[4,5,6,7,0,1,2]` )。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 `-1` 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 *O*(log *n*) 级别。

**示例 1:**

```
输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4
```

**示例 2:**

```
输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1
```

~~~java
class Solution {
    public int search(int[] nums, int target) {
        int lo=0, hi=nums.length-1, mid=0;
        while(lo <= hi){
            mid = lo + (hi-lo)/2;
            if(nums[mid]==target) return mid;
            if(nums[lo]<= nums[mid]){
                if(nums[lo]<=target && target<=nums[mid]){
                    hi = mid-1;
                }else {
                    lo = mid+1;
                }
            }else{
                if(nums[mid]<=target && target <= nums[hi]){
                    lo = mid+1;
                }else{
                    hi = mid-1;
                }
            }
        }
        return -1;
    }
}
~~~

## [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

难度中等466

给定一个按照升序排列的整数数组 `nums`，和一个目标值 `target`。找出给定目标值在数组中的开始位置和结束位置。

你的算法时间复杂度必须是 *O*(log *n*) 级别。

如果数组中不存在目标值，返回 `[-1, -1]`。

**示例 1:**

```
输入: nums = [5,7,7,8,8,10], target = 8
输出: [3,4]
```

**示例 2:**

```
输入: nums = [5,7,7,8,8,10], target = 6
输出: [-1,-1]
```

~~~java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int lo=0, hi=nums.length-1;
        while(lo <= hi){
            int mid = lo + (hi-lo)/2;
            if(nums[mid]>target) hi=mid-1;
            if(nums[mid]<target) lo=mid+1;
            if (nums[mid] == target){
                lo = mid;
                hi = mid;
                while(hi+1<nums.length && nums[hi+1]==target) hi++;
                while (lo-1>=0 && nums[lo-1]==target) lo--;
                return new int[]{lo, hi};
            }
        }
        return new int[]{-1, -1};//记住返回值的形式，怎么不创建对象返回答案。
    }
}
~~~

## [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

难度中等726

给定一个**无重复元素**的数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的数字可以无限制重复被选取。

**说明：**

- 所有数字（包括 `target`）都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: candidates = [2,3,6,7], target = 7,
所求解集为:
[
  [7],
  [2,2,3]
]
```

**示例 2:**

```
输入: candidates = [2,3,5], target = 8,
所求解集为:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

~~~java
import java.util.ArrayList;
import java.util.List;

class Solution {
    List<List<Integer>> lists = new ArrayList<>();
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        if(candidates==null || candidates.length==0 || target<0) return lists;
        List<Integer> list = new ArrayList<>();
        backtrack(0, candidates, target, list);
        return lists;
    }

    public void backtrack(int start, int[] candidates, int target, List<Integer> list){
        if(target< 0) return;
        if(target==0){
            lists.add(new ArrayList<>(list));
        }else {
            for (int i = start; i < candidates.length; i++) {
                list.add(candidates[i]);
                backtrack(i, candidates, target-candidates[i], list);
                list.remove(list.size()-1);
            }
        }
    }
}
~~~

## 42.接雨水

给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

![1590284813511](C:\Users\zhangshichong\AppData\Roaming\Typora\typora-user-images\1590284813511.png)

上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 感谢 Marcos 贡献此图。

示例:

输入: [0,1,0,2,1,0,1,3,2,1,2,1]
输出: 6

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/trapping-rain-water
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

~~~Java
class Solution {
    public int trap(int[] height) {
        if(height==null || height.length==0) return 0;
        int size = height.length;
        int[] leftmax = new int[size];
        int[] rightmax = new int[size];
        int ans = 0;
        leftmax[0] = height[0];
        for (int i = 1; i < size; i++) {
            leftmax[i] = Math.max(height[i], leftmax[i-1]);
        }
        rightmax[size-1] = height[size-1];
        for (int i = size-2; i >= 0 ; i--) {
            rightmax[i] = Math.max(rightmax[i+1], height[i]);
        }
        for (int i = 1; i < size-1; i++) {
            ans += Math.min(leftmax[i], rightmax[i])-height[i];
        }
        return ans;
    }
}
~~~

## 46.全排列

给定一个 没有重复 数字的序列，返回其所有可能的全排列。

示例:

输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/permutations
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

> 这是一道经典的回溯法题目，请记住代码流程

~~~Java
class Solution {
    List<List<Integer>> ans = new ArrayList<>();
    public List<List<Integer>> permute(int[] nums) {
        if(nums==null || nums.length<1) return ans;
        int[] visited = new int[nums.length];
        List<Integer> list = new ArrayList<>();
        backtrack(nums, visited, list);
        return ans;
    }

    private void backtrack(int[] nums, int[] visited, List<Integer> list){
        if(list.size()==nums.length){
            ans.add(new ArrayList<>(list));
            return;
        }
        for(int i=0; i<nums.length; i++){
            if(visited[i]==1) continue;
            visited[i] = 1;
            list.add(nums[i]);
            backtrack(nums, visited, list);
            visited[i] = 0;
            list.remove(list.size()-1);
        }
    }
}
~~~

## 48.旋转图像

给定一个 n × n 的二维矩阵表示一个图像。

将图像顺时针旋转 90 度。

说明：

你必须在原地旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要使用另一个矩阵来旋转图像。

示例 1:

给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
示例 2:

给定 matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

原地旋转输入矩阵，使其变为:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/rotate-image
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

> 先转置，再交换列

~~~Java
import java.util.Arrays;

class Solution {
    public static void main(String[] args) {
        int[][] a = new int[][]{{1,2 ,3}, {4, 5, 6}, {7, 8, 9}};
        rotate(a);
        System.out.println(Arrays.deepToString(a));

    }
    public static void rotate(int[][] matrix) {
        int n = matrix.length;
        for (int i=0; i<n; i++){//转置
            for (int j = i; j< n; j++){
                int tmp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = tmp;
            }
        }

        for (int i=0; i<n; i++){//交换对称列
            for (int j=0; j<n/2; j++){
                int tmp = matrix[i][j];
                matrix[i][j] = matrix[i][n-j-1];
                matrix[i][n-j-1] = tmp;
            }
        }
    }
}
~~~



## [49. 字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)

难度中等346

给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

**示例:**

```
输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

**说明：**

- 所有输入均为小写字母。
- 不考虑答案输出的顺序。

> 使用map，将排序好字符的str作为key, 值为 list, 加入。

~~~Java
import java.util.*;
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if(strs.length == 0) {
            return new ArrayList<>();
        }
            
        Map<String, List> map = new HashMap<>();
        for (String s: strs){
            char[] ca = s.toCharArray();
            Arrays.sort(ca);
            String key = String.valueOf(ca);
            if(!map.containsKey(key)){
                map.put(key, new ArrayList());
            }
            map.get(key).add(s);
        }
        return new ArrayList(map.values());
    }
}
~~~

## [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

难度简单2092

给定一个整数数组 `nums` ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**示例:**

```
输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

**进阶:**

如果你已经实现复杂度为 O(*n*) 的解法，尝试使用更为精妙的分治法求解。

~~~java
class Solution {
    public int maxSubArray(int[] nums) {
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        int max = dp[0];
        for(int i=1; i<nums.length; i++){
            dp[i] = Math.max(dp[i-1]+nums[i], nums[i]);
            max = Math.max(max, dp[i]);
        }
        return max;
    }
}
~~~

## [55. 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

难度中等671

给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个位置。

**示例 1:**

```
输入: [2,3,1,1,4]
输出: true
解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。
```

**示例 2:**

```
输入: [3,2,1,0,4]
输出: false
解释: 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。
```

~~~Java
class Solution {
    public boolean canJump(int[] nums) {
        int k=0;
        for (int i = 0; i < nums.length; i++) {
            if(i>k) return false;//判断能否达到当前位置
            k = Math.max(k, i+nums[i]);//能够跳的最远位置
        }
        return true;
    }
}
~~~

## [56. 合并区间](https://leetcode-cn.com/problems/merge-intervals/)

难度中等432

给出一个区间的集合，请合并所有重叠的区间。

**示例 1:**

```
输入: [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```

**示例 2:**

```
输入: [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。
```

~~~Java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public int[][] merge(int[][] intervals) {
        int len = intervals.length;
        if(len <=1|| intervals==null) return intervals;
        Arrays.sort(intervals, (v1, v2)->v1[0]-v2[0]);//注意排序方式实现，升序
        List<int[]> res = new ArrayList<>();
        res.add(intervals[0]);
        for (int i = 1; i < intervals.length; i++) {
            int[] cur = intervals[i];
            int[] last = res.get(res.size()-1);
            if(cur[0]>last[1]) {
                res.add(cur);
            }else {
                last[1] = Math.max(cur[1], last[1]);
            }
        }
        return res.toArray(new int[res.size()][]);//注意写法
    }
}
~~~

## [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/)

难度中等568

一个机器人位于一个 *m x n* 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

问总共有多少条不同的路径？

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/robot_maze.png)

例如，上图是一个7 x 3 的网格。有多少可能的路径？

 

**示例 1:**

```
输入: m = 3, n = 2
输出: 3
解释:
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向右 -> 向下
2. 向右 -> 向下 -> 向右
3. 向下 -> 向右 -> 向右
```

**示例 2:**

```
输入: m = 7, n = 3
输出: 28
```

 

**提示：**

- `1 <= m, n <= 100`
- 题目数据保证答案小于等于 `2 * 10 ^ 9`

~~~java
class Solution {
    public int uniquePaths(int m, int n) {
        if(m==0 || n==0){
            return 0;
        }
        int[][] dp = new int[m][n];
        for(int i=0; i<m; i++){
            dp[i][0] = 1;
        }
        for(int i=0; i<n; i++){
            dp[0][i] = 1;
        }

        for(int i=1; i<m; i++){
            for (int j=1; j<n; j++){
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }

        return dp[m-1][n-1];
    }
}
~~~

一维dp

~~~java
class Solution {
    public int uniquePaths(int m, int n) {
        int[] dp = new int[n];
        for(int i=0; i<n; i++){
            dp[i] = 1;
        }
        for(int i=1; i<m; i++){
            dp[0] = 1;
            for(int j=1; j<n; j++){
                dp[j] = dp[j-1]+dp[j];
            }
        }
        return dp[n-1];
    }
}
~~~

## [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)

难度中等491

给定一个包含非负整数的 *m* x *n* 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。

**示例:**

```
输入:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 7
解释: 因为路径 1→3→1→1→1 的总和最小。
```

~~~java
class Solution {
    public int minPathSum(int[][] grid) {
        int m=grid.length;
        int n=grid[0].length;
        if(m<=0 || n<=0) return 0;
        int[][] dp = new int[m][n];
        dp[0][0] = grid[0][0];
        for(int i=1; i<n; i++){
            dp[0][i] = dp[0][i-1]+grid[0][i];
        }
        for(int i=1; i<m; i++){
            dp[i][0] = dp[i-1][0] + grid[i][0];
        }
        for(int i=1; i<m; i++){
            for (int j=1; j<n; j++){
                dp[i][j] = Math.min(dp[i-1][j], dp[i][j-1])+grid[i][j];
            }
        }
        return dp[m-1][n-1];
    }
}
~~~

## [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

难度简单1086

假设你正在爬楼梯。需要 *n* 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**注意：**给定 *n* 是一个正整数。

**示例 1：**

```
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
```

**示例 2：**

```
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

~~~java
class Solution {
    public int climbStairs(int n) {
        if(n<=1) return n;
        int[] dp = new int[n+1];
        dp[0] = 0;
        dp[1] = 1;
        dp[2] = 2;
        for(int i=3; i<=n; i++){
            dp[i] = dp[i-1]+dp[i-2];
        }
        return dp[n];
    }
}
~~~

## [72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)

难度困难882

给你两个单词 *word1* 和 *word2*，请你计算出将 *word1* 转换成 *word2* 所使用的最少操作数 。

你可以对一个单词进行如下三种操作：

1. 插入一个字符
2. 删除一个字符
3. 替换一个字符

**示例 1：**

```
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
```

**示例 2：**

```
输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')
```

~~~Java
class Solution {
    public int minDistance(String word1, String word2) {
        int len1 = word1.length();
        int len2 = word2.length();
        int[][] dp = new int[len1+1][len2+1];
        for (int i = 1; i <= len1; i++) {
            dp[i][0] = dp[i-1][0]+1;
        }
        for (int i = 1; i <= len2; i++) {
            dp[0][i] = dp[0][i-1]+1;
        }
        for (int i = 1; i <= len1; i++) {
            for (int j = 1; j <= len2; j++) {
                if(word1.charAt(i-1)==word2.charAt(j-1))
                    dp[i][j] = dp[i-1][j-1];
                else {
                    dp[i][j] = Math.min(dp[i-1][j-1], Math.min(dp[i][j-1], dp[i-1][j]))+1;
                }
            }
        }
        return dp[len1][len2];
    }
}
~~~

## [75. 颜色分类](https://leetcode-cn.com/problems/sort-colors/)

难度中等433

给定一个包含红色、白色和蓝色，一共 *n* 个元素的数组，**[原地](https://baike.baidu.com/item/原地算法)**对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

**注意:**
不能使用代码库中的排序函数来解决这道题。

**示例:**

```
输入: [2,0,2,1,1,0]
输出: [0,0,1,1,2,2]
```

**进阶：**

- 一个直观的解决方案是使用计数排序的两趟扫描算法。
  首先，迭代计算出0、1 和 2 元素的个数，然后按照0、1、2的排序，重写当前数组。
- 你能想出一个仅使用常数空间的一趟扫描算法吗？

> 使用三路排序，当前值是0，与left交换，是1则++，是2则与right交换。

~~~Java
class Solution {
    public void sortColors(int[] nums) {
        if(nums.length < 2) return;
        int left = -1;
        int cur = 0;
        int right = nums.length-1;
        while (cur<=right){
            if(nums[cur]==0){
                left++;
                swap(nums, left, cur);
                cur++;
            }else if (nums[cur]==1){
                cur++;
            }else {
                swap(nums, right, cur);
                right--;
            }
        }
    }

    public void swap(int[] nums, int i1, int i2){
        int tmp = nums[i1];
        nums[i1] = nums[i2];
        nums[i2] = tmp;
    }
}
~~~

## [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

难度困难565

给你一个字符串 S、一个字符串 T，请在字符串 S 里面找出：包含 T 所有字符的最小子串。

**示例：**

```
输入: S = "ADOBECODEBANC", T = "ABC"
输出: "BANC"
```

**说明：**

- 如果 S 中不存这样的子串，则返回空字符串 `""`。
- 如果 S 中存在这样的子串，我们保证它是唯一的答案。

> 使用滑动窗口来解决

~~~Java
class Solution {
    public String minWindow(String s, String t) {
        if(s.length()==0 || s==null || t.length()==0 || t==null) return "";
        int[] map = new int[128];
        for(char c : t.toCharArray()){
            map[c]++;
        }
        int start=0, end=0;
        int left=0, right=0;
        int matches = 0;
        int minLen = s.length()+1;
        while (right < s.length()){
            char rightc = s.charAt(right);
            map[rightc]--;
            if(map[rightc]>=0) matches++;
            right++;//不要忘记！
            while(matches==t.length()){
                int size = right-left;
                if(size < minLen){
                    minLen = size;
                    start = left;
                    end = right;
                }
                char leftc = s.charAt(left);
                map[leftc]++;//不在t中出现的字符要移出窗口
                //最终达到的最大值map[leftc]==0
                if(map[leftc]>0) matches--;
                left++;//不要忘记！
            }
        }
        return s.substring(start, end);
    }
}
~~~

## [78. 子集](https://leetcode-cn.com/problems/subsets/)

难度中等582

给定一组**不含重复元素**的整数数组 *nums*，返回该数组所有可能的子集（幂集）。

**说明：**解集不能包含重复的子集。

**示例:**

```
输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

> 使用回溯法

~~~java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> subsets(int[] nums) {
        if(nums==null || nums.length==0) return res;
        List<Integer> list = new ArrayList<>();
        process(list, nums, 0);
        return res;
    }
    private void process(List<Integer> list, int[] nums, int start){
        res.add(new ArrayList(list));
        for (int i=start; i<nums.length; i++){
            list.add(nums[i]);
            process(list, nums, i+1);
            list.remove(list.size()-1);
        }
    }
}
~~~

## [79. 单词搜索](https://leetcode-cn.com/problems/word-search/)

难度中等423

给定一个二维网格和一个单词，找出该单词是否存在于网格中。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

 

**示例:**

```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

给定 word = "ABCCED", 返回 true
给定 word = "SEE", 返回 true
给定 word = "ABCB", 返回 false
```

 

**提示：**

- `board` 和 `word` 中只包含大写和小写英文字母。
- `1 <= board.length <= 200`
- `1 <= board[i].length <= 200`
- `1 <= word.length <= 10^3`

> 回溯法。

~~~java
class Solution {
    boolean hasFind=false;
    char[] words;
    boolean[][] visited;
    int row, col;
    int[][] directions = new int[][]{{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
    public boolean exist(char[][] board, String word) {
        words = word.toCharArray();
        row = board.length;
        col = board[0].length;
        visited = new boolean[row][col];
        if(row*col<word.length()) return false;
        for(int i=0; i<row; i++){
            for (int j=0; j<col; j++){
                if (board[i][j]==words[0]){
                    backtrack(board, words, 1, i, j);
                    if(hasFind) return true;
                }
            }
        }
        return false;
    }

    public void backtrack(char[][] board, char[] words, int curindex, int i, int j){
        if (hasFind) return;
        if (curindex == words.length) {
            hasFind = true;
            return;
        }
        visited[i][j] = true;
        for(int[] direction : directions){
            int x = i+direction[0];
            int y = j+direction[1];
            if(validIndex(x, y) && !visited[x][y] && board[x][y]==words[curindex]){
                backtrack(board, words, curindex+1, x, y);
            }
        }
        visited[i][j] = false;        
    }

    public boolean validIndex(int x, int y){
        return x>=0 && y>=0 && x<row && y<col;
    }
}
~~~

## [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

难度困难715

给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

 

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/histogram.png)

以上是柱状图的示例，其中每个柱子的宽度为 1，给定的高度为 `[2,1,5,6,2,3]`。

 

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/histogram_area.png)

图中阴影部分为所能勾勒出的最大矩形面积，其面积为 `10` 个单位。

 

**示例:**

```
输入: [2,1,5,6,2,3]
输出: 10
```

### 暴力

~~~Java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int area=0, n = heights.length;
        for(int i=0; i<n; i++){
            int w = 1, h = heights[i], j = i;
            while(--j >= 0 && heights[j]>=h){
                w++;
            }
            j = i;
            while(++j < n && heights[j]>=h){
                w++;
            }
            area = Math.max(area, w*h);
        }
        return area;
    }
}
~~~

###  单调栈

关键在于找到左右边界

单调栈
单调栈分为单调递增栈和单调递减栈

11. 单调递增栈即栈内元素保持单调递增的栈
12. 同理单调递减栈即栈内元素保持单调递减的栈

操作规则（下面都以单调递增栈为例）
21. 如果新的元素比栈顶元素大，就入栈
22. 如果新的元素较小，那就一直把栈内元素弹出来，直到栈顶比新元素小

加入这样一个规则之后，会有什么效果
31. 栈内的元素是递增的
32. 当元素出栈时，说明这个新元素是出栈元素向后找第一个比其小的元素

举个例子，配合下图，现在索引在 6 ，栈里是 1 5 6 。
接下来新元素是 2 ，那么 6 需要出栈。
当 6 出栈时，右边 2 代表是 6 右边第一个比 6 小的元素。

当元素出栈后，说明新栈顶元素是出栈元素向前找第一个比其小的元素
当 6 出栈时，5 成为新的栈顶，那么 5 就是 6 左边第一个比 6 小的元素。

代码模板

C++
stack<int> st;
for(int i = 0; i < nums.size(); i++)
{
	while(!st.empty() && st.top() > nums[i])
	{
		st.pop();
	}
	st.push(nums[i]);
}

作者：ikaruga
链接：https://leetcode-cn.com/problems/largest-rectangle-in-histogram/solution/84-by-ikaruga/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

~~~Java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int[] tmp = new int[heights.length+2];
        System.arraycopy(heights, 0, tmp, 1, heights.length);
        Deque<Integer> stack = new ArrayDeque<>();
        int area=0;
        for (int i=0; i<tmp.length; i++){
            while (!stack.isEmpty() && tmp[i]<tmp[stack.peek()]){
                int h = tmp[stack.pop()];
                area = Math.max(area, (i-stack.peek()-1)*h);
            }
            stack.push(i);
        }
        return area;
    }
}
~~~

## [85. 最大矩形](https://leetcode-cn.com/problems/maximal-rectangle/)

难度困难469

给定一个仅包含 0 和 1 的二维二进制矩阵，找出只包含 1 的最大矩形，并返回其面积。

**示例:**

```
输入:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
输出: 6
```

每一层看作是柱状图，可以套用84题柱状图的最大面积。

第一层柱状图的高度["1","0","1","0","0"]，最大面积为1；

第二层柱状图的高度["2","0","2","1","1"]，最大面积为3；

第三层柱状图的高度["3","1","3","2","2"]，最大面积为6；

第四层柱状图的高度["4","0","0","3","0"]，最大面积为4；

~~~java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        if (matrix.length == 0) return 0;
        int maxarea = 0;
        int[] dp = new int[matrix[0].length];
        for(int i=0; i<matrix.length; i++){
            for(int j=0; j<matrix[0].length; j++){
                dp[j] = matrix[i][j]=='1' ? dp[j]+1 : 0;//*********!!!
            }
            maxarea = Math.max(maxarea, largestRectangleArea(dp));
        }
        return maxarea;
    }

    public int largestRectangleArea(int[] heights) {
        int[] tmp = new int[heights.length+2];
        System.arraycopy(heights, 0, tmp, 1, heights.length);
        Deque<Integer> stack = new ArrayDeque<>();
        int area=0;
        for(int i=0; i<tmp.length; i++){
            while(!stack.isEmpty() && tmp[i]<tmp[stack.peek()]){
                int h = tmp[stack.pop()];
                area = Math.max(area, (i-stack.peek()-1)*h);
            }
            stack.push(i);
        }
        return area;
    }
}
~~~

## [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

难度中等514

给定一个二叉树，返回它的*中序* 遍历。

**示例:**

```
输入: [1,null,2,3]
   1
    \
     2
    /
   3

输出: [1,3,2]
```

**进阶:** 递归算法很简单，你可以通过迭代算法完成吗？

### 递归版本

~~~Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        helper(root, list);
        return list;
    }

    private void helper(TreeNode root, List<Integer> list){
        if(root!=null){
            if(root.left != null) helper(root.left, list);
            list.add(root.val);
            if(root.right != null) helper(root.right, list);
        }
    }
}
~~~

### 迭代版本

~~~Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        while(root!=null || !stack.isEmpty()){
            if(root != null){
                stack.push(root);
                root = root.left;
            }else{
                root = stack.pop();
                list.add(root.val);
                root = root.right;
            }
        }
        return list;
    }
}
~~~

## [96. 不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/)

难度中等527

给定一个整数 *n*，求以 1 ... *n* 为节点组成的二叉搜索树有多少种？

**示例:**

```
输入: 3
输出: 5
解释:
给定 n = 3, 一共有 5 种不同结构的二叉搜索树:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

思路
标签：动态规划
假设n个节点存在二叉排序树的个数是G(n)，令f(i)为以i为根的二叉搜索树的个数，则
G(n) = f(1) + f(2) + f(3) + f(4) + ... + f(n)        

G(n)=f(1)+f(2)+f(3)+f(4)+...+f(n)

当i为根节点时，其左子树节点个数为i-1个，右子树节点为n-i，则
f(i) = G(i-1)*G(n-i)

f(i)=G(i−1)∗G(n−i)

综合两个公式可以得到 卡特兰数 公式
G(n) = G(0)*G(n-1)+G(1)*(n-2)+...+G(n-1)*G(0)

G(n)=G(0)∗G(n−1)+G(1)∗(n−2)+...+G(n−1)∗G(0)

作者：guanpengchn
链接：https://leetcode-cn.com/problems/unique-binary-search-trees/solution/hua-jie-suan-fa-96-bu-tong-de-er-cha-sou-suo-shu-b/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

~~~Java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n+1];
        dp[0] = 1;
        dp[1] = 1;
        for (int i=2; i<n+1; i++){
            for (int j=1; j<i+1; j++){
                dp[i] += dp[j-1]*dp[i-j];
            }
        }
        return dp[n];
    }
}
~~~

## [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

难度中等596

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

- 节点的左子树只包含**小于**当前节点的数。
- 节点的右子树只包含**大于**当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

**示例 1:**

```
输入:
    2
   / \
  1   3
输出: true
```

**示例 2:**

```
输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
     根节点的值为 5 ，但是其右子节点值为 4 。
```

~~~Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    long pre = Long.MIN_VALUE;//按照系统输入 应该为long
    public boolean isValidBST(TreeNode root) {
        if(root==null) return true;
        if(!isValidBST(root.left)) return false;
        if(root.val <= pre) return false;
        pre = root.val;
        return isValidBST(root.right);
    }
}
~~~

## [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

难度简单825

给定一个二叉树，检查它是否是镜像对称的。

 

例如，二叉树 `[1,2,2,3,4,4,3]` 是对称的。

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

 

但是下面这个 `[1,2,2,null,3,null,3]` 则不是镜像对称的:

```
    1
   / \
  2   2
   \   \
   3    3
```

 

**进阶：**

你可以运用递归和迭代两种方法解决这个问题吗？

~~~Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return isMirror(root, root);
    }
    public boolean isMirror(TreeNode r1, TreeNode r2){
        if(r1==null && r2==null) return true;
        if(r1==null || r2==null) return false;
        return (r1.val==r2.val) && isMirror(r1.left, r2.right) && isMirror(r1.right, r2.left);
    }
}
~~~

## [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

难度中等517

给你一个二叉树，请你返回其按 **层序遍历** 得到的节点值。 （即逐层地，从左到右访问所有节点）。

 

**示例：**
二叉树：`[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回其层次遍历结果：

```
[
  [3],
  [9,20],
  [15,7]
]
```

~~~Java
//
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        Queue<TreeNode> queue = new ArrayDeque<>();
        if(root == null){
            return res;
        }else{
            queue.add(root);
        }

        while(!queue.isEmpty()){
            int n = queue.size();
            List<Integer> level = new ArrayList<>();
            for (int i=0;i<n; i++){
                TreeNode node = queue.poll();
                level.add(node.val);
                if(node.left!=null) {
                    queue.add(node.left);
                }
                if(node.right!=null){
                    queue.add(node.right);
                }
            }
            res.add(level);
        }
        return res;
    }
}

~~~

## [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

难度简单549

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

**说明:** 叶子节点是指没有子节点的节点。

**示例：**
给定二叉树 `[3,9,20,null,null,15,7]`，

```
    3
   / \
  9  20
    /  \
   15   7
```

返回它的最大深度 3 。

### 递归

~~~Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int maxDepth(TreeNode root) {
        if(root==null) return 0;
        int leftd = maxDepth(root.left);
        int rightd = maxDepth(root.right); 
        return Math.max(leftd, rightd)+1;
    }
}
~~~

### 迭代

#### BFS

~~~Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null) return 0;
        Queue<TreeNode> queue = new ArrayDeque<>();
        queue.add(root);
        int max = 0;
        while(!queue.isEmpty()){
            max++;
            int n = queue.size();
            for (int i=0; i<n; i++){
                TreeNode node = queue.poll();
                if(node.left != null) queue.add(node.left);
                if(node.right != null) queue.add(node.right);
            }
        }
        return max;
    }
}
~~~

#### DFS

## [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

难度中等515

根据一棵树的前序遍历与中序遍历构造二叉树。

**注意:**
你可以假设树中没有重复的元素。

例如，给出

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```

~~~Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        HashMap<Integer, Integer> map = new HashMap<>();
        for(int i=0; i<inorder.length; i++){
            map.put(inorder[i], i);
        }
        return build(preorder, map, 0, preorder.length-1, 0);
    }

    public TreeNode build(int[] preorder, HashMap<Integer, Integer> map, int preStart, int preEnd, int inStart){
        if(preEnd<preStart) return null;
        TreeNode root = new TreeNode(preorder[preStart]);
        int rootIndex = map.get(root.val);
        int len = rootIndex-inStart;
        root.left = build(preorder, map, preStart+1, preStart+len, inStart);
        root.right = build(preorder, map, preStart+len+1, preEnd, rootIndex+1);
        return root;
    }
}
~~~

## [114. 二叉树展开为链表](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)

难度中等357

给定一个二叉树，[原地](https://baike.baidu.com/item/原地算法/8010757)将它展开为一个单链表。

 

例如，给定二叉树

```
    1
   / \
  2   5
 / \   \
3   4   6
```

将其展开为：

```
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```

采用后序遍历

~~~Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public void flatten(TreeNode root) {
        if(root==null) return ;
        flatten(root.left);
        flatten(root.right);
        TreeNode tmp = root.right;
        root.right = root.left;
        root.left = null;
        while(root.right!=null) root=root.right;
        root.right=tmp;
    }
}
~~~

## [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

难度简单1028

给定一个数组，它的第 *i* 个元素是一支给定股票第 *i* 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。

注意：你不能在买入股票前卖出股票。

 

**示例 1:**

```
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

**示例 2:**

```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

~~~java 
class Solution {
    public int maxProfit(int[] prices) {
        int len = prices.length;
        if(len <= 1) return 0;
        int min = prices[0], max = 0;
        for(int i=0; i<len; i++){
            max = Math.max(max, prices[i]-min);
            min = Math.min(min, prices[i]);
        } 
        return max;
    }
}
~~~

## [124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

难度困难446

给定一个**非空**二叉树，返回其最大路径和。

本题中，路径被定义为一条从树中任意节点出发，达到任意节点的序列。该路径**至少包含一个**节点，且不一定经过根节点。

**示例 1:**

```
输入: [1,2,3]

       1
      / \
     2   3

输出: 6
```

**示例 2:**

```
输入: [-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

输出: 42
```

~~~Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
/*
求每个节点最大贡献值的贡献值
*/
class Solution {
    int max = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        getMax(root);
        return max;
    }
    public  int getMax(TreeNode root){
        if(root == null) return 0;
        int left = Math.max(0, getMax(root.left));
        int right = Math.max(0, getMax(root.right));
        int sum = root.val + left + right;
        max = Math.max(max, sum);
        return root.val + Math.max(left, right);
    }
}
~~~

## [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

难度困难346

给定一个未排序的整数数组，找出最长连续序列的长度。

要求算法的时间复杂度为 *O(n)*。

**示例:**

```
输入: [100, 4, 200, 1, 3, 2]
输出: 4
解释: 最长连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

~~~java
class Solution {
    public int longestConsecutive(int[] nums) {
        if(nums==null || nums.length==0) return 0;
        TreeSet<Integer> set = new TreeSet<>();
        for(int i=0; i<nums.length; i++){
            set.add(nums[i]);
        }
        int init = set.first()-1;
        int cnt = 0;
        int max = 0;
        for(Integer num : set){
            if(num-1==init){
                cnt++;
            }else{
                max = Math.max(max, cnt);
                cnt=1;
            }
            init = num;
        }
        max = Math.max(cnt, max);
        return max;
    }
}
~~~

## [136. 只出现一次的数字](https://leetcode-cn.com/problems/single-number/)

难度简单1296

给定一个**非空**整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

**说明：**

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

**示例 1:**

```
输入: [2,2,1]
输出: 1
```

**示例 2:**

```
输入: [4,1,2,1,2]
输出: 4
```

~~~Java
class Solution {
    public int singleNumber(int[] nums) {
        int ans = 0;
        for(int i=0; i<nums.length; i++){
            ans ^= nums[i];
        }
        return ans;
    }
}
~~~



## [139. 单词拆分](https://leetcode-cn.com/problems/word-break/)

难度中等440

给定一个**非空**字符串 *s* 和一个包含**非空**单词列表的字典 *wordDict*，判定 *s* 是否可以被空格拆分为一个或多个在字典中出现的单词。

**说明：**

- 拆分时可以重复使用字典中的单词。
- 你可以假设字典中没有重复的单词。

**示例 1：**

```
输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以被拆分成 "leet code"。
```

**示例 2：**

```
输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以被拆分成 "apple pen apple"。
     注意你可以重复使用字典中的单词。
```

**示例 3：**

```
输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false
```

~~~Java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        int n = s.length();
        //参考背包问题
        boolean[] dp = new boolean[n+1];
        dp[0] = true;
        //dp[i]表示 s 中以 i - 1 结尾的字符串是否可被 wordDict 拆分
        for (int i=1; i<=n; i++){
            for (int j=0; j<i; j++){
                if(dp[j] && wordDict.contains(s.substring(j, i))){
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[n];
    }
}
~~~

## [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

难度简单623

给定一个链表，判断链表中是否有环。

为了表示给定链表中的环，我们使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 `pos` 是 `-1`，则在该链表中没有环。

 

**示例 1：**

```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

**示例 2：**

```
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

**示例 3：**

```
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

 

**进阶：**

你能用 *O(1)*（即，常量）内存解决此问题吗？

~~~Java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle1(ListNode head) {
        Set<ListNode> set = new HashSet<>();
        while(head!=null){
            if(set.contains(head)) return true;
            else set.add(head);
            head=head.next;
        }
        return false;
    }
    public boolean hasCycle(ListNode head) {//O(1)
        if(head==null || head.next==null){
            return false;
        }
        ListNode fast = head.next;
        ListNode slow = head;
        while(slow != fast){
            if(fast==null || fast.next==null) return false;
            slow = slow.next;
            fast = fast.next.next;
        }
        return true;
    }
}
~~~



## [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

难度中等490

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 `null`。

为了表示给定链表中的环，我们使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 `pos` 是 `-1`，则在该链表中没有环。

**说明：**不允许修改给定的链表。

 

**示例 1：**

```
输入：head = [3,2,0,-4], pos = 1
输出：tail connects to node index 1
解释：链表中有一个环，其尾部连接到第二个节点。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

**示例 2：**

```
输入：head = [1,2], pos = 0
输出：tail connects to node index 0
解释：链表中有一个环，其尾部连接到第一个节点。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

**示例 3：**

```
输入：head = [1], pos = -1
输出：no cycle
解释：链表中没有环。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

> 相遇点不是环的入口！

**进阶：**
你是否可以不用额外空间解决此题？

~~~Java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        Set<ListNode> set = new HashSet<>();
        ListNode tmp = null;
        while(head!=null){
            if(set.contains(head)){
                tmp = head;
                break;
            }
            set.add(head);
            head=head.next;
        }
        return tmp;
    }
}

//常量内存
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode inter = intercept(head);
        if(inter == null) return null;
        ListNode ptr1 = head;
        while(ptr1 != inter){
            ptr1 = ptr1.next;
            inter = inter.next;
        }
        return ptr1;
    }

    public ListNode intercept(ListNode head){
        ListNode fast, slow;
        slow = fast = head;
        while(fast!=null && fast.next!=null){
            slow = slow.next;
            fast = fast.next.next;
            if(slow == fast){
                return fast;
            }
        }
        return null;
    }
}
~~~

## [146. LRU缓存机制](https://leetcode-cn.com/problems/lru-cache/)

难度中等650

运用你所掌握的数据结构，设计和实现一个 [LRU (最近最少使用) 缓存机制](https://baike.baidu.com/item/LRU)。它应该支持以下操作： 获取数据 `get` 和 写入数据 `put` 。

获取数据 `get(key)` - 如果关键字 (key) 存在于缓存中，则获取关键字的值（总是正数），否则返回 -1。
写入数据 `put(key, value)` - 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字/值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

 

**进阶:**

你是否可以在 **O(1)** 时间复杂度内完成这两种操作？

~~~java
class LRUCache {
    private int cap;
    private Map<Integer, Integer> map = new LinkedHashMap<>();
    public LRUCache(int capacity) {
        this.cap = capacity;
    }
    
    public int get(int key) {
        if(map.keySet().contains(key)){
            int value = map.get(key);
            map.remove(key);
            map.put(key, value);
            return value;
        }
        return -1;
    }
    
    public void put(int key, int value) {
        if(map.keySet().contains(key)){
            map.remove(key);
        }else if(map.size()==cap){
            Iterator<Map.Entry<Integer, Integer>> iter = map.entrySet().iterator();
            iter.next();
            iter.remove();
        }
        map.put(key, value);
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
~~~

## [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

难度中等560

在 *O*(*n* log *n*) 时间复杂度和常数级空间复杂度下，对链表进行排序。

**示例 1:**

```
输入: 4->2->1->3
输出: 1->2->3->4
```

**示例 2:**

```
输入: -1->5->3->4->0
输出: -1->0->3->4->5
```

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode sortList(ListNode head) {
        if(head == null) return null;
        if(head.next == null) return head;
        ListNode p1= head;
        ListNode p2= head;
        while(p2.next!=null && p2.next.next!=null){
            p1 = p1.next;
            p2 = p2.next.next;
        }
        ListNode tail = p1;
        p1 = p1.next;
        tail.next = null;
        ListNode l = sortList(head);
        ListNode r = sortList(p1);
        return merge(l, r);
    }

    public ListNode merge(ListNode left, ListNode right){
        ListNode pre = new ListNode(0);
        ListNode cur = pre;
        while(left!=null && right!=null){
            if(left.val <= right.val){
                cur.next = left;
                cur = cur.next;
                left = left.next;
            }else{
                cur.next = right;
                cur = cur.next;
                right = right.next;
            }
        }
        if(left != null) cur.next = left;
        if(right != null) cur.next = right;
        return pre.next;
    }
}
~~~

## [152. 乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/)

难度中等602

给你一个整数数组 `nums` ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

 

**示例 1:**

```
输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
```

**示例 2:**

```
输入: [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。
```

思路
标签：动态规划
遍历数组时计算当前最大值，不断更新
令imax为当前最大值，则当前最大值为 imax = max(imax * nums[i], nums[i])
由于存在负数，那么会导致最大的变最小的，最小的变最大的。因此还需要维护当前最小值imin，imin = min(imin * nums[i], nums[i])
当负数出现时则imax与imin进行交换再进行下一步计算
时间复杂度：O(n)O(n)

作者：guanpengchn
链接：https://leetcode-cn.com/problems/maximum-product-subarray/solution/hua-jie-suan-fa-152-cheng-ji-zui-da-zi-xu-lie-by-g/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

~~~java
class Solution {
    public int maxProduct(int[] nums) {
        int max=Integer.MIN_VALUE, imax=1, imin=1;
        for(int i=0; i<nums.length; i++){
            if(nums[i]<0){
                int tmp = imax;
                imax = imin;
                imin = tmp;
            }
            imax = Math.max(imax*nums[i], nums[i]);
            imin = Math.min(imin*nums[i], nums[i]);
            max = Math.max(max, imax);
        }
        return max;
    }
}
~~~

## [155. 最小栈](https://leetcode-cn.com/problems/min-stack/)

难度简单547

设计一个支持 `push` ，`pop` ，`top` 操作，并能在常数时间内检索到最小元素的栈。

- `push(x)` —— 将元素 x 推入栈中。
- `pop()` —— 删除栈顶的元素。
- `top()` —— 获取栈顶元素。
- `getMin()` —— 检索栈中的最小元素。

 

**示例:**

```
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```

 

**提示：**

- `pop`、`top` 和 `getMin` 操作总是在 **非空栈** 上调用。

思路：元素入栈两次（最小，两次都是入栈值，否则再入栈顶值），弹出两次。

~~~java
class MinStack {
    /** initialize your data structure here. */
    private Stack<Integer> stack;
    public MinStack() {
        stack = new Stack<>();
    }
    
    public void push(int x) {
        if(stack.isEmpty()){
            stack.push(x);
            stack.push(x);
        }else{
            int tmp = stack.peek();
            stack.push(x);
            if(tmp<x){
                stack.push(tmp);
            }else{
                stack.push(x);
            }
        }
    }
    
    public void pop() {
        stack.pop();
        stack.pop();
    }
    
    public int top() {
        return stack.get(stack.size()-2);
    }
    
    public int getMin() {
        return stack.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
~~~

## [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

难度简单674

编写一个程序，找到两个单链表相交的起始节点。

如下面的两个链表**：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

在节点 c1 开始相交。

 

**示例 1：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_1.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

 

**示例 2：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_2.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png)

```
输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Reference of the node with value = 2
输入解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```

 

**示例 3：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_3.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)

```
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
解释：这两个链表不相交，因此返回 null。
```

 

**注意：**

- 如果两个链表没有交点，返回 `null`.
- 在返回结果后，两个链表仍须保持原有的结构。
- 可假定整个链表结构中没有循环。
- 程序尽量满足 O(*n*) 时间复杂度，且仅用 O(*1*) 内存。

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode pa = headA;
        ListNode pb = headB;
        while(pa != pb){
           pa = pa==null?headB:pa.next;
           pb = pb==null?headA:pb.next; 
        }
        return pa;
    }
}
~~~

## [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

难度简单616

给定一个大小为 *n* 的数组，找到其中的多数元素。多数元素是指在数组中出现次数**大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

 

**示例 1:**

```
输入: [3,2,3]
输出: 3
```

**示例 2:**

```
输入: [2,2,1,1,1,2,2]
输出: 2
```

~~~java
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length/2];
    }
}
~~~

## [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)

难度简单868

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。

给定一个代表每个房屋存放金额的非负整数数组，计算你 **不触动警报装置的情况下** ，一夜之内能够偷窃到的最高金额。

 

**示例 1：**

```
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

**示例 2：**

```
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

 

**提示：**

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 400`

~~~java
class Solution {
    public int rob(int[] nums) {
        if(nums.length==0) return 0;
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        if(nums.length<2) return nums[0];
        dp[1] = nums[0]>nums[1]? nums[0]:nums[1];
        for(int i=2; i<nums.length; i++){
            dp[i] = (nums[i]+dp[i-2]) > dp[i-1] ? nums[i]+dp[i-2] : dp[i-1];
        }
        return dp[nums.length-1];
    }
}
~~~

## [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

难度中等586

给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

 

**示例 1:**

```
输入:
11110
11010
11000
00000
输出: 1
```

**示例 2:**

```
输入:
11000
11000
00100
00011
输出: 3
解释: 每座岛屿只能由水平和/或竖直方向上相邻的陆地连接而成。
```

- 思路：遍历岛这个二维数组，如果当前数为1，则进入感染函数并将岛个数+1
- 感染函数：其实就是一个递归标注的过程，它会将所有相连的1都标注成2。为什么要标注？这样就避免了遍历过程中的重复计数的情况，一个岛所有的1都变成了2后，遍历的时候就不会重复遍历了。建议没想明白的同学画个图看看。

~~~java
class Solution {
    public int numIslands(char[][] grid) {
        int num = 0;
        for(int i=0; i<grid.length; i++){
            for (int j=0; j<grid[0].length; j++){
                if(grid[i][j]=='1'){
                    infect(grid, i, j);
                    num++;
                }
            }
        }
        return num;
    }

    public void infect(char[][] grid, int i, int j){
        if(i<0 || i>=grid.length || j<0 || j>=grid[0].length || grid[i][j]!='1'){
            return ;
        }
        grid[i][j] = '2';
        infect(grid, i-1, j);
        infect(grid, i+1, j);
        infect(grid, i, j-1);
        infect(grid, i, j+1);
    }
}
~~~

## [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

难度简单987

反转一个单链表。

**示例:**

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

**进阶:**
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head==null || head.next==null) return head;
        ListNode newList = reverse(head.next);
        head.next.next = head;
        head.next = null;
        return newList;
    }
}


class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while(cur != null){
            ListNode nextNode = cur.next;
            cur.next = pre;
            pre = cur;
            cur = nextNode;
        }
        return pre;
    }
}
~~~

## [207. 课程表](https://leetcode-cn.com/problems/course-schedule/)

难度中等351

你这个学期必须选修 `numCourse` 门课程，记为 `0` 到 `numCourse-1` 。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们：`[0,1]`

给定课程总量以及它们的先决条件，请你判断是否可能完成所有课程的学习？

**示例 1:**

```
输入: 2, [[1,0]] 
输出: true
解释: 总共有 2 门课程。学习课程 1 之前，你需要完成课程 0。所以这是可能的。
```

**示例 2:**

```
输入: 2, [[1,0],[0,1]]
输出: false
解释: 总共有 2 门课程。学习课程 1 之前，你需要先完成课程 0；并且学习课程 0 之前，你还应先完成课程 1。这是不可能的。
```

 

**提示：**

1. 输入的先决条件是由 **边缘列表** 表示的图形，而不是 邻接矩阵 。详情请参见[图的表示法](http://blog.csdn.net/woaidapaopao/article/details/51732947)。
2. 你可以假定输入的先决条件中没有重复的边。
3. `1 <= numCourses <= 10^5`

~~~java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<List<Integer>> list = new ArrayList<>();
        for(int i = 0; i < numCourses; i++){
            list.add(new ArrayList<>());
        }
        for(int[] pair : prerequisites){
            list.get(pair[0]).add(pair[1]);
        }
        int[] flags = new int[numCourses];
        for(int i = 0; i < numCourses; i++){
            if(!dfs(list, flags, i)) return false;
        }
        return true;
    }
    private boolean dfs(List<List<Integer>> list, int[] flags, int i){
        if(flags[i] == 1) return false;
        if(flags[i] == -1) return true;
        flags[i] = 1;
        for(int j : list.get(i)){
            if(!dfs(list, flags, j)) return false;
        }
        flags[i] = -1;
        return true;
    }
}

作者：Utopiahiker
链接：https://leetcode-cn.com/problems/course-schedule/solution/li-yong-huan-de-xing-zhi-by-utopiahiker/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
~~~



## [208. 实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

难度中等320

实现一个 Trie (前缀树)，包含 `insert`, `search`, 和 `startsWith` 这三个操作。

**示例:**

```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // 返回 true
trie.search("app");     // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");   
trie.search("app");     // 返回 true
```

**说明:**

- 你可以假设所有的输入都是由小写字母 `a-z` 构成的。
- 保证所有输入均为非空字符串。

~~~java
class Trie {
    private List<String> words;
    /** Initialize your data structure here. */
    public Trie() {
        words = new ArrayList<>();
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        words.add(word);
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        if(words.contains(word)) return true;
        return false;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        for (int i=0; i<words.size(); i++){
            if (words.get(i).startsWith(prefix)){
                return true;
            }
        }
        return false;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
~~~

## [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

难度中等517

在未排序的数组中找到第 **k** 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

**示例 1:**

```
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
```

**示例 2:**

```
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
```

**说明:**

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。

~~~java
//时间复杂度 NlogK
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> heap = new PriorityQueue<>((n1, n2)->n1-n2);
        for(int n : nums){
            heap.add(n);
            if(heap.size()>k) heap.poll();
        }
        return heap.poll();
    }
}

//时间复杂度 nlogn


~~~







## [221. 最大正方形](https://leetcode-cn.com/problems/maximal-square/)

难度中等450

在一个由 0 和 1 组成的二维矩阵内，找到只包含 1 的最大正方形，并返回其面积。

**示例:**

```
输入: 

1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

输出: 4
```

~~~java
class Solution {
    public int maximalSquare(char[][] matrix) {
        if(matrix.length < 1 || matrix[0].length < 1) return 0;
        int rows = matrix.length;
        int cols = matrix[0].length;
        int[][] dp = new int[rows+1][cols+1];
        int max = 0;
        for(int i=1; i<=rows; i++){
            for (int j=1; j <=cols; j++){
                if(matrix[i-1][j-1]=='1'){
                    dp[i][j] = Math.min(Math.min(dp[i-1][j], dp[i][j-1]), dp[i-1][j-1])+1;
                    max = Math.max(max, dp[i][j]);
                }
            }
        }
        return max*max;
    }
}
~~~

## [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

翻转一棵二叉树。

**示例：**

输入：

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

输出：

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

难度简单465

翻转一棵二叉树。

**示例：**

输入：

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

输出：

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root==null) return root;
        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);
        root.left = right;
        root.right = left;
        return root;
    }
}
~~~

## [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

难度简单522

请判断一个链表是否为回文链表。

**示例 1:**

```
输入: 1->2
输出: false
```

**示例 2:**

```
输入: 1->2->2->1
输出: true
```

**进阶：**
你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？

~~~java
class Solution {
    public boolean isPalindrome(ListNode head) {
        // 要实现 O(n) 的时间复杂度和 O(1) 的空间复杂度，需要翻转后半部分
        if (head == null || head.next == null) {
            return true;
        }
        ListNode fast = head;
        ListNode slow = head;
        // 根据快慢指针，找到链表的中点
        while(fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        slow = reverse(slow.next);
        while(slow != null) {
            if (head.val != slow.val) {
                return false;
            }
            head = head.next;
            slow = slow.next;
        }
        return true;
    }

    private ListNode reverse(ListNode head){
        // 递归到最后一个节点，返回新的新的头结点
        if (head.next == null) {
            return head;
        }
        ListNode newHead = reverse(head.next);
        head.next.next = head;
        head.next = null;
        return newHead;
    }
}
~~~

## [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

难度中等598

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

[百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

例如，给定如下二叉树: root = [3,5,1,6,2,0,8,null,null,7,4]

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/binarytree.png)

 

**示例 1:**

```
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
```

**示例 2:**

```
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
```

 

**说明:**

- 所有节点的值都是唯一的。
- p、q 为不同节点且均存在于给定的二叉树中。

```java
 注意p,q必然存在树内, 且所有节点的值唯一!!!
        递归思想, 对以root为根的(子)树进行查找p和q, 如果root == null || p || q 直接返回root
        表示对于当前树的查找已经完毕, 否则对左右子树进行查找, 根据左右子树的返回值判断:
        1. 左右子树的返回值都不为null, 由于值唯一左右子树的返回值就是p和q, 此时root为LCA
        2. 如果左右子树返回值只有一个不为null, 说明只有p和q存在于左或右子树中, 最先找到的那个节点为LCA
        3. 左右子树返回值均为null, p和q均不在树中, 返回null
```

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root==null || root==p || root==q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if(left == null && right==null) return null;
        else if(left!=null && right!=null) return root;
        else return left == null ? right : left;        
    }
}
~~~

## [238. 除自身以外数组的乘积](https://leetcode-cn.com/problems/product-of-array-except-self/)

难度中等486

给你一个长度为 *n* 的整数数组 `nums`，其中 *n* > 1，返回输出数组 `output` ，其中 `output[i]` 等于 `nums` 中除 `nums[i]` 之外其余各元素的乘积。

 

**示例:**

```
输入: [1,2,3,4]
输出: [24,12,8,6]
```

 

**提示：**题目数据保证数组之中任意元素的全部前缀元素和后缀（甚至是整个数组）的乘积都在 32 位整数范围内。

**说明:** 请**不要使用除法，**且在 O(*n*) 时间复杂度内完成此题。

**进阶：**
你可以在常数空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组**不被视为**额外空间。）

~~~java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int left=1, right=1;
        int[] res = new int[nums.length];
        Arrays.fill(res, 1);
        int n = nums.length;
        for(int i=0; i<n; i++){
            res[i] *= left;
            left*=nums[i];
            res[n-i-1] *= right;
            right*=nums[n-i-1];
        }
        return res;
    }
}
~~~

## [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

难度困难398

给定一个数组 *nums*，有一个大小为 *k* 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 *k* 个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。

 

**进阶：**

你能在线性时间复杂度内解决此题吗？

 

**示例:**

```
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

 

**提示：**

- `1 <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`
- `1 <= k <= nums.length`

~~~java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums==null || nums.length<2) return nums;
        int len = nums.length;
        LinkedList<Integer> queue = new LinkedList<>();
        int[] res = new int[len-k+1];
        for (int i = 0; i < len; i++) {
            while (!queue.isEmpty() && nums[queue.peekLast()]<=nums[i]){
                queue.pollLast();
            }
            queue.addLast(i);
            if(queue.peek()<=i-k){
                queue.poll();
            }
            if(i+1>=k){
                res[i+1-k] = nums[queue.peek()];
            }
        }
        return res;
    }
}
~~~



## [240. 搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)

难度中等327

编写一个高效的算法来搜索 *m* x *n* 矩阵 matrix 中的一个目标值 target。该矩阵具有以下特性：

- 每行的元素从左到右升序排列。
- 每列的元素从上到下升序排列。

**示例:**

现有矩阵 matrix 如下：

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

给定 target = `5`，返回 `true`。

给定 target = `20`，返回 `false`。

~~~java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if(matrix==null || matrix.length==0) return false;
        int i=0, j=matrix[0].length-1;
        while(i<matrix.length && j>=0){
            if(matrix[i][j]>target){
                j--;
            }else if(matrix[i][j]<target){
                i++;
            }else{return true;}
        }
        return false;
    }
}
~~~

## [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

难度中等452

给定正整数 *n*，找到若干个完全平方数（比如 `1, 4, 9, 16, ...`）使得它们的和等于 *n*。你需要让组成和的完全平方数的个数最少。

**示例 1:**

```
输入: n = 12
输出: 3 
解释: 12 = 4 + 4 + 4.
```

**示例 2:**

```
输入: n = 13
输出: 2
解释: 13 = 4 + 9.
```

```cpp
/*这里使用动态规划来做。时间复杂度O(nlogn)，空间复杂度O(n)。代码非常精简

定义一个函数f(n)表示我们要求的解。f(n)的求解过程为：
f(n) = 1 + min{
  f(n-1^2), f(n-2^2), f(n-3^2), f(n-4^2), ... , f(n-k^2) //(k为满足k^2<=n的最大的k)
}
*/
```

~~~java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n+1];
        dp[0] = 0;
        for(int i=1; i<=n; i++){
            int min = Integer.MAX_VALUE;
            for (int j=1; j*j<=i; j++){
                min = Math.min(min, dp[i-j*j]);
            }
            dp[i] = min+1;
        }
        return dp[n];
    }
}
~~~

## [399. 除法求值](https://leetcode-cn.com/problems/evaluate-division/)

难度中等163

给出方程式 `A / B = k`, 其中 `A` 和 `B` 均为用字符串表示的变量， `k` 是一个浮点型数字。根据已知方程式求解问题，并返回计算结果。如果结果不存在，则返回 `-1.0`。

**示例 :**
给定 `a / b = 2.0, b / c = 3.0`
问题: `a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? `
返回 `[6.0, 0.5, -1.0, 1.0, -1.0 ]`

输入为: `vector<pair<string, string>> equations, vector<double>& values, vector<pair<string, string>> queries`(方程式，方程式结果，问题方程式)， 其中 `equations.size() == values.size()`，即方程式的长度与方程式结果长度相等（程式与结果一一对应），并且结果值均为正数。以上为方程式的描述。 返回`vector<double>`类型。

基于上述例子，输入如下：

```
equations(方程式) = [ ["a", "b"], ["b", "c"] ],
values(方程式结果) = [2.0, 3.0],
queries(问题方程式) = [ ["a", "c"], ["b", "a"], ["a", "e"], ["a", "a"], ["x", "x"] ]. 
```

输入总是有效的。你可以假设除法运算中不会出现除数为0的情况，且不存在任何矛盾的结果。





## [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

难度简单612

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**示例:**

```
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```

**说明**:

1. 必须在原数组上操作，不能拷贝额外的数组。
2. 尽量减少操作次数。

~~~java
class Solution {
    public void moveZeroes(int[] nums) {
        if(nums==null || nums.length==0) return;
        int index = 0;
        for(int i=0; i<nums.length; i++){
            if(nums[i]!=0){
                nums[index] = nums[i];
                index++;
            }
        }
        for (int i=index; i<nums.length; i++){
            nums[i] = 0;
        }
    }
}
~~~

## [287. 寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)

难度中等714

给定一个包含 *n* + 1 个整数的数组 *nums*，其数字都在 1 到 *n* 之间（包括 1 和 *n*），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。

**示例 1:**

```
输入: [1,3,4,2,2]
输出: 2
```

**示例 2:**

```
输入: [3,1,3,4,2]
输出: 3
```

**说明：**

1. **不能**更改原数组（假设数组是只读的）。
2. 只能使用额外的 *O*(1) 的空间。
3. 时间复杂度小于 *O*(*n*2) 。
4. 数组中只有一个重复的数字，但它可能不止重复出现一次。

思路：

```java
 快慢指针思想, fast 和 slow 是指针, nums[slow] 表示取指针对应的元素
        注意 nums 数组中的数字都是在 1 到 n 之间的(在数组中进行游走不会越界),
        因为有重复数字的出现, 所以这个游走必然是成环的, 环的入口就是重复的元素, 
        即按照寻找链表环入口的思路来做
```

~~~~~~java
class Solution {
    public int findDuplicate(int[] nums) {
        int fast=0, slow=0;
        while(true){
            fast = nums[nums[fast]];
            slow = nums[slow];
            if(fast == slow){
                fast = 0;
                while(nums[slow] != nums[fast]){
                    fast = nums[fast];
                    slow = nums[slow];
                }
                return nums[slow];
            }
        }
    }
}
~~~~~~

## [297. 二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

难度困难208

序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。

请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。

**示例:** 

```
你可以将以下二叉树：

    1
   / \
  2   3
     / \
    4   5

序列化为 "[1,2,3,null,null,4,5]"
```

**提示:** 这与 LeetCode 目前使用的方式一致，详情请参阅 [LeetCode 序列化二叉树的格式](https://leetcode-cn.com/faq/#binary-tree)。你并非必须采取这种方式，你也可以采用其他的方法解决这个问题。

**说明:** 不要使用类的成员 / 全局 / 静态变量来存储状态，你的序列化和反序列化算法应该是无状态的。

~~~java
public class Codec {
    public String rserialize(TreeNode root, String str) {
        if (root == null) {
            str += "None,";
        } else {
            str += str.valueOf(root.val) + ",";
            str = rserialize(root.left, str);
            str = rserialize(root.right, str);
        }
        return str;
    }
  
    public String serialize(TreeNode root) {
        return rserialize(root, "");
    }
  
    public TreeNode rdeserialize(List<String> l) {
        if (l.get(0).equals("None")) {
            l.remove(0);
            return null;
        }
  
        TreeNode root = new TreeNode(Integer.valueOf(l.get(0)));
        l.remove(0);
        root.left = rdeserialize(l);
        root.right = rdeserialize(l);
    
        return root;
    }
  
    public TreeNode deserialize(String data) {
        String[] data_array = data.split(",");
        List<String> data_list = new LinkedList<String>(Arrays.asList(data_array));
        return rdeserialize(data_list);
    }
};
~~~





## [300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

难度中等757收藏分享切换为英文关注反馈

给定一个无序的整数数组，找到其中最长上升子序列的长度。

**示例:**

```
输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
```

**说明:**

- 可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
- 你算法的时间复杂度应该为 O(*n2*) 。

**进阶:** 你能将算法的时间复杂度降低到 O(*n* log *n*) 吗?

~~~java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int len = nums.length;
        if(len<2) return len;
        int[] dp = new int[len];
        Arrays.fill(dp, 1);
        for(int i=1; i<len; i++){
            for(int j=0; j<i; j++){
                if(nums[j]<nums[i])
                dp[i] = Math.max(dp[i], dp[j]+1);
            }
        }
        int re = dp[0];
        for(int e : dp){
            re = Math.max(re, e);
        }
        return re;
    }
}
~~~

## [301. 删除无效的括号](https://leetcode-cn.com/problems/remove-invalid-parentheses/)

难度困难186

删除最小数量的无效括号，使得输入的字符串有效，返回所有可能的结果。

**说明:** 输入可能包含了除 `(` 和 `)` 以外的字符。

**示例 1:**

```
输入: "()())()"
输出: ["()()()", "(())()"]
```

**示例 2:**

```
输入: "(a)())()"
输出: ["(a)()()", "(a())()"]
```

**示例 3:**

```
输入: ")("
输出: [""]
```

~~~java 
class Solution {
    public List<String> removeInvalidParentheses(String s) {
        List<String> res = new ArrayList<>();
        if(s.equals("()") || s.equals("")){
            res.add(s);
            return res;
        }
        Deque<String> queue = new LinkedList<>();
        queue.offer(s);
        Set<String> set = new HashSet<>();
        boolean isFound = false;
        while(!queue.isEmpty()){
            String cur = queue.poll();
            if(isValid(cur)){
                res.add(cur);
                isFound = true;
            }
            if(isFound){
                continue;
            }
            for(int i=0; i<cur.length(); i++){
                if(cur.charAt(i)=='(' || cur.charAt(i)==')'){
                    String str;
                    if(i==cur.length()-1){
                        str = cur.substring(0, cur.length()-1);
                    }else{
                        str = cur.substring(0, i)+cur.substring(i+1);
                    }
                    if(set.add(str)){
                        queue.offer(str);
                    }
                }
            }
        }
        if(res.isEmpty()) {
            res.add("");
        }
        return res;
    }

    public boolean isValid(String s){
        int left = 0;
        for(int i=0; i<s.length(); i++){
            char cur = s.charAt(i);
            if(cur=='('){
                left++;
            }else if(cur==')'){
                if(left != 0){
                    left--;
                }else{
                    return false;
                }
            }
        }
        return left == 0;
    }
}
~~~

## [309. 最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

难度中等342

给定一个整数数组，其中第 *i* 个元素代表了第 *i* 天的股票价格 。

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

- 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
- 卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。

**示例:**

```
输入: [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]
```

操作：买股票、卖股票、持有（什么都不做）

状态：

* 第i天手中没股票
* 第i天手中有股票

~~~
dp[n][0] ----------第n天手中没有股票最大利润
dp[n][1] ----------第n天手中持有股票最大利润

dp[n][0]: maxProfit
 (a) n-1天没有股票，n天啥都不做 --- dp[n-1][0]
 (b) n-1持有股票，n天卖出了 --- dp[n-1][1]+price[n]
 
dp[n][1]: maxProfitHold
 (a) n-1天持有股票，n天啥都不做 --- dp[n-1][1]
 (b) n-2天卖出了，n-1天冷冻期，n天买入了 --- dp[n-2][0]-price[n]
 
 初始值：
 dp[n-1][0] = 0; oneDaybefore
 dp[n-2][0] = 0; twoDayBefore
 dp[n-1][1] = minValue; oneDayBeforeHold;
~~~

~~~java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices==null || prices.length == 0) return 0;
        int oneDayBefore = 0;//dp[n-1][0]=0
        int twoDayBefore = 0;//dp[n-2][0]=0
        //dp[n-1][1] = min
        int oneDayBeforeHold = Integer.MIN_VALUE;
        int maxProfit = 0;//dp[n][0]
        int maxProfitHold = 0;//dp[n][1]
        for (int i=0; i<prices.length; i++){
            maxProfit = Math.max(oneDayBefore, oneDayBeforeHold + prices[i]);
            maxProfitHold = Math.max(oneDayBeforeHold, twoDayBefore - prices[i]);
            twoDayBefore = oneDayBefore;
            oneDayBefore = maxProfit;
            oneDayBeforeHold = maxProfitHold;
        }
        return maxProfit;
    }
}
~~~

## [312. 戳气球](https://leetcode-cn.com/problems/burst-balloons/)

难度困难313

有 `n` 个气球，编号为`0` 到 `n-1`，每个气球上都标有一个数字，这些数字存在数组 `nums` 中。

现在要求你戳破所有的气球。每当你戳破一个气球 `i` 时，你可以获得 `nums[left] * nums[i] * nums[right]` 个硬币。 这里的 `left` 和 `right` 代表和 `i` 相邻的两个气球的序号。注意当你戳破了气球 `i` 后，气球 `left` 和气球 `right` 就变成了相邻的气球。

求所能获得硬币的最大数量。

**说明:**

- 你可以假设 `nums[-1] = nums[n] = 1`，但注意它们不是真实存在的所以并不能被戳破。
- 0 ≤ `n` ≤ 500, 0 ≤ `nums[i]` ≤ 100

**示例:**

```
输入: [3,1,5,8]
输出: 167 
解释: nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
     coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167
```

~~~java 
class Solution {
    public int maxCoins(int[] nums) {
        int[] newNums = new int[nums.length+2];
        newNums[0] = 1;
        newNums[newNums.length-1] = 1;
        for(int i=0; i<nums.length; i++){
            newNums[i+1] = nums[i];
        }
        int n = newNums.length;
        int[][] dp = new int[n][n];
        for(int i=n-2; i>=0; i--){
            for (int j=i+2; j<n; j++){
                for(int k=i+1; k < j; k++){
                    dp[i][j] = Math.max(dp[i][j], dp[i][k]+dp[k][j]+newNums[i]*newNums[k]*newNums[j]);
                }
            }
        }
        return dp[0][n-1];
    }
}
~~~

## [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

难度中等642

给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 `-1`。

 

**示例 1:**

```
输入: coins = [1, 2, 5], amount = 11
输出: 3 
解释: 11 = 5 + 5 + 1
```

**示例 2:**

```
输入: coins = [2], amount = 3
输出: -1
```

**说明**:
你可以认为每种硬币的数量是无限的。

dp[i]=x 表示钱为i时需要的最少金币数

~~~
dp[i] = min(dp[i], dp[i-c]+1);
~~~

~~~java
class Solution {
    public int coinChange(int[] coins, int amount) {
        if(coins==null || coins.length<1) return -1;
        int[] dp = new int[amount+1];
        Arrays.fill(dp, amount+1);
        dp[0] = 0;
        for(int i=1; i<= amount; i++){
            for(int c : coins){
                if (i-c<0) continue;
                dp[i] = Math.min(dp[i], dp[i-c]+1);
            }
        }
        return dp[amount]==amount+1 ? -1 : dp[amount];
    }
}
~~~

## [337. 打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/)

难度中等377

在上次打劫完一条街道之后和一圈房屋后，小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。

计算在不触动警报的情况下，小偷一晚能够盗取的最高金额。

**示例 1:**

```
输入: [3,2,3,null,3,null,1]

     3
    / \
   2   3
    \   \ 
     3   1

输出: 7 
解释: 小偷一晚能够盗取的最高金额 = 3 + 3 + 1 = 7.
```

**示例 2:**

```
输入: [3,4,5,1,3,null,1]

     3
    / \
   4   5
  / \   \ 
 1   3   1

输出: 9
解释: 小偷一晚能够盗取的最高金额 = 4 + 5 = 9.
```

```
    public int[] helper(TreeNode r){
        if(r == null) return new int[2];//边界条件，r为null时，跳出
        int[] left = helper(r.left);//对于以r.left为根的树，计算抢劫根节点(r.left)与不抢劫根节点可获得最大金额. left[0]则为不抢r.lrft可获得的最大金额,left[1]则为抢劫r.left可获得的最大金额  以下right[] 分析同理
        int[] right = helper(r.right);
        int[] res = new int[2];
        res[0] = Math.max(left[0],left[1]) + Math.max(right[0],right[1]);//计算不抢劫当前根节点可获得的最大金额(那么其左右子树可以随便抢)
        res[1] = r.val + left[0] + right[0];//计算若抢劫根节点可获得的最大金额(此时,其左右子树的根节点不能被抢)
        return res;
    }
```

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int rob(TreeNode root) {
        int[] res = helper(root);
        return Math.max(res[0], res[1]);
    }

    public int[] helper(TreeNode root){
        if(root==null) return new int[2];
        int[] left = helper(root.left);
        int[] right = helper(root.right);
        int[] res = new int[2];
        res[0] = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
        res[1] = root.val + left[0] + right[0];
        return res;
    }
}
~~~

## [338. 比特位计数](https://leetcode-cn.com/problems/counting-bits/)

难度中等326

给定一个非负整数 **num**。对于 **0 ≤ i ≤ num** 范围中的每个数字 **i** ，计算其二进制数中的 1 的数目并将它们作为数组返回。

**示例 1:**

```
输入: 2
输出: [0,1,1]
```

**示例 2:**

```
输入: 5
输出: [0,1,1,2,1,2]
```

**进阶:**

- 给出时间复杂度为**O(n\*sizeof(integer))**的解答非常容易。但你可以在线性时间**O(n)**内用一趟扫描做到吗？
- 要求算法的空间复杂度为**O(n)**。
- 你能进一步完善解法吗？要求在C++或任何其他语言中不使用任何内置函数（如 C++ 中的 **__builtin_popcount**）来执行此操作。

i & (i - 1)可以去掉i最右边的一个1（如果有），因此 i & (i - 1）是比 i 小的，而且i & (i - 1)的1的个数已经在前面算过了，所以i的1的个数就是 i & (i - 1)的1的个数加上1

~~~java
class Solution {
    public int[] countBits(int num) {
        int[] res = new int[num+1];
        for(int i=1; i<=num; i++){
            res[i] = res[i&(i-1)]+1;
        }
        return res;
    }
}
~~~

## [347. 前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)

难度中等363

给定一个非空的整数数组，返回其中出现频率前 ***k\*** 高的元素。

 

**示例 1:**

```
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
```

**示例 2:**

```
输入: nums = [1], k = 1
输出: [1]
```

**提示：**

- 你可以假设给定的 *k* 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。
- 你的算法的时间复杂度**必须**优于 O(*n* log *n*) , *n* 是数组的大小。
- 题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的。
- 你可以按任意顺序返回答案。

> 使用Map和优先队列。

~~~java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int num : nums){
            map.put(num, map.getOrDefault(num, 0)+1);
        }

        PriorityQueue<Map.Entry<Integer, Integer>> heap = new PriorityQueue<>((x,y)->x.getValue()-y.getValue());
        for(Map.Entry<Integer, Integer> entry : map.entrySet()){
            heap.add(entry);
            if(heap.size()>k){
                heap.poll();
            }
        }
        int[] res = new int[k];
        int i=0;
        while(!heap.isEmpty()){
            res[i++] = heap.poll().getKey();
        }
        return res;
    }
}
~~~

## [394. 字符串解码](https://leetcode-cn.com/problems/decode-string/)

难度中等383

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: `k[encoded_string]`，表示其中方括号内部的 *encoded_string* 正好重复 *k* 次。注意 *k* 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 *k* ，例如不会出现像 `3a` 或 `2[4]` 的输入。

 

**示例 1：**

```
输入：s = "3[a]2[bc]"
输出："aaabcbc"
```

**示例 2：**

```
输入：s = "3[a2[c]]"
输出："accaccacc"
```

**示例 3：**

```
输入：s = "2[abc]3[cd]ef"
输出："abcabccdcdcdef"
```

**示例 4：**

```
输入：s = "abc3[cd]xyz"
输出："abccdcdcdxyz"
```

~~~java
class Solution {
    public String decodeString(String s) {
        int len = s.length();
        int num = 0;
        Deque<String> str_stack = new LinkedList<>();
        Deque<Integer> num_stack = new LinkedList<>();
        String cur = "";
        String res = "";
        for (int i=0; i<len; i++){
            char c = s.charAt(i);
            if(c>='0' && c<='9'){
                num = 10*num + c - '0';
            }else if(c=='['){
                num_stack.push(num);
                str_stack.push(cur);
                num = 0;
                cur = "";
            }
            else if(Character.isAlphabetic(c)){
                cur+=s.substring(i,i+1);
            }
            else if(c==']'){
                int k = num_stack.pop();
                String tmp = str_stack.pop();
                for(int j=0; j<k; j++){
                    tmp+=cur;
                }
                cur = tmp;
            }
        }
        res += cur;
        return res;
    }
}
~~~

## [399. 除法求值](https://leetcode-cn.com/problems/evaluate-division/)

难度中等157

给出方程式 `A / B = k`, 其中 `A` 和 `B` 均为用字符串表示的变量， `k` 是一个浮点型数字。根据已知方程式求解问题，并返回计算结果。如果结果不存在，则返回 `-1.0`。

**示例 :**
给定 `a / b = 2.0, b / c = 3.0`
问题: `a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? `
返回 `[6.0, 0.5, -1.0, 1.0, -1.0 ]`

输入为: `vector<pair<string, string>> equations, vector<double>& values, vector<pair<string, string>> queries`(方程式，方程式结果，问题方程式)， 其中 `equations.size() == values.size()`，即方程式的长度与方程式结果长度相等（程式与结果一一对应），并且结果值均为正数。以上为方程式的描述。 返回`vector<double>`类型。

基于上述例子，输入如下：

```
equations(方程式) = [ ["a", "b"], ["b", "c"] ],
values(方程式结果) = [2.0, 3.0],
queries(问题方程式) = [ ["a", "c"], ["b", "a"], ["a", "e"], ["a", "a"], ["x", "x"] ]. 
```

输入总是有效的。你可以假设除法运算中不会出现除数为0的情况，且不存在任何矛盾的结果。



## [406. 根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)

难度中等360

假设有打乱顺序的一群人站成一个队列。 每个人由一个整数对`(h, k)`表示，其中`h`是这个人的身高，`k`是排在这个人前面且身高大于或等于`h`的人数。 编写一个算法来重建这个队列。

**注意：**
总人数少于1100人。

**示例**

```
输入:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

输出:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
```

思路：按照身高降序、K升序排序，按照k来插入，注意comparator的写法，前面减后面升序，后面减前面降序。

~~~java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        if(people.length==0 || people[0].length==0) return new int[0][0];
        Arrays.sort(people, new Comparator<int[]>(){
            public int compare(int[] o1, int[] o2){
                return o1[0]==o2[0] ? o1[1]-o2[1] : o2[0]-o1[0];
            }
        });
        List<int[]> list = new ArrayList<>();
        for(int[] i : people){
            list.add(i[1], i);
        }
        return list.toArray(new int[list.size()][]);
    }
}
~~~

## [416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

难度中等301

给定一个**只包含正整数**的**非空**数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

**注意:**

1. 每个数组中的元素不会超过 100
2. 数组的大小不会超过 200

**示例 1:**

```
输入: [1, 5, 11, 5]

输出: true

解释: 数组可以分割成 [1, 5, 5] 和 [11].
```

 

**示例 2:**

```
输入: [1, 2, 3, 5]

输出: false

解释: 数组不能分割成两个元素和相等的子集.
```

~~~java
class Solution {
    public boolean canPartition(int[] nums) {
        int n=nums.length;
        int sum=0;
        for(int e : nums){
            sum+=e;
        }
        if(sum%2==1) return false;
        boolean[][] dp = new boolean[n+1][sum/2+1];
        for(int i=0; i<=n; i++){
            dp[i][0] = true;
        }
        for(int i=1; i<=n; i++){
            for (int j=0; j<=sum/2; j++){
                if(j>=nums[i-1]){
                    dp[i][j] = dp[i-1][j] || dp[i-1][j-nums[i-1]];
                }else{
                    dp[i][j] = dp[i-1][j];
                }
            }
        }
        return dp[n][sum/2];
    }
}
~~~

## [437. 路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)

难度简单447收藏分享切换为英文关注反馈

给定一个二叉树，它的每个结点都存放着一个整数值。

找出路径和等于给定数值的路径总数。

路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

二叉树不超过1000个节点，且节点数值范围是 [-1000000,1000000] 的整数。

**示例：**

```
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

返回 3。和等于 8 的路径有:

1.  5 -> 3
2.  5 -> 2 -> 1
3.  -3 -> 11
```

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    int cnt;
    public int pathSum(TreeNode root, int sum) {
        if (root==null) return 0;
        sum(root, sum);
        pathSum(root.left, sum);
        pathSum(root.right, sum);
        return cnt;
    }

    private void sum(TreeNode root, int sum){
        if(root==null) return ;
        sum-=root.val;
        if(sum==0) cnt++;
        sum(root.left, sum);
        sum(root.right, sum);
    }
}
~~~

## [438. 找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

难度中等287

给定一个字符串 **s** 和一个非空字符串 **p**，找到 **s** 中所有是 **p** 的字母异位词的子串，返回这些子串的起始索引。

字符串只包含小写英文字母，并且字符串 **s** 和 **p** 的长度都不超过 20100。

**说明：**

- 字母异位词指字母相同，但排列不同的字符串。
- 不考虑答案输出的顺序。

**示例 1:**

```
输入:
s: "cbaebabacd" p: "abc"

输出:
[0, 6]

解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的字母异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的字母异位词。
```

 **示例 2:**

```
输入:
s: "abab" p: "ab"

输出:
[0, 1, 2]

解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的字母异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的字母异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的字母异位词。
```

~~~java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        Map<Character, Integer> pmap = new HashMap<>();
        Map<Character, Integer> smap = new HashMap<>();
        for(char c : p.toCharArray()){
            pmap.put(c, pmap.getOrDefault(c, 0)+1);
        }
        List<Integer> list = new ArrayList<>();
        int cnt=0, left=0, right=0;
        int len = p.length();
        while(right < s.length()){
            char ch = s.charAt(right);
            smap.put(ch, smap.getOrDefault(ch, 0)+1);
            if(pmap.containsKey(ch)&&(smap.get(ch)<=pmap.get(ch))){
                cnt++;
            }
            if(cnt==len) list.add(left);
            if(right-left+1 >= len){
                char left_ch = s.charAt(left);
                if(pmap.containsKey(left_ch)&&smap.get(left_ch)<=pmap.get(left_ch)){
                    cnt--;
                }
                smap.put(left_ch, smap.getOrDefault(left_ch, 1)-1);
                left++;
            }
            right++;
        }
        return list;
    }
}
~~~



## [448. 找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)

难度简单364收藏分享切换为英文关注反馈

给定一个范围在 1 ≤ a[i] ≤ *n* ( *n* = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。

找到所有在 [1, *n*] 范围之间没有出现在数组中的数字。

您能在不使用额外空间且时间复杂度为*O(n)*的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。

**示例:**

```
输入:
[4,3,2,7,8,2,3,1]

输出:
[5,6]
```

~~~java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> res = new ArrayList<>();
        for(int i=0; i<nums.length; i++){
            if(nums[Math.abs(nums[i])-1]>0){
                nums[Math.abs(nums[i])-1] = -nums[Math.abs(nums[i])-1];
            }
        }
        for(int i=0; i< nums.length; i++){
            if(nums[i]>0){
                res.add(i+1);
            }
        }
        return res;
    }
}
~~~

## [461. 汉明距离](https://leetcode-cn.com/problems/hamming-distance/)

难度简单288收藏分享切换为英文关注反馈

两个整数之间的[汉明距离](https://baike.baidu.com/item/汉明距离)指的是这两个数字对应二进制位不同的位置的数目。

给出两个整数 `x` 和 `y`，计算它们之间的汉明距离。

**注意：**
0 ≤ `x`, `y` < 231.

**示例:**

```
输入: x = 1, y = 4

输出: 2

解释:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑

上面的箭头指出了对应二进制位不同的位置。
```

~~~java
class Solution {
    public int hammingDistance(int x, int y) {
        int z = x^y;
        int cnt=0;
        while(z>0){
            if(z%2 == 1) cnt++;
            z = z>>1;
        }
        return cnt;
    }
}
~~~

## [494. 目标和](https://leetcode-cn.com/problems/target-sum/)

难度中等288

给定一个非负整数数组，a1, a2, ..., an, 和一个目标数，S。现在你有两个符号 `+` 和 `-`。对于数组中的任意一个整数，你都可以从 `+` 或 `-`中选择一个符号添加在前面。

返回可以使最终数组和为目标数 S 的所有添加符号的方法数。

 

**示例：**

```
输入：nums: [1, 1, 1, 1, 1], S: 3
输出：5
解释：

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

一共有5种方法让最终目标和为3。
```

 

**提示：**

- 数组非空，且长度不会超过 20 。
- 初始的数组的和不会超过 1000 。
- 保证返回的最终结果能被 32 位整数存下

~~~java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        int sum = 0;
        for(int e : nums){
            sum+=e;
        }
        if(sum<Math.abs(S) || (sum+S)%2 != 0) return 0;
        int target = (sum+S)/2;
        int[] dp = new int[target+1];
        dp[0] = 1;
        for(int i=1; i<=nums.length; i++){
            for(int j=target; j>=nums[i-1];j--){
                dp[j] = dp[j]+dp[j-nums[i-1]];
            }
        }
        return dp[target];
    }
}
~~~



## [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

难度简单263收藏分享切换为英文关注反馈

给定一个二叉搜索树（Binary Search Tree），把它转换成为累加树（Greater Tree)，使得每个节点的值是原来的节点值加上所有大于它的节点值之和。

 

**例如：**

```
输入: 原始二叉搜索树:
              5
            /   \
           2     13

输出: 转换为累加树:
             18
            /   \
          20     13
```

 

**注意：**本题和 1038: https://leetcode-cn.com/problems/binary-search-tree-to-greater-sum-tree/ 相同

中序遍历（左中右）：从小到大

反过来（右中左）：从大到小

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    int num=0;
    public TreeNode convertBST(TreeNode root) {
        if(root != null){
            convertBST(root.right);
            root.val = root.val + num;
            num = root.val;//重要
            convertBST(root.left);
            return root;
        }
        return null;
    }
}
~~~

## [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

难度简单378收藏分享切换为英文关注反馈

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。

 

**示例 :**
给定二叉树

```
          1
         / \
        2   3
       / \     
      4   5    
```

返回 **3**, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。

 

**注意：**两结点之间的路径长度是以它们之间边的数目表示。

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    int max=0;
    public int diameterOfBinaryTree(TreeNode root) {
        if(root==null) return 0;
        dfs(root);
        return max;
    }

    private int dfs(TreeNode root){
        if(root.left==null && root.right==null) return 0;
        int leftsize = root.left==null ? 0 : dfs(root.left)+1;
        int rightsize = root.right==null ? 0 : dfs(root.right)+1;
        max = Math.max(max, leftsize+rightsize);
        return Math.max(leftsize, rightsize);
    }
}
~~~

## [560. 和为K的子数组](https://leetcode-cn.com/problems/subarray-sum-equals-k/)

难度中等467

给定一个整数数组和一个整数 **k，**你需要找到该数组中和为 **k** 的连续的子数组的个数。

**示例 1 :**

```
输入:nums = [1,1,1], k = 2
输出: 2 , [1,1] 与 [1,1] 为两种不同的情况。
```

**说明 :**

1. 数组的长度为 [1, 20,000]。
2. 数组中元素的范围是 [-1000, 1000] ，且整数 **k** 的范围是 [-1e7, 1e7]。

暴力法

~~~java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int cnt=0;
        for(int i=0; i<nums.length; i++){
            int sum=0;
            for(int j=i; j<nums.length; j++){
                sum+=nums[j];
                cnt += sum==k ? 1 :0;            
            }
        }
        return cnt;
    }
}
~~~

~~~java
class Solution {
    public int subarraySum(int[] nums, int k) {
        /**
        扫描一遍数组, 使用map记录出现同样的和的次数, 对每个i计算累计和sum并判断map内是否有sum-k
        **/
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
        int sum = 0, ret = 0;

        for(int i = 0; i < nums.length; ++i) {
            sum += nums[i];
            if(map.containsKey(sum-k))
                ret += map.get(sum-k);
            map.put(sum, map.getOrDefault(sum, 0)+1);
        }
        
        return ret;
    }
}
~~~



## [581. 最短无序连续子数组](https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray/)

难度简单318收藏分享切换为英文关注反馈

给定一个整数数组，你需要寻找一个**连续的子数组**，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。

你找到的子数组应是**最短**的，请输出它的长度。

**示例 1:**

```
输入: [2, 6, 4, 8, 10, 9, 15]
输出: 5
解释: 你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
```

**说明 :**

1. 输入的数组长度范围在 [1, 10,000]。
2. 输入的数组可能包含**重复**元素 ，所以**升序**的意思是**<=。**

思路：

从左到右找出最后一个破坏递增的数

从右到左找出最后一个破坏递减的数

~~~java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int len = nums.length;
        if(len <= 1) return 0;
        int high = 0, low = len-1;
        int curMax = Integer.MIN_VALUE;
        int curMin = Integer.MAX_VALUE;
        for(int i=0; i<len; i++){
            if(nums[i] >= curMax){
                curMax = nums[i];
            }else{
                high = i;
            }

            if(nums[len-i-1] <= curMin){
                curMin = nums[len-i-1];
            }else{
                low = len - i - 1;
            }
        }
        return high>low ? high-low+1 : 0;
    }
}
~~~

## [621. 任务调度器](https://leetcode-cn.com/problems/task-scheduler/)

难度中等290

给定一个用字符数组表示的 CPU 需要执行的任务列表。其中包含使用大写的 A - Z 字母表示的26 种不同种类的任务。任务可以以任意顺序执行，并且每个任务都可以在 1 个单位时间内执行完。CPU 在任何一个单位时间内都可以执行一个任务，或者在待命状态。

然而，两个**相同种类**的任务之间必须有长度为 **n** 的冷却时间，因此至少有连续 n 个单位时间内 CPU 在执行不同的任务，或者在待命状态。

你需要计算完成所有任务所需要的**最短时间**。

 

**示例 ：**

```
输入：tasks = ["A","A","A","B","B","B"], n = 2
输出：8
解释：A -> B -> (待命) -> A -> B -> (待命) -> A -> B.
     在本示例中，两个相同类型任务之间必须间隔长度为 n = 2 的冷却时间，而执行一个任务只需要一个单位时间，所以中间出现了（待命）状态。 
```

 

**提示：**

1. 任务的总个数为 `[1, 10000]`。
2. `n` 的取值范围为 `[0, 100]`。



## [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

难度简单392收藏分享切换为英文关注反馈

给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。

你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则**不为** NULL 的节点将直接作为新二叉树的节点。

**示例 1:**

```
输入: 
	Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
输出: 
合并后的树:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7
```

**注意:** 合并必须从两个树的根节点开始。

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if(t1==null) return t2;
        if(t2==null) return t1;
        t1.val += t2.val;
        t1.left = subMerge(t1.left, t2.left);
        t1.right = subMerge(t1.right, t2.right);
        return t1;
    }

    public TreeNode subMerge(TreeNode t1, TreeNode t2){
        if(t1==null && t2==null) return null;
        TreeNode root = new TreeNode((t1==null ? 0 : t1.val)+(t2==null ? 0 : t2.val));
        root.left = subMerge(t1==null ? null : t1.left, t2==null ? null : t2.left);
        root.right = subMerge(t1==null ? null : t1.right, t2==null ? null : t2.right);
        return root;
    }
}
~~~

