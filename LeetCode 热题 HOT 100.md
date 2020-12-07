# 🔥 LeetCode 热题 HOT 100

## 1.两数之和

**题目：**

> 给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的那 **两个** 整数，并返回他们的数组下标。
>
> 你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。
>
> 
>
> **示例:**
>
> ```
> 给定 nums = [2, 7, 11, 15], target = 9
> 因为 nums[0] + nums[1] = 2 + 7 = 9
> 所以返回 [0, 1]
> ```
>
> 

**思路：**

**代码：**

``` java
class Solution {
    /*
    解题思路：哈希映射
    1.初始化：一个哈希表map,便于在O(1)时间找到目标值
    2.循环遍历nums
        1.查找map中是否存在target - nums[i]的key值，若存在则直接返回当前i和map对应的value
        2.若不存在则将当前数字与下标存入哈希表
    3.没有结果则抛出异常
    */
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; ++i) {
            if (map.containsKey(target - nums[i]))
                return new int[]{i, map.get(target - nums[i])};
            else
                map.put(nums[i], i);
        }
        return new int[2];
    }
}
```

## 2.两数相加

**题目：**

> 给出两个 **非空** 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 **逆序** 的方式存储的，并且它们的每个节点只能存储 **一位** 数字。
>
> 如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。
>
> 您可以假设除了数字 0 之外，这两个数都不会以 0 开头。
>
> **示例：**
>
> ```
> 输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
> 输出：7 -> 0 -> 8
> 原因：342 + 465 = 807
> ```
>
> 

**思路**：

**代码：**

``` java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    /*
    解题思路：
    1.初始化：last保存上一位产生的进位，初始为0，lastNode保存上一个节点,建立头节点head
    2.对l1,l2的公共位进行计算，将节点和与进位相加，将大于10的部分保存在last中，小于10的部分则创建一个新节点，将lastNode指向当前节点
    3.对l1,l2剩下的部分进行一样的操作
    4.返回head.next即可
    */
    int last = 0;
    ListNode l1, l2;
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        this.l1 = l1;
        this.l2 = l2;
        return build();
    }

    public ListNode build() {
        if (l1 == null && l2 == null && last == 0)
            return null;
        ListNode cur = new ListNode();
        if ((l1 == null && l2 != null) || (l1 != null && l2 == null)) {
            int val = l1 == null ? l2.val : l1.val;
            int sum = val + last;
            last = sum / 10;
            cur.val = sum % 10;
        } else if (l1 != null && l2 != null){
            int sum = l1.val + l2.val + last;
            last = sum / 10;
            cur.val = sum % 10;
        } else {
            cur.val = last;
            last = 0;
        }
        if (l1 != null)
            l1 = l1.next;
        if (l2 != null)
            l2 = l2.next;
        cur.next = build();
        return cur;
    }
}
```

## 3.无重复字符的最长子串

**题目：**

> 给定一个字符串，请你找出其中不含有重复字符的 **最长子串** 的长度。
>
> **示例 1:**
>
> ```
> 输入: "abcabcbb"
> 输出: 3 
> 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
> ```
>
> **示例 2:**
>
> ```
> 输入: "bbbbb"
> 输出: 1
> 解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
> ```
>

**思路：**

**代码：**

``` java
class Solution {
    /*
    解题思路：哈希表
    1.初始化：建立哈希表map，key与value分别存储数组元素与对应下标，最大值max默认为0，当前子串长度len默认也为0
    2.遍历s字符串，设s(i)为下标为i的字符
        1.判断前一个s(i)是否在当前子串中
            1.map.get(s(i)) + len < i ：前一个s(i)不在子串中，++len即可
            2.map.get(s(i)) + len >= i：前一个s(i)在子串中，len = i - map.get(s(i))
        2.将s(i)与i存入map中
        3.将max与len的较大者存入max
    3.直接返回max即可
    */
    public int lengthOfLongestSubstring(String s) {
        HashMap<Character, Integer> map = new HashMap<>();
        int max = 0, len = 0;
        for (int i = 0; i < s.length(); ++i) {
            if (map.containsKey(s.charAt(i))) {
                if (map.get(s.charAt(i)) + len < i) {//不在len中
                    ++len;
                } else
                    len = i - map.get(s.charAt(i));
            } else {
                ++len;
            }
            map.put(s.charAt(i), i);
            max = Math.max(max, len);
        }
        return max;
    }
}
```



## 4.寻找两个正序数组的中位数

**题目：**

> 给定两个大小为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出并返回这两个正序数组的中位数。
>
> 进阶：你能设计一个时间复杂度为 O(log (m+n)) 的算法解决此问题吗？

**示例 1：**

```
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2
```

**思路：**

**代码：**

``` java
class Solution {
    /*
    解题思路：
    设中位数是第k个数字，则在num1与num2中分别找到第k / 2个数字nums[i]与nums[j]
        *若nums[i] < nums[j]则表示nums[i]以及其之前的数字不可能为第k小的数字，则将它们排除，下一次应该从i + 1开始寻找
        *若nums[i] >= nums[j]则表示nums[j]及之前的数字不可能为第k小的数字，则将它们排序，下一次应该从j + 1开始寻找
        并且k = k - 被删除的数字的个数
    特殊情况：
        1.若其中一个数组以及没有数字，则直接可以返回另一个数组的第k个数字
        2.若k为1则直接返回两个数组的更小者
        3.若k / 2 大于数组长度则优先比较该数组的最后一个数字
    */
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length; 
        int left = (n + m + 1) / 2;
        int right = (n + m + 2) / 2;
        return (getKth(nums1, 0, m - 1, nums2, 0, n - 1, left) + getKth(nums1, 0, m - 1, nums2, 0 , n - 1, right)) / 2.0;
    }

    public int getKth(int[] nums1, int start1, int end1, int[] nums2, int start2, int end2, int k) {
        int len1 = end1 - start1 + 1;
        int len2 = end2 - start2 + 1;
        //将长度小的数组放到len1
        if (len1 > len2) {
            return getKth(nums2, start2, end2, nums1, start1, end1, k);
        }
        if (len1 == 0)//num1长度为0直接返回num2的第k个元素
            return nums2[start2 + k - 1];
        if (k == 1)
            return Math.min(nums1[start1], nums2[start2]);
        //若k / 2大于当前数组长度则优先比较数组的末尾元素
        int i = start1 + Math.min(k / 2, len1) - 1;
        int j = start2 + Math.min(k / 2, len2) - 1;

        if (nums1[i] < nums2[j]) {
            return getKth(nums1, i + 1, end1, nums2, start2, end2, k - (i - start1 + 1));
        } else
            return getKth(nums1, start1, end1, nums2, j + 1, end2, k - (j - start2 + 1));
    }
}
```



## 5.最长回文子串

**题目：**

> 给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。
>
> **示例** 1：
>
> ``` text
> 输入: "babad"
> 输出: "bab"
> 注意: "aba" 也是一个有效答案。
> 示例 2：
> 
> 输入: "cbbd"
> 输出: "bb"
> ```
>
> 

**思路：**

**代码：**

``` java
class Solution {
    /*
    解题思路：中心扩散法
    回文串一定是对称的，每次循环选择一个中心，进行左右扩展，判断左右字符是否相等即可
    */
    public String longestPalindrome(String s) {
        if (s.length() == 0)
            return "";
        int left = 0, right = 0;
        for (int i = 0; i < s.length(); ++i) {
            int len1 = getLength(s, i, i);//选择奇数个节点
            int len2 = getLength(s, i, i + 1);//选择偶数个节点
            int len = Math.max(len1, len2);
            if (len > right - left + 1) {
                left = i - (len - 1) / 2;
                right = i + len / 2;
            }
        }
        return s.substring(left, right + 1);
    }


    public int getLength(String s, int left, int right) {
        int l = left, r = right;
        while (l >= 0 && r < s.length() && s.charAt(l) == s.charAt(r)) {
            --l;
            ++r;
        }
        return r - l -1;
    }
}
```

``` java
class Solution {
    /*
    解题思路：动态规划
    若一个子串左右下标为i,j，若其首尾字符相等并且i+1,j-1也为回文串则它是个回文串
    定义dp[i][j]表示下标[i,j]是否为回文串，则有，dp[i][j] = (s[i] == s[j]) && dp[i + 1][j - 1]
    特例：当i+1 > j-1时直接返回true，所有单个字符都是回文串
    需要从底向上进行初始化
    */
    public String longestPalindrome(String s) {
        if (s.length() == 0)
            return "";
        int left = 0, right = 0, max = 1;
        int len = s.length();
        boolean[][] dp = new boolean[len][len];
        for (int i = len - 1; i >=0; --i) {
            for (int j = i; j < len; ++j) {
                if (i == j)
                    dp[i][j] = true;
                else if (s.charAt(i) == s.charAt(j) && (i + 1 > j - 1 || dp[i + 1][j - 1])) {
                    dp[i][j] = true;
                    if (max < j - i + 1) {
                        max = j - i + 1;
                        left = i;
                        right = j;
                    }
                }
            }
        }
        return s.substring(left, right + 1);
    }
} 
```



## 6.正则表达式匹配

**题目：**

> 请实现一个函数用来匹配包含`'. '`和`'*'`的正则表达式。模式中的字符`'.'`表示任意一个字符，而`'*'`表示它前面的字符可以出现任意次（含0次）。在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串`"aaa"`与模式`"a.a"`和`"ab*ac*a"`匹配，但与`"aa.a"`和`"ab*a"`均不匹配。
>
> **示例 1:**
>
> ```
> 输入:
> s = "aa"
> p = "a"
> 输出: false
> 解释: "a" 无法匹配 "aa" 整个字符串。
> ```
>
> **示例 2:**
>
> ```
> 输入:
> s = "aa"
> p = "a*"
> 输出: true
> 解释: 因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
> ```
>
> **示例 3:**
>
> ```
> 输入:
> s = "ab"
> p = ".*"
> 输出: true
> 解释: ".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。
> ```

**思路：**

[逐行详细讲解，由浅入深，dp和递归两种思路 - 正则表达式匹配 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof/solution/zhu-xing-xiang-xi-jiang-jie-you-qian-ru-shen-by-je/)

**代码：**

``` java
class Solution {
    /*
    解题思路：动态规划
    dp[i][j]表示字符串前i位与匹配串前j位是否能匹配成功
    根据p[j]的值可以分为以下几种情况：
        1.p[j]为字符：若p[j]=s[i]则只需要满足p[j - 1]=s[i - 1]即可满足，也就是dp[i][j] = dp[i - 1][j - 1]
        2.p[j]为'.'，与1类似，不作赘述
        3.p[j]为'*'也可以分为两种情况：
            1.p[j] != s[i]时：可以看作这个星号匹配0次，及只需要满足dp[i][j - 2]符合即可，dp[i][j] = dp[i][j - 2]
            2.p[j] == s[i]时：
                1.也可以匹配0次，不做赘述
                OR
                2.匹配多次，设这次的s[i]就是匹配的最后一个了，那么只需要保证前一个字符dp[i - 1][j]也能匹配该匹配即可，即dp[i][j] = dp[i - 1][j]
    
    初始化：
    1.字符串为空，匹配串也为空时为true
    2.字符串不为空，匹配串为空时为false，即dp[i][j] i > 0, j = 0 时
    3.字符串为空，匹配串不为空时，需要计算，比如s = "" , p = "a*b*c*";
    4.都不为空时也需要计算
    */
    public boolean isMatch(String s, String p) {
        int n = s.length();
        int m = p.length();
        boolean[][] dp = new boolean[n + 1][m + 1];
        for (int i = 0; i <= n; ++i) {
            for (int j = 0; j <= m; ++j) {
                if (j == 0)
                    dp[i][j] = i == 0;
                else {
                    if (p.charAt(j - 1) != '*') {
                        if (i > 0 && (s.charAt(i - 1) == p.charAt(j - 1) || p.charAt(j - 1) == '.'))
                            dp[i][j] = dp[i - 1][j - 1];
                    } else {
                        //匹配0次
                        if (j >= 2)
                            dp[i][j] |= dp[i][j - 2];
                        //假设是多次匹配的最后一次
                        if (i > 0 && j >= 2 && (s.charAt(i - 1) == p.charAt(j - 2) || p.charAt(j - 2) == '.'))
                            dp[i][j] |= dp[i - 1][j];
                    }
                }
            }
        }
        return dp[n][m];
    }
}
```



## 7.盛最多水的容器

**题目：**

> 给你 `n` 个非负整数 `a1，a2，...，a``n`，每个数代表坐标中的一个点 `(i, ai)` 。在坐标内画 `n` 条垂直线，垂直线 `i` 的两个端点分别为 `(i, ai)` 和 `(i, 0)` 。找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。
>
> **说明：**你不能倾斜容器。
>
>  
>
> **示例 1：**
>
> ![img](E:/program files/Topora images/question_11.jpg)
>
> ```
> 输入：[1,8,6,2,5,4,8,3,7]
> 输出：49 
> 解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
> ```
>
> **示例 2：**
>
> ```
> 输入：height = [1,1]
> 输出：1
> ```
>
> **示例 3：**
>
> ```
> 输入：height = [4,3,2,1,4]
> 输出：16
> ```

**思路：**

[盛最多水的容器（双指针法，易懂解析，图解） - 盛最多水的容器 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/container-with-most-water/solution/container-with-most-water-shuang-zhi-zhen-fa-yi-do/)

**代码：**

``` java
class Solution {
    /*
    解题思路：双指针法
    设置双指针位于容器两端，根据规则移动指针，并且更新面积最大值res，直到i == j 时返回res
    指针的移动规则：每次选定围成水槽两板的中较小的那个，向中间收缩1格。以下证明：
        *无论长板还是短板向内收缩都会使底边宽度减1
            若短板向内移动，水槽的短板min(h[i],h[j])可能变大，水槽面积s(i,j)可能增大
            若长板向内移动，水槽的短板只可能变小或不变，下一个水槽面积一定小于当前水槽面积
    可以得出削去的所有面积都<s(i,j)，也就是说我们削去的所有状态都不会导致面积最大值的丢失
    */
    public int maxArea(int[] height) {
        int i = 0, j = height.length - 1, res = 0;
        while (i < j) {
            res = height[i] > height[j] ?
            Math.max(res, (j - i) * height[j--]) :
            Math.max(res, (j - i) * height[i++]);
        }
        return res;
    }
}
```



## 8.三数之和

**题目：**

> 给你一个包含 *n* 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 *a，b，c ，*使得 *a + b + c =* 0 ？请你找出所有满足条件且不重复的三元组。
>
> **注意：**答案中不可以包含重复的三元组。
>
>  
>
> **示例：**
>
> ```
> 给定数组 nums = [-1, 0, 1, 2, -1, -4]，
> 
> 满足要求的三元组集合为：
> [
>   [-1, 0, 1],
>   [-1, -1, 2]
> ]
> ```

**示例**：

``` reStructuredText
给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```



**思路：**

[三数之和（排序+双指针，易懂图解） - 三数之和 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/3sum/solution/3sumpai-xu-shuang-zhi-zhen-yi-dong-by-jyd/)

**代码：**

``` java
class Solution {
    /*
    解题思路：暴力+双指针优化消去无效解优化效率
    双指针法铺垫：先将nums数组排序
    双指针法思路：循环遍历数组，固定当前元素nums[k]，双指针i,j位于[k + 1, len - 1]两端，通过双指针交替向中间移动
    记录对于k满足nums[k] + nums[i] + nums[j] = 0的i,j组合
    注意点：
    1.当k>0时也就是最小指针大于0时，之后也不可能有解了，直接break退出循环
    2.若nums[k] = nums[k - 1]即当前元素和之前的元素相等则跳过它，因为nums[k]的解肯定已经全部加入结集中了，本次搜索只会得到重复的解
    3.i,j搜索的值应该要满足0 - nums[k] = nums[i] + nums[j]，而此时可能还有诺干重复的nums[i]与nums[j]，应该跳过它们反之记录重复的解
    */
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        if (nums.length < 3)
            return res;
        Arrays.sort(nums);
        for (int i = 0; i <= nums.length - 3; ++i) {
            //因为num[i-1]以及把所有解存入res中了
            if (i > 0 && nums[i] == nums[i - 1])
                continue;
            //若3个数字都大于0则不可能有解
            if (nums[i] > 0)
                break;
            int left = i + 1, right = nums.length - 1, target = 0 - nums[i];
            while (left != right) {
                int sum = nums[left] + nums[right];
                if (target < sum)
                    --right;
                else if (target > sum)
                    ++left;
                else {
                    if (right < nums.length - 1 && nums[right] == nums[right + 1]) {
                        --right;
                        continue;
                    }
                    ArrayList<Integer> tmp = new ArrayList<>();
                    tmp.add(nums[i]);
                    tmp.add(nums[left]);
                    tmp.add(nums[right]);
                    res.add(tmp);
                    --right;
                }
            }
        }
        return res;
    }
}
```



## 9.电话号码的字母组合

**题目：**

> 给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。
>
> 给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。
>
> ![img](E:\program files\Topora images\17_telephone_keypad.png)
>
> 示例:
>
> ``` text
> 输入："23"
> 输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
> ```
>
> 说明:
> 尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。

**思路：**

**代码：**

``` java
class Solution {
    /*
    解题思路：递归回溯
    定义一个递归函数backtrack(int k),k代表数字的下标为k
    1.终止条件：当k == 字符串长度时将解添加到res集合中
    2.遍历k位的所有解集空间，并开启下一位递归
    */
    char[][] chars = {
        {'a','b','c'},
        {'d','e','f'},
        {'g','h','i'},
        {'j','k','l'},
        {'m','n','o'},
        {'p','q','r','s'},
        {'t','u','v'},
        {'w','x','y','z'}
    };
    List<String> res = new ArrayList<>();
    char[] digits;
    char[] tmp;
    public List<String> letterCombinations(String digits) {
        if (digits.length() == 0)
            return res;
         this.digits = digits.toCharArray();
         tmp = new char[digits.length()];
         backtrack(0);
         return res;
    }

    public void backtrack(int k) {
        if (k == digits.length) {
            res.add(new String(tmp));
            return;
        }
        for (char ch : chars[digits[k] - 50]) {
            tmp[k] = ch;
            backtrack(k + 1);
        }
    }
}
```



## 10.删除链表的倒数第N个节点

**题目：**

> 给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。
>
> **示例**：
>
> ``` text
> 给定一个链表: 1->2->3->4->5, 和 n = 2.
> 
> 当删除了倒数第二个节点后，链表变为 1->2->3->5.
> ```
>
> **说明**：
>
> 给定的 n 保证是有效的。
>
> **进阶**：
>
> 你能尝试使用一趟扫描实现吗？

**思路：**

**代码：**

``` java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    /*
    解题思路：快慢指针
    1.初始化：创建3个指针，分别指向前一个节点的last,指向当前节点的cur,始终比cur节点快n - 1步长的节点prelast来保证删除节点时链表不会发生断裂
    pre始与cur保持n的距离
    2.找到要删除的节点：各节点循环向后移动，直到pre的next指向null退出循环
    此时cur正好指向要删除的节点，将pre.next 指向cur.next即可
    */
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if (head == null)
            return null;
        ListNode pHead = new ListNode(0, head);
        ListNode last = pHead, cur = head, pre = head;
        //pre先走n - 1步
        for (int i = 0; i < n - 1; ++i) {
            pre = pre.next;
        }
        while (pre.next != null) {
            last = last.next;
            cur = cur.next;
            pre = pre.next;
        }
        //删除cur
        last.next = cur.next;
        return pHead.next;
    }
}
```





## 11.有效的括号

**题目：**

> 给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
>
> 有效字符串需满足：
>
> 左括号必须用相同类型的右括号闭合。
> 左括号必须以正确的顺序闭合。
> 注意空字符串可被认为是有效字符串。
>
> **示例** 1:
>
> ``` text
> 输入: "()"
> 输出: true
> ```
>
> **示例** 2:
>
> ``` text
> 输入: "()[]{}"
> 输出: true
> ```
>
> **示例** 3:
>
> ``` text
> 输入: "(]"
> 输出: false
> ```
>
> **示例** 4:
>
> ```  text
> 输入: "([)]"
> 输出: false
> ```

**思路：**

[有效的括号（辅助栈法，极简+图解） - 有效的括号 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/valid-parentheses/solution/valid-parentheses-fu-zhu-zhan-fa-by-jin407891080/)

**代码：**

``` java
class Solution {
    /*
    解题思路：消消乐法（辅助栈法）
    栈先入后出特点刚好与本题括号排序特点一致，即若遇到左括号入栈，遇到右括号时将对应栈顶左括号出栈，则遍历完所有括号后stack仍然然空
    建立哈希表dic构建左右括号对应关系：key左括号，value右括号；这样查询两个括号是否对应只需要O(1)的时间复杂度；建立栈stack，遍历字符串s并按照算法流程一一判断
    */
    public boolean isValid(String s) {
        HashMap<Character, Character> dic = new HashMap<>(){{
            put('[', ']');put('(',')');put('{','}');put('?','?');
        }};
        Stack<Character> stack = new Stack<>(){{
            push('?');
        }};
        for (char ch : s.toCharArray()) {
            //左括号直接入栈
            if (dic.containsKey(ch))
                stack.push(ch);
            else {//若ch是右括号则与stack出栈元素对应的右括号比较，不相符则直接返回false
                if (dic.get(stack.pop()) != ch)
                    return false;
            }
        }
        return stack.size() == 1;
    }
}
```



## 12.无重复字符的最长子串

**题目：**

> 将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 
>
> ``` text
> 示例：
> 
> 输入：1->2->4, 1->3->4
> 输出：1->1->2->3->4->4
> ```
>
> 

**思路**：

**代码：**

``` java
class Solution {
    /*
    解题思路：
    1.初始化：创建头指针head，用于返回头节点，last指针保存上一个指针，p,q分别指向l1,l2首部
    2.当l1,l2不为空时循环：
        1.新建一个指针cur
            1.若p为空，直接使last指向q并且退出循环
            2.若q为空，直接使last指向p并且退出循环
            3.p.val < q.val时使cur指向p,并将p后移1位
            4.p.val >= q.val时使cur指向q,并将q后移1位
        2.将last.next指向cur，last指向新的cur
    3.返回head.next即可
    */
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode p = l1, q = l2, head = new ListNode(), last = head;
        //不全为空时循环
        while (!(p == null && q == null)) {
            ListNode cur;
            //其中一个为空直接让last指向另一个节点即可，这样另一个节点之后的所有节点也会自然跟过来
            if (p == null) {
                last.next = q;
                break;
            } else if (q == null) {
                last.next = p;
                break;
            } else if (p.val < q.val) {
                cur = p;
                p = p.next;
            } else {
                cur = q;
                q = q.next;
            }
            last.next = cur;
            last = cur;
        }
        return head.next;
    }
}
```



## 13.括号生成

**题目：**

> 数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。
>
>  ``` text
> 示例：
> 
> 输入：n = 3
> 输出：[
>        "((()))",
>        "(()())",
>        "(())()",
>        "()(())",
>        "()()()"
>      ]
>  ```
>
> 

**思路：**

![img](E:/program files/Topora images/efbe574e5e6addcd1c9dc5c13a50c6f162a2b14a95d6aed2c394e18287a067fa-image.png)

**代码：**

``` java
class Solution {
    /*
    解题思路：回溯法
    关键点：左括号个数始终大于等于右括号个数,否则就剪枝
    
    */
    List<String> res = new ArrayList<>();
    char[] tmp;
    public List<String> generateParenthesis(int n) {
        tmp = new char[2 * n];
        backtrack(0, 0, 0);
        return res;
    }

    public void backtrack(int k, int left, int right) {
        //剪支
        if (left < right)
            return;
        if (left < tmp.length / 2) {
            tmp[k] = '(';
            backtrack(k + 1, left + 1, right);
        }
        if (right < tmp.length / 2) {
            tmp[k] = ')';
            backtrack(k + 1, left, right + 1);
        }
        if (k == tmp.length - 1) {
            res.add(new String(tmp));
        }
    }
}
```



## 14.合并K个升序链表

**题目：**

> 给你一个链表数组，每个链表都已经按升序排列。
>
> 请你将所有链表合并到一个升序链表中，返回合并后的链表。
>
>  
>
> 示例 1：
>
> ``` text
> 输入：lists = [[1,4,5],[1,3,4],[2,6]]
> 输出：[1,1,2,3,4,4,5,6]
> 解释：链表数组如下：
> [
>   1->4->5,
>   1->3->4,
>   2->6
> ]
> 将它们合并到一个有序链表中得到。
> 1->1->2->3->4->4->5->6
> ```
>
> 示例 2：
>
> ``` text
> 输入：lists = []
> 输出：[]
> ```
>
> 示例 3：
>
> ``` text
> 输入：lists = [[]]
> 输出：[]
> ```
>
> 

**思路：**

归并求解

**代码：**

``` java
class Solution {
    /*
    解题思路：分治法
    ListNode mergeAndDivide(ListNode[] lists, int left, int right):
    1.递归结束条件：数组只有一个元素即(left == right)
    2.开启递归(分)：mid = (left + right) / 2
        1.对left至mid进行递归
        2.对mid + 1至right进行递归
    3.合并：
        对两个递归返回的链表进行合并
    4.返回链表首元素即可
    时间复杂度：nlog(k),其中n为所有节点数，k为数组中链表个数
    */
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0)
            return null;
        return mergeAndDivide(lists, 0, lists.length - 1);
    }

    public ListNode mergeAndDivide(ListNode[] lists, int left, int right) {
        if (left == right)
            return lists[left];
        //开启左右递归,即分
        int mid = (left + right) / 2;
        ListNode lNode = mergeAndDivide(lists, left, mid);
        ListNode rNode = mergeAndDivide(lists, mid + 1, right);
        //合
        ListNode head = new ListNode(), last = head, p = lNode, q = rNode;
        while (!(p == null && q == null)) {
            if (p == null) {
                last.next = q;
                break;
            } else if (q == null) {
                last.next = p;
                break;
            } else if (p.val < q.val) {
                last.next = p;
                p = p.next;
            } else {
                last.next = q;
                q = q.next;
            }
            last = last.next;
        }
        return head.next;
    }
}
```

## 15.下一个排列

**题目：**

