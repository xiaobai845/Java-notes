# 力扣面试经典 150 题（Java 最简解法）

> 本笔记覆盖 LeetCode 官方题单「面试经典 150 题」全部 150 道题目。
> 每道题都给出 **最容易理解的写法**，关键步骤直接写在代码里（`//` 行内注释），配上例子演算。
> 适合面试前快速过一遍，Typora 打开即可用（左侧大纲自动列出全部题目）。

## 阅读说明

- **难度**：按力扣中文站标注（简单 / 中等 / 困难）。
- **思路**：一句话点破「这题在考什么、核心技巧是什么」。
- **代码**：可直接提交的 Java 实现，关键步骤用 `//` 行内注释标在对应代码旁边，一看就懂。
- **举例**：给「输入 → 逐步过程 → 输出」，帮你理解代码跑起来发生了什么。
- 题目顺序与官方题单一致，共 23 章、150 题。

## 目录

| 章节 | 题数 | 题号范围 |
| --- | --- | --- |
| 一、数组 / 字符串 | 24 | 1 ~ 24 |
| 二、双指针 | 5 | 25 ~ 29 |
| 三、滑动窗口 | 4 | 30 ~ 33 |
| 四、矩阵 | 5 | 34 ~ 38 |
| 五、哈希表 | 9 | 39 ~ 47 |
| 六、区间 | 4 | 48 ~ 51 |
| 七、栈 | 5 | 52 ~ 56 |
| 八、链表 | 11 | 57 ~ 67 |
| 九、二叉树 | 14 | 68 ~ 81 |
| 十、二叉树层次遍历 | 4 | 82 ~ 85 |
| 十一、二叉搜索树 | 3 | 86 ~ 88 |
| 十二、图 | 6 | 89 ~ 94 |
| 十三、图的广度优先搜索 | 3 | 95 ~ 97 |
| 十四、字典树 | 3 | 98 ~ 100 |
| 十五、回溯 | 7 | 101 ~ 107 |
| 十六、分治 | 4 | 108 ~ 111 |
| 十七、Kadane 算法 | 2 | 112 ~ 113 |
| 十八、二分查找 | 7 | 114 ~ 120 |
| 十九、堆 | 4 | 121 ~ 124 |
| 二十、位运算 | 6 | 125 ~ 130 |
| 二十一、数学 | 6 | 131 ~ 136 |
| 二十二、一维动态规划 | 5 | 137 ~ 141 |
| 二十三、多维动态规划 | 9 | 142 ~ 150 |

---

## 一、数组 / 字符串

### 1. 88. 合并两个有序数组

- **难度**：简单
- **思路**：两个数组都有序，从 **后往前** 填 `nums1` 的末尾，谁大放谁，不会覆盖还没用到的数。

```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
    int i = m - 1;                  // i 指向 nums1 最后一个有效元素
    int j = n - 1;                  // j 指向 nums2 最后一个元素
    int k = m + n - 1;              // k 指向 nums1 最终末尾，从后往前填
    while (j >= 0) {                // nums2 没放完就一直填
        if (i >= 0 && nums1[i] > nums2[j]) {   // nums1 的更大，放它
            nums1[k--] = nums1[i--];
        } else {                    // 否则放 nums2 的
            nums1[k--] = nums2[j--];
        }
    }
}
```

**举例**：

- 输入：`nums1 = [1,2,3,0,0,0], m = 3`，`nums2 = [2,5,6], n = 3`
- 过程：从后往前——6 和 3 比放 6；5 和 3 比放 5；2 和 3 比放 3；2 和 2 比放 2；1 和 2 比放 2；最后剩 1 不动。
- 输出：`[1,2,2,3,5,6]`

### 2. 27. 移除元素

- **难度**：简单
- **思路**：快慢指针。快指针扫全数组，遇到不等于 `val` 的就挪到慢指针位置。

```java
public int removeElement(int[] nums, int val) {
    int k = 0;                       // 慢指针：下一个可写入的位置
    for (int i = 0; i < nums.length; i++) {   // 快指针：从头扫到尾
        if (nums[i] != val) {       // 不是要删的值，就保留
            nums[k++] = nums[i];    // 挪到 k 处，k 后移
        }
    }
    return k;                        // k 就是剩余元素个数
}
```

**举例**：

- 输入：`nums = [3,2,2,3], val = 3`
- 过程：`i=0` 是 3 跳过；`i=1` 是 2 写入 `k=0`；`i=2` 是 2 写入 `k=1`；`i=3` 是 3 跳过。
- 输出：`2`（数组前两位为 `[2,2]`）

### 3. 26. 删除有序数组中的重复项

- **难度**：简单
- **思路**：有序数组重复元素一定挨着，和前一个比，不同才保留。

```java
public int removeDuplicates(int[] nums) {
    int k = 1;                       // 第 0 个元素一定保留，k 从 1 开始
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] != nums[i - 1]) {   // 和前一个不同 = 新元素
            nums[k++] = nums[i];        // 写到保留区
        }
    }
    return k;
}
```

**举例**：

- 输入：`nums = [1,1,2]`
- 过程：`i=1` 的 1 等于前一个 1，跳过；`i=2` 的 2 不等于 1，写入 `k=1` 的位置。
- 输出：`2`（数组为 `[1,2]`）

### 4. 80. 删除有序数组中的重复项 II

- **难度**：中等
- **思路**：允许每个元素出现 **两次**。新元素只要和「倒数第二个已保留元素」不同，就能留下。

```java
public int removeDuplicates(int[] nums) {
    int k = 0;
    for (int x : nums) {
        // 前两个无条件保留；之后只有 x 不等于倒数第二个已保留元素时才保留
        if (k < 2 || nums[k - 2] != x) {
            nums[k++] = x;
        }
    }
    return k;
}
```

**举例**：

- 输入：`nums = [1,1,1,2,2,3]`
- 过程：前两个 1 保留；第三个 1 等于 `nums[0]`（也是 1），跳过；2、2 保留；3 保留。
- 输出：`5`（数组为 `[1,1,2,2,3]`）

### 5. 169. 多数元素

- **难度**：简单
- **思路**：摩尔投票。多数元素数量超过一半，两两抵消后剩下的一定是它。

```java
public int majorityElement(int[] nums) {
    int candidate = 0, count = 0;
    for (int x : nums) {
        if (count == 0) candidate = x;          // 票数清零，换候选人
        count += (x == candidate) ? 1 : -1;     // 相同加票，不同减票
    }
    return candidate;                            // 多数元素过半，最后一定剩它
}
```

**举例**：

- 输入：`[2,2,1,1,1,2,2]`
- 过程：2(票1) → 2(票2) → 1(票1) → 1(票0) → 1(票1，换候选) → 2(票0) → 2(候选2, 票1)。
- 输出：`2`

### 6. 189. 轮转数组

- **难度**：中等
- **思路**：整体翻转 → 前 k 个翻转 → 后 n-k 个翻转，三步搞定，原地完成。

```java
public void rotate(int[] nums, int k) {
    k %= nums.length;                    // k 可能超过长度，先取模
    reverse(nums, 0, nums.length - 1);   // 1. 整体翻转
    reverse(nums, 0, k - 1);             // 2. 前 k 个翻转
    reverse(nums, k, nums.length - 1);   // 3. 后 n-k 个翻转
}

void reverse(int[] a, int l, int r) {
    while (l < r) {                      // 首尾交换
        int t = a[l]; a[l++] = a[r]; a[r--] = t;
    }
}
```

**举例**：

- 输入：`nums = [1,2,3,4,5,6,7], k = 3`
- 过程：整体翻转 `[7,6,5,4,3,2,1]` → 前 3 个翻转 `[5,6,7,4,3,2,1]` → 后 4 个翻转 `[5,6,7,1,2,3,4]`。
- 输出：`[5,6,7,1,2,3,4]`

### 7. 121. 买卖股票的最佳时机

- **难度**：简单
- **思路**：只能买卖一次。边走边记录 **历史最低价**，用当天价减去它，取最大利润。

```java
public int maxProfit(int[] prices) {
    int min = Integer.MAX_VALUE;        // 历史最低买入价
    int ans = 0;                        // 最大利润
    for (int p : prices) {
        min = Math.min(min, p);         // 更新最低价
        ans = Math.max(ans, p - min);   // 今天卖出能赚多少
    }
    return ans;
}
```

**举例**：

- 输入：`[7,1,5,3,6,4]`
- 过程：第 2 天最低价降到 1；第 3 天卖出赚 4；第 5 天卖出赚 5，为最大值。
- 输出：`5`

### 8. 122. 买卖股票的最佳时机 II

- **难度**：中等
- **思路**：可以买卖无数次，那就抓住 **每一段上涨**，累加所有相邻正差价即可。

```java
public int maxProfit(int[] prices) {
    int ans = 0;
    for (int i = 1; i < prices.length; i++) {
        if (prices[i] > prices[i - 1]) {        // 今天比昨天高
            ans += prices[i] - prices[i - 1];   // 累加这段上涨的差价
        }
    }
    return ans;
}
```

**举例**：

- 输入：`[7,1,5,3,6,4]`
- 过程：1 → 5 赚 4；3 → 6 赚 3；合计 7。
- 输出：`7`

### 9. 55. 跳跃游戏

- **难度**：中等
- **思路**：维护一个「能跳到的最远位置」`far`。只要当前下标没超过 `far` 就还能继续，否则卡死。

```java
public boolean canJump(int[] nums) {
    int far = 0;                        // 能跳到的最远下标
    for (int i = 0; i < nums.length; i++) {
        if (i > far) return false;      // 到不了 i，直接失败
        far = Math.max(far, i + nums[i]);   // 更新最远可达位置
    }
    return true;
}
```

**举例**：

- 输入：`[2,3,1,1,4]`
- 过程：`i=0` 时 `far=2`；`i=1` 时 `far=4`；`i=2`、`i=3` 都在 `far` 范围内，能到终点。
- 输出：`true`

### 10. 45. 跳跃游戏 II

- **难度**：中等
- **思路**：贪心。每次在「当前步能覆盖的范围」里找下一跳能到的最远位置，覆盖范围到头就跳一步。

```java
public int jump(int[] nums) {
    int end = 0;     // 当前这一步能覆盖的边界
    int far = 0;     // 下一步能到的最远位置
    int step = 0;    // 跳的步数
    for (int i = 0; i < nums.length - 1; i++) {  // 最后一位不用跳
        far = Math.max(far, i + nums[i]);        // 更新最远
        if (i == end) {                          // 走到当前边界，必须跳一步
            end = far;
            step++;
        }
    }
    return step;
}
```

**举例**：

- 输入：`[2,3,1,1,4]`
- 过程：`i=0` 时 `far=2`，走到 `end=0` 跳 1 步、`end=2`；`i=1` 时 `far=4`；走到 `end=2` 再跳 1 步、`end=4`，覆盖终点。
- 输出：`2`

### 11. 274. H 指数

- **难度**：中等
- **思路**：升序排序后，从前往后找第一个「引用数 >= 剩余篇数」的位置，`n - i` 就是答案。

```java
public int hIndex(int[] citations) {
    Arrays.sort(citations);             // 升序排序
    int n = citations.length;
    for (int i = 0; i < n; i++) {
        // 从 i 到最后有 n-i 篇，引用数都不少于 n-i，即 H 指数
        if (citations[i] >= n - i) return n - i;
    }
    return 0;
}
```

**举例**：

- 输入：`[3,0,6,1,5]`
- 过程：排序得 `[0,1,3,5,6]`；`i=2` 时 `3 >= 5-2=3` 成立。
- 输出：`3`（有 3 篇论文至少被引 3 次）

### 12. 380. O(1) 时间插入、删除和获取随机元素

- **难度**：中等
- **思路**：`List` 存值（O(1) 随机访问），`Map` 存「值 → 下标」。删除时把末尾元素搬到被删位置再删末尾。

```java
class RandomizedSet {
    List<Integer> list = new ArrayList<>();      // 存值，支持 O(1) 随机访问
    Map<Integer, Integer> map = new HashMap<>(); // 值 -> 在 list 中的下标
    Random r = new Random();

    public boolean insert(int val) {
        if (map.containsKey(val)) return false;  // 已存在
        map.put(val, list.size());               // 记录下标
        list.add(val);                           // 追加到末尾
        return true;
    }

    public boolean remove(int val) {
        if (!map.containsKey(val)) return false;
        int idx = map.get(val);                  // 被删元素的下标
        int last = list.get(list.size() - 1);    // 末尾元素
        list.set(idx, last);                     // 用末尾元素顶替被删位置
        map.put(last, idx);                      // 更新末尾元素下标
        list.remove(list.size() - 1);            // 删掉末尾
        map.remove(val);                         // 从 map 移除
        return true;
    }

    public int getRandom() {
        return list.get(r.nextInt(list.size())); // 随机取一个下标
    }
}
```

**举例**：

- 依次 `insert(1)`、`insert(2)`、`insert(3)` → `list = [1,2,3]`。
- `remove(2)` → 用末尾 3 顶替下标 1，`list = [1,3]`。
- `getRandom()` → 等概率返回 1 或 3。

### 13. 238. 除自身以外数组的乘积

- **难度**：中等
- **思路**：不能用除法。先从左往右算「左边乘积」，再从右往左乘「右边乘积」，两趟完成。

```java
public int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] ans = new int[n];
    ans[0] = 1;                                  // 第一个元素左边没有数，记 1
    for (int i = 1; i < n; i++) {
        ans[i] = ans[i - 1] * nums[i - 1];       // 先填「左边所有数的乘积」
    }
    int right = 1;                               // 从右往左记录右边乘积
    for (int i = n - 1; i >= 0; i--) {
        ans[i] *= right;                         // 乘上右边乘积
        right *= nums[i];                        // 更新右边乘积
    }
    return ans;
}
```

**举例**：

- 输入：`[1,2,3,4]`
- 过程：左边乘积 `[1,1,2,6]`；从右往左乘右边乘积，得到 `[24,12,8,6]`。
- 输出：`[24,12,8,6]`

### 14. 134. 加油站

- **难度**：中等
- **思路**：总油量 >= 总耗油才有解。从某站出发，油箱一旦为负，说明这段路上任何站都不行，直接从下一站重新开始。

```java
public int canCompleteCircuit(int[] gas, int[] cost) {
    int total = 0;   // 全程油差总和，判断有无解
    int cur = 0;     // 当前起点出发的累计油差
    int start = 0;   // 候选起点
    for (int i = 0; i < gas.length; i++) {
        int diff = gas[i] - cost[i];
        total += diff;
        cur += diff;
        if (cur < 0) {           // 从 start 到不了这一站
            start = i + 1;       // 换下一站为起点
            cur = 0;             // 油量清零重算
        }
    }
    return total >= 0 ? start : -1;  // 总油够才有解
}
```

**举例**：

- 输入：`gas = [1,2,3,4,5], cost = [3,4,5,1,2]`
- 过程：油差为 `[-2,-2,-2,3,3]`；前三个站累计为负，起点不断后移，最终 `start = 3`。
- 输出：`3`

### 15. 135. 分发糖果

- **难度**：困难
- **思路**：先从左往右保证「比左边高就多一颗」，再从右往左保证「比右边高就多一颗」，取两次最大值。

```java
public int candy(int[] ratings) {
    int n = ratings.length;
    int[] c = new int[n];
    Arrays.fill(c, 1);                       // 每人先发 1 颗
    for (int i = 1; i < n; i++) {            // 左 -> 右
        if (ratings[i] > ratings[i - 1]) {   // 比左边高
            c[i] = c[i - 1] + 1;             // 就比左边多一颗
        }
    }
    for (int i = n - 2; i >= 0; i--) {       // 右 -> 左
        if (ratings[i] > ratings[i + 1]) {   // 比右边高
            c[i] = Math.max(c[i], c[i + 1] + 1);  // 取较大者
        }
    }
    int ans = 0;
    for (int x : c) ans += x;                // 求和
    return ans;
}
```

**举例**：

- 输入：`[1,0,2]`
- 过程：左→右得 `[1,1,2]`；右→左，`0 < 1` 不变，得 `[2,1,2]`；合计 5。
- 输出：`5`

### 16. 42. 接雨水

- **难度**：困难
- **思路**：双指针。每个位置能接的水由「左右两边较矮的挡板」决定，谁矮就移动谁。

```java
public int trap(int[] height) {
    int l = 0, r = height.length - 1;   // 左右指针
    int lm = 0, rm = 0;                 // 左右最高挡板
    int ans = 0;
    while (l < r) {
        if (height[l] < height[r]) {    // 左边矮，先处理左
            if (height[l] >= lm) lm = height[l];   // 更新左挡板
            else ans += lm - height[l];            // 低于挡板，接水
            l++;
        } else {                        // 右边矮，先处理右
            if (height[r] >= rm) rm = height[r];   // 更新右挡板
            else ans += rm - height[r];            // 低于挡板，接水
            r--;
        }
    }
    return ans;
}
```

**举例**：

- 输入：`[0,1,0,2,1,0,1,3,2,1,2,1]`
- 过程：左右夹逼，各凹槽依次积水，累加得 6。
- 输出：`6`

### 17. 13. 罗马数字转整数

- **难度**：简单
- **思路**：遇到「小的在大的左边」就要减（如 IV=4），否则加。逐个判断即可。

```java
public int romanToInt(String s) {
    Map<Character, Integer> m = new HashMap<>();
    m.put('I', 1); m.put('V', 5); m.put('X', 10); m.put('L', 50);
    m.put('C', 100); m.put('D', 500); m.put('M', 1000);
    int ans = 0;
    for (int i = 0; i < s.length(); i++) {
        int v = m.get(s.charAt(i));
        // 小的在大的左边（如 IV）就减，否则加
        if (i + 1 < s.length() && v < m.get(s.charAt(i + 1))) ans -= v;
        else ans += v;
    }
    return ans;
}
```

**举例**：

- 输入：`"MCMXCIV"`
- 过程：1000 − 100 + 1000 − 10 + 100 − 1 + 5 = 1994。
- 输出：`1994`

