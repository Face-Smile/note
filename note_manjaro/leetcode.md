# 31. 下一个排列

实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须原地修改，只允许使用额外常数空间。

以下是一些例子，输入位于左侧列，其相应输出位于右侧列。
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1

思路：从右往左遍历，直到遇到降序数列，使用一个指针index指向它，然后从当前位置从左往右遍历直到遇到最后一个比index指向的位置的值大的数，交换他们的值，然后对index指针后面的数进行逆序

![Next Permutation](leetcode/1df4ae7eb275ba4ab944521f99c84d782d17df804d5c15e249881bafcf106173-file_1555696082944.gif)

python3 代码：

```python
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        if not nums:
            return 
        i = j = len(nums) - 1
        # 从右边往左遍历，查找降序位置
        while i > 0 and nums[i] <= nums[i-1]:
            i -= 1
        t = i
        index = i - 1
        while i < j:
            nums[i], nums[j] = nums[j], nums[i]
            i += 1
            j -= 1
        # 如果index < 0 表示没有比他大的排列数
        if index >= 0:
            while t < len(nums) and nums[t] <= nums[index]:
                t += 1
            nums[index], nums[t] = nums[t], nums[index]
```



java 代码：

```java
class Solution {
    public void nextPermutation(int[] nums) {
        if (nums == null || nums.length <= 1) {
            return;
        }
        int i, j;
        i = j = nums.length - 1;
        while (i > 0 && nums[i] <= nums[i - 1]) {
            i--;
        }

        int index = i - 1;
        while (i < j) {
            nums[i] = nums[i] ^ nums[j];
            nums[j] = nums[i] ^ nums[j];
            nums[i] = nums[i] ^ nums[j];
            i++;
            j--;
        }

        if (index >= 0) {
            i = index + 1;
            while (i < nums.length && nums[i] <= nums[index]) {
                i++;
            }
            nums[i] = nums[i] ^ nums[index];
            nums[index] = nums[i] ^ nums[index];
            nums[i] = nums[i] ^ nums[index];
        }
    }
}
```



# 32. 最长有效括号

给定一个只包含 '(' 和 ')' 的字符串，找出最长的包含有效括号的子串的长度。

示例 1:

输入: "(()"
输出: 2
解释: 最长有效括号子串为 "()"
示例 2:

输入: ")()())"
输出: 4
解释: 最长有效括号子串为 "()()"

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-valid-parentheses
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



方法一：暴力

在这种方法中，我们考虑给定字符串中每种可能的非空偶数长度子字符串，检查它是否是一个有效括号字符串序列。为了检查有效性，我们使用栈的方法。

每当我们遇到一个`（` ，我们把它放在栈顶。对于遇到的每个 `（` ，我们从栈中弹出一个 `（` ，如果栈顶没有 `（` ，或者遍历完整个子字符串后栈中仍然有元素，那么该子字符串是无效的。这种方法中，我们对每个偶数长度的子字符串都进行判断，并保存目前为止找到的最长的有效子字符串的长度。

```
例子:
"((())"

(( --> 无效
(( --> 无效
() --> 有效，长度为 2
)) --> 无效
((()--> 无效
(())--> 有效，长度为 4
最长长度为 4
```

```java
public class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<Character>();
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                stack.push('(');
            } else if (!stack.empty() && stack.peek() == '(') {
                stack.pop();
            } else {
                return false;
            }
        }
        return stack.empty();
    }
    public int longestValidParentheses(String s) {
        int maxlen = 0;
        for (int i = 0; i < s.length(); i++) {
            for (int j = i + 2; j <= s.length(); j+=2) {
                if (isValid(s.substring(i, j))) {
                    maxlen = Math.max(maxlen, j - i);
                }
            }
        }
        return maxlen;
    }
}

```



方法二：动态规划
     * 创建一个数组dp用于储存子串最长有效括号的长度
          * dp[i] 表示以是s[i]结尾的最长的有效括号的长度，很明显如果s[i]为 '(', 则dp[i] = 0,
          * 如果s[i] = ')' 且s[i-1] = '(' 则dp[i] = dp[i-2] + 2
               * 如果s[i] = ')' 且s[i-1] = ')'
               * 		如果 s[i - dp[i-1] - 1] = '(', 则dp[i] = dp[i-1] + dp[i - dp[i-1] - 2] + 2
                    * 		如果 s[i - dp[i-1] - 1] = ')', 则dp[i] = 0