> 实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。
>
> 如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。
>
> 必须**[原地](https://baike.baidu.com/item/原地算法)**修改，只允许使用额外常数空间。
>
> 以下是一些例子，输入位于左侧列，其相应输出位于右侧列。
> `1,2,3` → `1,3,2`
> `3,2,1` → `1,2,3`
> `1,1,5` → `1,5,1`

**思路：**

**代码：**

``` java
class Solution {
    /*
    解题思路：我们需要找到找到一个“大数字”与前面“小数字”交换，并且希望这个增加的幅度足够小
    1.寻找“小数字”：因为要足够小，所以我们从后向前寻找一个数字满足nums[k] < nums[k + 1],此时[k,len)为降序，其中len为数组长度
    *若找不到则直接将整个数组翻转，并返回即可
    2.寻找“大数字”：右因为要足够小，所有我们继续从[k+1,len)从后往前寻找第一个大于nums[k]的数字nums[m]
    然后交换m与k
    3.因为[k+1,len)仍然是升序的，我们将其翻转即可得到降序顺序，此时返回即可
    */
    public void nextPermutation(int[] nums) {
        int len = nums.length, k = len - 2;
        for (; k >= -1; --k) {
            if (k == -1)
                break;
            if (nums[k] < nums[k + 1])
                break;
        }
        int p = 0, q = len - 1;
        if (k != -1) {
            //寻找“大数字”
            int m = len - 1;
            for (; m > k; --m) {
                if (nums[m] > nums[k])
                    break;
            }
            swap(nums, m, k);
            p = k + 1;
        }
        while (p < q)
            swap(nums, p++, q--);
    }

    public void swap(int[] nums, int a, int b) {
        int tmp = nums[a];
        nums[a] = nums[b];
        nums[b] = tmp;
    }
}
```



## 16.最长有效括号

**题目：**

> 给定一个只包含 `'('` 和 `')'` 的字符串，找出最长的包含有效括号的子串的长度。
>
> **示例 1:**
>
> ```
> 输入: "(()"
> 输出: 2
> 解释: 最长有效括号子串为 "()"
> ```
>
> **示例 2:**
>
> ```
> 输入: ")()())"
> 输出: 4
> 解释: 最长有效括号子串为 "()()"
> ```

**思路：**

**代码：**

``` java
class Solution {
    /*
    解题思路：栈的活用（括号相关的题目都可以优先考虑使用栈来解决）
    1.初始化：创建一个保存括号下标的栈stack,初始化加入一个下标-1作为参考点，参考点是用来标识一段连续子串的，并且可以参与计算子串长度，可以简单看作是上一个子串的首节点的前一个节点的下标,最大值max=0
    2.遍历字符串s：
        1.判断s[i]为哪种括号：
            1.若s[i]为'('：直接将s[i]对应的下标入栈
            2.若s[i]为')'：直接使stack出栈，并判断此时stack是否为空
                1.若stack为空：表示上一个子串已经结束，需要新的标识，将当前右括号的下标压入stack
                2.若stack不为空：表示仍然在上一个字符串中，curLen = i - stack.peek(),若curLen大于max则更新max

    */
    public int longestValidParentheses(String s) {
        int max = 0;
        Deque<Integer> stack = new ArrayDeque<>();
        stack.push(-1);
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) == '(')
                stack.push(i);
            else {
                stack.pop();
                if (stack.isEmpty())
                    stack.push(i);
                else {
                    max = Math.max(max, i - stack.peek());
                }
            }
        }
        return max;
    }
}
```



## 17.搜索旋转排序数组

**题目：**

> 给你一个升序排列的整数数组 `nums` ，和一个整数 `target` 。
>
> 假设按照升序排序的数组在预先未知的某个点上进行了旋转。（例如，数组 `[0,1,2,4,5,6,7]` 可能变为 `[4,5,6,7,0,1,2]` ）。
>
> 请你在数组中搜索 `target` ，如果数组中存在这个目标值，则返回它的索引，否则返回 `-1` 。
>
>  
>
> **示例 1：**
>
> ```
> 输入：nums = [4,5,6,7,0,1,2], target = 0
> 输出：4
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [4,5,6,7,0,1,2], target = 3
> 输出：-1
> ```
>
> **示例 3：**
>
> ```
> 输入：nums = [1], target = 0
> 输出：-1
> ```

**思路：**

**代码：**

``` java
class Solution {
    /*
    解题思路：二分法
    1.初始化：low = 0, high = nums.length - 1;
    2.当low <= high时进行二分查找：
        1.mid = (low + high) >> 1，判断nums[mid]在左半段数组还是右半段数组：
            1.若nums[mid] == target则直接返回mid
            2.若nums[mid] >= nums[0]：说明nums[mid]在左半段：
                1. 判断target在nums[mid]的右侧还是左侧从而移动low或high：
                    1.（targetnums[mid]左侧）若target >= nums[0]（大于左半段最小值）并且target < nums[mid]说明target完全在左半段数组中
                    移动high指针，即high = mid - 1;
                    2.其他情况：移动low指针，即low = mid + 1;
            3.若nums[mid] < nums[0]：说明nums[mid]在右半段：
                与上面类似
    */
    public int search(int[] nums, int target) {
        int low = 0, high = nums.length - 1;
        while (low <= high) {
            int mid = (low + high) >> 1;
            if (nums[mid] == target)
                return mid;
            //判断在左半段还是右半段
            if (nums[mid] >= nums[0]) {
                if (target >= nums[0] && target < nums[mid])
                    high = mid - 1;
                else
                    low = mid + 1;
            } else { 
                if (target <= nums[nums.length - 1] && target > nums[mid])
                    low = mid + 1;
                else 
                    high = mid -1;
            }
        }
        return -1;
    }
}
```



## 18.在排序数组中查找元素的第一个和最后一个位置

**题目：**

> 给定一个按照升序排列的整数数组 `nums`，和一个目标值 `target`。找出给定目标值在数组中的开始位置和结束位置。
>
> 你的算法时间复杂度必须是 *O*(log *n*) 级别。
>
> 如果数组中不存在目标值，返回 `[-1, -1]`。
>
> **示例 1:**
>
> ```
> 输入: nums = [5,7,7,8,8,10], target = 8
> 输出: [3,4]
> ```
>
> **示例 2:**
>
> ```
> 输入: nums = [5,7,7,8,8,10], target = 6
> 输出: [-1,-1]
> ```

**思路：**

**代码：**

``` java
class Solution {
    /*
    解题思路：二分查找
    */
    public int[] searchRange(int[] nums, int target) {
        int[] res = {-1, -1};
        int left = binarySearch(nums, target) + 1;
        int right = binarySearch(nums, target + 1);
        if (right >=0 && right < nums.length && nums[right] == target) {
            res[0] = left;
            res[1] = right;
        }
        return res;
    }

    //找左边界
    public int binarySearch(int[] nums, int target) {
        int low = 0, high = nums.length - 1;
        while (low <= high) {
            int mid = (low + high) >> 1;
            if (nums[mid] >= target)
                high = mid - 1;
            else
                low = mid + 1;
        }
        return high;
    }
}
```



## 19.组合总和

**题目：**

> 给定一个**无重复元素**的数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。
>
> `candidates` 中的数字可以无限制重复被选取。
>
> **说明：**
>
> - 所有数字（包括 `target`）都是正整数。
> - 解集不能包含重复的组合。 
>
> **示例 1：**
>
> ```
> 输入：candidates = [2,3,6,7], target = 7,
> 所求解集为：
> [
>   [7],
>   [2,2,3]
> ]
> ```
>
> **示例 2：**
>
> ```
> 输入：candidates = [2,3,5], target = 8,
> 所求解集为：
> [
>   [2,2,2,2],
>   [2,3,3],
>   [3,5]
> ]
> ```

**思路：**

**代码：**

``` java
class Solution {
    /*
    解题思路：递归回溯+剪支
    回溯的终止条件：target == sum时，将当前子串加入结果集
    回溯以及剪支的条件：当前sum > target的时候进行回溯，并且进行剪支，需要nums数组排序才可以正常执行
    */
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> tmp = new LinkedList<>();
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);
        backtrack(candidates, target, 0, 0);
        return res;
    }

    public void backtrack(int[] nums, int target, int sum, int k) {
        if (target == sum) {
            res.add(new ArrayList(tmp));
        } else {
            for (int i = k; i < nums.length; ++i) {
                //剪支
                if (sum + nums[i] > target)
                    return;
                tmp.add(nums[i]);
                backtrack(nums, target, sum + nums[i], i);
                tmp.removeLast();
            }
        }
    }
}
```



## 20.接雨水

**题目：**

> 
> 给定 *n* 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。
>
>  
>
> **示例 1：**
>
> ![img](E:/program files/Topora images/rainwatertrap.png)
>
> ```
> 输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
> 输出：6
> 解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
> ```
>
> **示例 2：**
>
> ```
> 输入：height = [4,2,0,3,2,5]
> 输出：9
> ```

**思路：**

**代码：**

``` java
class Solution {
    /*
    解题思路：单调栈,按行计算水
            *什么是单调栈：单调栈就是比普通栈多一个性质，即维护一个栈内元素的单调
            *比如当前某个单调递减的栈的元素从栈底到栈顶分别是：[10, 9, 8, 3, 2]，如果要入栈元素5，需要把栈顶元素pop出去，
            *直到满足单调递减为止，即先变成[10, 9, 8]，再入栈5，就是[10, 9, 8, 5]。
    遍历柱子高度数组，将之前保存在单调栈中小于当前柱子高度height[i]的柱子全部出栈，出栈的同时可以想到
    由于这是单调栈，这个出栈元素k左侧若有柱子其高度必大于height[k],那么就是形成了“凹”，即可以接水，那么
    我们就可以将高度范围在[height[i], min(当前元素高度，栈顶元素高度)]，长度范围在栈顶柱子到当前柱子之间的水求出来了，将这个结果累加到sum里
    最后再将该元素压入栈中接着遍历其他元素即可
    */
    public int trap(int[] height) {
        if (height == null || height.length < 3)
            return 0;
        int sum = 0;
        ArrayDeque<Integer> stack = new ArrayDeque<>();//存储的是下标
        for (int i = 0; i < height.length; ++i) {
            //将stack中比当前柱子矮的柱子都出栈，并且计算它们与当前柱子之间的水
            while (!stack.isEmpty() && height[stack.peek()] <= height[i]) {
                int lastIndex = stack.poll();
                //高度相同元素不必要计算，直接出栈
                while (!stack.isEmpty() && height[stack.peek()] == height[lastIndex])
                    stack.poll();
                if (!stack.isEmpty()) {
                    //长度：当前柱子到stack栈顶柱子之间的长度
                    //高度：min(当前柱子，stack栈顶柱子) - 上一个出栈元素的高度
                    sum += (i - stack.peek() - 1) * (Math.min(height[i], height[stack.peek()]) - height[lastIndex]);
                }
            }
            stack.push(i);
        }
        return sum;
    }
}
```

``` java
class Solution {
    /*
    解题思路：双指针，计算每列的水量（每列水量由其左右最高柱子中较小的那个决定）
    1.初始化：left,right为分别指向数组头和尾的双指针，left_max与right_max分别保存左侧与右侧的最大柱子高度初始为0，sum为水量总和
    2.移动双指针循环每列的水量：
        1.left_max对于left指针来说是从左向右求来的，所以其值是可靠的，而right_max对于left指针来说
        是不可靠的,因为[i,right]范围内可能还存在必right_max更大的值，但是，如果left_max < righ_max则可以直接确定
        left的水量了，即满足left_max < right_max的话：
        若left_max高于left，则将其高于left的部分加到sum中
        2.right_max的分析过程与left_max一致，不做赘述
    3.直接返回sum即可
    */
    public int trap(int[] height) {
        int left = 0, right = height.length - 1;
        int left_max = 0, right_max = 0, sum = 0;
        //因为每一列都需要遍历到，所以一定要是<=若是<则会少一列
        while (left <= right) {
            //左最大值必右最大值小，即left指针所指的储水量必定由left_max决定
            if (left_max < right_max) {
                if (left_max > height[left]) {
                    sum += left_max - height[left];
                }
                left_max = Math.max(left_max, height[left]);
                ++left;
            } else {
                if (right_max > height[right]) {
                    sum += right_max - height[right];
                }
                right_max = Math.max(right_max, height[right]);
                --right;
            }
        }
        return sum;
    }
}
```

``` java
class Solution {
    /*
    解题思路：双指针法，按层计算水(不推荐，原理与单调栈类似)
    定义两个指针low,high分别指向height的首尾,h指示已经计算过的高度初始值为0,sum为总雨水和初始为）
    外循环 while (low < high - 1)：因为能接到雨水至少需要low和high之间距离为3
        1.右移动low，在不越界的情况下找到第一个 > h的柱子
        2.左移动high，在不越界的情况下找到第一个 > h的柱子
        3.若low 与 high 都没有越界说明它们都找到了合适的柱子
            1.sum加上[low + 1, high - 1]区间高度范围为[h, min(height[low], height[hight])]的水
            2.更新h为min(height[low], height[hight])
    最终返回sum即可
    */
    public int trap(int[] height) {
        int sum = 0, h = 0, low = 0, high = height.length - 1;
        //相差2格才能有水
        while (low < high - 1) {
            //找到大于h的左柱子
            while (low < high - 1 && height[low] <= h)
                ++low;
            //找到大于h的右柱子
            while (low < high - 1 && height[high] <= h)
                --high;
            if (low < high - 1) {
                int curHeight = Math.min(height[low], height[high]);
                sum += countWater(height, low + 1, high - 1, h, curHeight - h);
                //更新h
                h = curHeight;
            }
        }
        return sum;
    }
    //计算[left, right]范围内高度为[h, h + max]范围的水
    public int countWater(int[] nums, int left, int right, int h, int max) {
        int count = (right - left  + 1) * max;
        for (int i = left; i <= right; ++i) {
            if (nums[i] - h > 0) {
                count -= (nums[i] - h) > max ? max : nums[i] - h;
            }
        }
        return count;
    }
}
```



## 22.全排列

**题目：**

> 给定一个 **没有重复** 数字的序列，返回其所有可能的全排列。
>
> **示例:**
>
> ```
> 输入: [1,2,3]
> 输出:
> [
>   [1,2,3],
>   [1,3,2],
>   [2,1,3],
>   [2,3,1],
>   [3,1,2],
>   [3,2,1]
> ]
> ```

**思路：**

**代码：**

``` java
class Solution {
    /*
    解题思路：基于交换的回溯
    每次固定一位,先将每一位与其本身交换再和其后的所有数字交换
    */
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> permute(int[] nums) {
        backtrack(nums, 0);
        return res;
    }

    public void backtrack(int[] nums, int k) {
        if (k == nums.length - 1) {
            ArrayList<Integer> tmp = new ArrayList<>();
            for (int num : nums) {
                tmp.add(num);
            }
            res.add(tmp);
        } else {
            for (int i = k; i < nums.length; ++i) {
                swap(nums, i, k);
                backtrack(nums, k + 1);
                swap(nums, i, k);
            }
        }
    }

    public void swap(int[] nums, int a, int b) {
        int tmp = nums[a];
        nums[a] = nums[b];
        nums[b] = tmp;
    }
}
```



## 22.旋转图像

**题目：**

> 
> 给定一个 *n* × *n* 的二维矩阵表示一个图像。
>
> 将图像顺时针旋转 90 度。
>
> **说明：**
>
> 你必须在**[原地](https://baike.baidu.com/item/原地算法)**旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要**使用另一个矩阵来旋转图像。
>
> **示例 1:**
>
> ```
> 给定 matrix = 
> [
>   [1,2,3],
>   [4,5,6],
>   [7,8,9]
> ],
> 
> 原地旋转输入矩阵，使其变为:
> [
>   [7,4,1],
>   [8,5,2],
>   [9,6,3]
> ]
> ```

**思路：**

**代码：**

``` java
class Solution {
    /*
    解题思路：四指针
    分别定义上边界指针top,有边界指针right,下边界指针bottom，左边界指针left
    在左指针小于右指针，或者上指针小于下指针时循环：
        1.先将头指针所指示的一行单独拿出来到in[] tmp中
        2.依次从左指针、下指针、右指针顺时针将数据填入对应位置
        3.将tmp填到右指针指示的位置中，并将所有指针向内缩减
    */
    public void rotate(int[][] matrix) {
        int top = 0, right = matrix[0].length - 1, bottom = matrix.length - 1, left = 0;
        while (top < bottom || bottom - top == 0) {
            int[] tmp = new int[right - left + 1];
            //1.将top存入tmp
            for (int i = 0; i < tmp.length; ++i) {
                tmp[i] = matrix[top][left + i];
            }
            //2.依次旋转left,bottom,right
            for (int i = 0; i < tmp.length; ++i) {
                matrix[top][left + i] = matrix[bottom - i][left];
            }
            for (int i = 0; i < tmp.length; ++i) {
                matrix[top + i][left] = matrix[bottom][left + i];
            }
            for (int i = 0; i < tmp.length; ++i) {
                matrix[bottom][left + i] = matrix[bottom - i][right];
            }
            //3.填tmp,缩指针
            for (int i = 0; i < tmp.length; ++i) {
                matrix[top + i][right] = tmp[i];
            }
            ++top;--right;--bottom;++left;
        }
    }
} 
```

``` java
class Solution {
    /*
    解题思路：先沿着右上-坐下对角线翻转，再沿水平中线上下翻转，可以实现顺时针90度旋转的效果
    */
    public void rotate(int[][] matrix) {
        int len = matrix.length;
        for (int i = 0; i < len - 1; ++i) {
            for (int j = 0; j < len - i; ++j) {
                int tmp = matrix[i][j];
                matrix[i][j] = matrix[len - 1 - j][len - 1 - i];
                matrix[len - 1 - j][len - 1- i] = tmp;
            }
        }
        for (int i = 0; i  < (len >> 1); ++i) {
            for (int j = 0; j < len; ++j) {
                int tmp = matrix[i][j];
                matrix[i][j] = matrix[len - 1 - i][j];
                matrix[len - 1 - i][j] = tmp;
            }
        }
    }
}
```

``` java
class Solution {
    /*
    解题思路：与思路一相似，但是不是按行交换，而是逐个交换
    */
    public void rotate(int[][] matrix) {
        //外层循环控制每一圈，x,y为边界，每交换完一圈就像中心缩减
        for (int x = 0, y = matrix.length - 1; x < y; ++x, --y) {
            //内层循环进行不同边的节点交换，s,e为移动指针,s递增，e递减
            for (int s = x, e = y; s < y; s++, e--) {
                int tmp = matrix[x][s];
                matrix[x][s] = matrix[e][x];
                matrix[e][x] = matrix[y][e];
                matrix[y][e] = matrix[s][y];
                matrix[s][y] = tmp;
            }
        }
    }
}
```



## 23.字母异味词分组

**题目：**

> 给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。
>
> **示例:**
>
> ```
> 输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
> 输出:
> [
>   ["ate","eat","tea"],
>   ["nat","tan"],
>   ["bat"]
> ]
> ```
>
> **说明：**
>
> - 所有输入均为小写字母。
> - 不考虑答案输出的顺序。

**思路：**

**代码：**

``` java
class Solution {
    /*
    解题思路：哈希表以Ascii码排序的字符串为key,List<String>为value
    1.初始化：一个上述的哈希表map，一个结果集合res
    2.遍历strs字符数组：
        1.使用char[] tmp保存每个str,并进行升序排序
        2.判断map是否存在以tmp构建的字符串s
            1.若不存在：新建一个List并将s存入，再将s与该List存入map
            2.若存在：直接将该原字符串存入对应的List中
    3.直接返回res即可
    时间复杂度：O(M*N(logN))其中M为strs长度，N为字符串长度
    */
    public List<List<String>> groupAnagrams(String[] strs) {
        HashMap<String, List<String>> map = new HashMap<>();
        List<List<String>> res = new ArrayList<>();
        for (String str : strs) {
            char[] tmp = str.toCharArray();
            Arrays.sort(tmp);
            if (map.containsKey(String.valueOf(tmp))) {
                map.get(String.valueOf(tmp)).add(str);
            } else {
                List<String> list = new ArrayList<>(){{
                    add(str);
                }};
                map.put(String.valueOf(tmp), list);
                res.add(list);
            }
        }
        return res;
    }
}
```



## 24.最长子序和

**题目：**

> 给定一个整数数组 `nums` ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
>
> **示例:**
>
> ```
> 输入: [-2,1,-3,4,-1,2,1,-5,4]
> 输出: 6
> 解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
> ```
>
> **进阶:**
>
> 如果你已经实现复杂度为 O(*n*) 的解法，尝试使用更为精妙的分治法求解。

**思路：**

[最大子序和 - 最大子序和 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/maximum-subarray/solution/zui-da-zi-xu-he-by-leetcode-solution/)

**代码：**

``` java
class Solution {
    /*
    解题思路：动态规划
    设dp[i]为以nums[i]结尾的最大序和(必须以nums[i]结尾以保证连续性)，则dp[i] = max(dp[i - 1] + nums[i], dp[i])
    */
    public int maxSubArray(int[] nums) {
        if (nums.length < 1)
            return 0;
        int max = nums[0], last = max;
        for (int i = 1; i < nums.length; ++i) {
            int tmp = Math.max(last + nums[i], nums[i]);
            max = Math.max(tmp, max);
            last = tmp;
        }
        return max;
    }
}
```

``` java
class Solution {
    /*
    解题思路：分治法
    如果我们把 [0, n - 1][0,n−1] 分治下去出现的所有子区间的信息都用堆式存储的方式记忆化下来，即建成一颗真正的树之后，
    我们就可以在 O(log n)O(logn) 的时间内求到任意区间内的答案
    */
    public class Status {
        public int lSum, rSum, mSum, iSum;
        public Status (int lSum, int rSum, int mSum, int iSum) {
            this.lSum = lSum;//表示[l,r]内以l为左端点的最大字段和
            this.rSum = rSum;//表示[l,r]内以r为右端点的最大字段和
            this.mSum = mSum;//表示[l,r]内的最大字段和
            this.iSum = iSum;//表示[l,r]的总区间和
        }
    }

    public int maxSubArray(int[] nums) {
        if (nums.length < 1)
            return 0;
        return getInfo(nums, 0, nums.length - 1).mSum;
    }

    public Status getInfo(int[] nums, int l, int r) {
        if (l == r)
            return new Status(nums[l], nums[l], nums[l], nums[l]);
        int mid = (l + r) >> 1;
        Status lSub = getInfo(nums, l, mid);
        Status rSub = getInfo(nums, mid + 1, r);
        return pushUp(lSub, rSub);
    }

    public Status pushUp(Status l, Status r) {
        int iSum = l.iSum + r.iSum;
        int lSum = Math.max(l.lSum, l.iSum + r.lSum);
        int rSum = Math.max(r.rSum, r.iSum + l.rSum);
        int mSum = Math.max(Math.max(l.mSum, r.mSum), l.rSum + r.lSum);
        return new Status(lSum, rSum, mSum, iSum);
    }
}
```



## 25.跳跃游戏

**题目：**

> 给定一个非负整数数组，你最初位于数组的第一个位置。
>
> 数组中的每个元素代表你在该位置可以跳跃的最大长度。
>
> 判断你是否能够到达最后一个位置。
>
> **示例 1:**
>
> ```
> 输入: [2,3,1,1,4]
> 输出: true
> 解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。
> ```
>
> **示例 2:**
>
> ```
> 输入: [3,2,1,0,4]
> 输出: false
> 解释: 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。
> ```

**思路：**

**代码：**

```java
class Solution {
    public boolean canJump(int[] nums) {
        if (nums.length <= 1)
            return true;
        if (nums[0] == 0)
            return false;
        //表示到达该节点后最高还剩下能走的步数
        int max = nums[0]; 
        //最后一个节点不需要判断，只需要到达即可
        for (int i = 1; i < nums.length - 1; ++i) {
            max = Math.max(max - 1, nums[i - 1] - 1);
            //若到达步长为0的节点，并且之前的节点也只能刚好到0则不可能向前推进，直接返回false
            if (nums[i] == 0 && max == 0)
                return false;
        }
        return true;
    }
}
```



## 26.[合并区间](https://leetcode-cn.com/problems/merge-intervals/)

**题目：**

> 
> 给出一个区间的集合，请合并所有重叠的区间。
>
>  
>
> **示例 1:**
>
> ```
> 输入: intervals = [[1,3],[2,6],[8,10],[15,18]]
> 输出: [[1,6],[8,10],[15,18]]
> 解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
> ```
>
> **示例 2:**
>
> ```
> 输入: intervals = [[1,4],[4,5]]
> 输出: [[1,5]]
> 解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。
> ```
>
> **注意：**输入类型已于2019年4月15日更改。 请重置默认代码定义以获取新方法签名。
>
>  
>
> **提示：**
>
> - `intervals[i][0] <= intervals[i][1]`

**思路：**

[吃🐳！🤷‍♀️竟然一眼秒懂合并区间！ - 合并区间 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/merge-intervals/solution/chi-jing-ran-yi-yan-miao-dong-by-sweetiee/)

**代码：**

```java
class Solution {
    /*
    解题思路：
    先根据区间的起始位置排序，再进行n-1次两两合并
    */
    public int[][] merge(int[][] intervals) {
        //按照区间起始位置排序
        Arrays.sort(intervals, (v1, v2) -> v1[0] - v2[0]);
        int[][] res = new int[intervals.length][2];
        int i = 0;
        for (int[] arr : intervals) {
            //第一个区间或前一个区间的最大值小于当前区间的最小值则直接加入
            if (i == 0 || res[i - 1][1] < arr[0]) {
                res[i++] = arr;
            } else {
                res[i - 1][1] = Math.max(arr[1], res[i - 1][1]);
            }
        }
        return Arrays.copyOf(res, i);
    }
}
```



## 27.不同路径

**题目**：

> 一个机器人位于一个 *m x n* 网格的左上角 （起始点在下图中标记为“Start” ）。
>
> 机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。
>
> 问总共有多少条不同的路径？
>
> ![img](E:/program files/Topora images/robot_maze.png)
>
> 例如，上图是一个7 x 3 的网格。有多少可能的路径？
>
>  
>
> **示例 1:**
>
> ```
> 输入: m = 3, n = 2
> 输出: 3
> 解释:
> 从左上角开始，总共有 3 条路径可以到达右下角。
> 1. 向右 -> 向右 -> 向下
> 2. 向右 -> 向下 -> 向右
> 3. 向下 -> 向右 -> 向右
> ```
>
> **示例 2:**
>
> ```
> 输入: m = 7, n = 3
> 输出: 28
> ```
>
>  
>
> **提示：**
>
> - `1 <= m, n <= 100`
> - 题目数据保证答案小于等于 `2 * 10 ^ 9`

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：动态规划
    设dp[i][j]表示到达坐标为(j,i)的节点的总路径
    由于，机器人只能向下或者右移动一格，所以为了到达该点，机器人只可能来自(j - 1, i)获取(j, i - 1)两点
    则dp[i][j] = dp[i - 1][j] + dp[j - 1][i]
    并且由于机器人的路径只与上一层数据有关，所以可以使用两个数组来完成动规
    */
    public int uniquePaths(int m, int n) {
        int[] last = new int[m];
        int[] cur = new int[m];
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if (i == 0 || j == 0)
                    cur[j] = 1;
                else {
                    cur[j] = cur[j - 1] + last[j];
                }
            }
            int[] tmp = cur;
            cur = last;
            last = tmp;
        }
        return last[m - 1];
    }
}
```

## 28.最小路径和

**题目：**

> 给定一个包含非负整数的 `*m* x *n*` 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。
>
> **说明：**每次只能向下或者向右移动一步。
>
>  
>
> **示例 1：**
>
> ![img](https://assets.leetcode.com/uploads/2020/11/05/minpath.jpg)
>
> ```
> 输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
> 输出：7
> 解释：因为路径 1→3→1→1→1 的总和最小。
> ```
>
> **示例 2：**
>
> ```
> 输入：grid = [[1,2,3],[4,5,6]]
> 输出：12
> ```
>
>  
>
> **提示：**
>
> - `m == grid.length`
> - `n == grid[i].length`
> - `1 <= m, n <= 200`
> - `0 <= grid[i][j] <= 100`

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：动态规划
    设dp[i][j]表示到达第i行第j列的最小路径，
    由于只能通过dp[i - 1][j]与dp[i][j - 1]到达dp[i] += min(dp[i -1][j], dp[i][j -1])
    最后返回dp[n - 1][m - 1]即可,本题可以直接使用grid作为动规矩阵
    */
    public int minPathSum(int[][] grid) {
        for (int i = 0; i < grid.length; ++i) {
            for (int j = 0; j < grid[0].length; ++j) {
                if (i == 0 && j == 0) {
                    continue;
                } else if (i == 0) {
                    grid[i][j] += grid[i][j - 1];
                } else if (j == 0) {
                    grid[i][j] += grid[i - 1][j];
                } else {
                    grid[i][j] += Math.min(grid[i - 1][j], grid[i][j - 1]);
                }
            }
        }
        return grid[grid.length -1][grid[0].length - 1];
    }
}
```

## 29.爬楼梯

**题目：**

> 假设你正在爬楼梯。需要 *n* 阶你才能到达楼顶。
>
> 每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
>
> **注意：**给定 *n* 是一个正整数。
>
> **示例 1：**
>
> ```
> 输入： 2
> 输出： 2
> 解释： 有两种方法可以爬到楼顶。
> 1.  1 阶 + 1 阶
> 2.  2 阶
> ```
>
> **示例 2：**
>
> ```
> 输入： 3
> 输出： 3
> 解释： 有三种方法可以爬到楼顶。
> 1.  1 阶 + 1 阶 + 1 阶
> 2.  1 阶 + 2 阶
> 3.  2 阶 + 1 阶
> ```

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：动态规划
    设dp[i]表示爬到第i阶台阶的方法数量,因为每次可以爬1或者2个台阶
    可以直到爬到第i阶台阶是由i - 1爬1个台阶，i - 2爬2个台阶而来的
    所以dp[i] = dp[i - 1] + dp[i -2]
    i应该从1开始因为没有0个台阶，初始化：
    dp[1] = 1，dp[2] = 2，表示1阶台阶只有一种跳法，2阶台阶有2种跳法
    */
    public int climbStairs(int n) {
        if (n <= 2)
            return n;
        int minusOne = 2;
        int minusTwo = 1;
        for (int i = 3; i <= n; ++i) {
            int tmp = minusOne + minusTwo;
            minusTwo = minusOne;
            minusOne = tmp;
        }
        return minusOne;
    }
}
```



## 30.编辑距离

**题目：**

> 给你两个单词 `word1` 和 `word2`，请你计算出将 `word1` 转换成 `word2` 所使用的最少操作数 。
>
> 你可以对一个单词进行如下三种操作：
>
> - 插入一个字符
> - 删除一个字符
> - 替换一个字符
>
>  
>
> **示例 1：**
>
> ```
> 输入：word1 = "horse", word2 = "ros"
> 输出：3
> 解释：
> horse -> rorse (将 'h' 替换为 'r')
> rorse -> rose (删除 'r')
> rose -> ros (删除 'e')
> ```
>
> **示例 2：**
>
> ```
> 输入：word1 = "intention", word2 = "execution"
> 输出：5
> 解释：
> intention -> inention (删除 't')
> inention -> enention (将 'i' 替换为 'e')
> enention -> exention (将 'n' 替换为 'x')
> exention -> exection (将 'n' 替换为 'c')
> exection -> execution (插入 'u')
> ```
>
>  
>
> **提示：**
>
> - `0 <= word1.length, word2.length <= 500`
> - `word1` 和 `word2` 由小写英文字母组成

**思路：**

[【编辑距离】入门动态规划，你定义的 dp 里到底存了啥 - 编辑距离 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/edit-distance/solution/edit-distance-by-ikaruga/)

**代码：**

```java
class Solution { 
    /*
    解题思路：动态规划
    dp[i][j]表示word1的前i个字符转换成word2的前j个字符的最少操作数
    删除操作：即word1的前一个字符就已经可以与word2的前j个字符匹配，直接删除当前字符即可dp[i][j]=dp[i-1][j] + 1
    增加操作：即从word2的前j-1个字符的基础上直接加上wrod2[j]即可，dp[i][j] = dp[i][j - 1] + 1
    变换操作：直接将word1[i]变为wrod2[j]，即dp[i][j] = dp[i - 1][j - 1] + 1
    若word1[i] = word2[j]则不需要变换：dp[i][j] = dp[i - 1][j - 1]
    初始化：
    word1为空，word2也为空时，不需要操作，即dp[0][0] = 0
    当word1为空，word2不为时，每一步都是增加操作，即dp[0][j] = dp[0][j - 1] + 1
    当word1不为空，word2为空时，每一步都是删除操作，即dp[i][0] = dp[i - 1][0] + 1
    */
    public int minDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        //初始化
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 1; i <= n; ++i) {
            dp[0][i] = dp[0][i - 1] + 1;
        }
        for (int i = 1; i <= m; ++i) {
            dp[i][0] = dp[i - 1][0] + 1;
        }
        //遍历
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i -1][j - 1];
                } else {
                    dp[i][j] = Math.min(Math.min(dp[i - 1][j], dp[i][j - 1]), dp[i - 1][j - 1]) + 1;
                }
            }
        }
        return dp[m][n];
    }
}
```



## 31.颜色分类

**题目：**

> 给定一个包含红色、白色和蓝色，一共 *n* 个元素的数组，**[原地](https://baike.baidu.com/item/原地算法)**对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。
>
> 此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。
>
> **注意:**
> 不能使用代码库中的排序函数来解决这道题。
>
> **示例:**
>
> ```
> 输入: [2,0,2,1,1,0]
> 输出: [0,0,1,1,2,2]
> ```
>
> **进阶：**
>
> - 一个直观的解决方案是使用计数排序的两趟扫描算法。
>   首先，迭代计算出0、1 和 2 元素的个数，然后按照0、1、2的排序，重写当前数组。
> - 你能想出一个仅使用常数空间的一趟扫描算法吗？

**思路：**

**代码：**

``` java
class Solution {
    /*
    解题思路：双指针，交换
    定义两个指针p0,p2分别指向下一个0，2应该存放的位置
    遍历nums数组，但当前i应该小于等于p2指针,防止重复计算：
        1.若遇到0则交换当前元素和p0的值，并使p0向内收缩
        2.若遇到2则交换当前元素和p2的值，并使p2向内收缩，但由于p2指针交换过来的值未知，所以需要把遍历数组的指针i向后退一格继续判断一次
    */
    public void sortColors(int[] nums) {
        if (nums.length <= 1)
            return;
        int p0 = 0, p2 = nums.length - 1;
        for (int i = 0; i <= p2; ++i) {
            if (nums[i] == 0) {
                nums[i] = nums[p0];
                nums[p0++] = 0;
            } else if (nums[i] == 2) {
                nums[i--] = nums[p2];//对于p2指针交换过来的值还需要判断一次
                nums[p2--] = 2;
            }
        }
    }
}
```



## 32.最小覆盖子串

**题目：**

> 给你一个字符串 `s` 、一个字符串 `t` 。返回 `s` 中涵盖 `t` 所有字符的最小子串。如果 `s` 中不存在涵盖 `t` 所有字符的子串，则返回空字符串 `""` 。
>
> **注意：**如果 `s` 中存在这样的子串，我们保证它是唯一的答案。
>
>  
>
> **示例 1：**
>
> ```
> 输入：s = "ADOBECODEBANC", t = "ABC"
> 输出："BANC"
> ```
>
> **示例 2：**
>
> ```
> 输入：s = "a", t = "a"
> 输出："a"
> ```
>
>  
>
> **提示：**
>
> - `1 <= s.length, t.length <= 105`
> - `s` 和 `t` 由英文字母组成
>
>  
>
> **进阶：**你能设计一个在 `o(n)` 时间内解决此问题的算法吗？

**思路：**

[简简单单，非常容易理解的滑动窗口思想 - 最小覆盖子串 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/minimum-window-substring/solution/tong-su-qie-xiang-xi-de-miao-shu-hua-dong-chuang-k/)

**代码：**

```java
class Solution {
    /*
    解题思路：滑动窗口
    1.初始化：定义两个指针i,j是滑动窗口的左右边界，初始指向s的下标为0的位置，字典need为需求哈希表，记录着t字符串中每个
    字符需求的个数，countNeed为需求总数初始为t的长度用于计算还需要多少个元素子串才包含所有t中的字符，res保存最终结果
    2.当j小于s.length的时候持续遍历字符串s：
        1.右移动指针j使need中所有字符的计数都<=0（因为可能会重复），并同时维护countNeed（但它不能为负数）
        2.若此时窗口中已经囊括了所有需要的字符了：
            1.右移动指针i，通过排除不必要的元素来逐渐缩短滑动窗口，并在遍历到一个关键元素，即map(char) == 0的时候停下
            2.比较此时窗口大小j - i + 1与保存的最小字符串相比较，并同时更新res
            3.接着右移i，通过排除这个关键元素来寻找下一个子串，并同时跟新need与countNeed
        3.继续右移动i开启下一轮循环
    */
    public String minWindow(String s, String t) {
        int l = 0, r = 0, minLen = Integer.MAX_VALUE, countNeed = t.length();
        String res = "";
        //初始化字典
        int[] need = new int[128];
        for (char c : t.toCharArray()) {
            need[c]++;
        }
        while (r < s.length()) {
            char c = s.charAt(r);
            //是字典中需要的数
            if (need[c] > 0) {
                --countNeed;
            }
            --need[c];
            //满足要求了，需要右移动l指针
            if (countNeed == 0) {
                while (true) {
                    char ch = s.charAt(l);
                    //遇到关键字符退出
                    if (need[ch] == 0)
                        break;
                    ++need[ch];
                    ++l;
                }
                if (r - l + 1 < minLen) {
                    minLen = r - l + 1;
                    res = s.substring(l, r + 1);
                }
                ++countNeed;
                ++need[s.charAt(l)];
                ++l;
            }
            ++r;
        }
        return res;
    }
}
```



## 33.子集

**题目：**

> 给定一组**不含重复元素**的整数数组 *nums*，返回该数组所有可能的子集（幂集）。
>
> **说明：**解集不能包含重复的子集。
>
> **示例:**
>
> ```
> 输入: nums = [1,2,3]
> 输出:
> [
>   [3],
>   [1],
>   [2],
>   [1,2,3],
>   [1,3],
>   [2,3],
>   [1,2],
>   []
> ]
> ```

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：递归回溯
    */
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> tmp = new LinkedList<>();
    public List<List<Integer>> subsets(int[] nums) {
        res.add(new ArrayList(tmp));
        backtrack(nums, 0);
        return res;
    }

    public void backtrack(int[] nums, int k) {
        if (k == nums.length)
            return;
        else {
            for (int i = k; i < nums.length; ++i) {
                tmp.add(nums[i]);
                res.add(new ArrayList(tmp));
                backtrack(nums, i + 1);
                tmp.removeLast();
            }
        }
    }
}
```