### 18. 12. 整数转罗马数字

- **难度**：中等
- **思路**：贪心。把「面值 + 特殊组合」按从大到小排好，能减就减。

```java
public String intToRoman(int num) {
    // 面值从大到小（含 900/400/90/40/9/4 特殊组合）
    int[] vals = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
    String[] syms = {"M","CM","D","CD","C","XC","L","XL","X","IX","V","IV","I"};
    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < vals.length; i++) {
        while (num >= vals[i]) {        // 能减就减，同时拼对应符号
            num -= vals[i];
            sb.append(syms[i]);
        }
    }
    return sb.toString();
}
```

**举例**：

- 输入：`1994`
- 过程：1000 → M；900 → CM；90 → XC；4 → IV。
- 输出：`"MCMXCIV"`

### 19. 58. 最后一个单词的长度

- **难度**：简单
- **思路**：去掉末尾空格后，用最后一个空格的位置算长度。

```java
public int lengthOfLastWord(String s) {
    s = s.trim();                                   // 去掉首尾空格
    return s.length() - 1 - s.lastIndexOf(' ');     // 总长 - 最后空格下标 - 1
}
```

**举例**：

- 输入：`"   fly me   to   the moon  "`
- 过程：`trim()` 后为 `"fly me   to   the moon"`，最后一个空格在 `"moon"` 前，长度为 4。
- 输出：`4`

### 20. 14. 最长公共前缀

- **难度**：简单
- **思路**：拿第一个词当基准，不断和后面每个词比较，削短前缀直到匹配。

```java
public String longestCommonPrefix(String[] strs) {
    String p = strs[0];                          // 以第一个词为基准前缀
    for (int i = 1; i < strs.length; i++) {
        while (strs[i].indexOf(p) != 0) {        // 只要不以 p 开头
            p = p.substring(0, p.length() - 1);  // 就把末尾去掉一个字符
        }
        if (p.isEmpty()) return "";              // 前缀没了，直接返回
    }
    return p;
}
```

**举例**：

- 输入：`["flower","flow","flight"]`
- 过程：`"flower"` → 与 `"flow"` 比削成 `"flow"` → `"flo"` → `"fl"`；再与 `"flight"` 比仍是 `"fl"`。
- 输出：`"fl"`

### 21. 151. 反转字符串中的单词

- **难度**：中等
- **思路**：按空格切分（去掉多余空格），倒着拼起来。

```java
public String reverseWords(String s) {
    String[] w = s.trim().split("\\s+");   // 去首尾空格，按连续空格切分
    StringBuilder sb = new StringBuilder();
    for (int i = w.length - 1; i >= 0; i--) {  // 从后往前拼
        sb.append(w[i]);
        if (i > 0) sb.append(' ');         // 单词间加一个空格
    }
    return sb.toString();
}
```

**举例**：

- 输入：`"  the sky is blue  "`
- 过程：切分为 `["the","sky","is","blue"]`，倒序拼成 `"blue is sky the"`。
- 输出：`"blue is sky the"`

### 22. 6. Z 字形变换

- **难度**：中等
- **思路**：模拟。用多个 StringBuilder 代表每一行，让「行号」在 0 和 numRows-1 之间来回震荡。

```java
public String convert(String s, int numRows) {
    if (numRows == 1) return s;              // 只有一行，原样返回
    StringBuilder[] rows = new StringBuilder[numRows];
    for (int i = 0; i < numRows; i++) rows[i] = new StringBuilder();
    int r = 0, dir = 1;                      // r 当前行，dir 方向
    for (char c : s.toCharArray()) {
        rows[r].append(c);                   // 字符放到当前行
        if (r == 0) dir = 1;                 // 到顶往下走
        else if (r == numRows - 1) dir = -1; // 到底往上走
        r += dir;
    }
    StringBuilder ans = new StringBuilder();
    for (StringBuilder sb : rows) ans.append(sb);  // 按行拼接
    return ans.toString();
}
```

**举例**：

- 输入：`s = "PAYPALISHIRING", numRows = 3`
- 过程：三行分别为 `"PAHN"`、`"APLSIIG"`、`"YIR"`，按行拼接。
- 输出：`"PAHNAPLSIIGYIR"`

### 23. 28. 找出字符串中第一个匹配项的下标

- **难度**：简单
- **思路**：最直白的写法直接调 `indexOf`。面试想体现算法功底可写 KMP（见注释后的进阶版）。

```java
public int strStr(String haystack, String needle) {
    return haystack.indexOf(needle);   // 直接调用库方法
}

// 进阶：KMP（先算 next 数组，再匹配），不匹配时主串不回退
public int strStrKMP(String s, String p) {
    int n = s.length(), m = p.length();
    if (m == 0) return 0;
    int[] next = new int[m];                 // next[i]：失配时应回退的位置
    for (int i = 1, j = 0; i < m; i++) {     // 构建 next 数组
        while (j > 0 && p.charAt(i) != p.charAt(j)) j = next[j - 1];
        if (p.charAt(i) == p.charAt(j)) j++;
        next[i] = j;
    }
    for (int i = 0, j = 0; i < n; i++) {     // 匹配
        while (j > 0 && s.charAt(i) != p.charAt(j)) j = next[j - 1];
        if (s.charAt(i) == p.charAt(j)) j++;
        if (j == m) return i - m + 1;        // 匹配成功，返回起点
    }
    return -1;
}
```

**举例**：

- 输入：`haystack = "sadbutsad", needle = "sad"`
- 过程：`"sad"` 第一次出现在下标 0。
- 输出：`0`

### 24. 68. 文本左右对齐

- **难度**：困难
- **思路**：贪心按行填词。非末行的空格尽量均分、余数往左多给；末行或单单词左对齐补空格。

```java
public List<String> fullJustify(String[] words, int maxWidth) {
    List<String> ans = new ArrayList<>();
    int i = 0, n = words.length;
    while (i < n) {
        int j = i, len = 0;
        // 尽量多地把单词塞进一行（含单词间至少一个空格）
        while (j < n && len + words[j].length() + (j - i) <= maxWidth) {
            len += words[j].length();
            j++;
        }
        int cnt = j - i;                    // 本行单词数
        int spaces = maxWidth - len;        // 剩余空格总数
        StringBuilder sb = new StringBuilder(words[i]);
        if (j == n || cnt == 1) {           // 末行或单个词：左对齐
            for (int k = i + 1; k < j; k++) sb.append(' ').append(words[k]);
            while (sb.length() < maxWidth) sb.append(' ');  // 末尾补空格
        } else {                            // 均分空格，余数从左往右多给
            int gap = spaces / (cnt - 1), extra = spaces % (cnt - 1);
            for (int k = i + 1; k < j; k++) {
                for (int s = 0; s < gap; s++) sb.append(' ');
                if (extra > 0) { sb.append(' '); extra--; }
                sb.append(words[k]);
            }
        }
        ans.add(sb.toString());
        i = j;                              // 处理下一行
    }
    return ans;
}
```

**举例**：

- 输入：`words = ["This","is","an","example","of","text","justification."], maxWidth = 16`
- 过程：第一行放 `This/is/an`，多余空格均分；第二行 `example/of/text`；最后一行 `justification.` 左对齐补空格。
- 输出：`["This    is    an","example  of text","justification.  "]`

---

## 二、双指针

### 25. 125. 验证回文串

- **难度**：简单
- **思路**：左右两指针往中间走，跳过非字母数字，比较忽略大小写后是否相等。

```java
public boolean isPalindrome(String s) {
    int l = 0, r = s.length() - 1;          // 左右指针
    while (l < r) {
        while (l < r && !Character.isLetterOrDigit(s.charAt(l))) l++;  // 跳过左边非字母数字
        while (l < r && !Character.isLetterOrDigit(s.charAt(r))) r--;  // 跳过右边非字母数字
        // 忽略大小写比较，不同就返回 false
        if (Character.toLowerCase(s.charAt(l)) != Character.toLowerCase(s.charAt(r))) return false;
        l++; r--;                           // 继续向中间靠拢
    }
    return true;
}
```

**举例**：

- 输入：`"A man, a plan, a canal: Panama"`
- 过程：跳过空格和标点后，逐对比较 `a↔a`、`m↔m`、… 全部相同。
- 输出：`true`

### 26. 392. 判断子序列

- **难度**：简单
- **思路**：用指针在 `t` 里找 `s` 的每个字符，都找到了就是子序列。

```java
public boolean isSubsequence(String s, String t) {
    int i = 0;                       // 指向 s 中待匹配的字符
    for (char c : t.toCharArray()) { // 遍历 t
        if (i < s.length() && s.charAt(i) == c) i++;  // 匹配上就 i 后移
    }
    return i == s.length();          // s 都匹配完了就是子序列
}
```

**举例**：

- 输入：`s = "abc", t = "ahbgdc"`
- 过程：在 `t` 中依次找到 `a`、`b`、`c`，`i` 走到 3。
- 输出：`true`

### 27. 167. 两数之和 II - 输入有序数组

- **难度**：中等
- **思路**：数组有序，左右指针夹逼：和太小左移，太大右移。

```java
public int[] twoSum(int[] numbers, int target) {
    int l = 0, r = numbers.length - 1;   // 左右指针夹逼
    while (l < r) {
        int sum = numbers[l] + numbers[r];
        if (sum == target) return new int[]{l + 1, r + 1};  // 命中（下标从 1 开始）
        if (sum < target) l++;  // 太小，左指针右移
        else r--;               // 太大，右指针左移
    }
    return new int[]{};
}
```

**举例**：

- 输入：`numbers = [2,7,11,15], target = 9`
- 过程：`2+15=17` 太大右移；`2+11=13` 太大；`2+7=9` 命中。
- 输出：`[1,2]`

### 28. 11. 盛最多水的容器

- **难度**：中等
- **思路**：面积 = 底 × 两板较矮者。矮的那根决定上限，所以移动较矮的指针才可能变大。

```java
public int maxArea(int[] height) {
    int l = 0, r = height.length - 1, ans = 0;
    while (l < r) {
        // 面积 = 底 × 两板较矮者
        ans = Math.max(ans, (r - l) * Math.min(height[l], height[r]));
        if (height[l] < height[r]) l++;  // 移动较矮的板，才可能装更多
        else r--;
    }
    return ans;
}
```

**举例**：

- 输入：`[1,8,6,2,5,4,8,3,7]`
- 过程：左板 1、右板 7，面积 8；移动左板到 8，面积 49（最大值）；继续夹逼不再变大。
- 输出：`49`

### 29. 15. 三数之和

- **难度**：中等
- **思路**：先排序，固定第一个数，剩下两个用双指针夹逼；注意去重（跳过相同值）。

```java
public List<List<Integer>> threeSum(int[] nums) {
    Arrays.sort(nums);                        // 先排序
    List<List<Integer>> ans = new ArrayList<>();
    for (int i = 0; i < nums.length - 2; i++) {
        if (i > 0 && nums[i] == nums[i - 1]) continue;  // 去重第一个数
        int l = i + 1, r = nums.length - 1;   // 双指针夹逼
        while (l < r) {
            int sum = nums[i] + nums[l] + nums[r];
            if (sum == 0) {
                ans.add(Arrays.asList(nums[i], nums[l], nums[r]));
                while (l < r && nums[l] == nums[l + 1]) l++;  // 去重
                while (l < r && nums[r] == nums[r - 1]) r--;
                l++; r--;
            } else if (sum < 0) l++;          // 太小左移
            else r--;                         // 太大右移
        }
    }
    return ans;
}
```

**举例**：

- 输入：`[-1,0,1,2,-1,-4]`
- 过程：排序 `[-4,-1,-1,0,1,2]`；固定 -1 时找到 `(-1,-1,2)`、`(-1,0,1)`。
- 输出：`[[-1,-1,2],[-1,0,1]]`

---

## 三、滑动窗口

### 30. 209. 长度最小的子数组

- **难度**：中等
- **思路**：滑动窗口。右指针不断扩张累加，一旦窗口和达标就收缩左指针，记录最短长度。

```java
public int minSubArrayLen(int target, int[] nums) {
    int l = 0, sum = 0, ans = Integer.MAX_VALUE;
    for (int r = 0; r < nums.length; r++) {
        sum += nums[r];                     // 右指针扩张，累加进窗口
        while (sum >= target) {             // 窗口和达标了，尝试收缩
            ans = Math.min(ans, r - l + 1); // 记录当前窗口长度
            sum -= nums[l++];               // 左边界右移，窗口和变小
        }
    }
    return ans == Integer.MAX_VALUE ? 0 : ans;  // 没找到返回 0
}
```

**举例**：

- 输入：`target = 7, nums = [2,3,1,2,4,3]`
- 过程：窗口 `[2,3,1,2]` 和 8 达标（长 4）；收缩后 `[3,1,2,4]` 和 10；继续收缩得 `[4,3]` 和 7，长 2，最短。
- 输出：`2`

### 31. 3. 无重复字符的最长子串

- **难度**：中等
- **思路**：滑动窗口 + 集合。右指针扩张，遇到重复字符就收缩左指针，窗口始终无重复。

```java
public int lengthOfLongestSubstring(String s) {
    Set<Character> set = new HashSet<>();   // 窗口内出现过的字符
    int l = 0, ans = 0;
    for (int r = 0; r < s.length(); r++) {
        char c = s.charAt(r);
        while (set.contains(c)) {           // 出现重复，收缩左边界
            set.remove(s.charAt(l++));
        }
        set.add(c);                          // 加入当前字符
        ans = Math.max(ans, r - l + 1);      // 更新最大长度
    }
    return ans;
}
```

**举例**：

- 输入：`"abcabcbb"`
- 过程：`abc` 长 3；到第二个 `a` 重复，收缩掉第一个 `a`，得 `bca`；… 最长始终为 3。
- 输出：`3`

### 32. 30. 串联所有单词的子串

- **难度**：困难
- **思路**：滑动窗口。把 `s` 切成固定长度 `w`（每个单词的长度）的小块，用哈希表统计窗口内各单词数量是否与 `words` 一致。

```java
public List<Integer> findSubstring(String s, String[] words) {
    List<Integer> ans = new ArrayList<>();
    int w = words[0].length(), n = words.length;      // w 单词长度，n 单词个数
    Map<String, Integer> need = new HashMap<>();
    for (String word : words) need.merge(word, 1, Integer::sum);  // 统计所需单词
    for (int i = 0; i < w; i++) {                     // 起点偏移 0 ~ w-1 各跑一遍
        Map<String, Integer> cnt = new HashMap<>();   // 当前窗口单词统计
        int l = i, matched = 0;
        for (int r = i; r + w <= s.length(); r += w) {
            String cur = s.substring(r, r + w);       // 右指针取一个单词
            cnt.merge(cur, 1, Integer::sum);
            if (cnt.get(cur) <= need.getOrDefault(cur, 0)) matched++;  // 有效匹配
            if (r - l + w > n * w) {                  // 窗口超长，收缩左边一个单词
                String left = s.substring(l, l + w);
                cnt.merge(left, -1, Integer::sum);
                if (cnt.get(left) < need.getOrDefault(left, 0)) matched--;
                l += w;
            }
            if (matched == n) ans.add(l);             // 窗口内正好是所有单词
        }
    }
    return ans;
}
```

**举例**：

- 输入：`s = "barfoothefoobarman", words = ["foo","bar"]`
- 过程：窗口 `barfoo`、`foobar` 恰好覆盖两个单词，起始下标 0 和 9。
- 输出：`[0,9]`

### 33. 76. 最小覆盖子串

- **难度**：困难
- **思路**：滑动窗口。右指针扩张直到覆盖 `t` 全部字符，再收缩左指针求最短窗口。

```java
public String minWindow(String s, String t) {
    int[] need = new int[128];                  // t 各字符需求数
    for (char c : t.toCharArray()) need[c]++;
    int[] win = new int[128];                   // 窗口内各字符数
    int l = 0, matched = 0;                     // matched：已满足的需求字符数
    int start = 0, minLen = Integer.MAX_VALUE;
    for (int r = 0; r < s.length(); r++) {
        char c = s.charAt(r);
        win[c]++;                               // 右边界加入
        if (win[c] <= need[c]) matched++;       // 该字符还没超过需求，计入
        while (matched == t.length()) {         // 已覆盖全部字符，尝试收缩
            if (r - l + 1 < minLen) {           // 记录更短窗口
                minLen = r - l + 1;
                start = l;
            }
            char d = s.charAt(l);
            win[d]--;                           // 移出左边界
            if (win[d] < need[d]) matched--;    // 移出后不足，需求减少
            l++;
        }
    }
    return minLen == Integer.MAX_VALUE ? "" : s.substring(start, start + minLen);
}
```

**举例**：

- 输入：`s = "ADOBECODEBANC", t = "ABC"`
- 过程：右扩到 `ADOBEC` 覆盖 A、B、C；左收缩得到更短的 `BANC`。
- 输出：`"BANC"`

---

## 四、矩阵

### 34. 36. 有效的数独

- **难度**：中等
- **思路**：用三个二维数组分别记录「每行、每列、每个 3×3 宫格」里数字是否已出现。

```java
public boolean isValidSudoku(char[][] board) {
    boolean[][] row = new boolean[9][10];   // 行 [i][num] 是否出现过
    boolean[][] col = new boolean[9][10];   // 列 [j][num] 是否出现过
    boolean[][] box = new boolean[9][10];   // 宫格 [b][num] 是否出现过
    for (int i = 0; i < 9; i++) {
        for (int j = 0; j < 9; j++) {
            if (board[i][j] == '.') continue;
            int num = board[i][j] - '0';
            int b = i / 3 * 3 + j / 3;      // 计算属于哪个 3x3 宫格
            if (row[i][num] || col[j][num] || box[b][num]) return false;  // 重复
            row[i][num] = col[j][num] = box[b][num] = true;               // 标记
        }
    }
    return true;
}
```

**举例**：

- 输入：一个填写合法（行、列、宫格内均无重复）的数独盘面。
- 输出：`true`