复杂度分析

时间复杂度： `O(n) `。遍历整个字符串一次，就可以将 `dp` 数组求出来。
空间复杂度： `O(n) `。需要一个大小为 `n` 的 `dp` 数组。

```java
public int longestValidParenthesesWithStack(String s) {
    Stack<Integer> stack = new Stack<Integer>();
    int maxans = 0;
    stack.push(-1);

    for (int i = 0; i < s.length(); i ++) {
        if (s.charAt(i) == '(') {
            stack.push(i);
        }else{
            stack.pop();
            if (stack.isEmpty()) {
                stack.push(i);
            }else {
                maxans = Math.max(maxans, i - stack.peek());
            }
        }
    }

    return maxans;
}
```





方法 3：栈
算法

与找到每个可能的子字符串后再判断它的有效性不同，我们可以用栈在遍历给定字符串的过程中去判断到目前为止扫描的子字符串的有效性，同时能的都最长有效字符串的长度。我们首先将 -1 放入栈顶。

对于遇到的每个 `（` ，我们将它的下标放入栈中。
对于遇到的每个 `）` ，我们弹出栈顶的元素并将当前元素的下标与弹出元素下标作差，得出当前有效括号字符串的长度。通过这种方法，我们继续计算有效子字符串的长度，并最终返回最长有效子字符串的长度。

复杂度分析

- 时间复杂度： O(n) 。*n* 是给定字符串的长度。

- 空间复杂度： O(n) 。栈的大小最大达到 *n* 。

```java
public int longestValidParenthesesWithStack(String s) {
    Stack<Integer> stack = new Stack<Integer>();
    int maxans = 0;
    stack.push(-1);

    for (int i = 0; i < s.length(); i ++) {
        if (s.charAt(i) == '(') {
            stack.push(i);
        }else{
            stack.pop();
            if (stack.isEmpty()) {
                stack.push(i);
            }else {
                maxans = Math.max(maxans, i - stack.peek());
            }
        }
    }

    return maxans;
}
```





方法4 ：计数器法在这种方法中，我们利用两个计数器 left 和 right 。首先，我们从左到右遍历字符串，对于遇到的每个`（`，我们增加 left 计算器，对于遇到的每个 `）` ，我们增加 right 计数器。每当 left 计数器与 right 计数器相等时，我们计算当前有效字符串的长度，并且记录目前为止找到的最长子字符串。如果 right 计数器比 left 计数器大时，我们将 left 和 right 计数器同时变回 0 。

- 时间复杂度： O(n) 。遍历两遍字符串。
- 空间复杂度： O(1) 。仅有两个额外的变量 left 和 right 。

```java
public int LongestValidParenthesesWithDoublePoint(String s) {
    int left = 0, right = 0;
    int maxLen = 0;
    for (int i = 0; i < s.length(); i ++) {
        if (s.charAt(i) == '(') {
            left ++;
        }else {
            right ++;
        }
        if (left == right) {
            maxLen = Math.max(maxLen, 2 * right);
        }else if ( left < right) {
            left = right = 0;
        }
    }
    left = right = 0;
    for (int i = s.length() - 1; i >= 0; i --) {
        if (s.charAt(i) == '(')
            left ++;
        else
            right ++;
        if (left == right)
            maxLen = Math.max(maxLen, 2 * right);
        else if (left > right)
            left = right = 0;
    }
    return maxLen;
}
```





# 33. 搜索旋转排序数组



假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 `[0,1,2,4,5,6,7]` 可能变为 `[4,5,6,7,0,1,2]` )。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 O(log n) 级别。

示例 1:

```
输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4
示例 2:
```

示例 2:

```
输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/search-in-rotated-sorted-array
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



```java
public int search1(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        System.out.println("left:" + left + ", " + "right:" + right);
        if (nums[mid] == target)
            return mid;
        // 左边有序
        if (nums[left] <= nums[mid]) {
            if (target < nums[mid] && target >= nums[left])
                right = mid - 1;
            else
                left = mid + 1;
        } else { // 右边有序
            if (target > nums[mid] && target <= nums[right])
                left = mid + 1;
            else
                right = mid - 1;
        }
        System.out.println("left:" + left + ", " + "right:" + right);
    }
    return -1;
}
```

> 注意：nums = [3, 1], target = 1 这种情况下，左边有序的判断条件改为`nums[left] <= nums[mid]`而不是`<`