## 34.单词搜索

**题目：**

> 
> 给定一个二维网格和一个单词，找出该单词是否存在于网格中。
>
> 单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。
>
>  
>
> **示例:**
>
> ```
> board =
> [
>   ['A','B','C','E'],
>   ['S','F','C','S'],
>   ['A','D','E','E']
> ]
> 
> 给定 word = "ABCCED", 返回 true
> 给定 word = "SEE", 返回 true
> 给定 word = "ABCB", 返回 false
> ```
>
>  
>
> **提示：**
>
> - `board` 和 `word` 中只包含大写和小写英文字母。
> - `1 <= board.length <= 200`
> - `1 <= board[i].length <= 200`
> - `1 <= word.length <= 10^3`

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：循环遍历+递归回溯
    */
    //四个方向
    int[][] direc = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
    boolean[][] visited;
    int rows;
    int cols;
    public boolean exist(char[][] board, String word) {
        if (word.length() < 1)
            return true;
        rows = board.length;
        cols = board[0].length;
        visited = new boolean[rows][cols];
        for (int i = 0 ; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                if (board[i][j] == word.charAt(0)) {
                    if (backtrack(board, word, 0, i, j)) {
                        return true;
                    }
                }
            }
        }
        return false;
    }

    public boolean backtrack(char[][] board, String word, int k, int row, int col) {
        if (k == word.length())
            return true;
        //该节点不能越界，不能被访问，需要与word[k]相等
        if (row < 0 || row >= rows || col < 0 || col >= cols || visited[row][col] || word.charAt(k) != board[row][col]) {
            return false;
        }
        visited[row][col] = true;
        //对四个方向遍历
        boolean res = backtrack(board, word, k + 1, row + 1, col + 0) || backtrack(board, word, k + 1, row + 0, col + 1) 
        || backtrack(board, word, k + 1, row - 1, col + 0) || backtrack(board, word, k + 1, row + 0, col + -1);
        
        visited[row][col] = false;
        return res;
    }
}
```



## 35.柱状图中最大的矩形

**题目：**

> 给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。
>
> 求在该柱状图中，能够勾勒出来的矩形的最大面积。
>
>  
>
> ![img](E:/program files/Topora images/histogram.png)
>
> 以上是柱状图的示例，其中每个柱子的宽度为 1，给定的高度为 `[2,1,5,6,2,3]`。
>
>  
>
> ![img](E:/program files/Topora images/histogram_area.png)
>
> 图中阴影部分为所能勾勒出的最大矩形面积，其面积为 `10` 个单位。
>
>  
>
> **示例:**
>
> ```
> 输入: [2,1,5,6,2,3]
> 输出: 10
> ```

**思路：**

[暴力解法、栈（单调栈、哨兵技巧） - 柱状图中最大的矩形 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/solution/bao-li-jie-fa-zhan-by-liweiwei1419/)

**代码：**

```java
class Solution {
    /*
    解题思路：单调栈
    使用一个单调栈stack用来保存柱子的下标，并且他是根据height[]递增的
    我们要如何确定每个柱形的高度呢？
    从题目给出的图中我们可以发现，当某个柱形被低于自己的柱子包围的时候(也就是形成个“凸”形)就可以确定这个柱形的高度了
    我们可以基于这个思想使用stack来实现
    
    值得注意的是如何计算当前弹出栈柱形i的宽度，我们在算法中在加入i的时候已经将stack里所有高度比它小的元素全部出栈了，
    所以可以确定的是，现在的栈顶元素才是第一个高度小于height[i]的，那么，宽度区间就应该是这个栈顶元素到当其i的开区间
    即width = stack.peekLast() - i - 1;

    特殊情况：
    1.弹栈的时候，栈为空
    2.遍历完成以后，栈中还有元素
    为此我们可以在输入数组两端加上两个高度为0的柱形
    */
    public int largestRectangleArea(int[] heights) {
        int len = heights.length;
        if (len < 1)
            return 0;
        int maxArea = 0;
        int[] newHeights = new int[len + 2];
        newHeights[0] = 0;
        System.arraycopy(heights, 0, newHeights, 1, len);
        newHeights[len + 1] = 0;
        heights = newHeights;

        ArrayDeque<Integer> stack = new ArrayDeque<>();
        stack.addLast(0);
        for (int i = 1; i < heights.length; ++i) {
            //弹出比当前柱子高的节点
            while (heights[i] < heights[stack.peekLast()]) {
                int curHeight = heights[stack.pollLast()];
                int curWidth = i - stack.peekLast() - 1;
                //计算面积
                maxArea = Math.max(maxArea, curHeight * curWidth);
            }
            stack.addLast(i);
        }
        return maxArea;
    }
}
```



## 36.最大矩形

**题目：**

> 给定一个仅包含 `0` 和 `1` 、大小为 `rows x cols` 的二维二进制矩阵，找出只包含 `1` 的最大矩形，并返回其面积。
>
>  
>
> **示例 1：**
>
> ![img](https://assets.leetcode.com/uploads/2020/09/14/maximal.jpg)
>
> ```
> 输入：matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
> 输出：6
> 解释：最大矩形如上图所示。
> ```
>
> **示例 2：**
>
> ```
> 输入：matrix = []
> 输出：0
> ```
>
> **示例 3：**
>
> ```
> 输入：matrix = [["0"]]
> 输出：0
> ```
>
> **示例 4：**
>
> ```
> 输入：matrix = [["1"]]
> 输出：1
> ```
>
> **示例 5：**
>
> ```
> 输入：matrix = [["0","0"]]
> 输出：0
> ```
>
>  
>
> **提示：**
>
> - `rows == matrix.length`
> - `cols == matrix.length`
> - `0 <= row, cols <= 200`
> - `matrix[i][j]` 为 `'0'` 或 `'1'`

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：与上题一样，单调栈
    只需要求每层的柱高度数组heights[]即可
    */
    public int maximalRectangle(char[][] matrix) {
        if (matrix.length == 0)
            return 0;
        int max = 0, rows = matrix.length, cols = matrix[0].length;
        int[] heights = new int[matrix[0].length];
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                if (matrix[i][j] == '1') {
                    ++heights[j];
                } else {
                    heights[j] = 0;
                }
            }
            max = Math.max(largestRectangleArea(heights), max);
        }
        return max;
    }
    
    public int largestRectangleArea(int[] heights) {
        int len = heights.length;
        if (len < 1)
            return 0;
        int maxArea = 0;
        int[] newHeights = new int[len + 2];
        newHeights[0] = 0;
        System.arraycopy(heights, 0, newHeights, 1, len);
        newHeights[len + 1] = 0;
        heights = newHeights;

        ArrayDeque<Integer> stack = new ArrayDeque<>();
        stack.addLast(0);
        for (int i = 1; i < heights.length; ++i) {
            //弹出比当前柱子高的节点
            while (heights[i] < heights[stack.peekLast()]) {
                int curHeight = heights[stack.pollLast()];
                int curWidth = i - stack.peekLast() - 1;
                //计算面积
                maxArea = Math.max(maxArea, curHeight * curWidth);
            }
            stack.addLast(i);
        }
        return maxArea;
    }
}
```



## 37.二叉树的中序遍历

**题目：**

> 给定一个二叉树的根节点 `root` ，返回它的 **中序** 遍历。
>
>  
>
> **示例 1：**
>
> ![img](E:/program files/Topora images/inorder_1.jpg)
>
> ```
> 输入：root = [1,null,2,3]
> 输出：[1,3,2]
> ```
>
> **示例 2：**
>
> ```
> 输入：root = []
> 输出：[]
> ```
>
> **示例 3：**
>
> ```
> 输入：root = [1]
> 输出：[1]
> ```
>
> **示例 4：**
>
> ![img](https://assets.leetcode.com/uploads/2020/09/15/inorder_5.jpg)
>
> ```
> 输入：root = [1,2]
> 输出：[2,1]
> ```
>
> **示例 5：**
>
> ![img](https://assets.leetcode.com/uploads/2020/09/15/inorder_4.jpg)
>
> ```
> 输入：root = [1,null,2]
> 输出：[1,2]
> ```
>
>  
>
> **提示：**
>
> - 树中节点数目在范围 `[0, 100]` 内
> - `-100 <= Node.val <= 100`

**思路：**

[二叉树的中序遍历 - 迭代法 - 二叉树的中序遍历 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/solution/die-dai-fa-by-jason-2/)

**代码：**

```java
class Solution {
    /*
    解题思路：
    没到一个节点A，因为根的访问在中间，将A入栈，遍历左子树，接着访问A，最后遍历右子树
    在访问A后，A就可以出栈了。因为A和其左子树都已经访问完成
    */
    public List<Integer> inorderTraversal(TreeNode root) {
        LinkedList<TreeNode> stack = new LinkedList<>();
        List<Integer> res = new LinkedList<>();
        TreeNode pNode = root;
        while (pNode != null || !stack.isEmpty()) {
            while (pNode != null) {
                stack.add(pNode);
                pNode = pNode.left;
            }
            pNode = stack.pollLast();
            res.add(pNode.val);
            pNode = pNode.right;
        }
        return res;
    }
}
```



## 38.不同的二叉搜索树

**题目：**

> 给定一个整数 *n*，求以 1 ... *n* 为节点组成的二叉搜索树有多少种？
>
> **示例:**
>
> ```
> 输入: 3
> 输出: 5
> 解释:
> 给定 n = 3, 一共有 5 种不同结构的二叉搜索树:
> 
>    1         3     3      2      1
>     \       /     /      / \      \
>      3     2     1      1   3      2
>     /     /       \                 \
>    2     1         2                 3
> ```

**思路：**

[「手画图解」详解 DP、记忆化递归 | 96. 不同的二叉搜索树 - 不同的二叉搜索树 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/unique-binary-search-trees/solution/shou-hua-tu-jie-san-chong-xie-fa-dp-di-gui-ji-yi-h/)

[画解算法：96. 不同的二叉搜索树 - 不同的二叉搜索树 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/unique-binary-search-trees/solution/hua-jie-suan-fa-96-bu-tong-de-er-cha-sou-suo-shu-b/)

**代码：**

```java
class Solution {
    /*
    解题思路：动态规划
    在整数1~n中选k作为根节点的值，则 1 ~ k - 1会去构建左子树，k + 1 ~ n会去构建右子树
    左子树出来的形态有a种，右子树出来的形态有b种，则整个数的形态有a * b种
        以k为根节点的二叉搜索树种类数 = 左子树BST种类数 * 右子树BST种类数
    
    */
    public int numTrees(int n) {
        int[] dp = new int[n + 1];
        //没有节点时只能形成空树
        dp[0] = 1;
        // dp[1] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j <= i - 1; ++j) {
                dp[i] += dp[j] * dp[i - j - 1];
            }
        }
        return dp[n];
    }
}
```



## 39.验证二叉搜索树

**题目：**

> 给定一个二叉树，判断其是否是一个有效的二叉搜索树。
>
> 假设一个二叉搜索树具有如下特征：
>
> - 节点的左子树只包含**小于**当前节点的数。
> - 节点的右子树只包含**大于**当前节点的数。
> - 所有左子树和右子树自身必须也是二叉搜索树。
>
> **示例 1:**
>
> ```
> 输入:
>     2
>    / \
>   1   3
> 输出: true
> ```
>
> **示例 2:**
>
> ```
> 输入:
>     5
>    / \
>   1   4
>      / \
>     3   6
> 输出: false
> 解释: 输入为: [5,1,4,null,null,3,6]。
>      根节点的值为 5 ，但是其右子节点值为 4 。
> ```

**思路：**

[三种解决方式，两种击败了100%的用户 - 验证二叉搜索树 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/validate-binary-search-tree/solution/san-chong-jie-jue-fang-shi-liang-chong-ji-bai-liao/)

**代码：**

```java
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
    /*
    解题思路：中序遍历+循环
    */
    public boolean isValidBST(TreeNode root) {
        LinkedList<TreeNode> stack = new LinkedList<>();
        long lastValue = Long.MIN_VALUE;
        TreeNode pNode = root;
        while (pNode != null || !stack.isEmpty()) {
            while (pNode != null) {
                //因为是中序遍历，需要先遍历左子树
                stack.addLast(pNode);
                pNode = pNode.left;
            }
            //节点左子树已经全部遍历完成，可以从stack中出栈了
            pNode = stack.pollLast();
            if (lastValue >= pNode.val)
                return false;
            lastValue = pNode.val;
            //对右子节点进行递归（确信）
            pNode = pNode.right;
        }
        return true;
    }
}
```

```java
class Solution {
    /*
    解题思路：中序遍历
    设当前访问的根节点root，则上一个遍历的节点肯定是左子树中最大的节点，若其仍然满足小于root.val则继续遍历右子树，
    保存当前root.val作为上一个节点的值，则开启递归后根据中序遍历规则，第一个访问的一定是右子树最小的值，若其大于roo.val
    则仍然符合，继续保存值，递归....
    */
    Integer lastValue = null;
    public boolean isValidBST(TreeNode root) {
        if (root == null)
…}
```

```java
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
…        if (root.val <= minVal || root.val >= maxVal) {
            return false;
        }
        return isBST(root.left, minVal, root.val) && isBST(root.right, root.val, maxVal);
    }
}
```



## 40.对称二叉树

**题目：**

> 给定一个二叉树，检查它是否是镜像对称的。
>
>  
>
> 例如，二叉树 `[1,2,2,3,4,4,3]` 是对称的。
>
> ```
>     1
>    / \
>   2   2
>  / \ / \
> 3  4 4  3
> ```
>
>  
>
> 但是下面这个 `[1,2,2,null,3,null,3]` 则不是镜像对称的:
>
> ```
>     1
>    / \
>   2   2
>    \   \
>    3    3
> ```
>
>  
>
> **进阶：**
>
> 你可以运用递归和迭代两种方法解决这个问题吗？

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：同时dfs，一边进行前序，一边进行中右左
    */
    public boolean isSymmetric(TreeNode root) {
        if (root == null)
            return true;
        return isSame(root.left, root.right);
    }

    public boolean isSame(TreeNode left, TreeNode right) {
        if (left == null && right == null)
            return true;
        if (left == null || right == null || left.val != right.val)
            return false;
        return isSame(left.left, right.right) && isSame(left.right, right.left);
    }
}
```



## 41.二叉树的层序遍历

**题目：**

> 给你一个二叉树，请你返回其按 **层序遍历** 得到的节点值。 （即逐层地，从左到右访问所有节点）。
>
>  
>
> **示例：**
> 二叉树：`[3,9,20,null,null,15,7]`,
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
> 返回其层次遍历结果：
>
> ```
> [
>   [3],
>   [9,20],
>   [15,7]
> ]
> ```

**思路：**

[BFS 的使用场景总结：层序遍历、最短路径问题 - 二叉树的层序遍历 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/bfs-de-shi-yong-chang-jing-zong-jie-ceng-xu-bian-l/)

**代码：**

```java
class Solution {
    /*
    解题思路：队列
    */
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null)
            return res;
        LinkedList<TreeNode> queue = new LinkedList<>();
        //初始化
        queue.add(root);
        while (!queue.isEmpty()) {
            ArrayList<Integer> tmp = new ArrayList<>();
            //保存本层需要取出的节点数
            int count = queue.size();
            for (int i = 0; i < count; ++i) {
                tmp.add(queue.peek().val);
                TreeNode pNode = queue.poll();
                if (pNode.left != null) {
                    queue.add(pNode.left);
                }
                if (pNode.right != null) {
                    queue.add(pNode.right);
                }
            }
            res.add(tmp);
        }
        return res;
    }
}
```

**拓展题目：地图分析**

**题目：**

> 你现在手里有一份大小为 N x N 的 网格 `grid`，上面的每个 单元格 都用 `0` 和 `1` 标记好了。其中 `0` 代表海洋，`1` 代表陆地，请你找出一个海洋单元格，这个海洋单元格到离它最近的陆地单元格的距离是最大的。
>
> 我们这里说的距离是「曼哈顿距离」（ Manhattan Distance）：`(x0, y0)` 和 `(x1, y1)` 这两个单元格之间的距离是 `|x0 - x1| + |y0 - y1|` 。
>
> 如果网格上只有陆地或者海洋，请返回 `-1`。
>
>  
>
> **示例 1：**
>
> **![img](E:/program files/Topora images/1336_ex1.jpeg)**
>
> ```
> 输入：[[1,0,1],[0,0,0],[1,0,1]]
> 输出：2
> 解释： 
> 海洋单元格 (1, 1) 和所有陆地单元格之间的距离都达到最大，最大距离为 2。
> ```
>
> **示例 2：**
>
> **![img](E:/program files/Topora images/1336_ex2.jpeg)**
>
> ```
> 输入：[[1,0,0],[0,0,0],[0,0,0]]
> 输出：4
> 解释： 
> 海洋单元格 (2, 2) 和所有陆地单元格之间的距离都达到最大，最大距离为 4。
> ```
>
>  
>
> **提示：**
>
> 1. `1 <= grid.length == grid[0].length <= 100`
> 2. `grid[i][j]` 不是 `0` 就是 `1`

**思路：**

[🌊简单Java, 秒懂图的BFS～ - 地图分析 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/as-far-from-land-as-possible/solution/jian-dan-java-miao-dong-tu-de-bfs-by-sweetiee/)

**代码：**

```java
class Solution {
    /*
    解题思路：多源BFS
    我们只要先把所有的陆地都入队，然后从各个陆地同时开始一层一层的向海洋扩散，那么最后扩散到的海洋就是最远的海洋
    */
    public class Coor {
        public int row;
        public int col;
        public Coor (int row, int col) {
            this.row = row;
            this.col = col;
        }
    }
    public int maxDistance(int[][] grid) {
        LinkedList<Coor> queue = new LinkedList<>();
        int[][] direc = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        int rows = grid.length;
        int cols = grid[0].length;
        //将所有源点加入起始队列，多源同时遍历，防止某个节点多次计算
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                if (grid[i][j] == 1) {
                    queue.add(new Coor(i, j));
                }
            }
        }
        //如果只有陆地直接返回-1
        if (queue.size() == rows * cols)
            return -1;


        int dis = -1;
        while (!queue.isEmpty()) {
            ++dis;
            int count = queue.size();
            for (int k = 0; k < count; ++k) {
                Coor coor = queue.poll();
                for (int[] move : direc) {
                    int r = coor.row + move[0];
                    int c = coor.col + move[1];
                    if (r >= 0 && r < rows && c >= 0 && c < cols && grid[r][c] == 0) {
                        grid[r][c] = 2;
                        queue.add(new Coor(r, c));
                    }
                }
            }
        }

        return dis;
    }
}
```



## 42.二叉树的最大深度

**题目：**

> 给定一个二叉树，找出其最大深度。
>
> 二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。
>
> **说明:** 叶子节点是指没有子节点的节点。
>
> **示例：**
> 给定二叉树 `[3,9,20,null,null,15,7]`，
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
> 返回它的最大深度 3 。

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：DFS
    */
    public int maxDepth(TreeNode root) {
        return root == null ? 0 : Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
}
```

```java
class Solution {
    /*
    解题思路：BFS
    */
    public int maxDepth(TreeNode root) {
        if (root == null)
            return 0;
        int maxDepth = 0;
        LinkedList<TreeNode> queue = new LinkedList<>();
        queue.add(root);
…        }
        return maxDepth;
    }
}
```



## 43.从前序与中序遍历序列构造二叉树

**题目：**

> 根据一棵树的前序遍历与中序遍历构造二叉树。
>
> 注意:
> 你可以假设树中没有重复的元素。
>
> 例如，给出
>
> 前序遍历 preorder = [3,9,20,15,7]
> 中序遍历 inorder = [9,3,15,20,7]
> 返回如下的二叉树：
>
>     3
>    / \
>   9  20
>     /  \
>    15   7

**思路：**

**代码：**

```java
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
    /*
    解题思路：递归构建
    设前序序列的范围是[p_left, p_right],中序序列的范围是[i_left, i_right]
    则可以直到本次需要构建的节点是new TreeNode(preorder[p_left])，我们叫他pNode
    我们再在中序序列中找到与pNode值相等的元素，并记录它的下标假设为它为i
    于是就可以确定左子树的范围在inorder中是[i_left, i - 1]，距离为i - i_left
    右子树范围在inorder中是[i + 1, i_right],距离是i_right - i
    所以在前序序列中的左子树范围就是[p_left + 1, p_left + (i - i_left)]
    在前序序列中右子树的范围就是[p_left + (i - i_left) + 1, p_right]
    根据以上信息即可开启递归构建左右子树

    特例：
    当序列范围越界时表示没有节点可构建，直接返回null即可
    */
    int[] preorder;
    HashMap<Integer, Integer> map;
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        this.preorder = preorder;
        //使用哈希表减少每次在inorder中搜索元素的时间
        map = new HashMap<>();
        for (int i = 0; i < inorder.length; ++i) {
            map.put(inorder[i], i);
        }
        return build(0, preorder.length - 1, 0, inorder.length - 1);
    }

    public TreeNode build(int p_left, int p_right, int i_left, int i_right) {
        if (p_left > p_right)
            return null;
        TreeNode root = new TreeNode(preorder[p_left]);
        int i = map.get(root.val);
        root.left = build(p_left + 1, p_left + (i - i_left), i_left, i - 1);
        root.right = build(p_left + (i - i_left) + 1, p_right, i + 1, i_right);
        return root;
    }
}



```



## 44.二叉树展开为链表

**题目：**

> 给定一个二叉树，[原地](https://baike.baidu.com/item/原地算法/8010757)将它展开为一个单链表。
>
>  
>
> 例如，给定二叉树
>
> ```
>     1
>    / \
>   2   5
>  / \   \
> 3   4   6
> ```
>
> 将其展开为：
>
> ```
> 1
>  \
>   2
>    \
>     3
>      \
>       4
>        \
>         5
>          \
>           6
> ```

**思路：**

**代码：**

```java
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
    /*
    解题思路：后续遍历
    */
    TreeNode pre = null;
    public void flatten(TreeNode root) {
        if (root == null)
            return;
        flatten(root.right);
        flatten(root.left);
        root.right = pre;
        pre = root;
        root.left = null;
    }
}
```



## 45.[买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

**题目：**

> 给定一个数组，它的第 *i* 个元素是一支给定股票第 *i* 天的价格。
>
> 如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。
>
> 注意：你不能在买入股票前卖出股票。
>
>  
>
> **示例 1:**
>
> ```
> 输入: [7,1,5,3,6,4]
> 输出: 5
> 解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
>      注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
> ```
>
> **示例 2:**
>
> ```
> 输入: [7,6,4,3,1]
> 输出: 0
> 解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
> ```

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：单调栈+哨兵
    */
    public int maxProfit(int[] prices) {
        int max = 0;
        Deque<Integer> stack = new ArrayDeque<>();
        stack.add(Integer.MAX_VALUE);
        for (int price : prices) {
            if (price < stack.peekLast()) {
                stack.add(price);
            } else {
                max = Math.max(max, price - stack.peekLast());
            }
        }
        return max;
    }
}
```

```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices.length <= 1)
            return 0;
        int max = 0, lastMin = prices[0];
        for (int i = 1; i < prices.length; ++i) {
            max = Math.max(max, prices[i] - lastMin);
            lastMin = Math.min(lastMin, prices[i]);
        }
        return max;
    }
}
```



## 46.[二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

**题目：**

> 给定一个**非空**二叉树，返回其最大路径和。
>
> 本题中，路径被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。该路径**至少包含一个**节点，且不一定经过根节点。
>
>  
>
> **示例 1：**
>
> ```
> 输入：[1,2,3]
> 
>        1
>       / \
>      2   3
> 
> 输出：6
> ```
>
> **示例 2：**
>
> ```
> 输入：[-10,9,20,null,null,15,7]
> 
>    -10
>    / \
>   9  20
>     /  \
>    15   7
> 
> 输出：42
> ```

**思路：**

[【二叉树中的最大路径和】递归，条理清晰 - 二叉树中的最大路径和 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/solution/er-cha-shu-zhong-de-zui-da-lu-jing-he-by-ikaruga/)

**代码：**

```java
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
    /*
    解题思路：动规+后续遍历
    先计算当前节点cur的左子树与右子树的最大值，若其大于0则将其与cur.val相加并返回，否则直接返回cur.val
    有三种情况：
    1.左子树和 + root.val + 右子树和
    2.左子树和 + root.val + root的父节点和
    3.右子树和 + root.val + root的父节点和
    */
    int max;
    public int maxPathSum(TreeNode root) {
        max = root.val;
        dfs(root);
        return max;
    }

    public int dfs(TreeNode root) {
        if (root == null)
            return 0;
        int left = dfs(root.left);
        int right = dfs(root.right);
        //即第一种情况
        int curSUm = root.val + Math.max(0, left) + Math.max(0, right);
        //后两种情况
        int goOn = root.val + Math.max(0, Math.max(left, right));
        max = Math.max(curSUm, max);
        return goOn;
    }
}
```



## 47.[最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

**题目：**

> 给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。
>
>  
>
> **进阶：**你可以设计并实现时间复杂度为 `O(n)` 的解决方案吗？
>
>  
>
> **示例 1：**
>
> ```
> 输入：nums = [100,4,200,1,3,2]
> 输出：4
> 解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [0,3,7,2,5,8,4,6,0,1]
> 输出：9
> ```
>
>  
>
> **提示：**
>
> - `0 <= nums.length <= 104`
> - `-109 <= nums[i] <= 109`

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：动态规划 + 哈希表(key:i,value:len)
    遍历数组nums，对遍历到的每个元素nums[i]在map种寻找其左右元素nums[i] - 1与nums[] + 1
    若存在，则跟新它们两端数字的长度为len = map.get(nums[i] - 1) + map.get(nums[i] + 1)
    即更新nums[i] - map.get(nums[i] - 1) 与 nums[i] + map.get(nums[i] + 1)这两个端点
    因为只有序列的端点才与长度有关，序列中间元素皆可忽略
    */
    public int longestConsecutive(int[] nums) {
        HashMap<Integer, Integer> map = new HashMap<>();
        int max = 0;
        for (int num : nums) {
            if (map.containsKey(num))
                continue;
            int left = map.getOrDefault(num - 1, 0);
            int right = map.getOrDefault(num + 1, 0);
            int len = left + right + 1;
            map.put(num, len);
            map.put(num - left, len);
            map.put(num + right, len);
            max = Math.max(max, len);
        }
        return max;
    }
}
```

```java
class Solution {
    /*
    解题思路：排序后遍历数组
    */
    public int longestConsecutive(int[] nums) {
        if (nums.length < 1)
            return 0;
        Arrays.sort(nums);
        int maxLen = 1;
        int curLen = 1;
…        return maxLen;
    }
}
```



## 48.[只出现一次的数字](https://leetcode-cn.com/problems/single-number/)

**题目：**

> 给定一个**非空**整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
>
> **说明：**
>
> 你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？
>
> **示例 1:**
>
> ```
> 输入: [2,2,1]
> 输出: 1
> ```
>
> **示例 2:**
>
> ```
> 输入: [4,1,2,1,2]
> 输出: 4
> ```

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：异或的特性
    */
    public int singleNumber(int[] nums) {
        int res = 0;
        for (int num : nums) {
            res ^= num;
        }
        return res;
    }
}
```



## 49.[单词拆分](https://leetcode-cn.com/problems/word-break/)

**题目：**

> 给定一个**非空**字符串 *s* 和一个包含**非空**单词的列表 *wordDict*，判定 *s* 是否可以被空格拆分为一个或多个在字典中出现的单词。
>
> **说明：**
>
> - 拆分时可以重复使用字典中的单词。
> - 你可以假设字典中没有重复的单词。
>
> **示例 1：**
>
> ```
> 输入: s = "leetcode", wordDict = ["leet", "code"]
> 输出: true
> 解释: 返回 true 因为 "leetcode" 可以被拆分成 "leet code"。
> ```
>
> **示例 2：**
>
> ```
> 输入: s = "applepenapple", wordDict = ["apple", "pen"]
> 输出: true
> 解释: 返回 true 因为 "applepenapple" 可以被拆分成 "apple pen apple"。
>      注意你可以重复使用字典中的单词。
> ```
>
> **示例 3：**
>
> ```
> 输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
> 输出: false
> ```

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：记忆化回溯
    将回溯过程中遍历到的子串是否匹配保存到boolean数组中从而优化时间效率
    */

    HashSet<String> set;
    boolean[] isNotOk;
    public boolean wordBreak(String s, List<String> wordDict) {
        set = new HashSet<>(wordDict);
        isNotOk = new boolean[s.length()];
        return dfs(0, s);
    }

    public boolean dfs(int start, String str) {
        if (str.length() == 0)
            return true;
        if (isNotOk[start])
            return false;
        for (int i = 0; i < str.length(); ++i) {
            if (set.contains(str.substring(0, i + 1))) {
                if (dfs(start + i + 1, str.substring(i + 1)))
                    return true;
            }
        }
        isNotOk[start] = true;
        return false;
    }
}
```