### 35. 54. 螺旋矩阵

- **难度**：中等
- **思路**：用上下左右四个边界，按「右、下、左、上」的顺序一圈圈收缩遍历。

```java
public List<Integer> spiralOrder(int[][] matrix) {
    List<Integer> ans = new ArrayList<>();
    int top = 0, bottom = matrix.length - 1;
    int left = 0, right = matrix[0].length - 1;
    while (top <= bottom && left <= right) {
        for (int j = left; j <= right; j++) ans.add(matrix[top][j]);   // 上边：左→右
        top++;
        for (int i = top; i <= bottom; i++) ans.add(matrix[i][right]); // 右边：上→下
        right--;
        if (top <= bottom) {                                            // 还有下边
            for (int j = right; j >= left; j--) ans.add(matrix[bottom][j]);
            bottom--;
        }
        if (left <= right) {                                            // 还有左边
            for (int i = bottom; i >= top; i--) ans.add(matrix[i][left]);
            left++;
        }
    }
    return ans;
}
```

**举例**：

- 输入：`[[1,2,3],[4,5,6],[7,8,9]]`
- 过程：先上边 1,2,3 → 右边 6,9 → 下边 8,7 → 左边 4 → 内圈 5。
- 输出：`[1,2,3,6,9,8,7,4,5]`

### 36. 48. 旋转图像

- **难度**：中等
- **思路**：先沿对角线转置，再把每一行左右翻转，即可顺时针旋转 90°。

```java
public void rotate(int[][] matrix) {
    int n = matrix.length;
    for (int i = 0; i < n; i++) {           // 转置：沿左上-右下对角线交换
        for (int j = i + 1; j < n; j++) {
            int t = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = t;
        }
    }
    for (int i = 0; i < n; i++) {           // 每行左右翻转
        for (int j = 0; j < n / 2; j++) {
            int t = matrix[i][j];
            matrix[i][j] = matrix[i][n - 1 - j];
            matrix[i][n - 1 - j] = t;
        }
    }
}
```

**举例**：

- 输入：`[[1,2,3],[4,5,6],[7,8,9]]`
- 过程：转置得 `[[1,4,7],[2,5,8],[3,6,9]]`；每行翻转得 `[[7,4,1],[8,5,2],[9,6,3]]`。
- 输出：`[[7,4,1],[8,5,2],[9,6,3]]`

### 37. 73. 矩阵置零

- **难度**：中等
- **思路**：用第一行、第一列当「标记位」，记录哪些行/列需要置零，最后统一处理。

```java
public void setZeroes(int[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    boolean row0 = false, col0 = false;
    for (int j = 0; j < n; j++) if (matrix[0][j] == 0) row0 = true;  // 第一行是否要清零
    for (int i = 0; i < m; i++) if (matrix[i][0] == 0) col0 = true;  // 第一列是否要清零
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            if (matrix[i][j] == 0) {         // 用第一行、第一列打标记
                matrix[i][0] = 0;
                matrix[0][j] = 0;
            }
        }
    }
    for (int i = 1; i < m; i++) {            // 根据标记清零
        for (int j = 1; j < n; j++) {
            if (matrix[i][0] == 0 || matrix[0][j] == 0) matrix[i][j] = 0;
        }
    }
    if (row0) for (int j = 0; j < n; j++) matrix[0][j] = 0;   // 清第一行
    if (col0) for (int i = 0; i < m; i++) matrix[i][0] = 0;   // 清第一列
}
```

**举例**：

- 输入：`[[1,1,1],[1,0,1],[1,1,1]]`
- 过程：标记第 1 行、第 1 列需清零，据此把对应行列置 0。
- 输出：`[[1,0,1],[0,0,0],[1,0,1]]`

### 38. 289. 生命游戏

- **难度**：中等
- **思路**：原地更新，用特殊值标记状态变化（2 = 活→死，-1 = 死→活），最后统一还原，避免影响后续判断。

```java
public void gameOfLife(int[][] board) {
    int m = board.length, n = board[0].length;
    int[] dx = {-1,-1,-1,0,0,1,1,1};   // 8 个方向的偏移
    int[] dy = {-1,0,1,-1,1,-1,0,1};
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            int live = 0;
            for (int k = 0; k < 8; k++) {   // 统计周围活细胞数
                int x = i + dx[k], y = j + dy[k];
                if (x >= 0 && x < m && y >= 0 && y < n && Math.abs(board[x][y]) == 1) live++;
            }
            if (board[i][j] == 1 && (live < 2 || live > 3)) board[i][j] = 2;   // 活→死，记 2
            if (board[i][j] == 0 && live == 3) board[i][j] = -1;               // 死→活，记 -1
        }
    }
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (board[i][j] == 2) board[i][j] = 0;      // 还原死细胞
            if (board[i][j] == -1) board[i][j] = 1;     // 还原活细胞
        }
    }
}
```

**举例**：

- 输入：`[[0,1,0],[0,0,1],[1,1,1],[0,0,0]]`
- 过程：按规则「活细胞周围少于 2 或多于 3 个则死，死细胞周围恰好 3 个则活」演化一代。
- 输出：`[[0,0,0],[1,0,1],[0,1,1],[0,1,0]]`

---

## 五、哈希表

### 39. 383. 赎金信

- **难度**：简单
- **思路**：先统计杂志里每个字母的数量，再逐个扣掉赎金信需要的字母，不够用就失败。

```java
public boolean canConstruct(String ransomNote, String magazine) {
    int[] cnt = new int[26];
    for (char c : magazine.toCharArray()) cnt[c - 'a']++;   // 统计杂志里的字母
    for (char c : ransomNote.toCharArray()) {
        if (--cnt[c - 'a'] < 0) return false;   // 字母不够用，失败
    }
    return true;
}
```

**举例**：

- 输入：`ransomNote = "aa", magazine = "aab"`
- 过程：杂志里有 2 个 `a`、1 个 `b`，够赎金信的 2 个 `a`。
- 输出：`true`

### 40. 205. 同构字符串

- **难度**：简单
- **思路**：字符必须一一对应。用两个映射分别保证 `s→t` 和 `t→s` 都不冲突。

```java
public boolean isIsomorphic(String s, String t) {
    Map<Character, Character> s2t = new HashMap<>();   // s 的字符 -> t 的字符
    Map<Character, Character> t2s = new HashMap<>();   // t 的字符 -> s 的字符
    for (int i = 0; i < s.length(); i++) {
        char a = s.charAt(i), b = t.charAt(i);
        if (s2t.containsKey(a) && s2t.get(a) != b) return false;  // 一个 s 对应多个 t
        if (t2s.containsKey(b) && t2s.get(b) != a) return false;  // 一个 t 对应多个 s
        s2t.put(a, b);
        t2s.put(b, a);
    }
    return true;
}
```

**举例**：

- 输入：`s = "egg", t = "add"`
- 过程：`e→a`、`g→d` 一一对应，无冲突。
- 输出：`true`

### 41. 290. 单词规律

- **难度**：简单
- **思路**：和同构字符串一个套路，`pattern` 的字符和 `s` 里的单词做双向映射。

```java
public boolean wordPattern(String pattern, String s) {
    String[] words = s.split(" ");
    if (pattern.length() != words.length) return false;   // 长度不一致直接失败
    Map<Character, String> p2w = new HashMap<>();   // 字符 -> 单词
    Map<String, Character> w2p = new HashMap<>();   // 单词 -> 字符
    for (int i = 0; i < pattern.length(); i++) {
        char c = pattern.charAt(i);
        String w = words[i];
        if (p2w.containsKey(c) && !p2w.get(c).equals(w)) return false;  // 映射冲突
        if (w2p.containsKey(w) && w2p.get(w) != c) return false;
        p2w.put(c, w);
        w2p.put(w, c);
    }
    return true;
}
```

**举例**：

- 输入：`pattern = "abba", s = "dog cat cat dog"`
- 过程：`a→dog`、`b→cat`，回文对称，映射一致。
- 输出：`true`

### 42. 242. 有效的字母异位词

- **难度**：简单
- **思路**：用计数数组，一个字符串加、另一个减，最后全是 0 就是异位词。

```java
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) return false;   // 长度不同一定不是
    int[] cnt = new int[26];
    for (char c : s.toCharArray()) cnt[c - 'a']++;
    for (char c : t.toCharArray()) {
        if (--cnt[c - 'a'] < 0) return false;     // 字符不够减，不是异位词
    }
    return true;
}
```

**举例**：

- 输入：`s = "anagram", t = "nagaram"`
- 过程：各字母出现次数完全相同，计数归零。
- 输出：`true`

### 43. 49. 字母异位词分组

- **难度**：中等
- **思路**：把每个单词排序后的结果当 key，异位词排序后一定相同，自动分到同一组。

```java
public List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> map = new HashMap<>();
    for (String s : strs) {
        char[] arr = s.toCharArray();
        Arrays.sort(arr);                          // 排序后作为 key
        String key = new String(arr);
        map.computeIfAbsent(key, k -> new ArrayList<>()).add(s);  // 分组
    }
    return new ArrayList<>(map.values());
}
```

**举例**：

- 输入：`["eat","tea","tan","ate","nat","bat"]`
- 过程：`eat/tea/ate` 排序后都是 `aet`，归为一组；`tan/nat` 是 `ant`；`bat` 单独。
- 输出：`[["bat"],["nat","tan"],["ate","eat","tea"]]`（组内顺序不限）

### 44. 1. 两数之和

- **难度**：简单
- **思路**：用哈希表存「值 → 下标」，遍历时直接查「target − 当前值」是否出现过。

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();   // 值 -> 下标
    for (int i = 0; i < nums.length; i++) {
        int need = target - nums[i];               // 需要的另一个数
        if (map.containsKey(need)) return new int[]{map.get(need), i};
        map.put(nums[i], i);
    }
    return new int[]{};
}
```

**举例**：

- 输入：`nums = [2,7,11,15], target = 9`
- 过程：到 7 时查 9−7=2 已存在，返回下标。
- 输出：`[0,1]`

### 45. 202. 快乐数

- **难度**：简单
- **思路**：反复求「各位平方和」，用集合记录出现过的数，一旦重复说明进入死循环，不可能到 1。

```java
public boolean isHappy(int n) {
    Set<Integer> seen = new HashSet<>();
    while (n != 1 && seen.add(n)) {   // 记录出现过的数，重复就退出循环
        int sum = 0;
        while (n > 0) {
            int d = n % 10;           // 取个位
            sum += d * d;             // 平方累加
            n /= 10;                  // 去掉个位
        }
        n = sum;
    }
    return n == 1;
}
```

**举例**：

- 输入：`19`
- 过程：`1²+9²=82` → `68` → `100` → `1`。
- 输出：`true`

### 46. 219. 存在重复元素 II

- **难度**：简单
- **思路**：哈希表记录每个值「最近一次出现的下标」，判断两次出现距离是否 <= k。

```java
public boolean containsNearbyDuplicate(int[] nums, int k) {
    Map<Integer, Integer> map = new HashMap<>();   // 值 -> 最近一次下标
    for (int i = 0; i < nums.length; i++) {
        if (map.containsKey(nums[i]) && i - map.get(nums[i]) <= k) return true;  // 距离满足
        map.put(nums[i], i);
    }
    return false;
}
```

**举例**：

- 输入：`nums = [1,2,3,1], k = 3`
- 过程：值 1 出现在下标 0 和 3，距离 3 <= 3。
- 输出：`true`

### 47. 128. 最长连续序列

- **难度**：中等
- **思路**：全部放进集合，只从「连续序列的起点」（前一个数不存在）开始往后数，避免重复计算。

```java
public int longestConsecutive(int[] nums) {
    Set<Integer> set = new HashSet<>();
    for (int x : nums) set.add(x);
    int ans = 0;
    for (int x : set) {
        if (set.contains(x - 1)) continue;   // 不是序列起点，跳过
        int len = 1;
        while (set.contains(x + len)) len++; // 从起点往后数连续长度
        ans = Math.max(ans, len);
    }
    return ans;
}
```

**举例**：

- 输入：`[100,4,200,1,3,2]`
- 过程：从 1 开始能数到 4，长度 4；100、200 各长度为 1。
- 输出：`4`

---

## 六、区间

### 48. 228. 汇总区间

- **难度**：简单
- **思路**：遍历数组，把连续递增的段找出来，起点 == 终点输出单个数，否则输出 `起点->终点`。

```java
public List<String> summaryRanges(int[] nums) {
    List<String> ans = new ArrayList<>();
    int i = 0, n = nums.length;
    while (i < n) {
        int start = nums[i];
        while (i + 1 < n && nums[i + 1] == nums[i] + 1) i++;  // 找连续段末尾
        if (nums[i] == start) ans.add(String.valueOf(start)); // 只有一个数
        else ans.add(start + "->" + nums[i]);                 // 区间
        i++;
    }
    return ans;
}
```

**举例**：

- 输入：`[0,1,2,4,5,7]`
- 过程：`0,1,2` 连续 → `"0->2"`；`4,5` → `"4->5"`；`7` → `"7"`。
- 输出：`["0->2","4->5","7"]`

### 49. 56. 合并区间

- **难度**：中等
- **思路**：按起点排序，逐个和结果里最后一个区间比较，能重叠就合并，不能就新增。

```java
public int[][] merge(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);   // 按起点排序
    List<int[]> list = new ArrayList<>();
    for (int[] cur : intervals) {
        if (list.isEmpty() || list.get(list.size() - 1)[1] < cur[0]) {
            list.add(cur);                            // 无重叠，直接加入
        } else {                                      // 有重叠，扩展末尾
            list.get(list.size() - 1)[1] = Math.max(list.get(list.size() - 1)[1], cur[1]);
        }
    }
    return list.toArray(new int[0][]);
}
```

**举例**：

- 输入：`[[1,3],[2,6],[8,10],[15,18]]`
- 过程：`[1,3]` 和 `[2,6]` 重叠合并成 `[1,6]`；其余不重叠。
- 输出：`[[1,6],[8,10],[15,18]]`

### 50. 57. 插入区间

- **难度**：中等
- **思路**：分三段——左边无重叠的直接加，中间有重叠的和新区间合并，右边无重叠的直接加。

```java
public int[][] insert(int[][] intervals, int[] newInterval) {
    List<int[]> list = new ArrayList<>();
    int i = 0, n = intervals.length;
    while (i < n && intervals[i][1] < newInterval[0]) {   // 完全在左边，直接加
        list.add(intervals[i++]);
    }
    while (i < n && intervals[i][0] <= newInterval[1]) {  // 有重叠，不断合并
        newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
        i++;
    }
    list.add(newInterval);                                // 加入合并后的区间
    while (i < n) list.add(intervals[i++]);               // 右边剩余的直接加
    return list.toArray(new int[0][]);
}
```

**举例**：

- 输入：`intervals = [[1,3],[6,9]], newInterval = [2,5]`
- 过程：`[1,3]` 与 `[2,5]` 重叠，合并成 `[1,5]`；`[6,9]` 在右边不重叠。
- 输出：`[[1,5],[6,9]]`

### 51. 452. 用最少数量的箭引爆气球

- **难度**：中等
- **思路**：按右端点排序，一支箭尽量射在「当前气球右端」，覆盖所有重叠的气球，盖不住就加箭。

```java
public int findMinArrowShots(int[][] points) {
    Arrays.sort(points, (a, b) -> Integer.compare(a[1], b[1]));  // 按右端点排序
    int arrows = 1;
    int end = points[0][1];                    // 第一支箭射在第一个气球右端
    for (int[] p : points) {
        if (p[0] > end) {                      // 这个气球在箭的右边，需要新箭
            arrows++;
            end = p[1];
        }
    }
    return arrows;
}
```

**举例**：

- 输入：`[[10,16],[2,8],[1,6],[7,12]]`
- 过程：第一支箭射在 `[1,6]` 右端 6，覆盖 `[2,8]`；第二支箭射在 `[10,16]` 右端 16，覆盖 `[7,12]`。
- 输出：`2`

---

## 七、栈

> 栈相关的题目统一用 `Deque`（双端队列）实现栈，提交时记得 `import java.util.*;`。

### 52. 20. 有效的括号

- **难度**：简单
- **思路**：左括号入栈，遇到右括号就弹出栈顶配对，配不上或不匹配就返回 false。

```java
public boolean isValid(String s) {
    Deque<Character> stack = new ArrayDeque<>();
    for (char c : s.toCharArray()) {
        if (c == '(' || c == '[' || c == '{') {   // 左括号入栈
            stack.push(c);
        } else {                                   // 右括号，找栈顶配对
            if (stack.isEmpty()) return false;
            char top = stack.pop();
            if (c == ')' && top != '(') return false;
            if (c == ']' && top != '[') return false;
            if (c == '}' && top != '{') return false;
        }
    }
    return stack.isEmpty();                        // 最后栈要为空
}
```

**举例**：

- 输入：`"()[]{}"`
- 过程：`(` 入栈、`)` 配对弹出；`[`、`]` 配对；`{`、`}` 配对，最后栈空。
- 输出：`true`

### 53. 71. 简化路径

- **难度**：中等
- **思路**：按 `/` 切分，`.` 和空跳过，`..` 弹出上一级，正常目录入栈，最后拼接。

```java
public String simplifyPath(String path) {
    Deque<String> stack = new ArrayDeque<>();
    for (String dir : path.split("/")) {   // 按 / 切分
        if (dir.equals("") || dir.equals(".")) continue;   // 空或当前目录，跳过
        if (dir.equals("..")) {             // 返回上一级
            if (!stack.isEmpty()) stack.pop();
        } else {
            stack.push(dir);                // 正常目录入栈
        }
    }
    StringBuilder sb = new StringBuilder();
    while (!stack.isEmpty()) sb.insert(0, "/" + stack.pop());  // 从栈底到栈顶拼接
    return sb.length() == 0 ? "/" : sb.toString();
}
```

**举例**：

- 输入：`"/home//foo/"`
- 过程：切分为 `["", "home", "", "foo", ""]`，跳过空串，栈为 `[home, foo]`。
- 输出：`"/home/foo"`

### 54. 155. 最小栈

- **难度**：中等
- **思路**：用两个栈，一个存所有元素，另一个同步存「当前最小值」。

```java
class MinStack {
    Deque<Integer> stack = new ArrayDeque<>();    // 存所有元素
    Deque<Integer> minStack = new ArrayDeque<>(); // 存当前最小值