```java
class Solution {
    /*
    解题思路：哈希表 + 动规
    */
    public boolean wordBreak(String s, List<String> wordDict) {
        HashMap<String, Boolean> dic = new HashMap<>();
        for (String str : wordDict) {
            dic.put(str, false);
        }
        int len = s.length();
        boolean[] dp = new boolean[len + 1];
        dp[0] = true;
        for (int i = 1; i <= len; ++i) {
            for (int j = i - 1; j >= 0; j--) {
                String sub = s.substring(j, i);
                if (dic.containsKey(sub) && dp[j]) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[len];
    }
}
```

## 50.[环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

**题目：**

> 给定一个链表，判断链表中是否有环。
>
> 如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 `pos` 是 `-1`，则在该链表中没有环。**注意：`pos` 不作为参数进行传递**，仅仅是为了标识链表的实际情况。
>
> 如果链表中存在环，则返回 `true` 。 否则，返回 `false` 。
>
>  
>
> **进阶：**
>
> 你能用 *O(1)*（即，常量）内存解决此问题吗？
>
>  
>
> **示例 1：**
>
> ![img](E:/program files/Topora images/circularlinkedlist.png)
>
> ```
> 输入：head = [3,2,0,-4], pos = 1
> 输出：true
> 解释：链表中有一个环，其尾部连接到第二个节点。
> ```
>
> **示例 2：**
>
> ![img](E:/program files/Topora images/circularlinkedlist_test2.png)
>
> ```
> 输入：head = [1,2], pos = 0
> 输出：true
> 解释：链表中有一个环，其尾部连接到第一个节点。
> ```
>
> **示例 3：**
>
> ![img](E:/program files/Topora images/circularlinkedlist_test3.png)
>
> ```
> 输入：head = [1], pos = -1
> 输出：false
> 解释：链表中没有环。
> ```
>
>  
>
> **提示：**
>
> - 链表中节点的数目范围是 `[0, 104]`
> - `-105 <= Node.val <= 105`
> - `pos` 为 `-1` 或者链表中的一个 **有效索引** 。

**思路：**

[9种解决方式，8种击败了100%的用户 - 环形链表 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/linked-list-cycle/solution/3chong-jie-jue-fang-shi-liang-chong-ji-bai-liao-10/)

**代码：**

```java
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
    /*
    解题思路：快慢指针
    */
    public boolean hasCycle(ListNode head) {
        ListNode pre = head, slow = head;
        while (pre != null) {
            pre = pre.next;
            if (pre != null)
                pre = pre.next;
            if (slow == pre)
                return true;
            slow = slow.next;
        }
        return false;
    }
}
```

```java
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
    /*
    解题思路：链表反转
    若链表存在环，那么反转之后的链表的头节点会是原来的节点（画个图就能理解）
    */
    public boolean hasCycle(ListNode head) {
        ListNode newHead =  reverseList(head);
        //有可能只有一个节点所以需要两个非空判断
        if (head != null && head.next != null && head == newHead) {
            return true;
        }
        return false;
    }

    public ListNode reverseList(ListNode head) {
        ListNode last = null;
        while (head != null) {
            ListNode next = head.next;
            head.next = last;
            last = head;
            head = next;
        }
        return last;
    }
}
```



## 52.[环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

**题目：**

> 给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 `null`。
>
> 为了表示给定链表中的环，我们使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 `pos` 是 `-1`，则在该链表中没有环。**注意，`pos` 仅仅是用于标识环的情况，并不会作为参数传递到函数中。**
>
> **说明：**不允许修改给定的链表。
>
> **进阶：**
>
> - 你是否可以使用 `O(1)` 空间解决此题？
>
>  
>
> **示例 1：**
>
> ![img](E:/program files/Topora images/circularlinkedlist.png)
>
> ```
> 输入：head = [3,2,0,-4], pos = 1
> 输出：返回索引为 1 的链表节点
> 解释：链表中有一个环，其尾部连接到第二个节点。
> ```
>
> **示例 2：**
>
> ![img](E:/program files/Topora images/circularlinkedlist_test2.png)
>
> ```
> 输入：head = [1,2], pos = 0
> 输出：返回索引为 0 的链表节点
> 解释：链表中有一个环，其尾部连接到第一个节点。
> ```
>
> **示例 3：**
>
> ![img](E:/program files/Topora images/circularlinkedlist_test3.png)
>
> ```
> 输入：head = [1], pos = -1
> 输出：返回 null
> 解释：链表中没有环。
> ```
>
>  
>
> **提示：**
>
> - 链表中节点的数目范围在范围 `[0, 104]` 内
> - `-105 <= Node.val <= 105`
> - `pos` 的值为 `-1` 或者链表中的一个有效索引

**思路：**

**代码：**

```java
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
    /*
    解题思路：双指针（优化）
    设链表共有a + b个节点，其中链表头部到链表入口共有a个节点，链表环共有b个节点，设两指针分别走了f,s步，则有：
    1.fast走的步数一定是slow的2倍，即 f = 2 * s (解析:fast每轮走2步，slow每轮走1步)
    2.fast 比 slow 多走了n个环的长度，即f = s + n * b;(解析:双指针都走过a步，然后在环内绕圈直到重合，重合时fast比slow多走环的长度整数倍)
    以上两式相减得 f = 2nb, s = nb,即fast和slow指针分别走了2n,n个环周长
    如果让指针从链表头部一直向前走并统计步数k，那么所有走到链表入口节点时得步数是：k = a + n * b (解析：先走a步到入口节点，然后没绕一圈都会再次到入口节点)
    而目前slow与fast已经走了整数倍b，因此我们只要想办法再让fast再走a步停下来即可找到环得入口
    此时问题已经转换成类似寻找倒数第k个节点的问题了，我们只需要再将slow指针指向头节点，然后slow与fast同时向前走直到碰撞为止
    */
    public ListNode detectCycle(ListNode head) {
        ListNode pre = head;
        ListNode slow = head;
        //先让两个指针重合
        while (true) {
            if (pre == null || pre.next == null)
                return null;
            pre = pre.next.next;
            slow = slow.next;
            if (pre == slow)
                break;
        }
        slow = head;
        while (slow != pre) {
            slow = slow.next;
            pre = pre.next;
        }
        return pre;
    }
}
```

```java
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
    /*
    解题思路：哈希表
    每遍历一个节点就存入map中，若遇到相同的节点则返回它
    否则返回null
    */ 
    public ListNode detectCycle(ListNode head) {
        HashSet<ListNode> dic = new HashSet<>();
        ListNode pNode = head;
        while (pNode != null) {
            if (dic.contains(pNode))
                return pNode;
            dic.add(pNode);
            pNode = pNode.next;
        }
        return null;
    }
}
```

```java
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
    /*
    解题思路：快慢指针
    1.利用快慢指针计算出环的长度m
    2.计算倒数第k个节点
    */
    public ListNode detectCycle(ListNode head) {
        ListNode pre = head;
        ListNode slow = head;
        //1.1先让两个指针重合
        while (pre != null) {
            pre = pre.next;
            if (pre != null)
                pre = pre.next;
            if (pre == slow)
                break;
            slow = slow.next;
        }
        if (pre == null)
            return null;
        //1.2再让两个指针走一遍，根据快指针步数-慢指针步数即可计算出m
        int preCount = 0;
        int slowCount = 0;
        while (true) {
            pre = pre.next.next;
            preCount += 2;
            if (pre == slow)
                break;
            slow = slow.next;
            slowCount += 1;
        }
        int m = preCount - slowCount;
        //2.1再让pre与slow回到头节点，使pre先走m - 1步
        pre = head;
        slow = head;
        for (int i = 0; i < m - 1; ++i) {
            pre = pre.next;
        }
        while (pre.next != slow) {
            pre = pre.next;
            slow = slow.next;
        }
        return slow;
    }
}



```



## 52.[LRU 缓存机制](https://leetcode-cn.com/problems/lru-cache/)

**题目：**

> 运用你所掌握的数据结构，设计和实现一个 [LRU (最近最少使用) 缓存机制](https://baike.baidu.com/item/LRU) 。
>
> 实现 `LRUCache` 类：
>
> - `LRUCache(int capacity)` 以正整数作为容量 `capacity` 初始化 LRU 缓存
> - `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `-1` 。
> - `void put(int key, int value)` 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字-值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。
>
>  
>
> **进阶**：你是否可以在 `O(1)` 时间复杂度内完成这两种操作？
>
>  
>
> **示例：**
>
> ```
> 输入
> ["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
> [[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
> 输出
> [null, null, null, 1, null, -1, null, -1, 3, 4]
> 
> 解释
> LRUCache lRUCache = new LRUCache(2);
> lRUCache.put(1, 1); // 缓存是 {1=1}
> lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
> lRUCache.get(1);    // 返回 1
> lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
> lRUCache.get(2);    // 返回 -1 (未找到)
> lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
> lRUCache.get(1);    // 返回 -1 (未找到)
> lRUCache.get(3);    // 返回 3
> lRUCache.get(4);    // 返回 4
> ```
>
>  
>
> **提示：**
>
> - `1 <= capacity <= 3000`
> - `0 <= key <= 3000`
> - `0 <= value <= 104`
> - 最多调用 `3 * 104` 次 `get` 和 `put`

**思路：**

[源于 LinkedHashMap源码 - LRU 缓存机制 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/lru-cache/solution/yuan-yu-linkedhashmapyuan-ma-by-jeromememory/)

[哈希表 + 双向链表（Java） - LRU 缓存机制 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/lru-cache/solution/ha-xi-biao-shuang-xiang-lian-biao-java-by-liweiw-2/)

**代码：**

```java
class LRUCache {
    /*
    解题思路：模拟LinkedHashMap
    */
    final int capacity;
    //定义头尾哨兵节点
    ListNode head;
    ListNode tail;
    HashMap<Integer, ListNode> map;
    int size = 0;
    //自定义双向链表
    class ListNode {
        public int key;
        public int value;
        public ListNode pre;
        public ListNode next;
        public ListNode (int key, int value) {
            this.key = key;
            this.value = value;
        }
    }

    public LRUCache(int capacity) {
        this.capacity = capacity;
        map = new HashMap<>();
        head = new ListNode(-1, -1);
        tail = new ListNode(-1, -1);
        this.head.next = this.tail;
        this.tail.pre = this.head;
    }
    
    public int get(int key) {
        if (map.containsKey(key)) {
            ListNode pNode = map.get(key);
            moveNode2Head(pNode);
            return pNode.value;
        }
        return -1;
    }
    
    public void put(int key, int value) {
        //若存在
        if (map.containsKey(key)) {
            ListNode pNode = map.get(key);
            pNode.value = value;
            //放到头部
            moveNode2Head(pNode);
        } else {
            if (size == capacity) {
                 map.remove(removeTail().key);
                 --size;
            }
            ++size;
            ListNode pNode = new ListNode(key, value);
            map.put(key, pNode);
            addNode2Head(pNode);
        }
    }

    public void moveNode2Head(ListNode pNode) {
        //1.先删除该节点
        pNode.pre.next = pNode.next;
        pNode.next.pre = pNode.pre;
        //2.将该节点插入头节点后面
        addNode2Head(pNode);
    }

    public ListNode removeTail() {
        ListNode pNode = tail.pre;
        pNode.pre.next = tail;
        tail.pre = pNode.pre;
        pNode.pre = null;
        pNode.next = null;
        return pNode;
    }

    public void addNode2Head(ListNode pNode) {
        //调整该节点的指针
        pNode.next = head.next;
        pNode.pre = head;
        //挑战head与head.next的指针
        head.next.pre = pNode;
        head.next = pNode;
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

```java
class LRUCache extends LinkedHashMap<Integer, Integer>{
    private int capacity;
    
    public LRUCache(int capacity) {
        super(capacity, 0.75F, true);
        this.capacity = capacity;
    }

    public int get(int key) {
        return super.getOrDefault(key, -1);
    }

    // 这个可不写
    public void put(int key, int value) {
        super.put(key, value);
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
        return size() > capacity; 
    }
}

```

## 53.[排序链表](https://leetcode-cn.com/problems/sort-list/)

**题目：**

> 给你链表的头结点 `head` ，请将其按 **升序** 排列并返回 **排序后的链表** 。
>
> **进阶：**
>
> - 你可以在 `O(n log n)` 时间复杂度和常数级空间复杂度下，对链表进行排序吗？
>
>  
>
> **示例 1：**
>
> ![img](E:/program files/Topora images/sort_list_1.jpg)
>
> ```
> 输入：head = [4,2,1,3]
> 输出：[1,2,3,4]
> ```
>
> **示例 2：**
>
> ![img](E:/program files/Topora images/sort_list_2.jpg)
>
> ```
> 输入：head = [-1,5,3,4,0]
> 输出：[-1,0,3,4,5]
> ```
>
> **示例 3：**
>
> ```
> 输入：head = []
> 输出：[]
> ```
>
>  
>
> **提示：**
>
> - 链表中节点的数目在范围 `[0, 5 * 104]` 内
> - `-105 <= Node.val <= 105`

**思路：**

https://leetcode-cn.com/problems/sort-list/solution/sort-list-gui-bing-pai-xu-lian-biao-by-jyd/

**代码：**

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    /*
    解题思路：归并 + 虚拟头指针
    问题；如何将链表一分为二
    解决方案：遍历一遍链表，建立对应的集合，就可以使用指针来访问每个节点了
    */
    List<ListNode> list;
    public ListNode sortList(ListNode head) {
        if (head == null)
            return null;
        list = new ArrayList<ListNode>();
        ListNode pNode = head;
        while (pNode != null) {
            list.add(pNode);
            pNode = pNode.next;
        }
        return divedeAndMerge(0, list.size() - 1);
    }

    public ListNode divedeAndMerge(int left, int right) {
        if (left == right)
            return list.get(left);
        int mid = (left + right) / 2;
        ListNode leftHead = divedeAndMerge(left, mid);
        ListNode rightHead = divedeAndMerge(mid + 1, right);

        ListNode head = new ListNode(-1);
        ListNode pNode = head;

        int i = left, j = mid + 1;
        while (i <= mid || j <= right) {
            if (i == mid + 1) {
                pNode.next = rightHead;
                rightHead = rightHead.next;
                ++j;
            } else if (j == right + 1) {
                pNode.next = leftHead;
                leftHead = leftHead.next;
                ++i;
            } else if (leftHead.val < rightHead.val) {
                pNode.next = leftHead;
                leftHead = leftHead.next;
                ++i;
            } else {
                pNode.next = rightHead;
                rightHead = rightHead.next;
                ++j;
            }
            pNode = pNode.next;
        }
        pNode.next = null;
        return head.next;
    }
}
```



```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    /*
    解题思路：快慢指针分割 + 归并
    */
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null)
            return head;
        ListNode fast = head.next, slow = head;
        //使用快慢指针分割链表，当链表个数为奇数时，slow指向中间节点，为偶数则找到中心左侧的节点
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        ListNode tmp = slow.next;
        //断开链表防止成环
        slow.next = null;
        ListNode left = sortList(head);
        ListNode right = sortList(tmp);
        ListNode newHead = new ListNode(-1);
        ListNode pNode = newHead;
        while (left != null && right != null) {
            if (left.val < right.val) {
                pNode.next = left;
                left = left.next;
            } else {
                pNode.next = right;
                right = right.next;
            }
            pNode = pNode.next;
        }
        pNode.next = left == null ? right : left;
        return newHead.next;
    }
}
```



## 54.[乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/)

**题目：**

> 
> 给你一个整数数组 `nums` ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。
>
>  
>
> **示例 1:**
>
> ```
> 输入: [2,3,-2,4]
> 输出: 6
> 解释: 子数组 [2,3] 有最大乘积 6。
> ```
>
> **示例 2:**
>
> ```
> 输入: [-2,0,-1]
> 输出: 0
> 解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。
> ```

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：动态规划
    令imax为当前最大值 imax = max (imax * nums[i], nums[i])
    由于存在负数，可能会导致最大变为最小的，最小变为最大的。因此还需要维护当前最小值imin = min (imin * nums[i], nums[i])
    当前元素是负数时，将imax 与 imin进行交换再计算
    */
    public int maxProduct(int[] nums) {
        int len = nums.length;
        if (len == 0)
            return 0;
        int max = nums[0], imax = 1, imin = 1;
        for (int num : nums) {
            if (num < 0) {
                int tmp = imax;
                imax = imin;
                imin = tmp;
            }
            imax = Math.max(imax * num, num);
            imin = Math.min(imin * num, num);
            max = Math.max(max, imax);
        }
        return max;
    }
}
```

```java
class Solution {
    /*
    解题思路：暴力解决
    */
    public int maxProduct(int[] nums) {
        int len = nums.length;
        if (len <= 0)
            return 0;
        int max = nums[0];
        for (int i = 0; i < len; ++i) {
            int dp[] = new int[len];
            for (int j = i; j < len; ++j) {
                if (j == i)
                    dp[j] = nums[i];
                else
                    dp[j] = dp[j - 1] * nums[j];
                max = Math.max(max, dp[j]);
            }
        }
        return max;
    }
}
```



## 55.[最小栈](https://leetcode-cn.com/problems/min-stack/)

**题目：**

> 设计一个支持 `push` ，`pop` ，`top` 操作，并能在常数时间内检索到最小元素的栈。
>
> - `push(x)` —— 将元素 x 推入栈中。
> - `pop()` —— 删除栈顶的元素。
> - `top()` —— 获取栈顶元素。
> - `getMin()` —— 检索栈中的最小元素。
>
>  
>
> **示例:**
>
> ```
> 输入：
> ["MinStack","push","push","push","getMin","pop","top","getMin"]
> [[],[-2],[0],[-3],[],[],[],[]]
> 
> 输出：
> [null,null,null,null,-3,null,0,-2]
> 
> 解释：
> MinStack minStack = new MinStack();
> minStack.push(-2);
> minStack.push(0);
> minStack.push(-3);
> minStack.getMin();   --> 返回 -3.
> minStack.pop();
> minStack.top();      --> 返回 0.
> minStack.getMin();   --> 返回 -2.
> ```
>
>  
>
> **提示：**
>
> - `pop`、`top` 和 `getMin` 操作总是在 **非空栈** 上调用。

**思路：**

**代码：**

```java
class MinStack {
    /*
    解题思路：辅助栈
    栈的特点是先进后出，所以只需要设置一个最小元素栈，每次都存入当前的最小元素
    并随着主数据栈弹出元素即可
    */
    /** initialize your data structure here. */
    Deque<Integer> data;
    Deque<Integer> min;
    public MinStack() {
        data = new ArrayDeque<>();
        min = new ArrayDeque<>();
    }
    
    public void push(int x) {
        data.offer(x);
        if (min.isEmpty()) {
            min.offer(x);
        } else {
            if (min.peekLast() < x)
                min.offer(min.peekLast());
            else
                min.offer(x);
        }
    }
    
    public void pop() {
        data.pollLast();
        min.pollLast();
    }
    
    public int top() {
        return data.peekLast();
    }
    
    public int getMin() {
        return min.peekLast();
    }
}

```



## 56.[相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

**题目：**

> 编写一个程序，找到两个单链表相交的起始节点。
>
> 如下面的两个链表**：**
>
> [![img](E:/program files/Topora images/160_statement.png)](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)
>
> 在节点 c1 开始相交。
>
>  
>
> **示例 1：**
>
> [![img](E:/program files/Topora images/160_example_1.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)
>
> ```
> 输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
> 输出：Reference of the node with value = 8
> 输入解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
> ```
>
>  
>
> **示例 2：**
>
> [![img](E:/program files/Topora images/160_example_2.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png)
>
> ```
> 输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
> 输出：Reference of the node with value = 2
> 输入解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
> ```
>
>  
>
> **示例 3：**
>
> [![img](E:/program files/Topora images/160_example_3.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)
>
> ```
> 输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
> 输出：null
> 输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
> 解释：这两个链表不相交，因此返回 null。
> ```
>
>  
>
> **注意：**
>
> - 如果两个链表没有交点，返回 `null`.
> - 在返回结果后，两个链表仍须保持原有的结构。
> - 可假定整个链表结构中没有循环。
> - 程序尽量满足 O(*n*) 时间复杂度，且仅用 O(*1*) 内存。

**思路：**

**代码：**

```java
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
    /*
    解题思路：链表交换
    设长链表与短链表的长度差为k,两指针a,b分别指向headA与headB并同时向前移动
    当其中一个走到null时，它们的距离就为k,此时将a指向headB，继续向前走，
    直到b也指向null时使它指向headA，此时a指针就能恰好走完k步，两指针就能同步向前走
    */
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode nodeA = headA, nodeB = headB;
        while (nodeA != nodeB) {
            nodeA = nodeA == null ? headB : nodeA.next;
            nodeB = nodeB == null ? headA : nodeB.next;
        }
        return nodeB;
    }
}
```

```java
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
    /*
    解题思路：普通遍历
    */
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int lenA = 0, lenB = 0;
        ListNode nodeA = headA, nodeB = headB;
        while (nodeA != null) {
            ++lenA;
            nodeA = nodeA.next;
        }
        while (nodeB != null) {
            ++lenB;
            nodeB = nodeB.next;
        }
        nodeA = headA;
        nodeB = headB;
        //lenA总是更长的那个
        if (lenA < lenB) {
            int tmp = lenA;
            lenA = lenB;
            lenB = tmp;
            nodeA = headB;
            nodeB = headA;
        }
        for (int i = 0 ; i < lenA - lenB; ++i) {
            nodeA = nodeA.next;
        }
        while (nodeA != nodeB) {
            nodeA = nodeA.next;
            nodeB = nodeB.next;
        }
        return nodeA;
    }
}




```



## 57.[多数元素](https://leetcode-cn.com/problems/majority-element/)

**题目：**

> 给定一个大小为 *n* 的数组，找到其中的多数元素。多数元素是指在数组中出现次数**大于** `⌊ n/2 ⌋` 的元素。
>
> 你可以假设数组是非空的，并且给定的数组总是存在多数元素。
>
>  
>
> **示例 1:**
>
> ```
> 输入: [3,2,3]
> 输出: 3
> ```
>
> **示例 2:**
>
> ```
> 输入: [2,2,1,1,1,2,2]
> 输出: 2
> ```

**思路：**

[多数元素 - 多数元素 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/majority-element/solution/duo-shu-yuan-su-by-leetcode-solution/)

**代码：**

```java
class Solution {
    /*
    解题思路：摩尔投票法
    候选人(cand_num)初始化为nums[0]，票数count初始化为1。
    当遇到与cand_num相同的数，则票数count = count + 1，否则票数count = count - 1。
    当票数count为0时，更换候选人，并将票数count重置为1。
    遍历完数组后，cand_num即为最终答案。
    */
    public int majorityElement(int[] nums) {
        int len = nums.length;
        int cnt = 1, lastNum = nums[0];
        for (int i = 1; i < len; ++i) {
            if (lastNum == nums[i]) {
                ++cnt;
            } else {
                --cnt;
                if (cnt == 0) {
                    lastNum = nums[i];
                    ++cnt;
                }
            }
        }
        return lastNum;
    }
}
```



## 58.[ 打家劫舍](https://leetcode-cn.com/problems/house-robber/)

**题目：**

> 你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。
>
> 给定一个代表每个房屋存放金额的非负整数数组，计算你 **不触动警报装置的情况下** ，一夜之内能够偷窃到的最高金额。
>
>  
>
> **示例 1：**
>
> ```
> 输入：[1,2,3,1]
> 输出：4
> 解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
>      偷窃到的最高金额 = 1 + 3 = 4 。
> ```
>
> **示例 2：**
>
> ```
> 输入：[2,7,9,3,1]
> 输出：12
> 解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
>      偷窃到的最高金额 = 2 + 9 + 1 = 12 。
> ```
>
>  
>
> **提示：**
>
> - `0 <= nums.length <= 100`
> - `0 <= nums[i] <= 400`

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：动态规划
    dp[i] = max(dp[i - 2] + nums[i], dp[i - 1])
    */
    public int rob(int[] nums) {
        int twoBefor = 0, oneBefor = 0, tmp;
        for (int i = 0; i < nums.length; ++i) {
            tmp = oneBefor;
            oneBefor = Math.max(twoBefor + nums[i], oneBefor);
            twoBefor = tmp;
        }
        return oneBefor;
    }
}
```



## 59.[ 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

**题目：**

给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

 

**示例 1：**

```
输入：grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
输出：1
```

**示例 2：**

```
输入：grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
输出：3
```

 

**提示：**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` 的值为 `'0'` 或 `'1'`

**思路：**

[200. 岛屿数量（DFS / BFS） - 岛屿数量 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/number-of-islands/solution/number-of-islands-shen-du-you-xian-bian-li-dfs-or-/)

**代码：**

```java
class Solution {
    /*
    解题思路：DFS
    */
    int cols;
    int rows;
    char grid[][];
    public int numIslands(char[][] grid) {
        this.cols = grid[0].length;
        this.rows = grid.length;
        this.grid = grid;
        int cnt = 0;
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                if (grid[i][j] == '1') {
                    ++cnt;
                    dfs(i, j);
                }
            }
        }
        return cnt;
    }

    public void dfs(int i, int j) {
        if (i >= 0 && i < rows && j >= 0 && j < cols && grid[i][j] == '1') {
            grid[i][j] = '0';
            dfs(i + 1, j);
            dfs(i, j + 1);
            dfs(i - 1, j);
            dfs(i, j - 1);
        }
    }
}
```

```java
class Solution {
    /*
    解题思路：BFS
    遍历每个陆地节点，若其未被访问过，则使用bfs对其遍历，并将访问过的节点标记为2，并更新计数变量++cnt
    */
    int[][] direc = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
    int rows, cols;
    public int numIslands(char[][] grid) {
        int cnt = 0;
        this.rows = grid.length;
        this.cols = grid[0].length;
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                if (grid[i][j] == '1') {
                    ++cnt;
                    bfs(grid, i, j);
                }
            }
        }
        return cnt;
    }

    public void bfs(char[][] grid, int i, int j) {
        Deque<int[]> queue = new ArrayDeque<>();
        queue.add(new int[]{i, j});
        while (!queue.isEmpty()) {
            int[] cur = queue.remove();
            i = cur[0];
            j = cur[1];
            if (i >= 0 && i < rows && j >= 0 && j < cols && grid[i][j] == '1') {
                grid[i][j] = '0';
                for (int[] d : direc) {
                    queue.add(new int[]{i + d[0], j + d[1]});
                }
            }
        }
    }
}
```



## 60.[反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

**题目：**

> 反转一个单链表。
>
> **示例:**
>
> ```
> 输入: 1->2->3->4->5->NULL
> 输出: 5->4->3->2->1->NULL
> ```
>
> **进阶:**
> 你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

**思路：**

**代码：**

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    /*
    解题思路：迭代
    */
    public ListNode reverseList(ListNode head) {
        ListNode last = null, cur = head;
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = last;
            last = cur;
            cur = next;
        }
        return last;
    }
}
```

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    /*
    解题思路：递归
    */
    public ListNode reverseList(ListNode head) {
        reverse(head);
        return last;
    }

    ListNode last = null;
    public void reverse(ListNode pNode) {
        if (pNode == null)
            return;
        ListNode tmp = pNode.next;
        pNode.next = last;
        last = pNode;
        reverse(tmp);
    }
}
```



## 61.[课程表](https://leetcode-cn.com/problems/course-schedule/)

**题目：**

> 你这个学期必须选修 `numCourse` 门课程，记为 `0` 到 `numCourse-1` 。
>
> 在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们：`[0,1]`
>
> 给定课程总量以及它们的先决条件，请你判断是否可能完成所有课程的学习？
>
>  
>
> **示例 1:**
>
> ```
> 输入: 2, [[1,0]] 
> 输出: true
> 解释: 总共有 2 门课程。学习课程 1 之前，你需要完成课程 0。所以这是可能的。
> ```
>
> **示例 2:**
>
> ```
> 输入: 2, [[1,0],[0,1]]
> 输出: false
> 解释: 总共有 2 门课程。学习课程 1 之前，你需要先完成课程 0；并且学习课程 0 之前，你还应先完成课程 1。这是不可能的。
> ```
>
>  
>
> **提示：**
>
> 1. 输入的先决条件是由 **边缘列表** 表示的图形，而不是 邻接矩阵 。详情请参见[图的表示法](http://blog.csdn.net/woaidapaopao/article/details/51732947)。
> 2. 你可以假定输入的先决条件中没有重复的边。
> 3. `1 <= numCourses <= 10^5`

**思路：**

[课程表（拓扑排序：入度表BFS法 / DFS法，清晰图解） - 课程表 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/course-schedule/solution/course-schedule-tuo-bu-pai-xu-bfsdfsliang-chong-fa/)

**代码：**

```java
class Solution {
    /*
    解题思路：DFS
    遍历每个节点，判断每个节点是否存在环，存在则返回false
    关键步骤：创建一个表示状态的整形数组status[i]，i表示第i个节点
    为0表示该节点未被访问过
    为1表示该节点正在访问中
    为2表示该节点已经访问过了，且没有环
    */
    int[] status;
    List<List<Integer>> adjacency;
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        status = new int[numCourses];
        adjacency = new ArrayList<>();
        for (int i = 0; i < numCourses; ++i) {
            adjacency.add(new ArrayList<>());
        }
        for (int[] linear: prerequisites) {
            adjacency.get(linear[1]).add(linear[0]);
        }
        for (int i = 0; i < numCourses; ++i) {
            if (!dfs(i))
                return false;
        }
        return true;
    }

    public boolean dfs(int course) {
        if (status[course] == 1)
            return false;
        if (status[course] == 2)
            return true;
        status[course] = 1;
        for (int cur : adjacency.get(course)) {
            if (!dfs(cur))
                return false;
        }
        status[course] = 2;
        return true;
    }
}
```

```java
class Solution {
    /*
    解题思路：入度表（BFS）
    *入度：入度是图论算法中重要的概念之一。它通常指有向图中某点作为图中边的终点的次数之和。
    本题可看作：课程安排图是否是有向无环图(DAG)。即课程间规定了前置条件，但不能构成环路，否则前置条件将不成立
    思路是通过托普排序判断此课程安排图中是否有DAG。拓扑排序原理：对 DAG 的顶点进行排序，使得对每一条有向边 (u, v)，均有 u（在排序记录中）比 v 先出现。亦可理解为对某点 v 而言
    只有当 v 的所有源点均出现了，v 才能出现。
    通过课程前置条件列表 prerequisites 可以得到课程安排图的 邻接表 adjacency，以降低算法时间复杂度。

    算法流程：
    1.统计课程安排图中每个节点的入度，生成入度表
    2.借助一个队列queue，将所有入度为0的节点入队
    3.当queue为非空时，依次将队首节点出队，并删除课程安排图中的此节点pre:
        1.并不是真的从邻接表中删除此节点，而是将该节点对应所有邻接节点cur的入度-1,即indegrees[cur] -= 1 (即pre -> cur1, pre -> cur2这种边，在删除pre之后，变成了cur1, cue2，需要将对应cur1,cur2的入度-1)
        2.若入度-1后cur节点的入度为0，说明它的所有前驱节点已经被删除，此时将cur入队
    4.每次pre出队以后，执行numCourses--
        1.若整个课程安排图是有向无环图，则所有节点一定都入队并出队过，即完成拓扑排序。换而言之，若存在环，一定有节点入度不为0
        2.所以，拓扑排序出队次数等于课程个数，然后numCourses == 0 判断课程是否可以成功安排
    */
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        //入度表
        int[] indegree = new int[numCourses];
        //邻接表
        List<List<Integer>> adjacency = new ArrayList<>();
        for (int i = 0; i < numCourses; ++i) {
            adjacency.add(new ArrayList<>());
        }
        for (int[] linear : prerequisites) {
            ++indegree[linear[0]];
            //加入一条边有向边linear[1] -> linear[0]
            adjacency.get(linear[1]).add(linear[0]);
        }
        Deque<Integer> queue = new ArrayDeque<>();
        //将度为0的节点加入队列中
        for (int i = 0; i < numCourses; ++i) {
            if (indegree[i] == 0)
                queue.offer(i);
        }
        while (!queue.isEmpty()) {
            int pre = queue.poll();
            --numCourses;
            //更新以pre为前置节点的节点的度
            for (Integer cur : adjacency.get(pre)) {
                --indegree[cur];
                if (indegree[cur] == 0)
                    queue.offer(cur);
            }
        }
        return numCourses == 0;
    }
}
```



## 62.[实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

**题目：**

> 实现一个 Trie (前缀树)，包含 `insert`, `search`, 和 `startsWith` 这三个操作。
>
> **示例:**
>
> ```
> Trie trie = new Trie();
> 
> trie.insert("apple");
> trie.search("apple");   // 返回 true
> trie.search("app");     // 返回 false
> trie.startsWith("app"); // 返回 true
> trie.insert("app");   
> trie.search("app");     // 返回 true
> ```
>
> **说明:**
>
> - 你可以假设所有的输入都是由小写字母 `a-z` 构成的。
> - 保证所有输入均为非空字符串。

**思路：**

[实现 Trie (前缀树) - 实现 Trie (前缀树) - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/solution/shi-xian-trie-qian-zhui-shu-by-leetcode/)

**代码：**

```java
class Trie {
    /*
    解题思路：建立字符映射数组
    */
    boolean isEnd;
    Trie[] next = new Trie[26];

    /** Initialize your data structure here. */
    public Trie() {

    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        Trie root = this;
        char[] words = word.toCharArray();
        for (int i = 0; i < words.length; ++i) {
            if (root.next[words[i] - 'a'] == null) {
                root.next[words[i] - 'a'] = new Trie();
            }
            root = root.next[words[i] - 'a'];
        }
        root.isEnd = true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        Trie root = this;
        char[] words = word.toCharArray();
        for (int i = 0; i < words.length; ++i) {
            if (root.next[words[i] - 'a'] == null) {
                return false;
            }
            root = root.next[words[i] - 'a'];
        }
        return root.isEnd;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        Trie root = this;
        char[] words = prefix.toCharArray();
        for (int i = 0; i < words.length; ++i) {
            if (root.next[words[i] - 'a'] == null) {
                return false;
            }
            root = root.next[words[i] - 'a'];
        }
        return true;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```



## 63.[数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

**题目：**

> 在未排序的数组中找到第 **k** 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。
>
> **示例 1:**
>
> ```
> 输入: [3,2,1,5,6,4] 和 k = 2
> 输出: 5
> ```
>
> **示例 2:**
>
> ```
> 输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
> 输出: 4
> ```
>
> **说明:**
>
> 你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。

**思路：**

[通过 partition 减治 + 优先队列（Java、C++、Python） - 数组中的第K个最大元素 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/solution/partitionfen-er-zhi-zhi-you-xian-dui-lie-java-dai-/)

**代码：**

```java
class Solution {
    /*
    解题思路：快排思想
    */
    public int findKthLargest(int[] nums, int k) {
        if (nums.length < k) {
            return -1;
        }
        return quickSort(nums, 0, nums.length - 1, k);
    }

    public int quickSort(int[] nums, int l, int r, int k) {
        //引入随机算法，题目案例中具有极端案例
        int ran = l + (int)(Math.random() * (r - l + 1));
        int t = nums[ran];
        nums[ran] = nums[l];
        nums[l] = t;
        
        int base = nums[l];
        int p = l, q = r;
        int tmp = base;
        while (p < q) {
            while (p < q && nums[q] >= base)
                --q;
            while (p < q && nums[p] <= base)
                ++p;
             tmp = nums[p];
             nums[p] = nums[q];
             nums[q] = tmp;
        }
        nums[p] = base;
        nums[l] = tmp;
        //需要在右侧寻找
        int target = nums.length - k;
        if (p == target)
            return nums[p];
        if (p < target) {
            return quickSort(nums, p + 1, r, k);
        } else {
            return quickSort(nums, l, p - 1, k);
        }
    }
}
```



## 64.[最大正方形](https://leetcode-cn.com/problems/maximal-square/)

**题目：**

> 在一个由 `'0'` 和 `'1'` 组成的二维矩阵内，找到只包含 `'1'` 的最大正方形，并返回其面积。
>
>  
>
> **示例：**
>
> ```
> 输入：
> matrix = [["1","0","1","0","0"],
>           ["1","0","1","1","1"],
>           ["1","1","1","1","1"],
>           ["1","0","0","1","0"]]
> 
> 输出：4
> ```

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：从左到右自上而下遍历，将每个为'1'的格子作为正方形的左上角进行一圈一圈的遍历
    */
    public int maximalSquare(char[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return 0;
        }
        int maxSide = 0;
        for (int row = 0; row < matrix.length; ++row) {
            for (int col = 0; col < matrix[0].length; ++col) {
                if (matrix[row][col] == '1') {
                    maxSide = Math.max(maxSide, findMaxSide(matrix, row, col));
                }
            }
        }
        return maxSide * maxSide;
    }

    public int findMaxSide(char[][] matrix, int row, int col) {
        int side = 1;
        int rows = matrix.length;
        int cols = matrix[0].length;
        while (row + side < rows && col + side < cols) {
            for (int i = 0; i <= side; ++i) {
                //竖着判断，有不为'1'的直接返回
                if (matrix[row + i][col + side] != '1')
                    return side;
                //横着判断，有不为'1'的直接返回
                if (matrix[row + side][col + i] != '1')
                    return side;
            }
            ++side;
        }
        return side;
    }
}
```

```java
class Solution {
    /*
    解题思路：动态规划
    设dp[i][j]表示以matrix[i - 1][j - 1]为正方形右下角的最大边长,若其等于m，则需要满足dp[i - 1][j] = m - 1,dp[i][j - 1] = m - 1, dp[i - 1][j - 1] = m - 1
    即该上一个点，左边的点与左上的点都要满足等于 m - 1才可以,换句话说dp[i][j]为以上3个点的最小值 + 1，即取 min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1]) + 1
    */
    public int maximalSquare(char[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0)
            return 0;
        int maxSide = 0, cols = matrix[0].length;
        int[] dp = new int[cols + 1];
        for (char[] chars: matrix) {
            int leftTop = 0;
            for (int i = 0; i < cols; ++i) {
                //保存该点原来的值，作为下一个点的左上角点的值
                int nextLeftTop = dp[i + 1];
                if (chars[i] == '1') {
                    dp[i + 1] = Math.min(Math.min(dp[i], dp[i + 1]), leftTop) + 1;
                    maxSide = Math.max(maxSide, dp[i + 1]);
                } else {
                    dp[i + 1] = 0;
                }
                leftTop = nextLeftTop;
            }
        }
        return maxSide * maxSide;
    }
}
```



## 65.[翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

**题目：**

> 翻转一棵二叉树。
>
> **示例：**
>
> 输入：
>
> ```
>      4
>    /   \
>   2     7
>  / \   / \
> 1   3 6   9
> ```
>
> 输出：
>
> ```
>      4
>    /   \
>   7     2
>  / \   / \
> 9   6 3   1
> ```
>
> **备注:**
> 这个问题是受到 [Max Howell ](https://twitter.com/mxcl)的 [原问题](https://twitter.com/mxcl/status/608682016205344768) 启发的 ：
>
> > 谷歌：我们90％的工程师使用您编写的软件(Homebrew)，但是您却无法在面试时在白板上写出翻转二叉树这道题，这太糟糕了。

**思路：**

**代码：**

```java
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
    /*
    解题思路：BFS
    */
    public TreeNode invertTree(TreeNode root) {
        Deque<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            TreeNode cur = queue.poll();
            if (cur != null) {
                TreeNode tmp = cur.left;
                cur.left = cur.right;
                cur.right = tmp;
                queue.offer(cur.left);
                queue.offer(cur.right);
            }
        }
        return root;
    }
}
```

```java
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
    /*
    解题思路：前序遍历，交换左右子树
    */
    public TreeNode invertTree(TreeNode root) {
        reverse(root);
        return root;
    }

    public void reverse(TreeNode root) {
        if (root == null) {
            return;
        }
        TreeNode tmp = root.left;
        root.left = root.right;
        root.right = tmp;
        reverse(root.left);
        reverse(root.right);
    }
}
```



## 66.[ 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

**题目：**

> 请判断一个链表是否为回文链表。
>
> **示例 1:**
>
> ```
> 输入: 1->2
> 输出: false
> ```
>
> **示例 2:**
>
> ```
> 输入: 1->2->2->1
> 输出: true
> ```
>
> **进阶：**
> 你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？

**思路：**

**代码：**

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    /*
    解题思路：快慢指针 + 反转链表
    */
    public boolean isPalindrome(ListNode head) {
        if (head == null)
            return true;
        //1.利用快慢指针找到中间节点
        ListNode fast = head, slow = head;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        //2.将slow以及slow之前的节点分为一组，slow之后的分为一组
        ListNode secHead = slow.next;
        slow.next = null;
        //3.翻转链表
        ListNode pre = null;
        ListNode cur = secHead;
        while(cur != null) {
            ListNode tmp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = tmp; 
        }
        //4.同时遍历两个链表
        secHead = pre;
        while (secHead != null) {
            if (secHead.val != head.val)
                return false;
            secHead = secHead.next;
            head = head.next;
        }
        return true;
    }
}
```

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    /*
    解题思路：deque
    */
    public boolean isPalindrome(ListNode head) {
        Deque<ListNode> queue = new LinkedList<>();
        ListNode pNode = head;
        while (pNode != null) {
            queue.offer(pNode);
            pNode = pNode.next;
        }
        while (queue.size() > 1) {
            if (queue.pollLast().val != queue.poll().val) {
                return false;
            }
        }
        return true;
    }
}
```



## 67.[二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

**题目：**

> 给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。
>
> [百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”
>
> 例如，给定如下二叉树: root = [3,5,1,6,2,0,8,null,null,7,4]
>
> ![img](E:/program files/Topora images/binarytree.png)
>
>  
>
> **示例 1:**
>
> ```
> 输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
> 输出: 3
> 解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
> ```
>
> **示例 2:**
>
> ```
> 输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
> 输出: 5
> 解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
> ```
>
>  
>
> **说明:**
>
> - 所有节点的值都是唯一的。
> - p、q 为不同节点且均存在于给定的二叉树中。

**思路：**

**代码：**

```java
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
    /*
    解题思路：DFS
    1.若本节点是p或者q直接返回本节点,若为空则返回空
    2.对左右子节点递归：
        1.若左右同时不反null：说明左右子树各有一个节点，直接返回本节点
        2.若只有一个不返null,继续传递该返回值
    */
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null)
            return null;
        if (root.val == p.val || root.val == q.val)
            return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left != null && right != null)
            return root;
        if (left == null)
            return right;
        return left;
    }
}
```



## 68.[除自身以外数组的乘积](https://leetcode-cn.com/problems/product-of-array-except-self/)

**题目：**

> 给你一个长度为 *n* 的整数数组 `nums`，其中 *n* > 1，返回输出数组 `output` ，其中 `output[i]` 等于 `nums` 中除 `nums[i]` 之外其余各元素的乘积。
>
>  
>
> **示例:**
>
> ```
> 输入: [1,2,3,4]
> 输出: [24,12,8,6]
> ```
>
>  
>
> **提示：**题目数据保证数组之中任意元素的全部前缀元素和后缀（甚至是整个数组）的乘积都在 32 位整数范围内。
>
> **说明:** 请**不要使用除法，**且在 O(*n*) 时间复杂度内完成此题。
>
> **进阶：**
> 你可以在常数空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组**不被视为**额外空间。）

**思路：**

[除自身以外数组的乘积（上三角，下三角） - 除自身以外数组的乘积 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/product-of-array-except-self/solution/product-of-array-except-self-shang-san-jiao-xia-sa/)

**代码：**

```java
class Solution {
    /*
    解题思路：简单数组遍历
    output[i]因为缺少了nums[i]可以将其看作两部分: output[i] = partOne[i] * partTwo[i]
    1.partOne[i] = nums[0]*nums[1]....*nums[i - 1]
    2.partTwo[i] = nums[i + 1]*nums[i + 1]...*nums[n - 1]
    这两部分分别从上往下看，与从下往上看都是dp数组
    partOne从上往下看: partOne[i] = partOne[i - 1] * nums[i - 1]
    partTwo从下往上看: partTwo[i] = partTwo[i + 1] * nums[i + 1]
    */
    public int[] productExceptSelf(int[] nums) {
        int len = nums.length;
        if (len < 1)
            return nums;
        int[] partOne = new int[len];
        partOne[0] = 1;
        for (int i = 1; i < len; ++i) {
            partOne[i] = partOne[i - 1] * nums[i - 1];
        }
        int partTwo = 1;
        for (int i = len - 2; i >= 0; --i) {
            partTwo = partTwo * nums[i + 1];
            partOne[i] = partOne[i] * partTwo;
        }
        return partOne;
    }
}
```



## 69.[ 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

**题目：**

> 给定一个数组 *nums*，有一个大小为 *k* 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 *k* 个数字。滑动窗口每次只向右移动一位。
>
> 返回滑动窗口中的最大值。
>
>  
>
> **进阶：**
>
> 你能在线性时间复杂度内解决此题吗？
>
>  
>
> **示例:**
>
> ```
> 输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
> 输出: [3,3,5,5,6,7] 
> 解释: 
> 
>   滑动窗口的位置                最大值
> ---------------               -----
> [1  3  -1] -3  5  3  6  7       3
>  1 [3  -1  -3] 5  3  6  7       3
>  1  3 [-1  -3  5] 3  6  7       5
>  1  3  -1 [-3  5  3] 6  7       5
>  1  3  -1  -3 [5  3  6] 7       6
>  1  3  -1  -3  5 [3  6  7]      7
> ```
>
>  
>
> **提示：**
>
> - `1 <= nums.length <= 10^5`
> - `-10^4 <= nums[i] <= 10^4`
> - `1 <= k <= nums.length`

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：dp
    先将输入数组分割成若干个k个元素的块，例：1,3,-1,3,-3,5,3,6,7,9 k = 3 则变成[1,3,-1][3,-3,5][3,6,7][9]
    若n % k != 0则最后一块的元素可能更少
    开头元素为i,结尾元素为j的当前滑动窗口有两种情况：1.滑动窗口恰好在一个块内 2.滑动窗口在两个块中间
    情况1比较简单：
        建立数组left,其中left[j]是从块的开始到下标j最大的元素，方向是从左到右
    情况2稍显复杂：
        我们还需要另一个数组right,其中right[j]是从块的结尾到下标j的最大元素，方向是从右到左。
    有了以上提到的两个数组我们可以得知两个相邻块内元素的全部信息。考虑从下标i到下标j的滑动窗口。
    根据定义，right[i]是左侧块内的最大元素，left[j]是右侧块内的最大元素，因此滑动窗口的最大元素为max(left[j], right[i])
    */
    public int[] maxSlidingWindow(int[] nums, int k) {
        int len = nums.length;
        int[] left = new int[len];
        int[] right = new int[len];
        //初始化left与right的第一个元素，防止right产生越界,因为right是从右向左遍历的，第一个j可能不是k的整数倍
        left[0] = nums[0];
        right[len - 1] = nums[len - 1];
        for (int i = 1; i < len; ++i) {
            if (i % k == 0)
                left[i] = nums[i];
            else
                left[i] = Math.max(left[i - 1], nums[i]);
            int j = len - 1 - i;
            if (j % k == 0)
                right[j] = nums[j];
            else
                right[j] = Math.max(right[j + 1], nums[j]);
        }
        int[] res = new int[len - (k - 1)];
        for (int i = 0, j = k - 1; j < len; ++i, ++j) {
            res[i] = Math.max(left[j], right[i]);
        }
        return res;
    }
}
```

```java
class Solution {
    /*
    解题思路：存储下标的单调队列
    1.初始化：创建一个存储元素下标的单调栈递增队列queue，结果数组res,大小为nums.length - (k - 1)
    2.遍历nums数组，设每个元素为nums[i]:
        1.插入i：从queue的后面往前插入i,将遇到的小于num[i]的元素弹出(因为它们已经不能成为最大值了)
        2.若i >= k - 1则开启记录最大值：
            1.判断当前queue头部元素(即最大元素)是否过期，设a为头部元素，若a + (k - 1) < i 则为过期，
            过期则将其弹出，并循环找到第一个满足要求的元素（找到了就不需要弹出），并记录nums[a]为当前窗口的最大值
    */
    public int[] maxSlidingWindow(int[] nums, int k) {
        int len = nums.length;
        int[] res = new int[len - (k - 1)];
        Deque<Integer> queue = new LinkedList<>();
        int index = 0;
        for (int i = 0; i < len; ++i) {
            //插入i
            while (!queue.isEmpty() && nums[queue.peekLast()] < nums[i]) {
                queue.pollLast();
            }
            queue.offer(i);
            if (i >= k - 1) {
                while (queue.peek() + (k - 1) < i) {
                    queue.poll();
                }
                res[index++] = nums[queue.peek()];
            }
        }
        return res;
    }
}
```



## 70.[搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)

**题目：**

> 编写一个高效的算法来搜索 `*m* x *n*` 矩阵 `matrix` 中的一个目标值 `target` 。该矩阵具有以下特性：
>
> - 每行的元素从左到右升序排列。
> - 每列的元素从上到下升序排列。
>
>  
>
> **示例 1：**
>
> ![img](E:/program files/Topora images/searchgrid2.jpg)
>
> ```
> 输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
> 输出：true
> ```
>
> **示例 2：**
>
> ![img](E:/program files/Topora images/searchgrid.jpg)
>
> ```
> 输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20
> 输出：false
> ```
>
>  
>
> **提示：**
>
> - `m == matrix.length`
> - `n == matrix[i].length`
> - `1 <= n, m <= 300`
> - `-109 <= matix[i][j] <= 109`
> - 每行的所有元素从左到右升序排列
> - 每列的所有元素从上到下升序排列
> - `-109 <= target <= 109`

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：右上角搜索
    找到一个临界点，使该点满足，值比它大的其他点在一个确定的方向上，值比它小的其他点在另一个确定的方向上
    满足这两个条件的只有右上角的点与左下角的点
    从右上角的点开始，设其为marix[i][j]，若target < matrix[i][j] 则根据题意，应该向左移动变为matrix[i][j-1]
    若target > matrix[i][j]则向下移动变为matrix[i + 1][j]
    若target == matrix[i][j]则返回true
    若i或j越界说明查询失败
    */
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix.length == 0 || matrix[0].length == 0)
            return false;
        int rows = matrix.length;
        //初始化右上角的点
        int cols = matrix[0].length;
        int row = 0, col = cols - 1;
        while (row < rows && col >= 0) {
            if (target == matrix[row][col])
                return true;
            else if (target < matrix[row][col]) {
                --col;
            } else {
                ++row;
            }
        }
        return false;
    }
}
```

## 71.会议室II

**题目：**

**思路：**

**代码：**



## 72.[完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

**题目：**

> 给定正整数 *n*，找到若干个完全平方数（比如 `1, 4, 9, 16, ...`）使得它们的和等于 *n*。你需要让组成和的完全平方数的个数最少。
>
> **示例 1:**
>
> ```
> 输入: n = 12
> 输出: 3 
> 解释: 12 = 4 + 4 + 4.
> ```
>
> **示例 2:**
>
> ```
> 输入: n = 13
> 输出: 2
> 解释: 13 = 4 + 9.
> ```

**思路：**

[画解算法：279. 完全平方数 - 完全平方数 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/perfect-squares/solution/hua-jie-suan-fa-279-wan-quan-ping-fang-shu-by-guan/)

**代码：**

```java
class Solution {
    /*
    解题思路：dp
    此问题可以转换为求最小价值的完全背包问题
    1.外层循环控制的是前i个完全平方数，即 i = 1, sqrt = i *i; sqrt <= n; ++i, sqrt = i * i
    2.内层循环控制 剩余数字还能放多少第i个数字（背包容量)
    每个数字的价值val都相当于是1，我们要求最小，即Math.min(dp[j], dp[j - sqrt] + 1)
    */
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;
        for (int i = 1, sqrt = i * i; sqrt <= n; ++i, sqrt = i * i) {
            for (int j = sqrt; j <= n; ++j) {
                dp[j] = Math.min(dp[j], dp[j - sqrt] + 1);
            }
        }
        return dp[n];
    }
}
```



## 73.[移动零](https://leetcode-cn.com/problems/move-zeroes/)

**题目：**

> 给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。
>
> **示例:**
>
> ```
> 输入: [0,1,0,3,12]
> 输出: [1,3,12,0,0]
> ```
>
> **说明**:
>
> 1. 必须在原数组上操作，不能拷贝额外的数组。
> 2. 尽量减少操作次数。

**思路：**

[动画演示 283.移动零 - 移动零 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/move-zeroes/solution/dong-hua-yan-shi-283yi-dong-ling-by-wang_ni_ma/)

**代码：**

```java
class Solution {
    /*
    解题思路：快排思想
    将0作为中心点，把不等于0的放到中心点左边，等于0的放到中心点右边
    */
    public void moveZeroes(int[] nums) {
        int j = 0;
        for (int i = 0; i < nums.length; ++i) {
            if (nums[i] != 0) {
                int tmp = nums[i];
                nums[i] = nums[j];
                nums[j++] = tmp;
            }
        }
    }
}
```

```java
class Solution {
    /*
    解题思路：巧用指针
    1.第一次遍历：nums[i]表示第i个元素，指针j记录当前有多少非0元素，每遍历到一个非0元素将nums[i]与nums[j]交换然后++j
    2.第二次遍历：第一次遍历结束后j一定指向第一个0元素的位置，将该位置的元素与之后的所有元素置为0即可
    */
    public void moveZeroes(int[] nums) {
        int j = 0;
        for (int i = 0; i < nums.length; ++i) {
            if (nums[i] != 0) {
                int tmp = nums[i];
                nums[i] = nums[j];
                nums[j++] = tmp;
            }
        }
        for (; j < nums.length; ++j) {
            nums[j] = 0;
        }
    }
}
```

```java
class Solution {
    /*
    解题思路：冒泡排序
    */
    public void moveZeroes(int[] nums) {
        for (int i = nums.length - 1; i > 0; --i) {
            for (int j = 0; j < i; ++j) {
                if (nums[j] == 0 && nums[j + 1] != 0) {
                    swap(nums, j, j + 1);
                }
            }
        }
    }

    public void swap(int[] nums, int a, int b) {
        int tmp = nums[a];
        nums[a] = nums[b];
        nums[b] = tmp;
    }
}
```



## 74.[ 寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)

**题目：**

> 给定一个包含 *n* + 1 个整数的数组 *nums*，其数字都在 1 到 *n* 之间（包括 1 和 *n*），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。
>
> **示例 1:**
>
> ```
> 输入: [1,3,4,2,2]
> 输出: 2
> ```
>
> **示例 2:**
>
> ```
> 输入: [3,1,3,4,2]
> 输出: 3
> ```
>
> **说明：**
>
> 1. **不能**更改原数组（假设数组是只读的）。
> 2. 只能使用额外的 *O*(1) 的空间。
> 3. 时间复杂度小于 *O*(*n*2) 。
> 4. 数组中只有一个重复的数字，但它可能不止重复出现一次。

**思路：**

[使用二分法查找一个有范围的整数（结合抽屉原理） - 寻找重复数 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/find-the-duplicate-number/solution/er-fen-fa-si-lu-ji-dai-ma-python-by-liweiwei1419/)

[287.寻找重复数 - 寻找重复数 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/find-the-duplicate-number/solution/287xun-zhao-zhong-fu-shu-by-kirsche/)

**代码：**

```java
class Solution {
    /*
    解题思路：将数组看作链表，若有重复数字，则肯定是环形链表
    只需要找到环形链表的入口节点即可
    */
    public int findDuplicate(int[] nums) {
        int pre = nums[nums[0]];
        int slow = nums[0];
        while (pre != slow) {
            pre = nums[nums[pre]];
            slow = nums[slow];
        }
        pre = 0;
        while (pre != slow) {
            pre = nums[pre];
            slow = nums[slow];
        }
        return slow;
    }
}
```

```java
class Solution {
    /*
    解题思路：二分查找
    关键点：二分的依据不是数值，而是个数
    先猜一个数，该数为有效范围[left,right]里的中间数mid，然后统计原始数组中小于等于mid的元素个数cnt，如果cnt严格大于mid，根据抽屉原理，重复元素就在区间[left, mid]中
    抽屉原理：桌上有十个苹果，要把这十个苹果放到九个抽屉里，无论怎样放，我们会发现至少会有一个抽屉里面放不少于两个苹果
    */
    public int findDuplicate(int[] nums) {
        int len = nums.length;
        int left = 1, right = len - 1;
        while (left < right) {
            int cnt = 0;
            int mid = (left + right) >>> 1;
            for (int num : nums) {
                if (num <= mid)
                    ++cnt;
            }
            if (cnt > mid) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return right;
    }
}
```



## 75.[二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

**题目：**

> 序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。
>
> 请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。
>
> **示例:** 
>
> ```
> 你可以将以下二叉树：
> 
>     1
>    / \
>   2   3
>      / \
>     4   5
> 
> 序列化为 "[1,2,3,null,null,4,5]"
> ```
>
> **提示:** 这与 LeetCode 目前使用的方式一致，详情请参阅 [LeetCode 序列化二叉树的格式](https://leetcode-cn.com/faq/#binary-tree)。你并非必须采取这种方式，你也可以采用其他的方法解决这个问题。
>
> **说明:** 不要使用类的成员 / 全局 / 静态变量来存储状态，你的序列化和反序列化算法应该是无状态的。

**思路：**

[『手画图解』剖析DFS、BFS解法 | 二叉树的序列化与反序列化 - 二叉树的序列化与反序列化 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/solution/shou-hui-tu-jie-gei-chu-dfshe-bfsliang-chong-jie-f/)

**代码：**

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {
    /*
    解题思路：前序遍历
    */

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if (root == null) {
            return "#,";
        } else {
            return root.val + "," + serialize(root.left) + serialize(root.right);
        }
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        Deque<String> queue = new LinkedList<>(Arrays.asList(data.split(",")));
        return build(queue);
    }

    public TreeNode build(Deque<String> queue) {
        String node = queue.poll();
        TreeNode pNode = null;
        if (!"#".equals(node)) {
            pNode = new TreeNode(Integer.valueOf(node));
            pNode.left = build(queue);
            pNode.right = build(queue);
        }
        return pNode;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec ser = new Codec();
// Codec deser = new Codec();
// TreeNode ans = deser.deserialize(ser.serialize(root));
```

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {
    /*
    解题思路：BFS
    */

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if (root == null)
            return "";
        StringBuilder builder = new StringBuilder();
        Deque<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            TreeNode pNode = queue.poll();
            if (pNode == null) {
                builder.append("#,");
            } else {
                builder.append(pNode.val + ",");
                queue.offer(pNode.left);
                queue.offer(pNode.right);
            }
        }
        return builder.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if ("".equals(data))
            return null;
        String[] nodes = data.split(",");
        Deque<TreeNode> queue = new LinkedList<>();
        TreeNode root = new TreeNode(Integer.valueOf(nodes[0]));
        queue.offer(root);
        int i = 1;
        while (!queue.isEmpty()) {
            int len = queue.size();
            for (int j = 0; j < len; ++j) {
                TreeNode pNode = queue.poll();
                TreeNode left = null;
                TreeNode right = null;
                if (!"#".equals(nodes[i])) {
                    left = new TreeNode(Integer.valueOf(nodes[i]));
                    queue.offer(left);
                }
                ++i;
                if (!"#".equals(nodes[i])) {
                    right = new TreeNode(Integer.valueOf(nodes[i]));
                    queue.offer(right);
                }
                ++i;
                pNode.left = left;
                pNode.right = right;
            }
        }
        return root;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec ser = new Codec();
// Codec deser = new Codec();
// TreeNode ans = deser.deserialize(ser.serialize(root));
```



## 76.[最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

**题目：**

> 给定一个无序的整数数组，找到其中最长上升子序列的长度。
>
> **示例:**
>
> ```
> 输入: [10,9,2,5,3,7,101,18]
> 输出: 4 
> 解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
> ```
>
> **说明:**
>
> - 可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
> - 你算法的时间复杂度应该为 O(*n2*) 。
>
> **进阶:** 你能将算法的时间复杂度降低到 O(*n* log *n*) 吗?

**思路：**

[最长上升子序列（动态规划 + 二分查找，清晰图解） - 最长上升子序列 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/longest-increasing-subsequence/solution/zui-chang-shang-sheng-zi-xu-lie-dong-tai-gui-hua-2/)

[最长上升子序列/动态规划/二分查找 - 最长上升子序列 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/longest-increasing-subsequence/solution/zui-chang-shang-sheng-zi-xu-lie-dong-tai-gui-hua-e/)

**代码：**

```java
class Solution {
    /*
    解题思路：dp
    */
    public int lengthOfLIS(int[] nums) {
        int len = nums.length;
        if (len == 0)
            return 0;
        int[] dp = new int[len];
        //每个数字都至少为1
        Arrays.fill(dp, 1);
        int max = 0;
        for (int i = 0; i < len; ++i) {
            for (int j = 0; j < i; ++j) {
                if (nums[j] < nums[i]) {
                    dp[i] = Math.max(dp[j] + 1, dp[i]);
                }
            }
            max = Math.max(max, dp[i]);
        }
        return max;
    }
}
```

```java
class Solution {
    /*
    解题思路：dp + 二分
    很具小巧思。新建数组 cell，用于保存最长上升子序列。
    对原序列进行遍历，将每位元素二分插入 cell 中。
    如果 cell 中元素都比它小，将它插到最后
    否则，用它覆盖掉比它大的元素中最小的那个。
    总之，思想就是让 cell 中存储比较小的元素。这样，cell 未必是真实的最长上升子序列，但长度是对的。
    */
    public int lengthOfLIS(int[] nums) {
        int[] cell = new int[nums.length];
        //cell的右边界(不包含，可以看作cell的长度)
        int len = 0;
        for (int num : nums) {
            int i = 0, j = len;
            while (i < j) {
                int m = (i + j) >> 1;
                if (cell[m] < num) {
                    i = m + 1;
                } else {
                    j = m;
                }
            }
            cell[i] = num;
            if (j == len)
                ++len;
        }
        return len;
    }
}
```



## 77.[删除无效的括号](https://leetcode-cn.com/problems/remove-invalid-parentheses/)

**题目：**

> 删除最小数量的无效括号，使得输入的字符串有效，返回所有可能的结果。
>
> **说明:** 输入可能包含了除 `(` 和 `)` 以外的字符。
>
> **示例 1:**
>
> ```
> 输入: "()())()"
> 输出: ["()()()", "(())()"]
> ```
>
> **示例 2:**
>
> ```
> 输入: "(a)())()"
> 输出: ["(a)()()", "(a())()"]
> ```
>
> **示例 3:**
>
> ```
> 输入: ")("
> 输出: [""]
> ```

**思路：**

[详细通俗的思路分析，多解法 - 删除无效的括号 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/remove-invalid-parentheses/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by--56/)

[C++ 普通dfs(16 ms)和剪枝优化后的dfs(0 ms) - 删除无效的括号 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/remove-invalid-parentheses/solution/c-pu-tong-dfs16-mshe-jian-zhi-you-hua-hou-de-dfs0-/)

**代码：**

```java
class Solution {
    /*
    解题思路：dfs回溯
    1.先遍历一遍字符串s，计算出需要删除的左右括号个数left,right，为我们的剪支做准备
    2.建立Set集合来保存不重复的结果,tmp保存中间解
    3.再次遍历s的每个字符，对每个括号都只有两种选择：1.保留2.删除
    但同时删除括号的个数会收到left 与 right的限制，当删除的个数超过left或者right将直接return
    4.若索引等于s.length就将当前解加入Set中
    */
    Set<String> res = new HashSet<>();
    StringBuilder builder = new StringBuilder();
    int left;
    int right;
    int totalLen;
    public List<String> removeInvalidParentheses(String s) {
        for (int i = 0; i < s.length(); ++i) {
            char ch = s.charAt(i);
            if (ch == '(') {
                ++left;
            } else if (ch == ')') {
                if (left > 0) {
                    --left;
                } else {
                    ++right;
                }
            }
        }
        totalLen = s.length() - left - right;
        backtrack(s, 0, 0, 0, 0);
        if (res.size() == 0) {
            return new ArrayList<>(){{
                add("");
            }};
        } else {
            return new ArrayList<>(res);
        }
    }