    public void push(int val) {
        stack.push(val);
        // 若 val 不大于当前最小值，压入 minStack
        if (minStack.isEmpty() || val <= minStack.peek()) minStack.push(val);
    }

    public void pop() {
        if (stack.pop().equals(minStack.peek())) minStack.pop();  // 弹出的正好是最小值，同步弹出
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return minStack.peek();   // 栈顶始终是当前最小值
    }
}
```

**举例**：

- 操作：`push(-2)`、`push(0)`、`push(-3)`、`getMin()`
- 过程：`minStack` 依次为 `[-2]`、`[-2,-3]`，栈顶 -3 即最小值。
- 输出：`-3`

### 55. 150. 逆波兰表达式求值

- **难度**：中等
- **思路**：遇到数字入栈，遇到运算符弹出两个数计算，结果再入栈。

```java
public int evalRPN(String[] tokens) {
    Deque<Integer> stack = new ArrayDeque<>();
    for (String t : tokens) {
        if ("+".equals(t)) { int b = stack.pop(), a = stack.pop(); stack.push(a + b); }
        else if ("-".equals(t)) { int b = stack.pop(), a = stack.pop(); stack.push(a - b); }
        else if ("*".equals(t)) { int b = stack.pop(), a = stack.pop(); stack.push(a * b); }
        else if ("/".equals(t)) { int b = stack.pop(), a = stack.pop(); stack.push(a / b); }
        else stack.push(Integer.parseInt(t));   // 数字直接入栈
    }
    return stack.pop();
}
```

**举例**：

- 输入：`["2","1","+","3","*"]`
- 过程：2、1 入栈 → `+` 弹出得 3 → 3 入栈 → `*` 弹出 3、3 得 9。
- 输出：`9`

### 56. 224. 基本计算器

- **难度**：困难
- **思路**：遇到括号时把「当前结果和符号」压栈保存，先算括号内的，`)` 时再恢复。

```java
public int calculate(String s) {
    Deque<Integer> stack = new ArrayDeque<>();
    int num = 0, sign = 1, result = 0;
    for (char c : s.toCharArray()) {
        if (Character.isDigit(c)) {
            num = num * 10 + (c - '0');         // 累积多位数
        } else if (c == '+') {
            result += sign * num;               // 结算上一个数
            num = 0;
            sign = 1;
        } else if (c == '-') {
            result += sign * num;
            num = 0;
            sign = -1;
        } else if (c == '(') {
            stack.push(result);                 // 保存当前结果
            stack.push(sign);                   // 保存当前符号
            result = 0;                         // 重置，计算括号内
            sign = 1;
        } else if (c == ')') {
            result += sign * num;               // 结算括号内最后一个数
            num = 0;
            result *= stack.pop();              // 乘上括号前的符号
            result += stack.pop();              // 加上括号前的结果
        }
    }
    return result + sign * num;                 // 结算最后的数
}
```

**举例**：

- 输入：`"(1+(4+5+2)-3)+(6+8)"`
- 过程：逐层处理括号，最终累加得 23。
- 输出：`23`

---

## 八、链表

> 链表题目使用力扣内置的节点类 `ListNode`（`int val; ListNode next;`），提交时无需自己定义。

### 57. 141. 环形链表

- **难度**：简单
- **思路**：快慢指针。慢指针走一步、快指针走两步，若有环它们一定会相遇。

```java
public boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;   // 快慢指针
    while (fast != null && fast.next != null) {
        slow = slow.next;                // 慢指针走一步
        fast = fast.next.next;           // 快指针走两步
        if (slow == fast) return true;   // 相遇说明有环
    }
    return false;                        // 快指针到头，无环
}
```

**举例**：

- 输入：`head = [3,2,0,-4]`（尾节点指向下标 1）
- 过程：快慢指针最终在环内相遇。
- 输出：`true`

### 58. 2. 两数相加

- **难度**：中等
- **思路**：模拟竖式加法，逐位相加，处理好进位。

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);   // 哨兵节点
    ListNode cur = dummy;
    int carry = 0;                       // 进位
    while (l1 != null || l2 != null || carry != 0) {
        int sum = carry;
        if (l1 != null) { sum += l1.val; l1 = l1.next; }
        if (l2 != null) { sum += l2.val; l2 = l2.next; }
        cur.next = new ListNode(sum % 10);   // 当前位
        carry = sum / 10;                    // 进位
        cur = cur.next;
    }
    return dummy.next;
}
```

**举例**：

- 输入：`l1 = [2,4,3], l2 = [5,6,4]`
- 过程：`2+5=7`、`4+6=10`（进 1）、`3+4+1=8`。
- 输出：`[7,0,8]`

### 59. 21. 合并两个有序链表

- **难度**：简单
- **思路**：两个指针分别指向两条链表，谁小接谁，最后把剩余的直接接上。

```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);   // 哨兵节点
    ListNode cur = dummy;
    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {          // 谁小接谁
            cur.next = l1;
            l1 = l1.next;
        } else {
            cur.next = l2;
            l2 = l2.next;
        }
        cur = cur.next;
    }
    cur.next = (l1 != null) ? l1 : l2;  // 剩下的直接接上
    return dummy.next;
}
```

**举例**：

- 输入：`l1 = [1,2,4], l2 = [1,3,4]`
- 过程：1=1 接 l1 的 1，再比较接 l2 的 1，依次接 2、3、4、4。
- 输出：`[1,1,2,3,4,4]`

### 60. 138. 复制带随机指针的链表

- **难度**：中等
- **思路**：用哈希表建立「原节点 → 新节点」映射，第一遍复制节点，第二遍连 `next` 和 `random`。

```java
public Node copyRandomList(Node head) {
    if (head == null) return null;
    Map<Node, Node> map = new HashMap<>();   // 原节点 -> 新节点
    Node cur = head;
    while (cur != null) {                    // 第一遍：复制每个节点
        map.put(cur, new Node(cur.val));
        cur = cur.next;
    }
    cur = head;
    while (cur != null) {                    // 第二遍：连接 next 和 random
        map.get(cur).next = map.get(cur.next);
        map.get(cur).random = map.get(cur.random);
        cur = cur.next;
    }
    return map.get(head);
}
```

**举例**：

- 输入：一个带 `random` 指针的链表。
- 过程：复制出所有节点后，按映射关系把 `next`、`random` 都指向对应新节点。
- 输出：深拷贝的新链表（与原链表结构相同、指针独立）

### 61. 92. 反转链表 II

- **难度**：中等
- **思路**：定位到 `left` 前一个节点，用「头插法」把 `[left, right]` 区间的节点逐个插到前面，完成局部反转。

```java
public ListNode reverseBetween(ListNode head, int left, int right) {
    ListNode dummy = new ListNode(0);   // 哨兵，简化头节点处理
    dummy.next = head;
    ListNode pre = dummy;
    for (int i = 1; i < left; i++) pre = pre.next;   // pre 走到 left 前一个
    ListNode cur = pre.next;                          // 要反转的第一个节点
    for (int i = 0; i < right - left; i++) {          // 头插法反转
        ListNode nxt = cur.next;
        cur.next = nxt.next;
        nxt.next = pre.next;
        pre.next = nxt;
    }
    return dummy.next;
}
```

**举例**：

- 输入：`head = [1,2,3,4,5], left = 2, right = 4`
- 过程：反转 2→3→4 为 4→3→2，接上前后。
- 输出：`[1,4,3,2,5]`

### 62. 25. K 个一组翻转链表

- **难度**：困难
- **思路**：每数够 k 个节点就反转这一组，接回原链表；不足 k 个保持原样。

```java
public ListNode reverseKGroup(ListNode head, int k) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode pre = dummy;
    while (true) {
        ListNode tail = pre;
        for (int i = 0; i < k && tail != null; i++) tail = tail.next;  // 找本组最后一个节点
        if (tail == null) break;             // 不足 k 个，结束
        ListNode nextGroup = tail.next;      // 下一组的起点
        ListNode[] reversed = reverse(pre.next, tail);  // 反转 [pre.next, tail]
        pre.next = reversed[0];              // 接上反转后的头
        reversed[1].next = nextGroup;        // 反转后的尾接下一组
        pre = reversed[1];                   // pre 移到本组末尾（即下一组的前驱）
    }
    return dummy.next;
}

// 反转从 head 到 tail 的一段，返回 [新头, 新尾]
private ListNode[] reverse(ListNode head, ListNode tail) {
    ListNode prev = tail.next, cur = head;
    while (prev != tail) {
        ListNode nxt = cur.next;
        cur.next = prev;
        prev = cur;
        cur = nxt;
    }
    return new ListNode[]{tail, head};
}
```

**举例**：

- 输入：`head = [1,2,3,4,5], k = 2`
- 过程：翻转 `[1,2]` 为 `[2,1]`，翻转 `[3,4]` 为 `[4,3]`，5 不足一组保持。
- 输出：`[2,1,4,3,5]`

### 63. 19. 删除链表的倒数第 N 个结点

- **难度**：中等
- **思路**：快指针先走 n+1 步，再快慢一起走，快指针到头时，慢指针正好在待删节点的前一个。

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);   // 哨兵
    dummy.next = head;
    ListNode fast = dummy, slow = dummy;
    for (int i = 0; i <= n; i++) fast = fast.next;   // 快指针先走 n+1 步
    while (fast != null) {                           // 快慢一起走
        fast = fast.next;
        slow = slow.next;
    }
    slow.next = slow.next.next;         // 删掉 slow 后面的节点
    return dummy.next;
}
```

**举例**：

- 输入：`head = [1,2,3,4,5], n = 2`
- 过程：快慢指针拉开 2 个间隔，快指针到头时慢指针指向 3，删掉 4。
- 输出：`[1,2,3,5]`

### 64. 82. 删除排序链表中的重复元素 II

- **难度**：中等
- **思路**：一旦发现相邻两个值相等，就跳过所有等于该值的节点。

```java
public ListNode deleteDuplicates(ListNode head) {
    ListNode dummy = new ListNode(0);   // 哨兵
    dummy.next = head;
    ListNode cur = dummy;
    while (cur.next != null && cur.next.next != null) {
        if (cur.next.val == cur.next.next.val) {   // 发现重复
            int x = cur.next.val;
            while (cur.next != null && cur.next.val == x) {  // 跳过所有重复
                cur.next = cur.next.next;
            }
        } else {
            cur = cur.next;              // 不重复，正常前进
        }
    }
    return dummy.next;
}
```

**举例**：

- 输入：`[1,2,3,3,4,4,5]`
- 过程：3、4 都重复出现，整段跳过。
- 输出：`[1,2,5]`

### 65. 61. 旋转链表

- **难度**：中等
- **思路**：先连成环，找到新尾节点断开即可。

```java
public ListNode rotateRight(ListNode head, int k) {
    if (head == null || head.next == null) return head;
    ListNode cur = head;
    int len = 1;
    while (cur.next != null) { len++; cur = cur.next; }   // 求长度，cur 停在尾节点
    k %= len;                              // k 取模，去掉整圈
    if (k == 0) return head;
    cur.next = head;                       // 连成环
    for (int i = 0; i < len - k; i++) cur = cur.next;   // 走到新尾
    ListNode newHead = cur.next;           // 新头
    cur.next = null;                       // 断开环
    return newHead;
}
```

**举例**：

- 输入：`head = [1,2,3,4,5], k = 2`
- 过程：连成环后，从尾走 3 步到 3，断开，新头为 4。
- 输出：`[4,5,1,2,3]`

### 66. 86. 分隔链表

- **难度**：中等
- **思路**：建两个链表，小于 x 的接小链表，其余接大链表，最后拼起来。

```java
public ListNode partition(ListNode head, int x) {
    ListNode small = new ListNode(0);   // 小值链表的哨兵
    ListNode large = new ListNode(0);   // 大值链表的哨兵
    ListNode s = small, l = large;
    while (head != null) {
        if (head.val < x) {             // 小于 x 放到小链表
            s.next = head;
            s = s.next;
        } else {                        // 否则放大链表
            l.next = head;
            l = l.next;
        }
        head = head.next;
    }
    l.next = null;                      // 大链表末尾置空
    s.next = large.next;                // 小链表接大链表
    return small.next;
}
```

**举例**：

- 输入：`head = [1,4,3,2,5,2], x = 3`
- 过程：小于 3 的 `1,2,2` 进小链表，其余 `4,3,5` 进大链表，拼接。
- 输出：`[1,2,2,4,3,5]`

### 67. 146. LRU 缓存

- **难度**：中等
- **思路**：双向链表 + 哈希表。链表维护「最近使用」顺序（头部最新、尾部最旧），哈希表实现 O(1) 查找。

```java
class LRUCache {
    class Node {                         // 双向链表节点
        int key, val;
        Node prev, next;
        Node(int k, int v) { key = k; val = v; }
    }
    Map<Integer, Node> map = new HashMap<>();   // key -> 节点
    Node head = new Node(0, 0), tail = new Node(0, 0);  // 虚拟头尾
    int capacity;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        if (!map.containsKey(key)) return -1;
        Node node = map.get(key);
        remove(node);      // 先摘下来
        addFirst(node);    // 再插到最前（标记为最近使用）
        return node.val;
    }

    public void put(int key, int value) {
        if (map.containsKey(key)) remove(map.get(key));  // 已存在先移除
        Node node = new Node(key, value);
        addFirst(node);
        map.put(key, node);
        if (map.size() > capacity) {      // 超容量，删除最久未使用的（链表末尾）
            Node last = tail.prev;
            remove(last);
            map.remove(last.key);
        }
    }

    private void remove(Node node) {      // 从链表摘除节点
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    private void addFirst(Node node) {    // 插入到头部
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
    }
}
```

**举例**：

- 操作：容量 2，`put(1,1)`、`put(2,2)`、`get(1)`、`put(3,3)`
- 过程：`get(1)` 后 1 变为最近使用；`put(3,3)` 时淘汰最久未使用的 key=2。
- 结果：`get(2)` 返回 `-1`，`get(1)` 返回 `1`，`get(3)` 返回 `3`

---

## 九、二叉树

> 树节点用力扣内置的 `TreeNode`（`int val; TreeNode left, right;`），提交时无需自己定义。

### 68. 104. 二叉树的最大深度

- **难度**：简单
- **思路**：深度 = 左右子树深度的较大者 + 1，递归求解。

```java
public int maxDepth(TreeNode root) {
    if (root == null) return 0;                    // 空树深度 0
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));  // 左右较大者 + 1
}
```

**举例**：

- 输入：`[3,9,20,null,null,15,7]`
- 过程：左子树深 1，右子树深 2，取 2 加根得 3。
- 输出：`3`

### 69. 100. 相同的树

- **难度**：简单
- **思路**：同步递归比较两棵树的节点，值相同且左右子树都相同才相等。

```java
public boolean isSameTree(TreeNode p, TreeNode q) {
    if (p == null && q == null) return true;       // 都空，相同
    if (p == null || q == null) return false;      // 一空一非空，不同
    if (p.val != q.val) return false;              // 值不同
    return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);  // 递归比较左右
}
```

**举例**：

- 输入：`p = [1,2,3], q = [1,2,3]`
- 过程：根 1=1，左 2=2，右 3=3，全部一致。
- 输出：`true`

### 70. 226. 翻转二叉树

- **难度**：简单
- **思路**：交换每个节点的左右子树，再递归翻转。

```java
public TreeNode invertTree(TreeNode root) {
    if (root == null) return null;
    TreeNode t = root.left;          // 交换左右子树
    root.left = root.right;
    root.right = t;
    invertTree(root.left);           // 递归翻转
    invertTree(root.right);
    return root;
}
```

**举例**：

- 输入：`[4,2,7,1,3,6,9]`
- 输出：`[4,7,2,9,6,3,1]`

### 71. 101. 对称二叉树

- **难度**：简单
- **思路**：把树当成两棵镜像比较——左的左对右的右，左的右对右的左。

```java
public boolean isSymmetric(TreeNode root) {
    return check(root, root);
}

boolean check(TreeNode p, TreeNode q) {
    if (p == null && q == null) return true;
    if (p == null || q == null) return false;
    return p.val == q.val && check(p.left, q.right) && check(p.right, q.left);  // 镜像比较
}
```

**举例**：

- 输入：`[1,2,2,3,4,4,3]`
- 过程：根 1=1；左 2 的 left 3 对右 2 的 right 3；左 2 的 right 4 对右 2 的 left 4，都相等。
- 输出：`true`

### 72. 105. 从前序与中序遍历序列构造二叉树

- **难度**：中等
- **思路**：前序第一个是根，在中序里定位根，左边是左子树、右边是右子树，递归构造。

```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
    Map<Integer, Integer> map = new HashMap<>();   // 值 -> 中序下标
    for (int i = 0; i < inorder.length; i++) map.put(inorder[i], i);
    return build(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1, map);
}

TreeNode build(int[] pre, int ps, int pe, int[] in, int is, int ie, Map<Integer, Integer> map) {
    if (ps > pe) return null;
    TreeNode root = new TreeNode(pre[ps]);         // 前序第一个是根
    int idx = map.get(pre[ps]);                    // 根在中序的位置
    int leftSize = idx - is;                       // 左子树大小
    root.left = build(pre, ps + 1, ps + leftSize, in, is, idx - 1, map);
    root.right = build(pre, ps + leftSize + 1, pe, in, idx + 1, ie, map);
    return root;
}
```

**举例**：

- 输入：`preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]`
- 过程：根 3，中序里 9 在左、15/20/7 在右，递归构造。
- 输出：树 `[3,9,20,null,null,15,7]`

### 73. 106. 从中序与后序遍历序列构造二叉树

- **难度**：中等
- **思路**：后序最后一个是根，在中序里定位，左右递归（右子树范围要算准）。

```java
public TreeNode buildTree(int[] inorder, int[] postorder) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < inorder.length; i++) map.put(inorder[i], i);
    return build(inorder, 0, inorder.length - 1, postorder, 0, postorder.length - 1, map);
}