    public void backtrack(String s, int curLeft, int curRight, int k, int curLen) {
        if (curLeft > left || curRight > right || curLen > totalLen)
            return;
        if (k == s.length()) {
            String str = builder.toString();
            if (check(str)) {
                res.add(str);
            }
            return;
        }
        //1.保留
        builder.append(s.charAt(k));
        backtrack(s, curLeft, curRight, k + 1, curLen + 1);
        builder.deleteCharAt(builder.length() - 1);
        //2.删除左括号或者右括号,其他字符不能删
        if (s.charAt(k) == '(') {
            backtrack(s, ++curLeft, curRight, k + 1, curLen);
        } else if (s.charAt(k) == ')') {
            backtrack(s, curLeft, ++curRight, k + 1, curLen);
        }
    }

    //检查字符串是否合法
    public boolean check(String s) {
        int l = 0;
        int r = 0;
        for (int i = 0; i < s.length(); ++i) {
            char ch = s.charAt(i);
            if (ch == '(') {
                ++l;
            } else if (ch == ')') {
                if (l > 0) {
                    --l;
                } else {
                    return false;
                }
            }
        }
        return l == r;
    }
}
```

```java
class Solution {
    /*
    解题思路：dfs回溯
    1.先遍历一遍字符串s，计算出需要删除的左右括号个数left,right，为我们的剪支做准备
    2.建立Set集合来保存不重复的结果,tmp保存中间解
    3.再次遍历s的每个字符，对每个括号都只有两种选择：1.保留2.删除
    但同时删除括号的个数会收到left 与 right的限制，当删除的个数超过left或者right将直接return
    4.若索引等于s.length就将当前解加入Set中
    */
    List<String> res = new ArrayList<>();
    public List<String> removeInvalidParentheses(String s) {
        int left = 0, right = 0;
        for (int i = 0; i < s.length(); ++i) {
            char ch = s.charAt(i);
            if (ch == '(') {
                ++left;
            } else if (ch == ')') {
                if (left > 0) {
                    --left;
                } else {
                    ++right;
                }
            }
        }
        dfs(s, left, right, 0);
        return res;
    }

    public void dfs(String s, int curLeft, int curRight, int k) {
        if (curLeft == 0 && curRight == 0) {
            if (check(s)) {
                res.add(s);
            }
            return;
        }
        if (curRight < 0 || curLeft < 0)
            return;
        for (int i = k; i < s.length(); ++i) {
            //去重复
            if (i - 1 >= k && s.charAt(i) == s.charAt(i - 1)) {
                continue;
            }
            String tmp = s.substring(0, i) + s.substring(i + 1);
            if (curLeft > 0 && s.charAt(i) == '(') {
                dfs(tmp, curLeft - 1, curRight, i);
            }
            if (curRight > 0 && s.charAt(i) == ')') {
                dfs(tmp, curLeft, curRight - 1, i);
            }
        }
    }

    //检查字符串是否合法
    public boolean check(String s) {
        int l = 0;
        int r = 0;
        for (int i = 0; i < s.length(); ++i) {
            char ch = s.charAt(i);
            if (ch == '(') {
                ++l;
            } else if (ch == ')') {
                if (l > 0) {
                    --l;
                } else {
                    return false;
                }
            }
        }
        return l == 0;
    }
}
```



## 78.[最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

**题目：**

> 给定一个整数数组，其中第 *i* 个元素代表了第 *i* 天的股票价格 。
>
> 设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:
>
> - 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
> - 卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。
>
> **示例:**
>
> ```
> 输入: [1,2,3,0,2]
> 输出: 3 
> 解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]
> ```

**思路：**

[最佳买卖股票时机含冷冻期 - 最佳买卖股票时机含冷冻期 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/solution/zui-jia-mai-mai-gu-piao-shi-ji-han-leng-dong-qi-4/)

**代码：**

```java
class Solution {
    /*
    解题思路：dp
    设dp[i]表示第i天结束之后的【累计最大收益】。根据题目描述，由于我们最多只能同时持有一只股票，并且卖出股票后有冷冻期的限制，因此有三种不同状态：
    1.我们目前持有一支股票，对应的【累计最大收益】记为dp[i][0];
    2.我们目前不持有任何一只股票，并且处于冷冻期中，对应【累计最大收益】记为dp[i][1];
    3.我们目前不持有任何股票，并且不处于冷冻期中，对应【累计最大收益】记为dp[i][2];
    *这里的【处于冷冻期】是指在第i天结束之后的状态。也就是说：如果第i天结束之后处于冷冻期，那么第i + 1天无法买入股票
    那么我们在不违反规则的情况下对这三种情况进行分析：
    1.我们当前持有的股票可能是继承自昨天的，即dp[i - 1][0],也可能是不处于冷冻期的今天刚买的股票，也就是要满足昨天不可能售出股票并且也不能持有股票
    并且今天的股票将产生负价值，即dp[i - 1][2] - price[i]。
    总结：dp[i][0] = max(dp[i - 1][0], dp[i - 1][2] - price[i]);
    2.因为i + 1天处于冷冻期，所以第i天肯定进行了股票出售，由于只能出售昨天即以上的股票，即dp[i][1] = dp[i - 1][0] + price[i]
    总结：dp[i][1] = dp[i - 1][0] + price[i];
    3.i + 1天不处于冷冻期，且第i天没有任何股票，说明第i天可能处于冷冻期也就说i - 1天进行了股票出售，即dp[i][2] = dp[i - 1][1]
    或者第i天既没有进行股票购买也没有出售即dp[i][2] = dp[i - 1][2]
    总结：dp[i][2] = max(dp[i - 1][1], dp[i - 1][2]);
    综上所述，可以推出最后一天的价值为max(dp[n - 1][0], dp[n - 1][1], dp[n - 1][2])，但由于最后一天就算持有了股票也不可能出售，所以可以去掉dp[n - 1][0]这种情况
    即maxValue = max(dp[n - 1][1], dp[n - 1][2])
    初始化第0天：
    在第0天时，如果持有股票，那么只能是第0天买入的，对应的负收益-price[0]，dp[0][2]表示初始不持有任何股票，所以为0，由于dp[0][1]只会与dp[0][2]影响
    下一轮的dp[1][2]，所以只要使她小于或者等于dp[0][2]即可
    https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/solution/zui-jia-mai-mai-gu-piao-shi-ji-han-leng-dong-qi-4/
    */
    public int maxProfit(int[] prices) {
        int n = prices.length;
        if (n <= 1)
            return 0;
        int[][] dp = new int[n][3];
        dp[0][0] = -prices[0];
        for (int i = 1; i < n; ++i) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][2] - prices[i]);
            dp[i][1] = dp[i - 1][0] + prices[i];
            dp[i][2] = Math.max(dp[i - 1][1], dp[i - 1][2]);
        }
        return Math.max(dp[n - 1][1], dp[n - 1][2]);
    }
}
```



## 79.[戳气球](https://leetcode-cn.com/problems/burst-balloons/)

**题目：**

> 有 `n` 个气球，编号为`0` 到 `n-1`，每个气球上都标有一个数字，这些数字存在数组 `nums` 中。
>
> 现在要求你戳破所有的气球。如果你戳破气球 `i` ，就可以获得 `nums[left] * nums[i] * nums[right]` 个硬币。 这里的 `left` 和 `right` 代表和 `i` 相邻的两个气球的序号。注意当你戳破了气球 `i` 后，气球 `left` 和气球 `right` 就变成了相邻的气球。
>
> 求所能获得硬币的最大数量。
>
> **说明:**
>
> - 你可以假设 `nums[-1] = nums[n] = 1`，但注意它们不是真实存在的所以并不能被戳破。
> - 0 ≤ `n` ≤ 500, 0 ≤ `nums[i]` ≤ 100
>
> **示例:**
>
> ```
> 输入: [3,1,5,8]
> 输出: 167 
> 解释: nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
>      coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167
> ```

**思路：**

//https://leetcode-cn.com/problems/burst-balloons/solution/zhe-ge-cai-pu-zi-ji-zai-jia-ye-neng-zuo-guan-jian-/

**代码：**

```java
class Solution {
    /*
    解题思路：dp forever the god
    设dp[i][j]表示以i为左边界，j为右边界的开区间(i,j)的最大价值，假设我们已经将(i,j)内的气球戳破得只剩下一个k了
    那么此时(i,j)开区间的最大价值可以由dp[i][k]和dp[k][j]进行转移，如果此刻选择戳破k,那么我们得到的金币数量是：
    total = dp[i][k] + val[i]*val[k]*val[j] + dp[k][j];
    而(i,j)区间的气球可能不只k这一个，所以需要对该开区间的所有气球进行计算，并将最大值max赋值给dp[i][j]，即可算出(i,j)开区间的最大价值
    然后呢，就可以从 (i,j) 开区间只有三个数字的时候开始计算，储存每个小区间可以得到金币的最大值
    然后慢慢扩展到更大的区间，利用小区间里已经算好的数字来算更大的区间(即算出所有区间长度为3的dp，接着将长度+1计算为4的dp....直到长度覆盖整个数组)
    */
    public int maxCoins(int[] nums) {
        int n = nums.length;
        if (n < 1)
            return 0;
        int[] vals = new int[n + 2];
        System.arraycopy(nums, 0, vals, 1, n);
        //两个哨兵点防止越界
        vals[0] = 1;
        vals[n + 1] = 1;
        int[][] dp = new int[n + 2][n + 2];
        //区间长度len，从3开始,逐渐覆盖整个数组
        for (int len = 3; len <= n + 2; ++len) {
            //区间起始点i
            for (int i = 0; i <= n + 2 - len; ++i) {
                int max = 0;
                //要删除的点k
                for (int k = i + 1; k < i + len - 1; ++k) {
                    int left = dp[i][k];
                    int right = dp[k][i + len - 1];
                    max = Math.max(max, left + right + vals[i] * vals[k] * vals[i + len - 1]);
                }
                dp[i][i + len - 1] = max;
            }
        }
        return dp[0][n + 1];
    }
}
```



## 80.[零钱兑换](https://leetcode-cn.com/problems/coin-change/)

**题目：**

> 给定不同面额的硬币 `coins` 和一个总金额 `amount`。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 `-1`。
>
> 你可以认为每种硬币的数量是无限的。
>
>  
>
> **示例 1：**
>
> ```
> 输入：coins = [1, 2, 5], amount = 11
> 输出：3 
> 解释：11 = 5 + 5 + 1
> ```
>
> **示例 2：**
>
> ```
> 输入：coins = [2], amount = 3
> 输出：-1
> ```
>
> **示例 3：**
>
> ```
> 输入：coins = [1], amount = 0
> 输出：0
> ```
>
> **示例 4：**
>
> ```
> 输入：coins = [1], amount = 1
> 输出：1
> ```
>
> **示例 5：**
>
> ```
> 输入：coins = [1], amount = 2
> 输出：2
> ```
>
>  
>
> **提示：**
>
> - `1 <= coins.length <= 12`
> - `1 <= coins[i] <= 231 - 1`
> - `0 <= amount <= 104`

**思路：**

[Java 递归、记忆化搜索、动态规划 - 零钱兑换 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/coin-change/solution/javadi-gui-ji-yi-hua-sou-suo-dong-tai-gui-hua-by-s/)
[动态规划、完全背包、BFS（包含完全背包问题公式推导） - 零钱兑换 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/coin-change/solution/dong-tai-gui-hua-shi-yong-wan-quan-bei-bao-wen-ti-/)

**代码：**

```java
class Solution {
    /*
    解题思路：dp
    此问题可以转换为求最小价值的完全背包问题,显然每加一枚硬币相当于价值 + 1，设dp[i][j]表示使用前i种硬币凑齐金额j所需的最小硬币数
    1.外循环：对每种硬币coins[i]进行遍历，表示使用到了前i种硬币
    2.内循环：使用j来表示0 ~ amount的金额（顺序会覆盖以前的状态，所以存在多次选择的情况），考虑到j < coins[i]时不可能加入硬币i，所以
    j可以直接从coins[i]开始遍历：并且上限为k次，其中coins[i] * k <= j，并且可能遇到以下4种情况：
    1.若前i - 1种硬币凑不齐金额j，即dp[i - 1][j]为-1：
        1.若本次dp[i - 1][j - k * coins[i]] 也为-1，表示不能凑整，则dp[i][j] = -1，表示不能凑整；
        2.若本次dp[i - 1][j - k * coins[i]] 不为-1，则dp[i][j] = dp[i - 1][j - k * coins[i]] + 1；
    2.若前i - 1种硬币可以凑齐金额j，即dp[i - 1][j] >= 1：
        1.若本次dp[i - 1][j - k * coins[i]] 也为-1，表示不能凑整，则dp[i][j] = dp[i - 1][j]；
        2.若本次dp[i - 1][j - k * coins[i]] 不为-1，则本次应该取他们的更小者，即dp[i][j] = min(dp[i - 1][j], dp[i - 1][j - k * coins[i]] + 1)；
    初始化：dp[0][0] = 0，并且dp[0][i] = -1 (i > 0)表示没有物品且有金额时一定不能凑整
    优化：由于每轮i的值都只与i - 1轮有关，所以可以使用一维数组代替二维数组
    */
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, -1);
        dp[0] = 0;
        //外循环表示前i个硬币
        for (int i = 0; i < coins.length; ++i) {
            //内循环表示当前金额,应该从coins[i]开始
            for (int j = coins[i]; j <= amount; ++j) {
                if (dp[j - coins[i]] != -1) {
                    if (dp[j] == -1) {
                        dp[j] = dp[j - coins[i]] + 1;
                    } else {
                        dp[j] = Math.min(dp[j - coins[i]] + 1, dp[j]);
                    }
                }
            }
        }
        return dp[amount];
    }
}
```

```java
class Solution {
    /*
    解题思路：dfs + 记忆化搜索 (类dp)
    int dfs(int amount),返回值代表还需凑整的硬币数
    1.初始化：全局变量int cache[j] 表示金额j可以被换取的最少硬币数，为-1表示不能换取，全局变量count = 0
    2.结束条件：
        1.amount == 0表示凑整成功，本次没有使用硬币，直接return 0即可
        2.若amount < 0 并return -1,代表凑整失败
        3.若cache[amount]不为0表示该金额amount访问过，直接返回cache[amount]即可
    3.预先定义变量min用来保存最小值，需要初始化为int的最大值，然后对coins金额进行遍历，使用coins[i]表示：
        1.开启下一层递归dfs(amount - coins[i])并使用res接收其返回值
        2.判断res的大小和res与min的关系：
            1.res == -1:表示凑整失败，跳过即可
            2.res >=0 :表示凑整成功，min = min(min, res + 1),res + 1是因为res仅仅代表减去本次金额后所需的硬币，所以还需加上本次的这一枚硬币
    4.添加缓存：若min未被赋值过表示所有硬币都无法将本次amount凑整成功，直接将cache[amount] = -1，
        反之将其赋值为min即可
    3.返回值：返回cache[amount]即可
    */
    int[] cache;
    int[] coins;
    public int coinChange(int[] coins, int amount) {
        cache = new int[amount + 1];
        this.coins = coins;
        return dfs(amount);
    }

    public int dfs(int amount) {
        if (amount == 0) {
            return 0;
        } else if (amount < 0) {
            return - 1;
        } else if (cache[amount] != 0) {
            return cache[amount];
        }
        int min = Integer.MAX_VALUE;
        for (int i = 0; i < coins.length; ++i) {
            int res = dfs(amount - coins[i]);
            //res为-1表示已经访问过，且不能凑整
            if (res >= 0 && res < min) {
                min = res + 1;//加上本次的硬币
            }
        }
        //本次的结果缓存起来，若min仍未int最大值则表示无法凑整
        cache[amount] = min == Integer.MAX_VALUE ? -1 : min;
        return cache[amount];
    }
}


```

```java
class Solution {
    /*
    解题思路：bfs
    1.初始化：定义队列queue存储剩余金额节点，visited数组保存0 ~ amount金额的访问情况,count保存当前硬币个数
    2.将初始化金额amount加入queue种，当queue不为空时进行循环：
        1.变量size保存本次需要遍历queue的节点个数，size = queue.size()
        2.取出queue种前size个节点，设其为k：
            1.若k == 0：则直接返回count
            2.遍历coins[i],若k - coins[i] > 0且visited[k - coins[i]]为未访问过则将其加入queue中,标记visited[k - coins[i]]为访问过
        3.++count
    3.返回-1表示未找到
    */
    public int coinChange(int[] coins, int amount) {
        if (amount == 0)
            return 0;
        Deque<Integer> queue = new LinkedList<>();
        boolean[] visited = new boolean[amount + 1];
        Arrays.sort(coins);
        int count = 1;
        queue.offer(amount);
        visited[amount] = true;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; ++i) {
                int curAmount = queue.poll();
                for (int coin : coins) {
                    int next = curAmount - coin;
                    if (next == 0) {
                        return count;
                    }
                    if (next < 0) {
                        break;
                    }
                    if (!visited[next]) {
                        queue.offer(next);
                        visited[next] = true;
                    }
                }
            }
            ++count;
        }
        return -1;
    }
}
```

```java
class Solution {
    /*
    解题思路：dp
    此问题可以转换为求最小价值的完全背包问题,显然每加一枚硬币相当于价值 + 1，设dp[i][j]表示使用前i种硬币凑齐金额j所需的最小硬币数
    1.外循环：对每种硬币coins[i]进行遍历，表示使用到了前i种硬币
    2.内循环：使用j来表示0 ~ amount的金额（顺序会覆盖以前的状态，所以存在多次选择的情况），考虑到j < coins[i]时不可能加入硬币i，所以
    j可以直接从coins[i]开始遍历：并且上限为k次，其中coins[i] * k <= j，并且可能遇到以下4种情况：
    1.若前i - 1种硬币凑不齐金额j，即dp[i - 1][j]为-1：
        1.若本次dp[i - 1][j - k * coins[i]] 也为-1，表示不能凑整，则dp[i][j] = -1，表示不能凑整；
        2.若本次dp[i - 1][j - k * coins[i]] 不为-1，则dp[i][j] = dp[i - 1][j - k * coins[i]] + 1；
    2.若前i - 1种硬币可以凑齐金额j，即dp[i - 1][j] >= 1：
        1.若本次dp[i - 1][j - k * coins[i]] 也为-1，表示不能凑整，则dp[i][j] = dp[i - 1][j]；
        2.若本次dp[i - 1][j - k * coins[i]] 不为-1，则本次应该取他们的更小者，即dp[i][j] = min(dp[i - 1][j], dp[i - 1][j - k * coins[i]] + 1)；
    初始化：dp[0][0] = 0，并且dp[0][i] = -1 (i > 0)表示没有物品且有金额时一定不能凑整
    优化：由于每轮i的值都只与i - 1轮有关，所以可以使用一维数组代替二维数组
    */
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        //将数组填充为最小不可能到达的值，也就是假设被全为1的硬币占领的基础上再 + 1
        //其实也能初始化为-1，表示不能凑整，但这样做会导致在循环中增加更多的判断语句，变相加大时间复杂度，而使用最小不可达值则只需判断更小值即可
        Arrays.fill(dp, amount + 1);
        dp[0] = 0;
        //外循环表示前i个硬币
        for (int i = 0; i < coins.length; ++i) {
            //内循环表示当前金额,可以直接从从coins[i]开始，直接顺序遍历表示可以重复取
            for (int j = coins[i]; j <= amount; ++j) {
                dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1);                
            }
        }
        return dp[amount] == amount + 1 ? -1 : dp[amount];
    }
}
```



## 81.[打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/)

**题目：**

> 在上次打劫完一条街道之后和一圈房屋后，小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。
>
> 计算在不触动警报的情况下，小偷一晚能够盗取的最高金额。
>
> **示例 1:**
>
> ```
> 输入: [3,2,3,null,3,null,1]
> 
>      3
>     / \
>    2   3
>     \   \ 
>      3   1
> 
> 输出: 7 
> 解释: 小偷一晚能够盗取的最高金额 = 3 + 3 + 1 = 7.
> ```
>
> **示例 2:**
>
> ```
> 输入: [3,4,5,1,3,null,1]
> 
>      3
>     / \
>    4   5
>   / \   \ 
>  1   3   1
> 
> 输出: 9
> 解释: 小偷一晚能够盗取的最高金额 = 4 + 5 = 9.
> ```

**思路：**

[三种方法解决树形动态规划问题-从入门级代码到高效树形动态规划代码实现 - 打家劫舍 III - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/house-robber-iii/solution/san-chong-fang-fa-jie-jue-shu-xing-dong-tai-gui-hu/)

**代码：**

```java
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
    /*
    解题思路：dfs + 记忆法

    */
    public int rob(TreeNode root) {
        Map<TreeNode, Integer> cache = new HashMap<>();
        return dfs(root, cache);
    }

    public int dfs(TreeNode root, Map<TreeNode, Integer> cache) {
        if (root == null) {
            return 0;
        } else if (cache.containsKey​(root)) {
            return cache.get(root);
        }
        //情况1：加上本次节点与子节点的子节点：
        int sumA = 0;
        sumA += root.val;
        if (root.left != null) {
            sumA += dfs(root.left.left, cache);
            sumA += dfs(root.left.right, cache);
        }
        if (root.right != null) {
            sumA += dfs(root.right.left, cache);
            sumA += dfs(root.right.right, cache);
        }
        //情况2：只加上子节点的值
        int sumB = 0;
        sumB += dfs(root.left, cache);
        sumB += dfs(root.right, cache);
        int max = Math.max(sumB, sumA);
        cache.put(root, max);
        return max;
    }
}
```

```java
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
    /*
    解题思路：dp + 后序遍历
    dp[pNode]表示在pNode能打劫的最大价值
    情况1：选择本节点：dp[pNode] = pNode.val + dp[pNode.left.left] + dp[pNode.left.right] + dp[pNode.right.left] + dp[pNode.right.right]
    情况2：不选择本节点：dp[pNode] = dp[pNode.left] + dp[pNode.right]
    dp[pNode] = Max()
    */
    Map<TreeNode, Integer> dp = new HashMap<>();
    public int rob(TreeNode root) {
        if (root == null)
            return 0;
        dfs(root);
        return dp.get(root);
    }

    public void dfs(TreeNode root) {
        if (root == null) {
            return;
        }
        dfs(root.left);
        dfs(root.right);
        int sumA = 0;
        if (root.left != null) {
            if (root.left.left != null) {
                sumA += dp.get(root.left.left);
            }
            if (root.left.right != null) {
                sumA += dp.get(root.left.right);
            }
        }
        if (root.right != null) {
            if (root.right.left != null) {
                sumA += dp.get(root.right.left);
            }
            if (root.right.right != null) {
                sumA += dp.get(root.right.right);
            }
        }
        sumA += root.val;
        int sumB = 0;
        if (root.left != null) {
            sumB += dp.get(root.left);
        }
        if (root.right != null) {
            sumB += dp.get(root.right);
        }
        int max = Math.max(sumA, sumB);
        dp.put(root, max);
    }
}

```

```java
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
    /*
    解题思路：dp + 后续遍历 + 空间优化
    刚刚的dp使用了空间复杂度为n的dp哈希表，而实际上每轮只与上一轮有关，完全可以省去这n的空间，那么如何进行新的状态转移呢？
    假设每个节点可以选择偷或者不偷两种状态，根据题目意思，相邻节点不能一起偷
    1.当前节点选择偷时，那么两个孩子节点就不能选择偷
    2.当前节点选择不偷时，两个孩子节点只需要拿更多的钱就行（管孩子们偷不偷呢，给我最多就行）
    我们使用大小为2的数组来表示 int[] res = new int[2] 0代表不偷，1代表偷
    任何一个节点能偷到的最大钱的状态可以定义为：
    1.当前节点选择不偷：当前节点能偷到的最大钱数 = 左孩子能偷盗钱的最大 + 右孩子能偷到的钱的最大
    2.当前节点选择偷：当前节点能偷盗的最大钱数 = 左孩子选择自己不偷时能得到的最大钱 + 右孩子选择不偷时能得到的最大钱 + 当前节点的钱
    */
    public int rob(TreeNode root) {
        int[] res = dfs(root);
        return Math.max(res[0], res[1]);
    }

    public int[] dfs(TreeNode root) {
        if (root == null) {
            return new int[2];
        }
        int[] left = dfs(root.left);
        int[] right = dfs(root.right);
        //选择不偷
        int[] res = new int[2];
        res[0] = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
        //选择偷
        res[1] = root.val + left[0] + right[0];
        return res;
    }
}
//参考：https://leetcode-cn.com/problems/house-robber-iii/solution/san-chong-fang-fa-jie-jue-shu-xing-dong-tai-gui-hu/
```

```java
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
    /*
    解题思路：dp + 后续遍历 + 空间优化
    刚刚的dp使用了空间复杂度为n的dp哈希表，而实际上每轮只与上一轮有关，完全可以省去这n的空间，那么如何进行新的状态转移呢？
    假设每个节点可以选择偷或者不偷两种状态，根据题目意思，相邻节点不能一起偷
    1.当前节点选择偷时，那么两个孩子节点就不能选择偷
    2.当前节点选择不偷时，两个孩子节点只需要拿更多的钱就行（管孩子们偷不偷呢，给我最多就行）
    我们使用大小为2的数组来表示 int[] res = new int[2] 0代表不偷，1代表偷
    任何一个节点能偷到的最大钱的状态可以定义为：
    1.当前节点选择不偷：当前节点能偷到的最大钱数 = 左孩子能偷盗钱的最大钱数 + 右孩子能偷到的钱的最大钱数
    2.当前节点选择偷：当前节点能偷盗的最大钱数 = 左孩子选择自己不偷时能得到的最大钱 + 右孩子选择不偷时能得到的最大钱 + 当前节点的钱
    */
    public int rob(TreeNode root) {
        int[] res = dfs(root);
        return res[1];
    }

    public int[] dfs(TreeNode root) {
        if (root == null) {
            return new int[2];
        }
        int[] left = dfs(root.left);
        int[] right = dfs(root.right);
        //选择不偷
        int[] res = new int[2];
        res[0] = left[1] + right[1];
        //选择偷
        res[1] = root.val + left[0] + right[0];
        //res[1]保存两者中更大的
        res[1] = Math.max(res[0], res[1]);
        return res;
    }
}
//参考：https://leetcode-cn.com/problems/house-robber-iii/solution/san-chong-fang-fa-jie-jue-shu-xing-dong-tai-gui-hu/

```

## 82.[比特位计数](https://leetcode-cn.com/problems/counting-bits/)

**题目：**

> 给定一个非负整数 **num**。对于 **0 ≤ i ≤ num** 范围中的每个数字 **i** ，计算其二进制数中的 1 的数目并将它们作为数组返回。
>
> **示例 1:**
>
> ```
> 输入: 2
> 输出: [0,1,1]
> ```
>
> **示例 2:**
>
> ```
> 输入: 5
> 输出: [0,1,1,2,1,2]
> ```
>
> **进阶:**
>
> - 给出时间复杂度为**O(n\*sizeof(integer))**的解答非常容易。但你可以在线性时间**O(n)**内用一趟扫描做到吗？
> - 要求算法的空间复杂度为**O(n)**。
> - 你能进一步完善解法吗？要求在C++或任何其他语言中不使用任何内置函数（如 C++ 中的 **__builtin_popcount**）来执行此操作。

**思路：**

[清晰的思路 - 比特位计数 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/counting-bits/solution/hen-qing-xi-de-si-lu-by-duadua/)

[比特位计数 - 比特位计数 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/counting-bits/solution/bi-te-wei-ji-shu-by-leetcode/)

**代码：**

```java
class Solution {
    /*
    解题思路：dp
    以二进制数的位数分层，逐层求解，例如[10, 11], [100, 101, 110, 111]
    2k为x位的最小二进制数的十进制数，例：1000 -> 8
    res[2k + i] = res[k + i]  其中k > i >=0 
    res[2k + i] = res[k + i] + 1   其中2k > i >= k
    即可按位来分层递推
    */
    public int[] countBits(int num) {
        int[] res = new int[num + 1];
        if (num == 0)
            return res;
        //初始化第0层与第1层
        res[0] = 0;
        res[1] = 1;
        //表示该层起始点与该层个数
        int k = 2;
        while (k <= num) {
            for (int i = k, j = (k >> 1); i < k + (k >> 1) && i <= num; ++i, ++j) {
                res[i] = res[j];
            }
            for (int i = k + (k >> 1), j = (k >> 1); i < (k << 1) && i <= num; ++i, ++j) {
                res[i] = res[j] + 1;
            }
            k = k << 1;
        }
        return res;
    }
}
```

```java
class Solution {
    /*
    解题思路：dp
    对于所有的数字，只有两类：
    1.奇数：二进制数中，奇数一定比前面的偶数加一
    2.偶数：二进制数中，偶数中1的个数一定和除以2之后的那个数一样多
    */
    public int[] countBits(int num) {
        int[] res = new int[num + 1];
        for (int i = 1; i <= num; ++i) {
            if ((i & 1) == 1) {
                res[i] = res[i - 1] + 1;
            } else {
                res[i] = res[i >> 1];
            }
        }
        return res;
    }
}
//参考：https://leetcode-cn.com/problems/counting-bits/solution/hen-qing-xi-de-si-lu-by-duadua/
```

```java
class Solution {
    /*
    解题思路：观察法
    x1 = (1001001)2
    x2 = (100100)2
    可以发现x1与x2只有1位不同，这是因为x2可以看做x1移除最低有效位的结果
    res[x] = res[x / 2] + (x mod 2)
    */
    public int[] countBits(int num) {
        int[] res = new int[num + 1];
        for (int i = 1; i <= num; ++i) {
            res[i] = res[i >> 1] + (i & 1);
        }
        return res;
    }
}
//参考：https://leetcode-cn.com/problems/counting-bits/solution/bi-te-wei-ji-shu-by-leetcode/
```

## 83.[前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)

**题目：**

> 给定一个非空的整数数组，返回其中出现频率前 ***k\*** 高的元素。
>
>  
>
> **示例 1:**
>
> ```
> 输入: nums = [1,1,1,2,2,3], k = 2
> 输出: [1,2]
> ```
>
> **示例 2:**
>
> ```
> 输入: nums = [1], k = 1
> 输出: [1]
> ```
>
>  
>
> **提示：**
>
> - 你可以假设给定的 *k* 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。
> - 你的算法的时间复杂度**必须**优于 O(*n* log *n*) , *n* 是数组的大小。
> - 题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的。
> - 你可以按任意顺序返回答案。

**思路：**

[347. 前 K 个高频元素 --两种常用算法解决 - 前 K 个高频元素 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/top-k-frequent-elements/solution/347-qian-k-ge-gao-pin-yuan-su-by-echo__www/)

**代码：**

```java
class Solution {
    /*
    解题思路：哈希表 + 桶排序
    */
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        //1.桶数组的下标是元素出现的频率，nums有可能只有一个元素，那么需要桶数组下标满足nums.length，所以长度为nums.length + 1，并且遍历时也应该
        //从后往前遍历，应该需要的是出现频率最高的前k个元素
        //2.因为可能存在出现频率相同的元素，所以应该使用集合存储
        List<Integer>[] bucket = new List[nums.length + 1];
        for (int num: map.keySet()) {
            int i = map.get(num);
            if (bucket[i] == null) {
                bucket[i] = new ArrayList();
            }
            bucket[i].add(num);
        }
        int[] res = new int[k];
        int idx = 0;
        //从后向前遍历桶数组
        for (int i = nums.length; i >=0; --i) {
            //遍历不为空的桶
            if (bucket[i] != null) {
                for (int j = 0; j < bucket[i].size(); ++j) {
                    res[idx++] = bucket[i].get(j);
                    if (idx == k)
                        return res;
                }
            }
        }
        return res;
    }
}
```

```java
class Solution {
    /*
    解题思路：小根堆 + 哈希表
    1.建立哈希表map,以数字为键，频率为值，遍历nums并将元素存入
    2.建立按照频率为排序关键的小根堆(根节点小于堆节点)，容量大小为k，遍历map将元素存入
        1.若堆中元素个数少于k则直接存入
        2.若堆中元素个数等于K：则将当前元素于堆头元素比较，若大于堆头元素则先删除堆头，然后加入堆中
    3.将堆中元素弹出保存到结果数组res中
    4.返回res即可
    */
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap();
        for (int num: nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        //按照频率排序的最小堆
        PriorityQueue<Integer> minHeap = new  PriorityQueue((x1, x2) -> map.get(x1) - map.get(x2));
        for (int num: map.keySet()) {
            if (minHeap.size() < k) {
                minHeap.offer(num);
            } else {
                if (map.get(minHeap.peek()) < map.get(num)) {
                    minHeap.poll();
                    minHeap.offer(num);
                }
            }
        }
        int[] res = new int[k];
        for (int i = 0; i < k; ++i) {
            res[i] = minHeap.poll();
        }
        return res;
    }
}
/*参考：
https://leetcode-cn.com/problems/top-k-frequent-elements/solution/347-qian-k-ge-gao-pin-yuan-su-by-echo__www/
*/
```



## 84.[字符串解码](https://leetcode-cn.com/problems/decode-string/)

**题目：**

> 给定一个经过编码的字符串，返回它解码后的字符串。
>
> 编码规则为: `k[encoded_string]`，表示其中方括号内部的 *encoded_string* 正好重复 *k* 次。注意 *k* 保证为正整数。
>
> 你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。
>
> 此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 *k* ，例如不会出现像 `3a` 或 `2[4]` 的输入。
>
>  
>
> **示例 1：**
>
> ```
> 输入：s = "3[a]2[bc]"
> 输出："aaabcbc"
> ```
>
> **示例 2：**
>
> ```
> 输入：s = "3[a2[c]]"
> 输出："accaccacc"
> ```
>
> **示例 3：**
>
> ```
> 输入：s = "2[abc]3[cd]ef"
> 输出："abcabccdcdcdef"
> ```
>
> **示例 4：**
>
> ```
> 输入：s = "abc3[cd]xyz"
> 输出："abccdcdcdxyz"
> ```

**思路：**

[字符串解码（辅助栈法 / 递归法，清晰图解） - 字符串解码 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/decode-string/solution/decode-string-fu-zhu-zhan-fa-di-gui-fa-by-jyd/)

**代码：**

```java
class Solution {
    /*
    解题思路：找边界的递归
    */
    public String decodeString(String s) {
        int idx = 0;
        StringBuilder builder = new StringBuilder();
        while (idx < s.length()) {
            char ch = s.charAt(idx);
            if (ch >= '0' && ch <= '9') {
                int numEnd = findNumEnd(s, idx + 1);
                //将String转为int
                int count = Integer.valueOf(s.substring(idx, numEnd));
                //找到右括号
                int strEnd = findStrEnd(s, ++numEnd);
                String sub = decodeString(s.substring(numEnd, strEnd));
                buildString(builder, count, sub);
                idx = ++strEnd;
            } else {
                builder.append(ch);
                ++idx;
            }
        }
        return builder.toString();
    }

    public void buildString(StringBuilder builder, int count, String sub) {
        for (int i = 0; i < count; ++i) {
            builder.append(sub);
        }
    }

    public int findNumEnd(String s, int start) {
        int i = start;
        while (s.charAt(i) != '[') {
            ++i;
        }
        return i;
    }

    //找到被[]包括的字符串的最右侧元素 + 1的下标
    public int findStrEnd(String s, int start) {
        int i = start;
        int left = 1;
        while (left != 0) {
            char ch = s.charAt(i);
            if (ch == '[') {
                ++left;
            } else if (ch == ']') {
                --left;
            }
            ++i;
        }
        return i - 1;
    }
}

```

```java
class Solution {
    /*
    解题思路：辅助栈
    本题难点在于括号内嵌套括号，需要从内向外生成与拼接字符串，这与栈的先入后出的特性对应。
    1.构建辅助栈stack,遍历字符串s中的每个字符c:
        ·当c为数字时，将数字字符转化为数字multi,用于后续遍历计算;
        ·当c为字母时，在res尾部添加c;
        ·当c为[时，将当前multi和res入栈(即保存状态)，并将muilt与res分别置0置空
        ·当c为]时，stack出栈，拼接字符串res = last_res + cur_multi * res
            ·last_res是上个[到当前[的字符串,例如"3[a2[c]]"中的a;
            ·cur_multi是当前[到]内字符串的重复倍数例如"3[a2[c]]"中的2
    2.返回字符串res
    */
    public String decodeString(String s) {
        Deque<Integer> numStack = new LinkedList();
        Deque<String> strStack = new LinkedList();
        int multi = 0;
        StringBuilder res = new StringBuilder();
        for (char ch: s.toCharArray()) {
            if (ch >= '0' && ch <= '9') {
                multi = 10 * multi + (ch - '0');
            } else if (ch == '[') {
                numStack.offer(multi);
                strStack.offer(res.toString());
                multi = 0;
                res = new StringBuilder();
            } else if (ch == ']') {
                StringBuilder tmp = new StringBuilder();
                Integer cur_multi = numStack.pollLast();
                for (int i = 0; i < cur_multi; ++i) {
                    tmp.append(res);
                }
                res = new StringBuilder(strStack.pollLast() + tmp);
            } else {
                res.append(ch);
            }
        }
        return res.toString();
    }
}
/*参考：
https://leetcode-cn.com/problems/decode-string/solution/decode-string-fu-zhu-zhan-fa-di-gui-fa-by-jyd/
*/
```

```java
class Solution {
    /*
    解题思路：递归法
    ·总体思路与辅助栈法一致，不同点在于将[和]分别作为递归开启与终止的条件：
        ·当s[i] == ']'时，返回当前括号记录的res字符串与]的索引i(用于更新上次递归指针)
        ·当s[i] == '['时，开启新的一轮递归，记录此[...]内的字符串tmp和递归后的新索引i
        并执行res + multi * tmp拼接字符串
        ·遍历完毕后返回res
    */
    public String decodeString(String s) {
        return dfs(s);
    }
    int idx = 0;
    public String dfs(String s) {
        StringBuilder res = new StringBuilder();
        int multi = 0;
        while (idx < s.length()) {
            char ch = s.charAt(idx);
            if (ch >= '0' && ch <= '9') {
                multi = multi * 10 + (ch - '0');
            } else if (ch == '[') {
                ++idx;
                String sub = dfs(s);
                for (int i = 0; i < multi; ++i) {
                    res.append(sub);
                }
                multi = 0;
            } else if (ch == ']') {
                return res.toString();
            } else {
                res.append(ch);
            }
            ++idx;
        }
        return res.toString();
    }
}
/*参考：
https://leetcode-cn.com/problems/decode-string/solution/decode-string-fu-zhu-zhan-fa-di-gui-fa-by-jyd/
*/
```

## 85.[ 除法求值](https://leetcode-cn.com/problems/evaluate-division/)

**题目：**

> 给出方程式 `A / B = k`, 其中 `A` 和 `B` 均为用字符串表示的变量， `k` 是一个浮点型数字。根据已知方程式求解问题，并返回计算结果。如果结果不存在，则返回 `-1.0`。
>
> 输入总是有效的。你可以假设除法运算中不会出现除数为 0 的情况，且不存在任何矛盾的结果。
>
>  
>
> **示例 1：**
>
> ```
> 输入：equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
> 输出：[6.00000,0.50000,-1.00000,1.00000,-1.00000]
> 解释：
> 给定：a / b = 2.0, b / c = 3.0
> 问题：a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ?
> 返回：[6.0, 0.5, -1.0, 1.0, -1.0 ]
> ```
>
> **示例 2：**
>
> ```
> 输入：equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
> 输出：[3.75000,0.40000,5.00000,0.20000]
> ```
>
> **示例 3：**
>
> ```
> 输入：equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
> 输出：[0.50000,2.00000,-1.00000,-1.00000]
> ```
>
>  
>
> **提示：**
>
> - `1 <= equations.length <= 20`
> - `equations[i].length == 2`
> - `1 <= equations[i][0].length, equations[i][1].length <= 5`
> - `values.length == equations.length`
> - `0.0 < values[i] <= 20.0`
> - `1 <= queries.length <= 20`
> - `queries[i].length == 2`
> - `1 <= queries[i][0].length, queries[i][1].length <= 5`
> - `equations[i][0], equations[i][1], queries[i][0], queries[i][1]` 由小写英文字母与数字组成

**思路：**

https://www.cnblogs.com/wangyuliang/p/9216365.html
https://leetcode-cn.com/problems/evaluate-division/solution/san-chong-jie-fa-by-baymaxhwy/



**代码：**

```java
class Solution {
    /*
    解题思路：Floyd算法
    */
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
        int count = 0;
        //给每个节点赋值一个下标，map用来实现节点名与下标的映射
        Map<String, Integer> map = new HashMap();
        for (List<String> equation: equations) {
            for (String s: equation) {
                if (!map.containsKey(s)) {
                    map.put(s, count++);
                }
            }
        }
        //构建一个矩阵代替图结构
        double[][] graph = new double[count][count];
        //初始化
        for (String s: map.keySet()) {
            int idx = map.get(s);
            graph[idx][idx] = 1.0;
        }
        //构建双向边
        int index = 0;
        for (List<String> equation: equations) {
            String a = equation.get(0), b = equation.get(1);
            int idxA = map.get(a), idxB = map.get(b);
            double value = values[index++];
            graph[idxA][idxB] = value;
            graph[idxB][idxA] = 1 / value;
        }
        //Floyd算法,将每个点逐个加入中转点中
        for (int k = 0; k < count; ++k) {
            for (int i = 0; i < count; ++i) {
                for (int j = 0; j < count; ++j) {
                    if (i == j || graph[i][j] != 0) {
                        continue;
                    }
                    if (graph[i][k] != 0 && graph[k][j] != 0) {
                        graph[i][j] = graph[i][k] * graph[k][j];
                    }
                }
            }
        }
        
        double[] res = new double[queries.size()];
        for (int i = 0; i < res.length; ++i) {
            List<String> query = queries.get(i);
            String a = query.get(0), b = query.get(1);
            if (map.containsKey(a) && map.containsKey(b)) {
                double ans = graph[map.get(a)][map.get(b)];
                res[i] = ans == 0 ? -1.0 : ans;
            } else {
                res[i] = -1.0;
            }
        }
        return res;
    }
}
/*参考：
https://www.cnblogs.com/wangyuliang/p/9216365.html
https://leetcode-cn.com/problems/evaluate-division/solution/san-chong-jie-fa-by-baymaxhwy/
*/
```

```java
class Solution {
    /*
    解题思路：带权并查集--路径压缩
    实现思路：
    1.维护两个hash表，parent表记录每个节点的祖先节点，dist表记录该节点到祖先节点(root)的距离(dist[i] = i / root)
    2.初始化每个节点为一个集合parent[i] = i, dist[i] = 1
    3.遍历equations、values数组，每次将两个不同集合合并
    4.遍历queries数组，如果存在这两个节点，并且它们(a, b)都处于同一个集合中，则向结果数组res添加dist[a] / dist[b],否则添加-1.0
    集合合并（ra为a的父节点，rb为b的父节点），现在已知的条件有：
    ·x1 = a / ra， x2 = b / rb
    ·a / b = v
    现在需要将两个集合进行合并的话，就是将a / b = v这条关系加入到集合中，那么可以给ra -> b连一条关系
    设ra -> b的这条边的值为 t ,那么可以表示为 ra / b = t，现在只需要求出t的值即可，经过公式推导可以得到 t = v / x1
    公式推导：
    ra -> b ,假设t = ra / b
    a / b = (a / ra) * (ra / b) = v 且 a / ra = x1, ra / b = t
    所以t = v / x1

    如何计算a / b
    前提：需要保证a、b处于同一集合，否则无法计算
    ·因为dist[a] = a / ra, dist[b] = b / rb
    ·且a、b处于同一集合，那么ra = rb = root
    ·可以得到a / b = (a / root) / (b / root) = dist[a] / dist[b]
    */
    Map<String, String> parent = new HashMap();
    Map<String, Double> dist = new HashMap();
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
        //初始化所有点
        for (List<String> equation : equations) {
            String a = equation.get(0), b = equation.get(1);
            parent.put(a, a);
            parent.put(b, b);
            dist.put(a, 1.0);
            dist.put(b, 1.0);
        }
        //集合合并
        for (int i = 0; i < equations.size(); ++i) {
            List<String> equation = equations.get(i);
            String a = equation.get(0), b = equation.get(1);
            //以下代码完成的是ra -> b
            String ra = findParent(a);//找到a的祖先节点
            //更新ra节点
            parent.put(ra, b);
            dist.put(ra, values[i] / dist.get(a));
        }
        double[] res = new double[queries.size()];
        int i = 0;
        for (List<String> query: queries) {
            String a = query.get(0), b = query.get(1);
            if (!parent.containsKey(a) || !parent.containsKey(b) || !findParent(a).equals(findParent(b))) {
                res[i] = -1.0;
            } else {
                res[i] = dist.get(a) / dist.get(b);
            }
            ++i;
        }
        return res;
    }

    //寻找父节点的同时压缩路径
    public String findParent(String cur) {
        //当cur的父节点不为本身时递归寻找父节点，并同时压缩路径
        if (!parent.get(cur).equals(cur)) {
            String papa = findParent(parent.get(cur));
            //可以从数学意义上推导：a / c = (a / b) * (b / c)
            dist.put(cur, dist.get(cur) * dist.get(parent.get(cur)));
            parent.put(cur, papa);
        }
        return parent.get(cur);
    }
}
/*参考：
https://leetcode-cn.com/problems/evaluate-division/solution/san-chong-jie-fa-by-baymaxhwy/
*/
```



## 86.[根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)

**题目：**

> 假设有打乱顺序的一群人站成一个队列，数组 `people` 表示队列中一些人的属性（不一定按顺序）。每个 `people[i] = [hi, ki]` 表示第 `i` 个人的身高为 `hi` ，前面 **正好** 有 `ki` 个身高大于或等于 `hi` 的人。
>
> 请你重新构造并返回输入数组 `people` 所表示的队列。返回的队列应该格式化为数组 `queue` ，其中 `queue[j] = [hj, kj]` 是队列中第 `j` 个人的属性（`queue[0]` 是排在队列前面的人）。
>
>  
>
> 
>
> **示例 1：**
>
> ```
> 输入：people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
> 输出：[[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
> 解释：
> 编号为 0 的人身高为 5 ，没有身高更高或者相同的人排在他前面。
> 编号为 1 的人身高为 7 ，没有身高更高或者相同的人排在他前面。
> 编号为 2 的人身高为 5 ，有 2 个身高更高或者相同的人排在他前面，即编号为 0 和 1 的人。
> 编号为 3 的人身高为 6 ，有 1 个身高更高或者相同的人排在他前面，即编号为 1 的人。
> 编号为 4 的人身高为 4 ，有 4 个身高更高或者相同的人排在他前面，即编号为 0、1、2、3 的人。
> 编号为 5 的人身高为 7 ，有 1 个身高更高或者相同的人排在他前面，即编号为 1 的人。
> 因此 [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] 是重新构造后的队列。
> ```
>
> **示例 2：**
>
> ```
> 输入：people = [[6,0],[5,0],[4,0],[3,2],[2,2],[1,4]]
> 输出：[[4,0],[5,0],[2,2],[3,2],[1,4],[6,0]]
> ```
>
>  
>
> **提示：**
>
> - `1 <= people.length <= 2000`
> - `0 <= hi <= 106`
> - `0 <= ki < people.length`
> - 题目数据确保队列可以被重建

**思路：**

https://leetcode-cn.com/problems/queue-reconstruction-by-height/solution/xian-pai-xu-zai-cha-dui-dong-hua-yan-shi-suan-fa-g/

**代码：**

```java
class Solution {
    /*
    解题思路：排序
    *一般对于这种数对，还涉及排序的，根据第一个元素正向排序，根据第二个元素反向排序，或者根据第一个元素反向排序，根据第二个元素正向排序，往往能够简化解题过程
    在本题中，首先对数对进行排序，按照数对的元素0降序排序，按照数对的元素1升序排序，原因是，按照元素0进行降序排序，对每个元素，在其之前的元素个数，就是大于等于
    他的元素数量，而按照第二个元素正向排序，我们希望k大的尽量在后面，减少插入操作次数(可以作为它的下标)
    具体实现：
    1.按第一个元素降序，第二个元素升序排序
    2.创建结果集合res大小为people.length
    3.遍历从前往后遍历people：
        1.若res容量小于等于p[1]则直接将其加入res尾部
        2.若res容量大于p[1]，由于前面加入的元素经过排序后必定大于等于该元素，所以直接插入到res[p[1]]位置即可
    */
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people, (o1, o2) -> o1[0] - o2[0] == 0 ? o1[1] - o2[1] : o2[0] - o1[0]);
        LinkedList<int[]> res = new LinkedList();
        for (int[] p: people) {
            res.add(p[1], p);
        }
        return res.toArray(new int[people.length][]);
    }
}
/*参考：
https://leetcode-cn.com/problems/queue-reconstruction-by-height/solution/xian-pai-xu-zai-cha-dui-dong-hua-yan-shi-suan-fa-g/
*/
```



## 87.[分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

**题目：**

> 给定一个**只包含正整数**的**非空**数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。
>
> **注意:**
>
> 1. 每个数组中的元素不会超过 100
> 2. 数组的大小不会超过 200
>
> **示例 1:**
>
> ```
> 输入: [1, 5, 11, 5]
> 
> 输出: true
> 
> 解释: 数组可以分割成 [1, 5, 5] 和 [11].
> ```
>
>  
>
> **示例 2:**
>
> ```
> 输入: [1, 2, 3, 5]
> 
> 输出: false
> 
> 解释: 数组不能分割成两个元素和相等的子集.
> ```

**思路：**

https://leetcode-cn.com/problems/partition-equal-subset-sum/solution/0-1-bei-bao-wen-ti-xiang-jie-zhen-dui-ben-ti-de-yo/

https://leetcode-cn.com/problems/partition-equal-subset-sum/solution/shou-hua-tu-jie-fen-ge-deng-he-zi-ji-dfshui-su-si-/

**代码：**

```java
class Solution {
    /*
    解题思路：dp
    ·状态定义：dp[i][j]表示从数组的[0,i]这个子区间挑选一些正整数，每个数只能使用一次，使得它们的和恰好等于j
    ·状态转移方程：
        ·不选择nums[i]，如果在[0, i - 1]这个子区间内已经有一部分元素，使得它们的和等于j,那么dp[i][j] = true
        ·选择nuns[i],如果在[0, i - 1]这个子区间内找到一部分元素使得它们和为j - nums[i]
    dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i]]
    但是还需要考虑nums[i] = j (不考虑也无所谓，但是需要进行初始化首个元素，毕竟我们依然可以求出nums[i]之外的和为j)所以完整的方程为
    dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i]] || nums[i] == j
    */
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int num: nums) {
            sum += num;
        }
        //为奇数不可分
        if ((sum & 1) == 1)
            return false;
        int target = sum >> 1;
        boolean[] dp = new boolean[target + 1];
        //初始化
        if (nums[0] <= target) {
            dp[nums[0]] = true;
        }
        for (int i = 1; i < nums.length; ++i) {
            for (int j = target; j >= nums[i]; --j) {
                if (dp[target])
                    return true;
                dp[j] = dp[j] || dp[j - nums[i]];
            }
        }
        return dp[target];
    }
}
/*参考：
https://leetcode-cn.com/problems/partition-equal-subset-sum/solution/0-1-bei-bao-wen-ti-xiang-jie-zhen-dui-ben-ti-de-yo/
*/
```

```java
class Solution {
    /*
    解题思路：dfs + 记忆化搜索
    */
    public boolean canPartition(int[] nums) {
        Arrays.sort(nums);
        int sum = 0;
        for (int num: nums) {
            sum += num;
        }
        if ((sum & 1) == 1)
            return false;
        int target = sum >> 1;
        return dfs(nums, 0, target);
    }

    Map<String, Boolean> cache = new HashMap();
    public boolean dfs(int[] nums, int i, int sum) {
        String key = sum + "&" + i;
        if (cache.containsKey(key))
            return cache.get(key);
        if (i >= nums.length || sum < 0) {
            return false;
        }
        if (sum == 0)
            return true;
        boolean res = dfs(nums, i + 1, sum - nums[i]) || dfs(nums, i + 1, sum);
        cache.put(key, res);
        return res;
    }
}
/*参考：
https://leetcode-cn.com/problems/partition-equal-subset-sum/solution/shou-hua-tu-jie-fen-ge-deng-he-zi-ji-dfshui-su-si-/
*/
```



## 88.[ 路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)

**题目：**

> 给定一个二叉树，它的每个结点都存放着一个整数值。
>
> 找出路径和等于给定数值的路径总数。
>
> 路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。
>
> 二叉树不超过1000个节点，且节点数值范围是 [-1000000,1000000] 的整数。
>
> **示例：**
>
> ```
> root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8
> 
>       10
>      /  \
>     5   -3
>    / \    \
>   3   2   11
>  / \   \
> 3  -2   1
> 
> 返回 3。和等于 8 的路径有:
> 
> 1.  5 -> 3
> 2.  5 -> 2 -> 1
> 3.  -3 -> 11
> ```

**思路：**

https://leetcode-cn.com/problems/path-sum-iii/solution/qian-zhui-he-di-gui-hui-su-by-shi-huo-de-xia-tian/

https://leetcode-cn.com/problems/path-sum-iii/solution/c-dfs-qian-xu-he-by-cui-jin-hao-_official/

**代码：**

```java
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
    /*
    解题思路：前缀和
    ·前缀和：到达当前元素的路径上，之前所有元素的和
    设节点A和节点B相差target，则位于节点A和节点B之间的元素之和是target.
    因为本题中路径是一棵树，从根往任一节点的路径上，有且仅有一条路径，因为不存在环
    抵达节点B后，将前缀和累加，然后查找在前缀和上，有没有前缀和curSum - target的节点(即A节点)，存在即表示从A到B有一条路径之和满足条件的情况。
    结果加上满足前缀和curSum - target 的节点数量。然后递归左右子树
    左右子树递归完毕后，回到当前层，需要把当前节点的前缀和去除。避免回溯之后影响上一层。

    时间复杂度：每个节点只遍历一次,O(N).
    空间复杂度：开辟了一个hashMap,O(N).
    */
    public int pathSum(TreeNode root, int sum) {
        Map<Integer, Integer> prefixSumCnt = new HashMap();
        //初始化
        prefixSumCnt.put(0, 1);
        return help(root, 0, sum, prefixSumCnt);
    }

    public int help(TreeNode root, int curSum, int target, Map<Integer, Integer> prefixSumCnt) {
        if (root == null)
            return 0;
        curSum += root.val;
        int res = 0;
        res += prefixSumCnt.getOrDefault(curSum - target, 0);
        //将对应前缀+1
        prefixSumCnt.put(curSum, prefixSumCnt.getOrDefault(curSum, 0) + 1);
        //左右子树递归
        res += help(root.left, curSum, target, prefixSumCnt);
        res += help(root.right, curSum, target, prefixSumCnt);
        //还原
        prefixSumCnt.put(curSum, prefixSumCnt.get(curSum) - 1);
        return res;
    }
}
/*参考：
https://leetcode-cn.com/problems/path-sum-iii/solution/qian-zhui-he-di-gui-hui-su-by-shi-huo-de-xia-tian/
*/
```

```java
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
    /*
    解题思路：dfs
    关键：boolean变量isHead保证不是头节点的链一定能走到叶子节点并且不会重新置0
    */
    public int pathSum(TreeNode root, int sum) {
        return dfs(root, sum, true);
    }

    public int dfs(TreeNode root, int sum, boolean isHead) {
        if (root == null)
            return 0;
        int count = 0;
        if (sum - root.val == 0)
            ++count;
        if (isHead) {
            count += dfs(root.left, sum, true);
            count += dfs(root.right, sum, true);
        }
        count += dfs(root.left, sum - root.val, false);
        count += dfs(root.right, sum - root.val, false);
        return count;
    }
}
/*参考：
https://leetcode-cn.com/problems/path-sum-iii/solution/c-dfs-qian-xu-he-by-cui-jin-hao-_official/
*/


```

## 89.[找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

**题目：**

> 给定一个字符串 **s** 和一个非空字符串 **p**，找到 **s** 中所有是 **p** 的字母异位词的子串，返回这些子串的起始索引。
>
> 字符串只包含小写英文字母，并且字符串 **s** 和 **p** 的长度都不超过 20100。
>
> **说明：**
>
> - 字母异位词指字母相同，但排列不同的字符串。
> - 不考虑答案输出的顺序。
>
> **示例 1:**
>
> ```
> 输入:
> s: "cbaebabacd" p: "abc"
> 
> 输出:
> [0, 6]
> 
> 解释:
> 起始索引等于 0 的子串是 "cba", 它是 "abc" 的字母异位词。
> 起始索引等于 6 的子串是 "bac", 它是 "abc" 的字母异位词。
> ```
>
>  **示例 2:**
>
> ```
> 输入:
> s: "abab" p: "ab"
> 
> 输出:
> [0, 1, 2]
> 
> 解释:
> 起始索引等于 0 的子串是 "ab", 它是 "ab" 的字母异位词。
> 起始索引等于 1 的子串是 "ba", 它是 "ab" 的字母异位词。
> 起始索引等于 2 的子串是 "ab", 它是 "ab" 的字母异位词。
> ```

**思路：**

https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/solution/javayou-hua-labuladongda-lao-hua-dong-chuang-kou-t/

**代码：**

```java
class Solution {
    /*
    解题思路：滑动窗口
    1.初始化：需求字符数组need，窗口内被need所需要的字符数组window，左右指针left, right初始为0，检测窗口中是否
    已经涵盖p中所有字符的int变量total，初始为p的长度
    2.初始化need[]：遍历p中的每个元素p[i],并将need中对应字符的个数 + 1，即need[p[i]]++
    3.在right指针不越界的条件下遍历s中的每个字符：
        1.若当前right所指示字符为需求字符，则将window中对应字符个数 + 1
        若当前windows中字符个数不超过需求字符个数(防止重复)，则将 total - 1
        2.若total为0表示当前窗口中已经涵盖了所有需求字符，进行循环：
            1.若当前窗口大小恰好等于p的长度，说明该字符串一定满足要求，将left加入res中即可
            2.右移left指针：
                1.若当前字符是被need所需要的，则将window对应字符个数 - 1
                2.如果window将对应字符 - 1后小于需求字符，说明窗口减去的字符是个关键字符，对应的total需要 + 1
                3.left指针右移一位
        4.经过上述操作后，total肯定不为0，需要加入新的元素，所以右移一位right
    */
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> res = new ArrayList();
        if (s == null || p.length() == 0)
            return res;
        int[] need = new int[26];
        int[] window = new int[26];
        int left = 0, right = 0, total = p.length();
        for (char ch: p.toCharArray()) {
            ++need[ch - 'a'];
        }
        while (right < s.length()) {
            char ch = s.charAt(right);
            if (need[ch - 'a'] > 0) {
                ++window[ch - 'a'];
                if (window[ch - 'a'] <= need[ch - 'a']) {
                    --total;
                }
            }
            while (total == 0) {
                if (right - left + 1 == p.length()) {
                    res.add(left);
                }
                char c = s.charAt(left);
                if (need[c - 'a'] > 0) {
                    --window[c - 'a'];
                    if (window[c - 'a'] < need[c - 'a']) {
                        ++total;
                    }
                }
                ++left;
            }
            ++right;
        }
        return res;
    }
}
/*参考：
https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/solution/javayou-hua-labuladongda-lao-hua-dong-chuang-kou-t/
*/
```



## 90.找到所有数组中消失的数字

**题目：**

> 给定一个范围在  1 ≤ a[i] ≤ n ( n = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。
>
> 找到所有在 [1, n] 范围之间没有出现在数组中的数字。
>
> 您能在不使用额外空间且时间复杂度为O(n)的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。
>
> 示例:
>
> 输入:
> [4,3,2,7,8,2,3,1]
>
> 输出:
> [5,6]

**思路：**

https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/solution/xiang-xi-tu-jie-448zhao-dao-suo-you-shu-zu-zhong-x/

**代码：**

```java
class Solution {
    /*
    解题思路：原地哈希
    1.遍历nums数组，设每个元素为nums[i]：
    若当前元素值 - 1不等于下标并且nums[nums[i] - 1]不等于nums[i - 1]时交换nums[i]与nums[nums[i] - 1]
    2.再次遍历数组，将下标 + 1不等于nums[i]的i + 1加入res即可
    3.返回res
    */
    public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> res = new ArrayList();
        for (int i = 0; i < nums.length; ++i) {
            while (nums[nums[i] - 1] != nums[i] && nums[i] - 1 != i) {
                int tmp = nums[nums[i] - 1];
                nums[nums[i] - 1] = nums[i];
                nums[i] = tmp;
            }
        }
        for (int i = 0; i < nums.length; ++i) {
            if (nums[i] - 1 != i) {
                res.add(i + 1);
            }
        }
        return res;
    }
}
```

```java
class Solution {
    /*
    解题思路：原地哈希 + 判断正负
    遍历nums,将abs(nums[i]) - 1作为下标index,若nums[index]为负数说明该数被访问过，若为正数则表示未被访问过，将其置为-nums[index]
    遍历结束后，若仍有nums[i]大于0说明其下标未被访问过（未被置为负数过）
    所以再次遍历数组返回这些正数的元素对应的下标 + 1即可
    */
    public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> res = new ArrayList();
        for (int i = 0; i < nums.length; ++i) {
            int index = Math.abs(nums[i]) - 1;
            if (nums[index] > 0) {
                nums[index] *= -1;
            }
        }
        for (int i = 0; i < nums.length; ++i) {
            if (nums[i] > 0) {
                res.add(i + 1);
            }
        }
        return res;
    }
}
/*参考：
https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/solution/xiang-xi-tu-jie-448zhao-dao-suo-you-shu-zu-zhong-x/
*/
```



## 91.[汉明距离](https://leetcode-cn.com/problems/hamming-distance/)

**题目：**

> 两个整数之间的[汉明距离](https://baike.baidu.com/item/汉明距离)指的是这两个数字对应二进制位不同的位置的数目。
>
> 给出两个整数 `x` 和 `y`，计算它们之间的汉明距离。
>
> **注意：**
> 0 ≤ `x`, `y` < 231.
>
> **示例:**
>
> ```
> 输入: x = 1, y = 4
> 
> 输出: 2
> 
> 解释:
> 1   (0 0 0 1)
> 4   (0 1 0 0)
>        ↑   ↑
> 
> 上面的箭头指出了对应二进制位不同的位置。
> ```

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：异或 + 逐位求1的个数
    */
    public int hammingDistance(int x, int y) {
        int tmp  = (x ^ y);
        int count = 0;
        for (int i = 0, mult = 1; i < 32; ++i, mult = mult << 1) {
            if ((tmp & mult) != 0)
                ++count;
        }
        return count;
    }
}
```