TreeNode build(int[] in, int is, int ie, int[] post, int ps, int pe, Map<Integer, Integer> map) {
    if (is > ie) return null;
    TreeNode root = new TreeNode(post[pe]);        // 后序最后一个是根
    int idx = map.get(post[pe]);
    int leftSize = idx - is;
    root.left = build(in, is, idx - 1, post, ps, ps + leftSize - 1, map);
    root.right = build(in, idx + 1, ie, post, ps + leftSize, pe - 1, map);
    return root;
}
```

**举例**：

- 输入：`inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]`
- 过程：根 3，中序里 9 在左、15/20/7 在右，递归构造。
- 输出：树 `[3,9,20,null,null,15,7]`

### 74. 117. 填充每个节点的下一个右侧节点指针 II

- **难度**：中等
- **思路**：用「下一层哨兵 + 尾指针」逐层串起 next，处理任意二叉树。

```java
public Node connect(Node root) {
    Node cur = root;
    while (cur != null) {                // 逐层处理
        Node dummy = new Node(0);        // 下一层的哨兵
        Node tail = dummy;               // 下一层尾指针
        for (Node p = cur; p != null; p = p.next) {   // 遍历当前层
            if (p.left != null)  { tail.next = p.left;  tail = tail.next; }
            if (p.right != null) { tail.next = p.right; tail = tail.next; }
        }
        cur = dummy.next;                // 进入下一层
    }
    return root;
}
```

**举例**：

- 输入：任意二叉树（节点带 `next` 指针）。
- 过程：逐层把每个节点的左、右孩子依次串起来。
- 输出：每一层的节点通过 `next` 连接（末尾指向 null）

### 75. 114. 二叉树展开为链表

- **难度**：中等
- **思路**：对每个有左子树的节点，把右子树接到左子树最右节点，再把左子树搬到右边。

```java
public void flatten(TreeNode root) {
    TreeNode cur = root;
    while (cur != null) {
        if (cur.left != null) {
            TreeNode pre = cur.left;
            while (pre.right != null) pre = pre.right;   // 找左子树最右节点
            pre.right = cur.right;                       // 右子树接到左子树最右
            cur.right = cur.left;                        // 左子树变右子树
            cur.left = null;                             // 左指针置空
        }
        cur = cur.right;
    }
}
```

**举例**：

- 输入：`[1,2,5,3,4,null,6]`
- 输出：`[1,null,2,null,3,null,4,null,5,null,6]`

### 76. 112. 路径总和

- **难度**：简单
- **思路**：自顶向下减去节点值，到叶子时判断剩余值是否刚好为 0。

```java
public boolean hasPathSum(TreeNode root, int targetSum) {
    if (root == null) return false;
    if (root.left == null && root.right == null) return root.val == targetSum;  // 叶子判断
    int remain = targetSum - root.val;                                          // 减去当前值
    return hasPathSum(root.left, remain) || hasPathSum(root.right, remain);
}
```

**举例**：

- 输入：`[5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22`
- 过程：路径 5→4→11→2 和为 22。
- 输出：`true`

### 77. 129. 求根节点到叶节点数字之和

- **难度**：中等
- **思路**：DFS 一路累加数字（`sum*10 + 当前值`），到叶子就把这个数加进总和。

```java
public int sumNumbers(TreeNode root) {
    return dfs(root, 0);
}

int dfs(TreeNode node, int sum) {
    if (node == null) return 0;
    sum = sum * 10 + node.val;                       // 累加当前位
    if (node.left == null && node.right == null) return sum;  // 到叶子，返回这个数
    return dfs(node.left, sum) + dfs(node.right, sum);        // 左右子树求和
}
```

**举例**：

- 输入：`[1,2,3]`
- 过程：两条路径 12 和 13，相加得 25。
- 输出：`25`

### 78. 124. 二叉树中的最大路径和

- **难度**：困难
- **思路**：DFS 返回「单边最大贡献」，同时用「左 + 右 + 当前节点」更新全局最大路径。

```java
int ans = Integer.MIN_VALUE;

public int maxPathSum(TreeNode root) {
    dfs(root);
    return ans;
}

int dfs(TreeNode node) {
    if (node == null) return 0;
    int left = Math.max(0, dfs(node.left));      // 左子树最大贡献（负数不要）
    int right = Math.max(0, dfs(node.right));    // 右子树最大贡献
    ans = Math.max(ans, left + right + node.val);  // 经过当前节点的最大路径
    return Math.max(left, right) + node.val;       // 返回单边最大贡献
}
```

**举例**：

- 输入：`[1,2,3]`
- 过程：左贡献 2、右贡献 3，`2+3+1=6`。
- 输出：`6`

### 79. 173. 二叉搜索树迭代器

- **难度**：中等
- **思路**：用栈实现中序遍历，`next()` 弹出栈顶并压入其右子树的最左路径。

```java
class BSTIterator {
    Deque<TreeNode> stack = new ArrayDeque<>();

    public BSTIterator(TreeNode root) {
        pushLeft(root);          // 把最左路径全部入栈
    }

    public int next() {
        TreeNode node = stack.pop();
        pushLeft(node.right);    // 处理右子树的最左路径
        return node.val;
    }

    public boolean hasNext() {
        return !stack.isEmpty();
    }

    void pushLeft(TreeNode node) {
        while (node != null) {
            stack.push(node);
            node = node.left;
        }
    }
}
```

**举例**：

- 输入：树 `[7,3,15,null,null,9,20]`
- 过程：`next()` 依次返回 3、7、9、15、20（中序顺序）。
- 输出：`3`（第一次调用）

### 80. 222. 完全二叉树的节点个数

- **难度**：中等
- **思路**：若左右高度相等则是满二叉树，直接用公式；否则递归统计。

```java
public int countNodes(TreeNode root) {
    if (root == null) return 0;
    int lh = 0, rh = 0;
    TreeNode l = root, r = root;
    while (l != null) { lh++; l = l.left; }    // 左高度
    while (r != null) { rh++; r = r.right; }   // 右高度
    if (lh == rh) return (1 << lh) - 1;        // 满二叉树，公式 2^h - 1
    return 1 + countNodes(root.left) + countNodes(root.right);  // 否则递归
}
```

**举例**：

- 输入：`[1,2,3,4,5,6]`
- 过程：右子树为满二叉树，公式算出节点数。
- 输出：`6`

### 81. 236. 二叉树的最近公共祖先

- **难度**：中等
- **思路**：自底向上，左右子树各找到一个目标则当前节点就是最近公共祖先。

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null || root == p || root == q) return root;  // 找到 p/q 或空
    TreeNode left = lowestCommonAncestor(root.left, p, q);
    TreeNode right = lowestCommonAncestor(root.right, p, q);
    if (left != null && right != null) return root;  // 左右各一个，当前就是 LCA
    return left != null ? left : right;              // 否则在找到的那一侧
}
```

**举例**：

- 输入：`[3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1`
- 过程：左子树找到 5，右子树找到 1，根 3 即为最近公共祖先。
- 输出：`3`

---

## 十、二叉树层次遍历

### 82. 199. 二叉树的右视图

- **难度**：中等
- **思路**：DFS 每层先访问右子树，「该层第一个访问到的节点」就是最右可见节点。

```java
public List<Integer> rightSideView(TreeNode root) {
    List<Integer> ans = new ArrayList<>();
    dfs(root, 0, ans);
    return ans;
}

void dfs(TreeNode node, int depth, List<Integer> ans) {
    if (node == null) return;
    if (depth == ans.size()) ans.add(node.val);   // 每层第一个访问（最右）
    dfs(node.right, depth + 1, ans);              // 先右后左
    dfs(node.left, depth + 1, ans);
}
```

**举例**：

- 输入：`[1,2,3,null,5,null,4]`
- 过程：每层最右分别是 1、3、4。
- 输出：`[1,3,4]`

### 83. 637. 二叉树的层平均值

- **难度**：简单
- **思路**：BFS 逐层处理，每层求和除以节点数。

```java
public List<Double> averageOfLevels(TreeNode root) {
    List<Double> ans = new ArrayList<>();
    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);
    while (!q.isEmpty()) {
        int size = q.size();                 // 当前层节点数
        double sum = 0;
        for (int i = 0; i < size; i++) {     // 处理当前层
            TreeNode node = q.poll();
            sum += node.val;
            if (node.left != null) q.offer(node.left);
            if (node.right != null) q.offer(node.right);
        }
        ans.add(sum / size);                 // 平均值
    }
    return ans;
}
```

**举例**：

- 输入：`[3,9,20,null,null,15,7]`
- 过程：三层和分别为 3、29、22，均值 3、14.5、11。
- 输出：`[3.0, 14.5, 11.0]`

### 84. 102. 二叉树的层序遍历

- **难度**：中等
- **思路**：BFS 用队列逐层处理，每次处理一层并收集该层所有值。

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> ans = new ArrayList<>();
    if (root == null) return ans;
    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);
    while (!q.isEmpty()) {
        int size = q.size();                // 当前层节点数
        List<Integer> level = new ArrayList<>();
        for (int i = 0; i < size; i++) {    // 处理当前层
            TreeNode node = q.poll();
            level.add(node.val);
            if (node.left != null) q.offer(node.left);
            if (node.right != null) q.offer(node.right);
        }
        ans.add(level);
    }
    return ans;
}
```

**举例**：

- 输入：`[3,9,20,null,null,15,7]`
- 输出：`[[3],[9,20],[15,7]]`

### 85. 103. 二叉树的锯齿形层序遍历

- **难度**：中等
- **思路**：层序遍历基础上，用双向链表根据方向决定「加尾部」还是「加头部」。

```java
public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
    List<List<Integer>> ans = new ArrayList<>();
    if (root == null) return ans;
    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);
    boolean leftToRight = true;              // 当前层方向
    while (!q.isEmpty()) {
        int size = q.size();
        LinkedList<Integer> level = new LinkedList<>();
        for (int i = 0; i < size; i++) {
            TreeNode node = q.poll();
            if (leftToRight) level.addLast(node.val);   // 正向加尾部
            else level.addFirst(node.val);              // 反向加头部
            if (node.left != null) q.offer(node.left);
            if (node.right != null) q.offer(node.right);
        }
        ans.add(level);
        leftToRight = !leftToRight;          // 切换方向
    }
    return ans;
}
```

**举例**：

- 输入：`[3,9,20,null,null,15,7]`
- 过程：第 1 层 `[3]`，第 2 层反向 `[20,9]`，第 3 层正向 `[15,7]`。
- 输出：`[[3],[20,9],[15,7]]`

---

## 十一、二叉搜索树

> 二叉搜索树（BST）中序遍历得到的是递增序列，多数题目围绕这个性质展开。

### 86. 530. 二叉搜索树的最小绝对差

- **难度**：简单
- **思路**：中序遍历得到递增序列，相邻两数之差的最小值就是答案。

```java
int prev = -1, minDiff = Integer.MAX_VALUE;

public int getMinimumDifference(TreeNode root) {
    dfs(root);
    return minDiff;
}

void dfs(TreeNode node) {
    if (node == null) return;
    dfs(node.left);                          // 中序遍历（有序）
    if (prev != -1) minDiff = Math.min(minDiff, node.val - prev);  // 相邻差值
    prev = node.val;
    dfs(node.right);
}
```

**举例**：

- 输入：`[4,2,6,1,3]`
- 过程：中序 `1,2,3,4,6`，最小相邻差 1。
- 输出：`1`

### 87. 230. 二叉搜索树中第K小的元素

- **难度**：中等
- **思路**：中序遍历到第 k 个节点就是答案。

```java
int count = 0, ans = -1;

public int kthSmallest(TreeNode root, int k) {
    dfs(root, k);
    return ans;
}

void dfs(TreeNode node, int k) {
    if (node == null) return;
    dfs(node.left, k);                       // 中序遍历
    if (++count == k) { ans = node.val; return; }  // 第 k 个
    dfs(node.right, k);
}
```

**举例**：

- 输入：`[3,1,4,null,2], k = 1`
- 过程：中序 `1,2,3,4`，第 1 个是 1。
- 输出：`1`

### 88. 98. 验证二叉搜索树

- **难度**：中等
- **思路**：每个节点必须在区间 `(min, max)` 内，递归时不断收紧左右边界。

```java
public boolean isValidBST(TreeNode root) {
    return check(root, Long.MIN_VALUE, Long.MAX_VALUE);
}

boolean check(TreeNode node, long min, long max) {
    if (node == null) return true;
    if (node.val <= min || node.val >= max) return false;  // 超出范围
    return check(node.left, min, node.val) && check(node.right, node.val, max);
}
```

**举例**：

- 输入：`[2,1,3]`
- 过程：根 2 在 (-∞,∞)；左 1 在 (-∞,2)；右 3 在 (2,∞)，都合法。
- 输出：`true`

---

## 十二、图

### 89. 200. 岛屿数量

- **难度**：中等
- **思路**：遍历每个格子，发现陆地就计数 +1，并用 DFS 把整块岛「淹没」避免重复计数。

```java
public int numIslands(char[][] grid) {
    int count = 0;
    for (int i = 0; i < grid.length; i++) {
        for (int j = 0; j < grid[0].length; j++) {
            if (grid[i][j] == '1') {   // 发现一块新陆地
                count++;
                dfs(grid, i, j);       // 把整块岛淹没
            }
        }
    }
    return count;
}

void dfs(char[][] grid, int i, int j) {
    if (i < 0 || j < 0 || i >= grid.length || j >= grid[0].length || grid[i][j] != '1') return;
    grid[i][j] = '0';                  // 标记已访问
    dfs(grid, i + 1, j);               // 上下左右扩散
    dfs(grid, i - 1, j);
    dfs(grid, i, j + 1);
    dfs(grid, i, j - 1);
}
```

**举例**：

- 输入：`[["1","1","0","0","0"],["1","1","0","0","0"],["0","0","1","0","0"],["0","0","0","1","1"]]`
- 过程：左上角一块、中间一块、右下角一块。
- 输出：`3`

### 90. 130. 被围绕的区域

- **难度**：中等
- **思路**：从边界 `O` 出发 DFS 标记为「不可翻转」，剩下的 `O` 都被围绕，翻成 `X`。

```java
public void solve(char[][] board) {
    int m = board.length, n = board[0].length;
    for (int i = 0; i < m; i++) {       // 左右边界
        dfs(board, i, 0);
        dfs(board, i, n - 1);
    }
    for (int j = 0; j < n; j++) {       // 上下边界
        dfs(board, 0, j);
        dfs(board, m - 1, j);
    }
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (board[i][j] == 'O') board[i][j] = 'X';       // 被围绕，翻转
            else if (board[i][j] == '#') board[i][j] = 'O';  // 边界连通，还原
        }
    }
}