```java
class Solution {
    /*
    解题思路：异或 + 逐位求1的个数
    */
    public int hammingDistance(int x, int y) {
        int tmp  = x ^ y;
        int count = 0;
        while (tmp != 0) {
            count += tmp & 1;
            tmp = tmp >> 1;
        }
        return count;
    }
}
```



## 92.[目标和](https://leetcode-cn.com/problems/target-sum/)

**题目：**

> 给定一个非负整数数组，a1, a2, ..., an, 和一个目标数，S。现在你有两个符号 `+` 和 `-`。对于数组中的任意一个整数，你都可以从 `+` 或 `-`中选择一个符号添加在前面。
>
> 返回可以使最终数组和为目标数 S 的所有添加符号的方法数。
>
>  
>
> **示例：**
>
> ```
> 输入：nums: [1, 1, 1, 1, 1], S: 3
> 输出：5
> 解释：
> 
> -1+1+1+1+1 = 3
> +1-1+1+1+1 = 3
> +1+1-1+1+1 = 3
> +1+1+1-1+1 = 3
> +1+1+1+1-1 = 3
> 
> 一共有5种方法让最终目标和为3。
> ```
>
>  
>
> **提示：**
>
> - 数组非空，且长度不会超过 20 。
> - 初始的数组的和不会超过 1000 。
> - 保证返回的最终结果能被 32 位整数存下。

**思路：**

https://leetcode-cn.com/problems/target-sum/solution/dong-tai-gui-hua-si-kao-quan-guo-cheng-by-keepal/

https://leetcode-cn.com/problems/target-sum/solution/huan-yi-xia-jiao-du-ke-yi-zhuan-huan-wei-dian-xing/
https://leetcode-cn.com/problems/target-sum/solution/c-dfshe-01bei-bao-by-bao-bao-ke-guai-liao/

**代码：**

```java
class Solution {
    /*
    解题思路：转换成背包问题,dp
    设dp[i][j]表示前i个数字组成目标数为j的个数，根据题意可得，nums[i]可以加也可以减
    则dp[i][j] = dp[i - 1][j - nums[i]] + dp[i][j + nums[i]]，需要判断其越界
    值得注意的是，因为每个数字都能取到正号或符号，nums[i]的和的范围是[-sum, +sum]，再加上一个0
    那么上述的目标数的大小就应该是2 * sum + 1，其中 [0, sum - 1]表示负数，sum表示0，[sum, 2 * sum]表示正数
    初始化：
    若nums[0]为0，那么无论+0还是-0结果都是0，直接置dp[0][sum] = 2即可
    若nums[0]不为0，直接置dp[0][sum - num[i]] = 1,dp[0][sum + nums[i]] = 1
    空间优化：
    由于每次的结果只与上次的有关，所以可以只使用2个数组即可
    */
    public int findTargetSumWays(int[] nums, int S) {
        if (nums.length == 0)
            return 0;
        int sum = 0;
        for (int num: nums) {
            sum += num;
        }
        //S不可能比sum还大
        if (Math.abs(S) > sum) {
            return 0;
        }
        int t = 2 * sum + 1;
        int[][] dp = new int[2][t];
        int flag = 0;
        dp[flag][sum - nums[0]] += 1;
        dp[flag][sum + nums[0]] += 1;
        flag ^= 1;
        for (int i = 1; i < nums.length; ++i) {
            for (int j = 0; j < t; ++j) {
                int l = j - nums[i] < 0 ? 0 : j - nums[i];
                int r = j + nums[i] >= t ? 0 : j + nums[i];
                dp[flag][j] = dp[flag ^ 1][l] + dp[flag ^ 1][r];
            }
            flag ^= 1;
        }
        return dp[flag ^ 1][sum + S];
    }
}
/*参考：
https://leetcode-cn.com/problems/target-sum/solution/dong-tai-gui-hua-si-kao-quan-guo-cheng-by-keepal/
*/
```

```java
class Solution {
    /*
    解题思路：转换为01背包

    */
    public int findTargetSumWays(int[] nums, int S) {
        int sum = 0;
        for (int num: nums) {
            sum += num;
        }
        if (((sum + S) & 1) == 1 || S > sum)
            return 0;
        int len = (sum + S) >> 1;
        int[] dp = new int[len + 1];
        dp[0] = 1;
        for (int num: nums) {
            for (int i = len; i >= num; --i) {
                dp[i] += dp[i - num];
            }
        }
        return dp[len];
    }
}
/*参考
https://leetcode-cn.com/problems/target-sum/solution/huan-yi-xia-jiao-du-ke-yi-zhuan-huan-wei-dian-xing/
https://leetcode-cn.com/problems/target-sum/solution/c-dfshe-01bei-bao-by-bao-bao-ke-guai-liao/
*/
```



## 93.[把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

**题目：**

> 给出二叉 **搜索** 树的根节点，该树的节点值各不相同，请你将其转换为累加树（Greater Sum Tree），使每个节点 `node` 的新值等于原树中大于或等于 `node.val` 的值之和。
>
> 提醒一下，二叉搜索树满足下列约束条件：
>
> - 节点的左子树仅包含键 **小于** 节点键的节点。
> - 节点的右子树仅包含键 **大于** 节点键的节点。
> - 左右子树也必须是二叉搜索树。
>
> **注意：**本题和 1038: https://leetcode-cn.com/problems/binary-search-tree-to-greater-sum-tree/ 相同
>
>  
>
> **示例 1：**
>
> ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/05/03/tree.png)
>
> ```
> 输入：[4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
> 输出：[30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
> ```
>
> **示例 2：**
>
> ```
> 输入：root = [0,null,1]
> 输出：[1,null,1]
> ```
>
> **示例 3：**
>
> ```
> 输入：root = [1,0,2]
> 输出：[3,3,2]
> ```
>
> **示例 4：**
>
> ```
> 输入：root = [3,2,4,1]
> 输出：[7,9,4,10]
> ```
>
>  
>
> **提示：**
>
> - 树中的节点数介于 `0` 和 `104` 之间。
> - 每个节点的值介于 `-104` 和 `104` 之间。
> - 树中的所有值 **互不相同** 。
> - 给定的树为二叉搜索树。

**思路：**

https://leetcode-cn.com/problems/convert-bst-to-greater-tree/solution/shou-hua-tu-jie-zhong-xu-bian-li-fan-xiang-de-by-x/

**代码：**

```java
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
    /*
    解题思路：右中左的方式遍历
    每个节点的新值 = 节点值 + 右子树的和
    */
    public TreeNode convertBST(TreeNode root) {
        rebuild(root);
        return root;
    }

    int sum = 0;
    public void rebuild(TreeNode root) {
        if (root == null) {
            return;
        }
        rebuild(root.right);
        sum += root.val;
        root.val = sum;
        rebuild(root.left);
    }
}
```

```java
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
    /*
    解题思路：循环反向中序遍历
    */
    public TreeNode convertBST(TreeNode root) {
        int sum = 0;
        Deque<TreeNode> stack = new LinkedList<>();
        TreeNode cur = root;
        while (cur != null) {
            stack.offer(cur);
            cur = cur.right;
        }
        while (!stack.isEmpty()) {
            cur = stack.pollLast();
            sum += cur.val;
            cur.val = sum;
            cur = cur.left;
            while (cur != null) {
                stack.offer(cur);
                cur = cur.right;
            }
        }
        return root;
    }
}
/*参考：
https://leetcode-cn.com/problems/convert-bst-to-greater-tree/solution/shou-hua-tu-jie-zhong-xu-bian-li-fan-xiang-de-by-x/
*/
```



## 94.[二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

**题目：**

> 给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。
>
>  
>
> **示例 :**
> 给定二叉树
>
> ```
>           1
>          / \
>         2   3
>        / \     
>       4   5    
> ```
>
> 返回 **3**, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。
>
>  
>
> **注意：**两结点之间的路径长度是以它们之间边的数目表示。

**思路：**

**代码：**

```java
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
    /*
    解题思路：后序遍历
    一个节点的长度为 左子树长度 + 右子树长度 + 1
    返回时只能返回左右子树中更长的那个
    */
    public int diameterOfBinaryTree(TreeNode root) {
        diamter(root);
        return max;
    }

    int max = 0;
    public int diamter(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int left = diamter(root.left);
        int right = diamter(root.right);
        max = Math.max(left + right, max);
        return Math.max(left + 1, right + 1);
    }
}
```



## 95.[和为K的子数组](https://leetcode-cn.com/problems/subarray-sum-equals-k/)

**题目：**

> 给定一个整数数组和一个整数 **k，**你需要找到该数组中和为 **k** 的连续的子数组的个数。
>
> **示例 1 :**
>
> ```
> 输入:nums = [1,1,1], k = 2
> 输出: 2 , [1,1] 与 [1,1] 为两种不同的情况。
> ```
>
> **说明 :**
>
> 1. 数组的长度为 [1, 20,000]。
> 2. 数组中元素的范围是 [-1000, 1000] ，且整数 **k** 的范围是 [-1e7, 1e7]。

**思路：**

https://leetcode-cn.com/problems/subarray-sum-equals-k/solution/bao-li-jie-fa-qian-zhui-he-qian-zhui-he-you-hua-ja/

**代码：**

```java
class Solution {
    /*
    解题思路：dp
    设dp[i]表示前i个元素的元素和，则dp[i] = dp[i - 1] + nums[i]
    设sum(i,j)表示元素i到元素j的和，则sum(i,j) = dp[j] - dp[i] + nums[i]
    只要遍历以每个节点为连续子数组最后节点的所有可能，并将满足要求的加入res中即可
    */
    public int subarraySum(int[] nums, int k) {
        if (nums.length == 0)
            return 0;
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        for (int i = 1; i < nums.length; ++i) {
            dp[i] = dp[i - 1] + nums[i];
        }
        int cnt = 0;
        for (int j = nums.length - 1; j >= 0; --j) {
            for (int i = j; i >= 0; --i) {
                if (dp[j] - dp[i] + nums[i] == k)
                    ++cnt;
            }
        }
        return cnt;
    }
}
```

```java
class Solution {
    /*
    解题思路：前缀和
    在计算完包含了当前数的前缀和preSum之后，就去查询有多少前缀和满足preSum - k，因为preSum - (preSum - k) = k，将满足要求的解的个数加入结果中即可，最后再加入本节点的前缀和
    初始化：需要添加一个前缀和为0，计数为1的键值对到哈希表中，表示当前元素本身就满足k的情况
    */
    public int subarraySum(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap();
        map.put(0, 1);
        int cnt = 0;
        int preSum = 0;
        for (int num: nums) {
            preSum += num;
            cnt += map.getOrDefault(preSum - k, 0);
            map.put(preSum, map.getOrDefault(preSum, 0) + 1);
        }
        return cnt;
    }
}
/*参考：
https://leetcode-cn.com/problems/subarray-sum-equals-k/solution/bao-li-jie-fa-qian-zhui-he-qian-zhui-he-you-hua-ja/
*/
```



## 96.[最短无序连续子数组](https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray/)

**题目：**

> 给定一个整数数组，你需要寻找一个**连续的子数组**，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。
>
> 你找到的子数组应是**最短**的，请输出它的长度。
>
> **示例 1:**
>
> ```
> 输入: [2, 6, 4, 8, 10, 9, 15]
> 输出: 5
> 解释: 你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
> ```
>
> **说明 :**
>
> 1. 输入的数组长度范围在 [1, 10,000]。
> 2. 输入的数组可能包含**重复**元素 ，所以**升序**的意思是**<=。**

**思路：**

https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray/solution/si-lu-qing-xi-ming-liao-kan-bu-dong-bu-cun-zai-de-/

**代码：**

```java
class Solution {
    /*
    解题思路：单调栈 + 哨兵
    1.初始化：创建一个单调递增栈stack，用来保存不需要调整的数字，为了方便计算距离，所以应该存储下标，并且应该先加入一个哨兵-1
    2.正序遍历nums，设每个数字为nums[i]：
        1.数字可能出现重复，应该分为两种情况：
            1.nums[i - 1] != nums[i]：
                1.判断stack顶部对应元素x与当前元素的大小关系：
                    1. x < nums[i]：
                        直接将nums[i]入栈即可
                    2. x > nums[i]：
                        将stack中除了-1之外的仍然大于nums[i]的元素全部出栈，由于nums[i]也是需要调整的数字，所以本身不入栈
            2.nums[i - 1] == nums[i]：
                1.判断前一个相同的数字是否成功入栈了，设stack的栈顶元素为x
                    1. x == nums[i]：
                        直接入栈即可
                    2. x != nums[i]：
                        说明nums[i]与前一个元素一样需要调整，不入栈
    3.加入尾部哨兵nums.length
    4.依次出栈，若发现前一个出队元素 - 当前元素 > 1 则立即返回前一个元素 - 当前 元素 - 1
    5.若执行到此仍吴返回，则说明此数组是升序的，返回0即可
    */
    public int findUnsortedSubarray(int[] nums) {
        if (nums.length <= 1)
            return 0;
        Deque<Integer> stack = new LinkedList();
        int max = Integer.MIN_VALUE;
        stack.offer(-1);
        for (int i = 0; i < nums.length; ++i) {
            if (nums[i] >= max) {
                stack.offer(i);
                max = nums[i];
            } else {
                //清除stack中比num[i]大的数字
                while (stack.peekLast() != - 1 && nums[stack.peekLast()] > nums[i]) {
                    stack.pollLast();
                }
            }
        }
        stack.offer(nums.length);
        //找到最左侧的left和最右侧的right
        int left = -2, right = -1;
        int last = stack.poll();
        while (!stack.isEmpty()) {
            if (stack.peek() - last > 1) {
                if (left == -2) {
                    left = last;
                }
                right = stack.peek();
            }
            last = stack.poll();
        }
        return left == -2 ? 0 : right - left - 1;
    }
}
```



```java
class Solution {
    /*
    解题思路：双指针
    我们可以把这个数组分为3段，左段和右段都是标准的升序数组，中段数组虽然是无序的，但满足最小值大于左段的最大值，最大值小于右段的最小值
    那么我们只要找到中段的左右边界即可，我们分别定义它们为begin和end，分两头开始找：
    ·从左到右维护一个最大值max,那么在进入右段之前，遍历到的nums[i]肯定小于max
    我们只需要最后一个nums[i] < max的元素end即可
    ·同理，不再赘述，找到start即可
    最后返回end - start + 1即可
    */
    public int findUnsortedSubarray(int[] nums) {
        if (nums.length <= 1)
            return 0;
        int len = nums.length;
        int max = nums[0], min = nums[len - 1];
        int start = 0, end = -1;
        for (int i = 0; i < len; ++i) {
            //找右边界
            if (nums[i] < max) {
                end = i;
            } else {
                max = nums[i];
            }
            //找左边界
            if (nums[len - 1 - i] > min) {
                start = len - 1 - i;
            } else {
                min = nums[len - 1 - i];
            }
        }
        return end - start + 1;
    }
}
/*参考：
https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray/solution/si-lu-qing-xi-ming-liao-kan-bu-dong-bu-cun-zai-de-/
*/
```

## 97.[合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

**题目：**

> 给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。
>
> 你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则**不为** NULL 的节点将直接作为新二叉树的节点。
>
> **示例 1:**
>
> ```
> 输入: 
> 	Tree 1                     Tree 2                  
>           1                         2                             
>          / \                       / \                            
>         3   2                     1   3                        
>        /                           \   \                      
>       5                             4   7                  
> 输出: 
> 合并后的树:
> 	     3
> 	    / \
> 	   4   5
> 	  / \   \ 
> 	 5   4   7
> ```
>
> **注意:** 合并必须从两个树的根节点开始。

**思路：**

**代码：**

```java
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
    /*
    解题思路：前序遍历
    若其中一个节点为空，则直接返回另一个即可
    */
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null) {
            return t2;
        }
        if (t2 == null) {
            return t1;
        }
        t1.val += t2.val;
        t1.left = mergeTrees(t1.left, t2.left);
        t1.right = mergeTrees(t1.right, t2.right);
        return t1;
    }
}
```



## 98.[任务调度器](https://leetcode-cn.com/problems/task-scheduler/)

**题目：**

> 给你一个用字符数组 `tasks` 表示的 CPU 需要执行的任务列表。其中每个字母表示一种不同种类的任务。任务可以以任意顺序执行，并且每个任务都可以在 1 个单位时间内执行完。在任何一个单位时间，CPU 可以完成一个任务，或者处于待命状态。
>
> 然而，两个 **相同种类** 的任务之间必须有长度为整数 `n` 的冷却时间，因此至少有连续 `n` 个单位时间内 CPU 在执行不同的任务，或者在待命状态。
>
> 你需要计算完成所有任务所需要的 **最短时间** 。
>
>  
>
> **示例 1：**
>
> ```
> 输入：tasks = ["A","A","A","B","B","B"], n = 2
> 输出：8
> 解释：A -> B -> (待命) -> A -> B -> (待命) -> A -> B
>      在本示例中，两个相同类型任务之间必须间隔长度为 n = 2 的冷却时间，而执行一个任务只需要一个单位时间，所以中间出现了（待命）状态。 
> ```
>
> **示例 2：**
>
> ```
> 输入：tasks = ["A","A","A","B","B","B"], n = 0
> 输出：6
> 解释：在这种情况下，任何大小为 6 的排列都可以满足要求，因为 n = 0
> ["A","A","A","B","B","B"]
> ["A","B","A","B","A","B"]
> ["B","B","B","A","A","A"]
> ...
> 诸如此类
> ```
>
> **示例 3：**
>
> ```
> 输入：tasks = ["A","A","A","A","A","A","B","C","D","E","F","G"], n = 2
> 输出：16
> 解释：一种可能的解决方案是：
>      A -> B -> C -> A -> D -> E -> A -> F -> G -> A -> (待命) -> (待命) -> A -> (待命) -> (待命) -> A
> ```
>
>  
>
> **提示：**
>
> - `1 <= task.length <= 104`
> - `tasks[i]` 是大写英文字母
> - `n` 的取值范围为 `[0, 100]`

**思路：**

https://leetcode-cn.com/problems/task-scheduler/solution/ren-wu-diao-du-qi-by-leetcode-solution-ur9w/

https://leetcode-cn.com/problems/task-scheduler/solution/jian-ming-yi-dong-de-javajie-da-by-lan-s-jfl9/
https://leetcode-cn.com/problems/task-scheduler/solution/tong-zi-by-popopop/

**代码：**

```java
class Solution {
    /*
    解题思路：模拟
    按时间顺序，依次给每个时间单位分配任务。
    关键点：如果当前有多种任务不在冷却中，我们应该选择剩余执行次数最多的那个任务执行，将每种任务的剩余执行次数尽可能平均，使得CPU的处于待命状态的时间尽可能的少。
    具体操作：可以使用二元组(nextValid_i, rest_i)表示第i个任务，其中nextValid_i表示其因冷却限制，最早可以执行的时间；rest_i表示其剩余的执行次数。初始时，所有的
    nextValid_i都为1，而rest_i即为任务i在tasks中的出现次数。
    用time记录当前时间，我们需要选择的是不在冷却中且剩余执行次数最多的那个任务，也就是要找到nextValid_i <= time并且rest_i最大的索引。那么我们需要遍历二元素来查找。
    在这之后，我们还需要将(nextValid_i, rest_i)更新为(nextValid_i + (n + 1), rest_i - 1)，如果更新后的rest_i为0表示任务i全部做完，那么我们在遍历二元组时可以忽略它了
    如何减少时间复杂度：
    对于time的更新，我们可以选择不断增加+1来模拟每个时间片，但这会导致CPU处于待命状态，对二元数组进行不必要的遍历。为了减少时间复杂度，我们可以在每一次遍历之前，将time更新
    为所有nextValid_i中的最小值，直接跳过待命状态，保证每一次对二元素的遍历都是有效的。需要注意的是，只有当这个最小值大于time时，才需要这样更新。
    */
    public int leastInterval(char[] tasks, int n) {
        Map<Character, Integer> map = new HashMap();
        for (char ch: tasks) {
            map.put(ch, map.getOrDefault(ch, 0) + 1);
        }
        int len = map.size();
        List<Integer> nextValid = new ArrayList();
        List<Integer> rest = new ArrayList();
        for (Map.Entry<Character, Integer> entry: map.entrySet()) {
            nextValid.add(1);
            rest.add(entry.getValue());
        }
        int time = 0;
        for (int i = 0; i < tasks.length; ++i) {
            ++time;
            //跳过待命
            int minTime = Integer.MAX_VALUE;
            for (int j = 0; j < len; ++j) {
                if (rest.get(j) != 0) {
                    minTime = Math.min(minTime, nextValid.get(j));
                }
            }
            time = Math.max(time, minTime);
            //找到剩余次数最多的
            int best = -1;
            for (int j = 0; j < len; ++j) {
                if (rest.get(j) != 0 && nextValid.get(j) <= time) {
                    if (best == -1 || rest.get(j) > rest.get(best)) {
                        best = j;
                    }
                }
            }
            nextValid.set(best, time + n + 1);
            rest.set(best, rest.get(best) - 1);
        }
        return time;
    }
}
/*参考：
https://leetcode-cn.com/problems/task-scheduler/solution/ren-wu-diao-du-qi-by-leetcode-solution-ur9w/
*/
```

```java
class Solution {
    /*
    解题思路：基于贪心的桶子算法
    容易想到的一种贪心策略为：先安排出现次数最多的任务，让这个任务两次执行的时间间隔正好为n。再在这个时间间隔内填充其他的任务。

例如：tasks = ["A","A","A","B","B","B"], n = 2

我们先安排出现次数最多的任务"A",并且让两次执行"A"的时间间隔为2。在这个时间间隔内，我们用其他任务类型去填充，又因为其他任务类型只有"B"一个，不够填充2的时间间隔，因此额外需要一个冷却时间间隔
其中，maxTimes为出现次数最多的那个任务出现的次数。maxCount为一共有多少个任务和出现最多的那个任务出现次数一样。

图中一共占用的方格即为完成所有任务需要的时间，即：

(maxTimes - 1)*(n + 1) + maxCount
(maxTimes−1)∗(n+1)+maxCount

此外，如果任务种类很多，在安排时无需冷却时间，只需要在一个任务的两次出现间填充其他任务，然后从左到右从上到下依次执行即可，由于每一个任务占用一个时间单位，我们又正正好好地使用了tasks中的所有任务，而且我们只使用tasks中的任务来占用方格（没用冷却时间）。因此这种情况下，所需要的时间即为tasks的长度。

由于这种情况时再用上述公式计算会得到一个不正确且偏小的结果，因此，我们只需把公式计算的结果和tasks的长度取最大即为最终结果。
    */
    public int leastInterval(char[] tasks, int n) {
        int len = tasks.length;
        int[] buckets = new int[26];
        for (char ch: tasks) {
            ++buckets[ch - 'A'];
        }
        Arrays.sort(buckets);
        int lastCount = 1;
        int maxCount = buckets[buckets.length - 1];
        for (int i = buckets.length - 1; i >= 1; --i) {
            if (buckets[i - 1] == buckets[i])
                ++lastCount;
            else
                break;
        }
        int res = (maxCount - 1) * (n + 1) + lastCount;
        return Math.max(res, len);
    }
}
/*参考：
https://leetcode-cn.com/problems/task-scheduler/solution/jian-ming-yi-dong-de-javajie-da-by-lan-s-jfl9/
https://leetcode-cn.com/problems/task-scheduler/solution/tong-zi-by-popopop/
*/
```



## 99.[回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)

**题目：**

> 
> 给定一个字符串，你的任务是计算这个字符串中有多少个回文子串。
>
> 具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。
>
>  
>
> **示例 1：**
>
> ```
> 输入："abc"
> 输出：3
> 解释：三个回文子串: "a", "b", "c"
> ```
>
> **示例 2：**
>
> ```
> 输入："aaa"
> 输出：6
> 解释：6个回文子串: "a", "a", "a", "aa", "aa", "aaa"
> ```
>
>  
>
> **提示：**
>
> - 输入的字符串长度不会超过 1000 。

**思路：**

**代码：**

```java
class Solution {
    /*
    解题思路：动态规划
    https://leetcode-cn.com/problems/longest-palindromic-substring/
    */
    public int countSubstrings(String s) {
        if (s.length() == 0)
            return 0;
        int left = 0, right = 0;
        int len = s.length();
        int cnt = 0;
        boolean[][] dp = new boolean[len][len];
        for (int i = len - 1; i >=0; --i) {
            for (int j = i; j < len; ++j) {
                if (i == j) {
                    dp[i][j] = true;
                    ++cnt;
                } else if (s.charAt(i) == s.charAt(j) && (i + 1 > j - 1 || dp[i + 1][j - 1])) {
                    dp[i][j] = true;
                    ++cnt;
                }
            }
        }
        return cnt;
    }
}
```

```java
class Solution {
    /*
    解题思路：中心扩展法
    回文串一定是对称的，每次循环选择一个中心，来进行左右扩展，判断左右字符是否相等
    */
    public int countSubstrings(String s) {
        if (s.length() == 0)
            return 0;
        int cnt = 0;
        for (int i = 0; i < s.length(); ++i) {
            cnt += count(s, i, i);
            cnt += count(s, i, i + 1);
        }
        return cnt;
    }

    public int count(String s, int left, int right) {
        int l = left, r = right, cnt = 0;
        while (l >= 0 && r < s.length() && s.charAt(l) == s.charAt(r)) {
            --l;
            ++r;
            ++cnt;
        }
        return cnt;
    }
}
```

## 100.[每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

**题目：**

> 请根据每日 `气温` 列表，重新生成一个列表。对应位置的输出为：要想观测到更高的气温，至少需要等待的天数。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。
>
> 例如，给定一个列表 `temperatures = [73, 74, 75, 71, 69, 72, 76, 73]`，你的输出应该是 `[1, 1, 4, 2, 1, 1, 0, 0]`。
>
> **提示：**`气温` 列表长度的范围是 `[1, 30000]`。每个气温的值的均为华氏度，都是在 `[30, 100]` 范围内的整数。

**思路：**

https://leetcode-cn.com/problems/daily-temperatures/solution/java-by-sdwwld/

**代码：**

```java
class Solution {
    /*
    解题思路：单调栈
    维护两个单调栈desire与who,用来描述第i个元素的需求：
    将T[i]入栈时，需要将desire栈中小于它的元素全部出栈，因为它们表示之前节点的升温需求，
    设栈顶元素为x，T[i] > x 表示当前温度大于那个节点的温度，也就满足了升温，所以同时
    将who中存储着的元素下标j出栈，将当前下标存到需求下标的距离distance入res[j]即可

    空间优化：
    只用一个栈存储需求元素的下标即可，因为任然可以在O(1)时间内使用T[i]查询其温度
    */
    public int[] dailyTemperatures(int[] T) {
        Deque<Integer> desires = new LinkedList();
        int len = T.length;
        int[] res = new int[len];
        for (int i = 0; i < len; ++i) {
            while (!desires.isEmpty() && T[i] > T[desires.peekLast()]) {
                int desire = desires.pollLast();
                res[desire] = i - desire;
            }
            desires.offer(i);
        }
        return res;
    }
}
```

```java
class Solution {
    /*
    解题思路：从后往前找 + 剪支
    */
    public int[] dailyTemperatures(int[] T) {
        int[] res = new int[T.length];
        for (int i = T.length - 2; i >= 0; --i) {
            int j = i + 1;
            while (j < T.length) {
                if (T[j] > T[i]) {
                    res[i] = j - i;
                    break;
                } else if (res[j] == 0) {
                    //剪支，若res[j]为0表示后面的元素都比它小，而T[j] <= T[i]则之后不可能有更高的温度了
                    res[i] = 0;
                    break;
                } else {
                    //直接跳到大于j位置元素的位置
                    j += res[j];
                }
            }
        }
        return res;
    }
}
/*参考：
https://leetcode-cn.com/problems/daily-temperatures/solution/java-by-sdwwld/
*/
```