void dfs(char[][] board, int i, int j) {
    if (i < 0 || j < 0 || i >= board.length || j >= board[0].length || board[i][j] != 'O') return;
    board[i][j] = '#';                  // 标记为边界连通
    dfs(board, i + 1, j);
    dfs(board, i - 1, j);
    dfs(board, i, j + 1);
    dfs(board, i, j - 1);
}
```

**举例**：

- 输入：`[["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]`
- 过程：中间三个 O 被 X 围住翻转，最下面那个 O 连通边界保留。
- 输出：`[["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]`

### 91. 133. 克隆图

- **难度**：中等
- **思路**：DFS + 哈希表记录「原节点 → 新节点」，递归复制每个节点和它的邻居。

```java
Map<Node, Node> map = new HashMap<>();

public Node cloneGraph(Node node) {
    if (node == null) return null;
    if (map.containsKey(node)) return map.get(node);   // 已克隆过，直接返回
    Node copy = new Node(node.val);
    map.put(node, copy);
    for (Node nei : node.neighbors) {                  // 递归克隆邻居
        copy.neighbors.add(cloneGraph(nei));
    }
    return copy;
}
```

**举例**：

- 输入：一个无向连通图。
- 过程：每个节点只克隆一次，哈希表避免重复克隆形成死循环。
- 输出：与原图结构相同、节点独立的新图

### 92. 399. 除法求值

- **难度**：中等
- **思路**：带权并查集。用父节点和「到父节点的权重」表示变量间的倍数关系。

```java
Map<String, String> parent = new HashMap<>();   // 父节点
Map<String, Double> weight = new HashMap<>();   // 到父节点的权重（比值）

public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
    for (int i = 0; i < equations.size(); i++) {
        union(equations.get(i).get(0), equations.get(i).get(1), values[i]);  // a / b = values[i]
    }
    double[] ans = new double[queries.size()];
    for (int i = 0; i < queries.size(); i++) {
        String a = queries.get(i).get(0), b = queries.get(i).get(1);
        if (!parent.containsKey(a) || !parent.containsKey(b) || !find(a).equals(find(b))) {
            ans[i] = -1.0;                        // 不在同一集合，无法计算
        } else {
            ans[i] = weight.get(a) / weight.get(b);
        }
    }
    return ans;
}

String find(String x) {                            // 带路径压缩的查找
    if (!parent.containsKey(x)) { parent.put(x, x); weight.put(x, 1.0); }
    if (!parent.get(x).equals(x)) {
        String p = parent.get(x);
        String root = find(p);
        weight.put(x, weight.get(x) * weight.get(p));   // 更新到根节点的权重
        parent.put(x, root);
    }
    return parent.get(x);
}

void union(String a, String b, double v) {         // 合并：a / b = v
    String ra = find(a), rb = find(b);
    if (ra.equals(rb)) return;
    parent.put(ra, rb);
    weight.put(ra, v * weight.get(b) / weight.get(a));
}
```

**举例**：

- 输入：`equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"]]`
- 过程：`a=2b`、`b=3c`，故 `a=6c`、`b/a=0.5`、e 未知。
- 输出：`[6.0, 0.5, -1.0]`

### 93. 207. 课程表

- **难度**：中等
- **思路**：拓扑排序。入度为 0 的课先学，不断删去已学课程，最后能学完全部就说明无环。

```java
public boolean canFinish(int numCourses, int[][] prerequisites) {
    int[] indegree = new int[numCourses];               // 每门课的入度
    List<List<Integer>> adj = new ArrayList<>();        // 邻接表
    for (int i = 0; i < numCourses; i++) adj.add(new ArrayList<>());
    for (int[] p : prerequisites) {
        adj.get(p[1]).add(p[0]);                        // 先学 p[1] 再学 p[0]
        indegree[p[0]]++;
    }
    Queue<Integer> q = new LinkedList<>();
    for (int i = 0; i < numCourses; i++) {
        if (indegree[i] == 0) q.offer(i);               // 入度为 0 的先学
    }
    int count = 0;
    while (!q.isEmpty()) {
        int cur = q.poll();
        count++;
        for (int next : adj.get(cur)) {
            if (--indegree[next] == 0) q.offer(next);   // 依赖解除，可学
        }
    }
    return count == numCourses;                         // 全学完说明无环
}
```

**举例**：

- 输入：`numCourses = 2, prerequisites = [[1,0]]`
- 过程：先学 0，再学 1，全学完。
- 输出：`true`

### 94. 210. 课程表 II

- **难度**：中等
- **思路**：和课程表一样做拓扑排序，顺便记录学习顺序；有环则返回空数组。

```java
public int[] findOrder(int numCourses, int[][] prerequisites) {
    int[] indegree = new int[numCourses];
    List<List<Integer>> adj = new ArrayList<>();
    for (int i = 0; i < numCourses; i++) adj.add(new ArrayList<>());
    for (int[] p : prerequisites) {
        adj.get(p[1]).add(p[0]);
        indegree[p[0]]++;
    }
    Queue<Integer> q = new LinkedList<>();
    for (int i = 0; i < numCourses; i++) {
        if (indegree[i] == 0) q.offer(i);
    }
    int[] ans = new int[numCourses];
    int idx = 0;
    while (!q.isEmpty()) {
        int cur = q.poll();
        ans[idx++] = cur;
        for (int next : adj.get(cur)) {
            if (--indegree[next] == 0) q.offer(next);
        }
    }
    return idx == numCourses ? ans : new int[0];        // 有环返回空数组
}
```

**举例**：

- 输入：`numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]`
- 过程：拓扑排序得到一种可行顺序。
- 输出：`[0,1,2,3]`（或 `[0,2,1,3]`，只要合法即可）

---

## 十三、图的广度优先搜索

### 95. 909. 蛇梯棋

- **难度**：中等
- **思路**：先把棋盘展平成一维数组，再用 BFS 模拟掷骰子，遇到蛇或梯就跳转。

```java
public int snakesAndLadders(int[][] board) {
    int n = board.length;
    int[] arr = new int[n * n + 1];     // 展平成 1D
    int k = 1;
    boolean leftToRight = true;
    for (int i = n - 1; i >= 0; i--) {  // 从最后一行开始（蛇梯棋盘方向交替）
        if (leftToRight) {
            for (int j = 0; j < n; j++) arr[k++] = board[i][j];
        } else {
            for (int j = n - 1; j >= 0; j--) arr[k++] = board[i][j];
        }
        leftToRight = !leftToRight;
    }
    Queue<Integer> q = new LinkedList<>();
    boolean[] visited = new boolean[n * n + 1];
    q.offer(1);
    visited[1] = true;
    int step = 0;
    while (!q.isEmpty()) {
        int size = q.size();
        for (int i = 0; i < size; i++) {
            int cur = q.poll();
            if (cur == n * n) return step;
            for (int d = 1; d <= 6; d++) {          // 掷骰子 1~6
                int next = cur + d;
                if (next > n * n) break;
                if (arr[next] != -1) next = arr[next];  // 有蛇或梯，跳转
                if (!visited[next]) { visited[next] = true; q.offer(next); }
            }
        }
        step++;
    }
    return -1;
}
```

**举例**：

- 输入：`board = [[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,35,-1,-1,13,-1],[-1,-1,-1,-1,-1,-1],[-1,15,-1,-1,-1,-1]]`
- 过程：走 4 步可到终点。
- 输出：`4`

### 96. 433. 最小基因变化

- **难度**：中等
- **思路**：把基因串当作图节点，BFS 逐位替换，找到最短变换到目标基因的步数。

```java
public int minMutation(String startGene, String endGene, String[] bank) {
    Set<String> bankSet = new HashSet<>(Arrays.asList(bank));
    if (!bankSet.contains(endGene)) return -1;
    char[] genes = {'A', 'C', 'G', 'T'};
    Queue<String> q = new LinkedList<>();
    Set<String> visited = new HashSet<>();
    q.offer(startGene);
    visited.add(startGene);
    int step = 0;
    while (!q.isEmpty()) {
        int size = q.size();
        for (int i = 0; i < size; i++) {
            String cur = q.poll();
            if (cur.equals(endGene)) return step;
            char[] arr = cur.toCharArray();
            for (int j = 0; j < arr.length; j++) {
                char old = arr[j];
                for (char g : genes) {              // 尝试换成其他字符
                    arr[j] = g;
                    String next = new String(arr);
                    if (bankSet.contains(next) && !visited.contains(next)) {
                        visited.add(next);
                        q.offer(next);
                    }
                }
                arr[j] = old;                        // 还原
            }
        }
        step++;
    }
    return -1;
}
```

**举例**：

- 输入：`start = "AACCGGTT", end = "AACCGGTA", bank = ["AACCGGTA"]`
- 过程：一步变到最后一位。
- 输出：`1`

### 97. 127. 单词接龙

- **难度**：困难
- **思路**：BFS。逐位替换字母，能命中词典就入队，直到找到目标单词。

```java
public int ladderLength(String beginWord, String endWord, List<String> wordList) {
    Set<String> wordSet = new HashSet<>(wordList);
    if (!wordSet.contains(endWord)) return 0;
    Queue<String> q = new LinkedList<>();
    q.offer(beginWord);
    int step = 1;
    while (!q.isEmpty()) {
        int size = q.size();
        for (int i = 0; i < size; i++) {
            String cur = q.poll();
            char[] arr = cur.toCharArray();
            for (int j = 0; j < arr.length; j++) {
                char old = arr[j];
                for (char c = 'a'; c <= 'z'; c++) {  // 逐位替换
                    arr[j] = c;
                    String next = new String(arr);
                    if (next.equals(endWord)) return step + 1;
                    if (wordSet.contains(next)) {
                        wordSet.remove(next);        // 标记访问
                        q.offer(next);
                    }
                }
                arr[j] = old;
            }
        }
        step++;
    }
    return 0;
}
```

**举例**：

- 输入：`beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]`
- 过程：`hit → hot → dot → dog → cog`，共 5 步。
- 输出：`5`

---

## 十四、字典树

### 98. 208. 实现 Trie (前缀树)

- **难度**：中等
- **思路**：每个节点存 26 个子节点和一个「是否单词结尾」标记，顺着字符逐层插入、查找。

```java
class Trie {
    class Node {
        Node[] children = new Node[26];   // 26 个子节点
        boolean isEnd = false;            // 是否是单词结尾
    }
    Node root = new Node();

    public void insert(String word) {
        Node cur = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (cur.children[idx] == null) cur.children[idx] = new Node();
            cur = cur.children[idx];
        }
        cur.isEnd = true;                  // 标记单词结尾
    }

    public boolean search(String word) {
        Node node = find(word);
        return node != null && node.isEnd;
    }

    public boolean startsWith(String prefix) {
        return find(prefix) != null;
    }

    Node find(String s) {                  // 顺着字符走，找不到返回 null
        Node cur = root;
        for (char c : s.toCharArray()) {
            cur = cur.children[c - 'a'];
            if (cur == null) return null;
        }
        return cur;
    }
}
```

**举例**：

- 操作：`insert("apple")`、`search("apple")`、`search("app")`、`startsWith("app")`
- 结果：`true`、`false`、`true`

### 99. 211. 添加与搜索单词 - 数据结构设计

- **难度**：中等
- **思路**：字典树 + DFS。遇到 `.` 就尝试所有子节点。

```java
class WordDictionary {
    class Node {
        Node[] children = new Node[26];
        boolean isEnd = false;
    }
    Node root = new Node();

    public void addWord(String word) {
        Node cur = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (cur.children[idx] == null) cur.children[idx] = new Node();
            cur = cur.children[idx];
        }
        cur.isEnd = true;
    }

    public boolean search(String word) {
        return dfs(word, 0, root);
    }

    boolean dfs(String word, int i, Node node) {
        if (node == null) return false;
        if (i == word.length()) return node.isEnd;
        char c = word.charAt(i);
        if (c == '.') {                       // 通配符，匹配任意子节点
            for (Node child : node.children) {
                if (dfs(word, i + 1, child)) return true;
            }
            return false;
        }
        return dfs(word, i + 1, node.children[c - 'a']);
    }
}
```

**举例**：

- 操作：`addWord("bad")`、`search("b.d")`
- 过程：`.` 可匹配任意字母，`b.d` 匹配 `bad`。
- 结果：`true`

### 100. 212. 单词搜索 II

- **难度**：困难
- **思路**：把所有单词建成字典树，在棋盘上 DFS，用字典树快速剪枝。

```java
class Solution {
    class TrieNode {
        TrieNode[] children = new TrieNode[26];
        String word = null;   // 到此处若是一个完整单词，就存下它
    }

    public List<String> findWords(char[][] board, String[] words) {
        TrieNode root = new TrieNode();
        for (String w : words) {           // 建字典树
            TrieNode cur = root;
            for (char c : w.toCharArray()) {
                if (cur.children[c - 'a'] == null) cur.children[c - 'a'] = new TrieNode();
                cur = cur.children[c - 'a'];
            }
            cur.word = w;
        }
        List<String> ans = new ArrayList<>();
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                dfs(board, i, j, root, ans);
            }
        }
        return ans;
    }

    void dfs(char[][] board, int i, int j, TrieNode node, List<String> ans) {
        if (i < 0 || j < 0 || i >= board.length || j >= board[0].length) return;
        char c = board[i][j];
        if (c == '#' || node.children[c - 'a'] == null) return;
        node = node.children[c - 'a'];
        if (node.word != null) {           // 找到一个单词
            ans.add(node.word);
            node.word = null;              // 去重
        }
        board[i][j] = '#';                 // 标记访问
        dfs(board, i + 1, j, node, ans);
        dfs(board, i - 1, j, node, ans);
        dfs(board, i, j + 1, node, ans);
        dfs(board, i, j - 1, node, ans);
        board[i][j] = c;                   // 还原
    }
}
```

**举例**：

- 输入：`board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]`
- 输出：`["eat","oath"]`

---

## 十五、回溯

> 回溯题的通用套路：`选择 → 递归 → 撤销`。所有回溯题几乎都是这个骨架，只是「选择和剪枝条件」不同。

### 101. 17. 电话号码的字母组合

- **难度**：中等
- **思路**：每个数字对应几个字母，逐位选一个字母，递归下去，到末尾记录结果。

```java
String[] map = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
List<String> ans = new ArrayList<>();

public List<String> letterCombinations(String digits) {
    if (digits.isEmpty()) return ans;
    dfs(digits, 0, new StringBuilder());
    return ans;
}

void dfs(String digits, int i, StringBuilder sb) {
    if (i == digits.length()) {           // 处理完所有数字
        ans.add(sb.toString());
        return;
    }
    for (char c : map[digits.charAt(i) - '0'].toCharArray()) {  // 当前数字的每个字母
        sb.append(c);
        dfs(digits, i + 1, sb);           // 递归下一个数字
        sb.deleteCharAt(sb.length() - 1); // 回溯
    }
}
```

**举例**：

- 输入：`"23"`
- 过程：2 对应 abc，3 对应 def，两两组合。
- 输出：`["ad","ae","af","bd","be","bf","cd","ce","cf"]`

### 102. 77. 组合

- **难度**：中等
- **思路**：从 `start` 开始依次选数（保证递增避免重复），选满 k 个记录。

```java
List<List<Integer>> ans = new ArrayList<>();

public List<List<Integer>> combine(int n, int k) {
    dfs(n, k, 1, new ArrayList<>());
    return ans;
}

void dfs(int n, int k, int start, List<Integer> path) {
    if (path.size() == k) {              // 选满 k 个
        ans.add(new ArrayList<>(path));
        return;
    }
    for (int i = start; i <= n; i++) {   // 从 start 开始，避免重复
        path.add(i);
        dfs(n, k, i + 1, path);
        path.remove(path.size() - 1);    // 回溯
    }
}
```

**举例**：

- 输入：`n = 4, k = 2`
- 输出：`[[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]`

### 103. 46. 全排列

- **难度**：中等
- **思路**：用 `used` 数组标记哪些数用过，逐个填入，填满记录。

```java
List<List<Integer>> ans = new ArrayList<>();

public List<List<Integer>> permute(int[] nums) {
    dfs(nums, new boolean[nums.length], new ArrayList<>());
    return ans;
}

void dfs(int[] nums, boolean[] used, List<Integer> path) {
    if (path.size() == nums.length) {
        ans.add(new ArrayList<>(path));
        return;
    }
    for (int i = 0; i < nums.length; i++) {
        if (used[i]) continue;           // 已用过，跳过
        used[i] = true;
        path.add(nums[i]);
        dfs(nums, used, path);
        path.remove(path.size() - 1);    // 回溯
        used[i] = false;
    }
}
```

**举例**：

- 输入：`[1,2,3]`
- 输出：6 种排列 `[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]`

### 104. 39. 组合总和

- **难度**：中等
- **思路**：可重复选同一个数，所以递归时 `start` 不 +1；`target` 减到 0 记录，负数剪枝。

```java
List<List<Integer>> ans = new ArrayList<>();

public List<List<Integer>> combinationSum(int[] candidates, int target) {
    dfs(candidates, target, 0, new ArrayList<>());
    return ans;
}

void dfs(int[] candidates, int target, int start, List<Integer> path) {
    if (target == 0) { ans.add(new ArrayList<>(path)); return; }
    if (target < 0) return;
    for (int i = start; i < candidates.length; i++) {
        path.add(candidates[i]);
        dfs(candidates, target - candidates[i], i, path);  // 注意传 i，可重复使用
        path.remove(path.size() - 1);
    }
}
```

**举例**：

- 输入：`candidates = [2,3,6,7], target = 7`
- 输出：`[[2,2,3],[7]]`

### 105. 52. N 皇后 II

- **难度**：困难
- **思路**：逐行放皇后，用三个数组标记列、两条对角线是否被占，冲突就跳过。

```java
int count = 0;

public int totalNQueens(int n) {
    dfs(n, 0, new boolean[n], new boolean[2 * n], new boolean[2 * n]);
    return count;
}

void dfs(int n, int row, boolean[] cols, boolean[] diag1, boolean[] diag2) {
    if (row == n) { count++; return; }
    for (int col = 0; col < n; col++) {
        int d1 = row + col, d2 = row - col + n;   // 两条对角线的编号
        if (cols[col] || diag1[d1] || diag2[d2]) continue;  // 冲突跳过
        cols[col] = diag1[d1] = diag2[d2] = true;
        dfs(n, row + 1, cols, diag1, diag2);
        cols[col] = diag1[d1] = diag2[d2] = false;  // 回溯
    }
}
```

**举例**：

- 输入：`n = 4`
- 输出：`2`（4 皇后有 2 种摆法）

### 106. 22. 括号生成

- **难度**：中等
- **思路**：还剩几个左括号就放几个，右括号必须少于左括号才能放，保证括号合法。

```java
List<String> ans = new ArrayList<>();

public List<String> generateParenthesis(int n) {
    dfs(n, n, new StringBuilder());
    return ans;
}

void dfs(int left, int right, StringBuilder sb) {
    if (left == 0 && right == 0) { ans.add(sb.toString()); return; }
    if (left > 0) {                        // 还能放左括号
        sb.append('(');
        dfs(left - 1, right, sb);
        sb.deleteCharAt(sb.length() - 1);
    }
    if (right > left) {                    // 右括号数必须小于左括号
        sb.append(')');
        dfs(left, right - 1, sb);
        sb.deleteCharAt(sb.length() - 1);
    }
}
```

**举例**：

- 输入：`n = 3`
- 输出：`["((()))","(()())","(())()","()(())","()()()"]`

### 107. 79. 单词搜索

- **难度**：中等
- **思路**：从每个格子出发 DFS，逐字符匹配，走过的格子标记避免回头。

```java
public boolean exist(char[][] board, String word) {
    for (int i = 0; i < board.length; i++) {
        for (int j = 0; j < board[0].length; j++) {
            if (dfs(board, word, i, j, 0)) return true;
        }
    }
    return false;
}

boolean dfs(char[][] board, String word, int i, int j, int k) {
    if (k == word.length()) return true;   // 匹配完
    if (i < 0 || j < 0 || i >= board.length || j >= board[0].length || board[i][j] != word.charAt(k)) return false;
    char t = board[i][j];
    board[i][j] = '#';                     // 标记访问
    boolean found = dfs(board, word, i + 1, j, k + 1) || dfs(board, word, i - 1, j, k + 1)
                 || dfs(board, word, i, j + 1, k + 1) || dfs(board, word, i, j - 1, k + 1);
    board[i][j] = t;                       // 还原
    return found;
}
```

**举例**：

- 输入：`board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"`
- 输出：`true`

---

## 十六、分治

### 108. 108. 将有序数组转换为二叉搜索树

- **难度**：简单
- **思路**：取中间元素作根，左右两半递归建树，天然平衡。

```java
public TreeNode sortedArrayToBST(int[] nums) {
    return build(nums, 0, nums.length - 1);
}

TreeNode build(int[] nums, int l, int r) {
    if (l > r) return null;
    int mid = l + (r - l) / 2;             // 中间作为根，保证平衡
    TreeNode root = new TreeNode(nums[mid]);
    root.left = build(nums, l, mid - 1);
    root.right = build(nums, mid + 1, r);
    return root;
}
```

**举例**：

- 输入：`[-10,-3,0,5,9]`
- 输出：`[0,-3,9,-10,null,5]`

### 109. 148. 排序链表

- **难度**：中等
- **思路**：归并排序。快慢指针找中点断开，递归排序两半，再合并两个有序链表。

```java
public ListNode sortList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode slow = head, fast = head, prev = null;
    while (fast != null && fast.next != null) {   // 快慢指针找中点
        prev = slow;
        slow = slow.next;
        fast = fast.next.next;
    }
    prev.next = null;                    // 断开成两半
    ListNode l = sortList(head);         // 递归排序左半
    ListNode r = sortList(slow);         // 递归排序右半
    return merge(l, r);                  // 归并
}

ListNode merge(ListNode l, ListNode r) {
    ListNode dummy = new ListNode(0), cur = dummy;
    while (l != null && r != null) {
        if (l.val < r.val) { cur.next = l; l = l.next; }
        else { cur.next = r; r = r.next; }
        cur = cur.next;
    }
    cur.next = (l != null) ? l : r;
    return dummy.next;
}
```

**举例**：

- 输入：`[4,2,1,3]`
- 输出：`[1,2,3,4]`

### 110. 427. 建立四叉树

- **难度**：中等
- **思路**：递归把区域分成四块，若四块都是叶子且值相同，则合并成一个叶子。

```java
public Node construct(int[][] grid) {
    return build(grid, 0, 0, grid.length);
}

Node build(int[][] grid, int x, int y, int len) {
    if (len == 1) return new Node(grid[x][y] == 1, true);   // 单格就是叶子
    int half = len / 2;
    Node topLeft = build(grid, x, y, half);
    Node topRight = build(grid, x, y + half, half);
    Node bottomLeft = build(grid, x + half, y, half);
    Node bottomRight = build(grid, x + half, y + half, half);
    // 四个都是叶子且值相同，合并为一个叶子
    if (topLeft.isLeaf && topRight.isLeaf && bottomLeft.isLeaf && bottomRight.isLeaf
        && topLeft.val == topRight.val && topLeft.val == bottomLeft.val && topLeft.val == bottomRight.val) {
        return new Node(topLeft.val, true);
    }
    return new Node(true, false, topLeft, topRight, bottomLeft, bottomRight);
}
```

**举例**：

- 输入：`grid = [[1,1,0,0],[1,1,0,0],[1,1,0,0],[1,1,0,0]]`
- 过程：四个 2×2 子块里，左上全 1、其余全 0，各自成叶子，根为带四个叶子的节点。
- 输出：四叉树（根非叶子，四个孩子分别为 1、0、0、0 的叶子）

### 111. 23. 合并 K 个升序链表

- **难度**：困难
- **思路**：用最小堆维护每个链表的当前头，每次弹出最小的接上，再把它后继压回堆。

```java
public ListNode mergeKLists(ListNode[] lists) {
    PriorityQueue<ListNode> pq = new PriorityQueue<>((a, b) -> a.val - b.val);  // 小顶堆
    for (ListNode head : lists) {
        if (head != null) pq.offer(head);
    }
    ListNode dummy = new ListNode(0), cur = dummy;
    while (!pq.isEmpty()) {
        ListNode node = pq.poll();       // 弹出最小节点
        cur.next = node;
        cur = cur.next;
        if (node.next != null) pq.offer(node.next);  // 该链下一个入堆
    }
    return dummy.next;
}
```

**举例**：

- 输入：`[[1,4,5],[1,3,4],[2,6]]`
- 输出：`[1,1,2,3,4,4,5,6]`

---

## 十七、Kadane 算法

### 112. 53. 最大子数组和

- **难度**：中等
- **思路**：每个位置要么「从当前重新开始」，要么「接上之前的和」，取较大者，全程维护最大值。

```java
public int maxSubArray(int[] nums) {
    int cur = 0, ans = nums[0];
    for (int x : nums) {
        cur = Math.max(x, cur + x);   // 要么重新开始，要么延续
        ans = Math.max(ans, cur);
    }
    return ans;
}
```

**举例**：

- 输入：`[-2,1,-3,4,-1,2,1,-5,4]`
- 过程：子数组 `[4,-1,2,1]` 和为 6，最大。
- 输出：`6`

### 113. 918. 环形子数组的最大和

- **难度**：中等
- **思路**：环形最大有两种情况——普通最大子数组，或「总和 − 最小子数组」（跨环）。取两者较大。

```java
public int maxSubarraySumCircular(int[] nums) {
    int total = 0, maxSum = nums[0], minSum = nums[0], curMax = 0, curMin = 0;
    for (int x : nums) {
        total += x;
        curMax = Math.max(x, curMax + x);   // 最大子数组
        maxSum = Math.max(maxSum, curMax);
        curMin = Math.min(x, curMin + x);   // 最小子数组
        minSum = Math.min(minSum, curMin);
    }
    // 跨环 = 总和 - 最小子数组；全负数时直接取 maxSum
    return maxSum > 0 ? Math.max(maxSum, total - minSum) : maxSum;
}
```

**举例**：

- 输入：`[5,-3,5]`
- 过程：普通最大 5，跨环 = 10 − (−3) = 10，取 10。
- 输出：`10`

---

## 十八、二分查找

### 114. 35. 搜索插入位置

- **难度**：简单
- **思路**：标准二分查找，找不到时 `l` 正好停在应插入的位置。

```java
public int searchInsert(int[] nums, int target) {
    int l = 0, r = nums.length - 1;
    while (l <= r) {
        int mid = l + (r - l) / 2;
        if (nums[mid] == target) return mid;
        if (nums[mid] < target) l = mid + 1;
        else r = mid - 1;
    }
    return l;    // 没找到，返回应插入位置
}
```

**举例**：

- 输入：`nums = [1,3,5,6], target = 2`
- 过程：二分后 2 应插在下标 1。
- 输出：`1`

### 115. 74. 搜索二维矩阵

- **难度**：中等
- **思路**：矩阵逐行递增且行首大于前行尾，可当成一个有序一维数组二分。

```java
public boolean searchMatrix(int[][] matrix, int target) {
    int m = matrix.length, n = matrix[0].length;
    int l = 0, r = m * n - 1;          // 把矩阵当成一维有序数组
    while (l <= r) {
        int mid = l + (r - l) / 2;
        int val = matrix[mid / n][mid % n];   // 一维下标转二维
        if (val == target) return true;
        if (val < target) l = mid + 1;
        else r = mid - 1;
    }
    return false;
}
```

**举例**：

- 输入：`matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3`
- 输出：`true`

### 116. 162. 寻找峰值

- **难度**：中等
- **思路**：比较 `mid` 和 `mid+1`，上升段峰值在右，下降段峰值在左，不断缩小区间。

```java
public int findPeakElement(int[] nums) {
    int l = 0, r = nums.length - 1;
    while (l < r) {
        int mid = l + (r - l) / 2;
        if (nums[mid] < nums[mid + 1]) l = mid + 1;   // 上升段，峰值在右
        else r = mid;                                 // 下降段，峰值在左（含 mid）
    }
    return l;
}
```

**举例**：

- 输入：`[1,2,3,1]`
- 过程：`nums[1]=2 < nums[2]=3` 往右，`nums[2]=3` 为峰值。
- 输出：`2`

### 117. 33. 搜索旋转排序数组

- **难度**：中等
- **思路**：先判断哪半是有序的，再判断 target 是否落在有序半，据此收缩区间。

```java
public int search(int[] nums, int target) {
    int l = 0, r = nums.length - 1;
    while (l <= r) {
        int mid = l + (r - l) / 2;
        if (nums[mid] == target) return mid;
        if (nums[l] <= nums[mid]) {              // 左半有序
            if (nums[l] <= target && target < nums[mid]) r = mid - 1;   // target 在左半
            else l = mid + 1;
        } else {                                 // 右半有序
            if (nums[mid] < target && target <= nums[r]) l = mid + 1;   // target 在右半
            else r = mid - 1;
        }
    }
    return -1;
}
```

**举例**：

- 输入：`nums = [4,5,6,7,0,1,2], target = 0`
- 输出：`4`

### 118. 34. 在排序数组中查找元素的第一个和最后一个位置

- **难度**：中等
- **思路**：两次二分，一次往左找第一个，一次往右找最后一个。

```java
public int[] searchRange(int[] nums, int target) {
    int first = find(nums, target, true);   // 找第一个
    int last = find(nums, target, false);   // 找最后一个
    return new int[]{first, last};
}

int find(int[] nums, int target, boolean first) {
    int l = 0, r = nums.length - 1, ans = -1;
    while (l <= r) {
        int mid = l + (r - l) / 2;
        if (nums[mid] == target) {
            ans = mid;
            if (first) r = mid - 1;   // 找第一个，往左缩
            else l = mid + 1;         // 找最后一个，往右扩
        } else if (nums[mid] < target) l = mid + 1;
        else r = mid - 1;
    }
    return ans;
}
```

**举例**：

- 输入：`nums = [5,7,7,8,8,10], target = 8`
- 输出：`[3,4]`

### 119. 153. 寻找旋转排序数组中的最小值

- **难度**：中等
- **思路**：比较 `mid` 和 `r`，`mid` 比 `r` 大说明最小值在右半，否则在左半（含 mid）。

```java
public int findMin(int[] nums) {
    int l = 0, r = nums.length - 1;
    while (l < r) {
        int mid = l + (r - l) / 2;
        if (nums[mid] > nums[r]) l = mid + 1;   // 最小值在右半
        else r = mid;                            // 最小值在左半（含 mid）
    }
    return nums[l];
}
```

**举例**：

- 输入：`[3,4,5,1,2]`
- 输出：`1`

### 120. 4. 寻找两个正序数组的中位数

- **难度**：困难
- **思路**：在较短数组上二分找「分割点」，使左半全部小于右半，从而定位中位数。

```java
public double findMedianSortedArrays(int[] nums1, int[] nums2) {
    if (nums1.length > nums2.length) return findMedianSortedArrays(nums2, nums1);  // 保证 nums1 较短
    int m = nums1.length, n = nums2.length;
    int l = 0, r = m;
    while (l <= r) {
        int i = l + (r - l) / 2;      // nums1 的分割点
        int j = (m + n + 1) / 2 - i;  // nums2 的分割点
        int left1 = i == 0 ? Integer.MIN_VALUE : nums1[i - 1];
        int right1 = i == m ? Integer.MAX_VALUE : nums1[i];
        int left2 = j == 0 ? Integer.MIN_VALUE : nums2[j - 1];
        int right2 = j == n ? Integer.MAX_VALUE : nums2[j];
        if (left1 <= right2 && left2 <= right1) {   // 找到正确分割
            if ((m + n) % 2 == 1) return Math.max(left1, left2);
            return (Math.max(left1, left2) + Math.min(right1, right2)) / 2.0;
        }
        if (left1 > right2) r = i - 1;   // 左半过大，i 左移
        else l = i + 1;
    }
    return 0;
}
```

**举例**：

- 输入：`nums1 = [1,3], nums2 = [2]`
- 输出：`2.0`（合并后 `[1,2,3]` 的中位数）

---

## 十九、堆

### 121. 215. 数组中的第K个最大元素

- **难度**：中等
- **思路**：维护大小为 k 的小顶堆，遍历完堆顶就是第 k 大。

```java
public int findKthLargest(int[] nums, int k) {
    PriorityQueue<Integer> pq = new PriorityQueue<>();  // 小顶堆，保留最大的 k 个
    for (int x : nums) {
        pq.offer(x);
        if (pq.size() > k) pq.poll();   // 超过 k 个，弹出最小的
    }
    return pq.peek();                   // 堆顶就是第 k 大
}
```

**举例**：

- 输入：`nums = [3,2,1,5,6,4], k = 2`
- 过程：堆里始终保留最大的 2 个，最终堆顶是 5。
- 输出：`5`

### 122. 502. IPO

- **难度**：困难
- **思路**：按启动资金排序，把当前能做项目按利润放进大顶堆，每次做利润最高的。

```java
public int findMaximizedCapital(int k, int w, int[] profits, int[] capital) {
    int n = profits.length;
    int[][] projects = new int[n][2];
    for (int i = 0; i < n; i++) projects[i] = new int[]{capital[i], profits[i]};
    Arrays.sort(projects, (a, b) -> a[0] - b[0]);   // 按启动资金排序
    PriorityQueue<Integer> pq = new PriorityQueue<>((a, b) -> b - a);  // 大顶堆存利润
    int idx = 0;
    for (int i = 0; i < k; i++) {
        while (idx < n && projects[idx][0] <= w) {   // 能启动的项目入堆
            pq.offer(projects[idx++][1]);
        }
        if (pq.isEmpty()) break;                      // 无项目可做
        w += pq.poll();                               // 做利润最高的项目
    }
    return w;
}
```

**举例**：

- 输入：`k = 2, w = 0, profits = [1,2,3], capital = [0,1,1]`
- 过程：先做利润 1 的项目（资金 0 可做），资金变 1；再做利润 3 的项目，资金变 4。
- 输出：`4`

### 123. 373. 查找和最小的 K 对数字

- **难度**：中等
- **思路**：最小堆按「两数之和」排序，先把 `(i, 0)` 入堆，每次弹出后推入同行的下一列。

```java
public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) ->
        nums1[a[0]] + nums2[a[1]] - nums1[b[0]] - nums2[b[1]]);  // 按和排序的小顶堆
    List<List<Integer>> ans = new ArrayList<>();
    for (int i = 0; i < Math.min(nums1.length, k); i++) {
        pq.offer(new int[]{i, 0});   // 先把每行的第一列入堆
    }
    while (k-- > 0 && !pq.isEmpty()) {
        int[] cur = pq.poll();
        ans.add(Arrays.asList(nums1[cur[0]], nums2[cur[1]]));
        if (cur[1] + 1 < nums2.length) pq.offer(new int[]{cur[0], cur[1] + 1});  // 下一列
    }
    return ans;
}
```

**举例**：

- 输入：`nums1 = [1,7,11], nums2 = [2,4,6], k = 3`
- 输出：`[[1,2],[1,4],[1,6]]`

### 124. 295. 数据流的中位数

- **难度**：困难
- **思路**：大顶堆存较小一半、小顶堆存较大一半，中位数在两堆顶之间。

```java
class MedianFinder {
    PriorityQueue<Integer> small = new PriorityQueue<>((a, b) -> b - a);  // 大顶堆，存较小一半
    PriorityQueue<Integer> large = new PriorityQueue<>();                  // 小顶堆，存较大一半

    public void addNum(int num) {
        small.offer(num);
        large.offer(small.poll());        // 保证 small 的元素都 <= large
        if (large.size() > small.size()) small.offer(large.poll());  // 保持 small 元素数 >= large
    }

    public double findMedian() {
        if (small.size() > large.size()) return small.peek();   // 奇数个
        return (small.peek() + large.peek()) / 2.0;             // 偶数个
    }
}
```

**举例**：

- 操作：`addNum(1)`、`addNum(2)`、`findMedian()`
- 过程：`small=[1]`、`large=[2]`，中位数 `(1+2)/2=1.5`。
- 输出：`1.5`

---

## 二十、位运算

### 125. 67. 二进制求和

- **难度**：简单
- **思路**：模拟二进制竖式加法，从低位逐位相加并处理进位。

```java
public String addBinary(String a, String b) {
    StringBuilder sb = new StringBuilder();
    int i = a.length() - 1, j = b.length() - 1, carry = 0;
    while (i >= 0 || j >= 0 || carry > 0) {
        int sum = carry;
        if (i >= 0) sum += a.charAt(i--) - '0';
        if (j >= 0) sum += b.charAt(j--) - '0';
        sb.append(sum % 2);   // 当前位
        carry = sum / 2;      // 进位
    }
    return sb.reverse().toString();
}
```

**举例**：

- 输入：`a = "11", b = "1"`
- 过程：1+1=0 进 1；1+0+1=0 进 1；最后进位 1。
- 输出：`"100"`

### 126. 190. 颠倒二进制位

- **难度**：简单
- **思路**：逐位取出 n 的最低位，拼到结果的末尾，重复 32 次。

```java
public int reverseBits(int n) {
    int ans = 0;
    for (int i = 0; i < 32; i++) {
        ans = (ans << 1) | (n & 1);   // ans 左移一位，加上 n 的最低位
        n >>>= 1;                      // n 无符号右移
    }
    return ans;
}
```

**举例**：

- 输入：`n = 00000010100101000001111010011100`
- 输出：`964176192`（二进制 `00111001011110000010100101000000`）

### 127. 191. 位1的个数

- **难度**：简单
- **思路**：`n & (n-1)` 会消掉最低位的 1，循环计数。

```java
public int hammingWeight(int n) {
    int count = 0;
    while (n != 0) {
        n &= (n - 1);   // 消掉最低位的 1
        count++;
    }
    return count;
}
```

**举例**：

- 输入：`n = 11`（二进制 `1011`）
- 过程：`1011 → 1010 → 1000 → 0`，共 3 次。
- 输出：`3`

### 128. 136. 只出现一次的数字

- **难度**：简单
- **思路**：异或运算，成对出现的数字抵消，最后剩下只出现一次的那个。

```java
public int singleNumber(int[] nums) {
    int ans = 0;
    for (int x : nums) ans ^= x;   // 异或，成对抵消
    return ans;
}
```

**举例**：

- 输入：`[2,2,1]`
- 过程：`2^2=0`，`0^1=1`。
- 输出：`1`

### 129. 137. 只出现一次的数字 II

- **难度**：中等
- **思路**：用 `ones`、`twos` 两个变量按位统计，出现三次的位会归零，剩一次的数字留在 `ones`。

```java
public int singleNumber(int[] nums) {
    int ones = 0, twos = 0;   // ones：出现一次的位，twos：出现两次的位
    for (int x : nums) {
        ones = (ones ^ x) & ~twos;
        twos = (twos ^ x) & ~ones;
    }
    return ones;
}
```

**举例**：

- 输入：`[2,2,3,2]`
- 过程：三次出现的 2 被消掉，3 留下。
- 输出：`3`

### 130. 201. 数字范围按位与

- **难度**：中等
- **思路**：找 `left` 和 `right` 的公共前缀，后面的位在范围里必然既有 0 又有 1，与完是 0。

```java
public int rangeBitwiseAnd(int left, int right) {
    int shift = 0;
    while (left < right) {   // 一起右移，找公共前缀
        left >>= 1;
        right >>= 1;
        shift++;
    }
    return left << shift;    // 公共前缀左移回去
}
```

**举例**：

- 输入：`left = 5 (101), right = 7 (111)`
- 过程：右移一次得 `2 (10)` 和 `3 (11)`，再右移得 `1 (1)` 和 `1 (1)`，公共前缀 1，左移 2 位得 4。
- 输出：`4`

---

## 二十一、数学

### 131. 9. 回文数

- **难度**：简单
- **思路**：反转数字的一半，与剩下的一半比较，避免整型溢出。

```java
public boolean isPalindrome(int x) {
    if (x < 0 || (x % 10 == 0 && x != 0)) return false;   // 负数或末尾 0（非 0）不是
    int rev = 0;
    while (x > rev) {           // 只反转一半
        rev = rev * 10 + x % 10;
        x /= 10;
    }
    return x == rev || x == rev / 10;   // 偶数位或奇数位
}
```

**举例**：

- 输入：`121`
- 过程：反转一半得 `rev=12`、`x=1`，`1 == 12/10`。
- 输出：`true`

### 132. 66. 加一

- **难度**：简单
- **思路**：从末位开始加一，遇 9 就进位置 0，全 9 则新建数组。

```java
public int[] plusOne(int[] digits) {
    for (int i = digits.length - 1; i >= 0; i--) {
        if (digits[i] < 9) {   // 不满 9，直接加一返回
            digits[i]++;
            return digits;
        }
        digits[i] = 0;         // 是 9，置 0 进位
    }
    int[] ans = new int[digits.length + 1];   // 全是 9，如 999 -> 1000
    ans[0] = 1;
    return ans;
}
```

**举例**：

- 输入：`[1,2,3]` → `[1,2,4]`
- 输入：`[9,9,9]` → `[1,0,0,0]`

### 133. 172. 阶乘后的零

- **难度**：中等
- **思路**：末尾 0 由 2×5 产生，2 的数量远多于 5，只需统计因子 5 的个数。

```java
public int trailingZeroes(int n) {
    int count = 0;
    while (n > 0) {
        n /= 5;         // 统计 5、25、125… 的贡献
        count += n;
    }
    return count;
}
```

**举例**：

- 输入：`5`（5! = 120）
- 过程：`5/5=1`，共 1 个因子 5。
- 输出：`1`

### 134. 69. x 的平方根

- **难度**：简单
- **思路**：二分查找满足 `mid² <= x` 的最大整数（用除法比较避免溢出）。

```java
public int mySqrt(int x) {
    if (x == 0) return 0;
    int l = 1, r = x, ans = 0;
    while (l <= r) {
        int mid = l + (r - l) / 2;
        if (mid <= x / mid) {   // mid² <= x（用除法防溢出）
            ans = mid;
            l = mid + 1;
        } else {
            r = mid - 1;
        }
    }
    return ans;
}
```

**举例**：

- 输入：`8`
- 过程：2²=4 ≤ 8，3²=9 > 8，取 2。
- 输出：`2`

### 135. 50. Pow(x, n)

- **难度**：中等
- **思路**：快速幂。把指数按二进制拆，对应位为 1 就乘上当前的 x。

```java
public double myPow(double x, int n) {
    long N = n;                    // 防 -2^31 溢出
    if (N < 0) { x = 1 / x; N = -N; }
    double ans = 1;
    while (N > 0) {
        if ((N & 1) == 1) ans *= x;   // 二进制位为 1 时乘
        x *= x;                       // x 平方
        N >>= 1;
    }
    return ans;
}
```

**举例**：

- 输入：`x = 2.0, n = 10`
- 过程：10 的二进制 `1010`，对应 `2^2 * 2^8 = 1024`。
- 输出：`1024.0`

### 136. 149. 直线上最多的点数

- **难度**：困难
- **思路**：固定一个点，统计它到其它点的「化简后的斜率」，出现次数最多的斜率就是过该点的最多共线数。

```java
public int maxPoints(int[][] points) {
    int n = points.length;
    if (n <= 2) return n;
    int ans = 0;
    for (int i = 0; i < n; i++) {
        Map<String, Integer> map = new HashMap<>();   // 斜率 -> 点数
        int same = 0, max = 0;
        for (int j = i + 1; j < n; j++) {
            int dx = points[j][0] - points[i][0];
            int dy = points[j][1] - points[i][1];
            if (dx == 0 && dy == 0) { same++; continue; }   // 重合点
            int g = gcd(dx, dy);         // 用最大公约数化简斜率
            String key = (dx / g) + "/" + (dy / g);
            map.merge(key, 1, Integer::sum);
            max = Math.max(max, map.get(key));
        }
        ans = Math.max(ans, max + same + 1);   // 加上基准点和重合点
    }
    return ans;
}

int gcd(int a, int b) { return b == 0 ? a : gcd(b, a % b); }
```

**举例**：

- 输入：`[[1,1],[2,2],[3,3]]`
- 过程：三点同斜率，共线。
- 输出：`3`

---

## 二十二、一维动态规划

### 137. 70. 爬楼梯

- **难度**：简单
- **思路**：第 i 阶只能从 i-1 或 i-2 阶上来，所以 `dp[i] = dp[i-1] + dp[i-2]`，即斐波那契。

```java
public int climbStairs(int n) {
    int a = 1, b = 1;   // a = dp[0], b = dp[1]
    for (int i = 2; i <= n; i++) {
        int c = a + b;  // 第 i 阶 = 前两阶之和
        a = b;
        b = c;
    }
    return b;
}
```

**举例**：

- 输入：`n = 3`
- 过程：`1, 2, 3` 种方法。
- 输出：`3`

### 138. 198. 打家劫舍

- **难度**：中等
- **思路**：每间房要么不抢（继承前一家最优），要么抢（前两家最优 + 本家），取较大。

```java
public int rob(int[] nums) {
    int prev2 = 0, prev1 = 0;   // 前两家、前一家的最优
    for (int x : nums) {
        int cur = Math.max(prev1, prev2 + x);   // 不抢 vs 抢
        prev2 = prev1;
        prev1 = cur;
    }
    return prev1;
}
```

**举例**：

- 输入：`[1,2,3,1]`
- 过程：抢 1 和 3，共 4。
- 输出：`4`

### 139. 139. 单词拆分

- **难度**：中等
- **思路**：`dp[i]` 表示前 i 个字符能否拆分，枚举切分点，前段可拆且后段是单词即可。

```java
public boolean wordBreak(String s, List<String> wordDict) {
    Set<String> set = new HashSet<>(wordDict);
    boolean[] dp = new boolean[s.length() + 1];
    dp[0] = true;                          // 空前缀可拆分
    for (int i = 1; i <= s.length(); i++) {
        for (int j = 0; j < i; j++) {
            if (dp[j] && set.contains(s.substring(j, i))) {   // 前 j 可拆且 [j,i) 是单词
                dp[i] = true;
                break;
            }
        }
    }
    return dp[s.length()];
}
```

**举例**：

- 输入：`s = "leetcode", wordDict = ["leet","code"]`
- 过程：`leet` + `code` 正好拆完。
- 输出：`true`

### 140. 322. 零钱兑换

- **难度**：中等
- **思路**：`dp[i]` 表示凑出 i 元的最少硬币数，枚举每种硬币转移。

```java
public int coinChange(int[] coins, int amount) {
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, amount + 1);   // 初始化为不可达
    dp[0] = 0;
    for (int i = 1; i <= amount; i++) {
        for (int c : coins) {
            if (c <= i) dp[i] = Math.min(dp[i], dp[i - c] + 1);  // 用一枚 c
        }
    }
    return dp[amount] > amount ? -1 : dp[amount];
}
```

**举例**：

- 输入：`coins = [1,2,5], amount = 11`
- 过程：`5 + 5 + 1`，共 3 枚。
- 输出：`3`

### 141. 300. 最长递增子序列

- **难度**：中等
- **思路**：`dp[i]` 表示以 `nums[i]` 结尾的最长递增子序列长度，枚举前面所有比它小的数。

```java
public int lengthOfLIS(int[] nums) {
    int[] dp = new int[nums.length];
    int ans = 0;
    for (int i = 0; i < nums.length; i++) {
        dp[i] = 1;                          // 自身至少为 1
        for (int j = 0; j < i; j++) {
            if (nums[j] < nums[i]) dp[i] = Math.max(dp[i], dp[j] + 1);  // 接在 j 后面
        }
        ans = Math.max(ans, dp[i]);
    }
    return ans;
}
```

**举例**：

- 输入：`[10,9,2,5,3,7,101,18]`
- 过程：`2,5,7,101` 长度 4。
- 输出：`4`

---

## 二十三、多维动态规划

### 142. 120. 三角形最小路径和

- **难度**：中等
- **思路**：自底向上，每个位置取下一行相邻两个的较小值累加。

```java
public int minimumTotal(List<List<Integer>> triangle) {
    int n = triangle.size();
    int[] dp = new int[n + 1];   // 从最后一行开始，dp[j] 表示到当前行第 j 个的最小和
    for (int i = n - 1; i >= 0; i--) {
        List<Integer> row = triangle.get(i);
        for (int j = 0; j <= i; j++) {
            dp[j] = row.get(j) + Math.min(dp[j], dp[j + 1]);  // 从下一行相邻两个转移
        }
    }
    return dp[0];
}
```

**举例**：

- 输入：`[[2],[3,4],[6,5,7],[4,1,8,3]]`
- 过程：自底向上累加，最小路径 2→3→5→1。
- 输出：`11`

### 143. 64. 最小路径和

- **难度**：中等
- **思路**：每个格子只能从上面或左边来，取较小值累加，原地更新。

```java
public int minPathSum(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (i == 0 && j == 0) continue;                    // 起点
            if (i == 0) grid[i][j] += grid[i][j - 1];          // 只能从左来
            else if (j == 0) grid[i][j] += grid[i - 1][j];     // 只能从上
            else grid[i][j] += Math.min(grid[i - 1][j], grid[i][j - 1]);  // 取较小
        }
    }
    return grid[m - 1][n - 1];
}
```

**举例**：

- 输入：`[[1,3,1],[1,5,1],[4,2,1]]`
- 过程：最优路径 `1→3→1→1→1`，和为 7。
- 输出：`7`

### 144. 63. 不同路径 II

- **难度**：中等
- **思路**：滚动数组，障碍格置 0，其余格子等于左 + 上。

```java
public int uniquePathsWithObstacles(int[][] obstacleGrid) {
    int m = obstacleGrid.length, n = obstacleGrid[0].length;
    int[] dp = new int[n];
    dp[0] = obstacleGrid[0][0] == 1 ? 0 : 1;   // 起点有障碍则无路
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (obstacleGrid[i][j] == 1) { dp[j] = 0; continue; }   // 障碍
            if (j > 0) dp[j] += dp[j - 1];   // 左 + 上（dp[j] 即上一行的值）
        }
    }
    return dp[n - 1];
}
```

**举例**：

- 输入：`[[0,0,0],[0,1,0],[0,0,0]]`
- 过程：绕过中间障碍，有 2 条路径。
- 输出：`2`

### 145. 5. 最长回文子串

- **难度**：中等
- **思路**：中心扩展法，每个位置作为中心（奇数/偶数）向两边扩展。

```java
public String longestPalindrome(String s) {
    int start = 0, maxLen = 0;
    for (int i = 0; i < s.length(); i++) {
        int l1 = expand(s, i, i);     // 奇数长度
        int l2 = expand(s, i, i + 1); // 偶数长度
        int len = Math.max(l1, l2);
        if (len > maxLen) {
            maxLen = len;
            start = i - (len - 1) / 2;
        }
    }
    return s.substring(start, start + maxLen);
}

int expand(String s, int l, int r) {   // 向两边扩展
    while (l >= 0 && r < s.length() && s.charAt(l) == s.charAt(r)) { l--; r++; }
    return r - l - 1;                  // 返回回文长度
}
```

**举例**：

- 输入：`"babad"`
- 过程：以 b 为中心得 `bab`，以 a 为中心得 `aba`。
- 输出：`"bab"`（或 `"aba"`）

### 146. 97. 交错字符串

- **难度**：中等
- **思路**：`dp[j]` 表示 s1 前 i 个、s2 前 j 个能否交错成 s3 前 i+j 个，滚动一维。

```java
public boolean isInterleave(String s1, String s2, String s3) {
    int m = s1.length(), n = s2.length();
    if (m + n != s3.length()) return false;
    boolean[] dp = new boolean[n + 1];
    dp[0] = true;
    for (int j = 1; j <= n; j++) dp[j] = dp[j - 1] && s2.charAt(j - 1) == s3.charAt(j - 1);  // 第一行
    for (int i = 1; i <= m; i++) {
        dp[0] = dp[0] && s1.charAt(i - 1) == s3.charAt(i - 1);   // 第一列
        for (int j = 1; j <= n; j++) {
            dp[j] = (dp[j] && s1.charAt(i - 1) == s3.charAt(i + j - 1))
                 || (dp[j - 1] && s2.charAt(j - 1) == s3.charAt(i + j - 1));
        }
    }
    return dp[n];
}
```

**举例**：

- 输入：`s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"`
- 输出：`true`

### 147. 72. 编辑距离

- **难度**：中等
- **思路**：经典 DP。字符相同不操作，不同取「删/插/替」三种操作的最小值 + 1。

```java
public int minDistance(String word1, String word2) {
    int m = word1.length(), n = word2.length();
    int[] dp = new int[n + 1];
    for (int j = 0; j <= n; j++) dp[j] = j;   // 空串到 word2 需要 j 次插入
    for (int i = 1; i <= m; i++) {
        int prev = dp[0];
        dp[0] = i;
        for (int j = 1; j <= n; j++) {
            int temp = dp[j];
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[j] = prev;                 // 相等，不操作
            } else {
                dp[j] = 1 + Math.min(Math.min(dp[j], dp[j - 1]), prev);  // 删/插/替
            }
            prev = temp;
        }
    }
    return dp[n];
}
```

**举例**：

- 输入：`word1 = "horse", word2 = "ros"`
- 过程：`horse → rorse → rose → ros`，3 步。
- 输出：`3`

### 148. 123. 买卖股票的最佳时机 III

- **难度**：困难
- **思路**：状态机，用四个变量分别表示「第一次买、第一次卖、第二次买、第二次卖」的最优值。

```java
public int maxProfit(int[] prices) {
    int buy1 = Integer.MIN_VALUE, sell1 = 0;
    int buy2 = Integer.MIN_VALUE, sell2 = 0;
    for (int p : prices) {
        buy1 = Math.max(buy1, -p);           // 第一次买入
        sell1 = Math.max(sell1, buy1 + p);   // 第一次卖出
        buy2 = Math.max(buy2, sell1 - p);    // 第二次买入
        sell2 = Math.max(sell2, buy2 + p);   // 第二次卖出
    }
    return sell2;
}
```

**举例**：

- 输入：`[3,3,5,0,0,3,1,4]`
- 过程：`3 买 5 卖`赚 2，`0 买 4 卖`赚 4，共 6。
- 输出：`6`

### 149. 188. 买卖股票的最佳时机 IV

- **难度**：困难
- **思路**：k 次交易的通用版，用两个数组维护第 j 次买卖的最优；k 足够大时退化为无限次。

```java
public int maxProfit(int k, int[] prices) {
    if (k == 0 || prices.length == 0) return 0;
    if (k >= prices.length / 2) {   // k 足够大，等价于无限次交易
        int ans = 0;
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > prices[i - 1]) ans += prices[i] - prices[i - 1];
        }
        return ans;
    }
    int[] buy = new int[k];
    int[] sell = new int[k];
    Arrays.fill(buy, Integer.MIN_VALUE);
    for (int p : prices) {
        for (int j = 0; j < k; j++) {
            buy[j] = Math.max(buy[j], (j == 0 ? 0 : sell[j - 1]) - p);
            sell[j] = Math.max(sell[j], buy[j] + p);
        }
    }
    return sell[k - 1];
}
```

**举例**：

- 输入：`k = 2, prices = [3,2,6,5,0,3]`
- 过程：`2 买 6 卖`赚 4，`0 买 3 卖`赚 3，共 7。
- 输出：`7`

### 150. 221. 最大正方形

- **难度**：中等
- **思路**：`dp[j]` 表示以当前位置为右下角的最大正方形边长，取左、上、左上三者的最小值 + 1。

```java
public int maximalSquare(char[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    int[] dp = new int[n + 1];
    int maxSide = 0, prev = 0;
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            int temp = dp[j];
            if (matrix[i - 1][j - 1] == '1') {
                dp[j] = Math.min(Math.min(dp[j], dp[j - 1]), prev) + 1;  // 三方向最小 + 1
                maxSide = Math.max(maxSide, dp[j]);
            } else {
                dp[j] = 0;
            }
            prev = temp;
        }
    }
    return maxSide * maxSide;   // 面积 = 边长平方
}
```

**举例**：

- 输入：`[["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]`
- 过程：右下 2×2 全为 1，边长 2。
- 输出：`4`

---

> **全文完**：共 23 章、150 题。每道题都是「思路 + 带 `//` 注释的代码 + 举例」三段式，可直接提交。
