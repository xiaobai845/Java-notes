# 力扣 Hot 100 · 复习速查手册（Java）

> **用法**：先读「思路」→ 合上书自己顺着思路敲一遍 → 再对照下方代码（**代码已逐行注释**）→ 最后看「例子讲解」验证自己的推导。
>
> **全部 100 题**，按官方 17 个专题分类，一题一节，目录可折叠（Typora 侧边栏大纲 / `[TOC]` 均可跳转）。

[TOC]

---

## 通用节点定义

```java
// LeetCode 平台已内置，本地补全用
class ListNode {
    int val;                 // 结点值
    ListNode next;           // 后继指针
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}
class TreeNode {
    int val;                 // 结点值
    TreeNode left, right;    // 左右孩子
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
}
```

## 分类索引

| 分类 | 题量 | 题目 |
| :--- | :--: | :--- |
| 一、哈希 | 3 | 1 两数之和 · 49 字母异位词分组 · 128 最长连续序列 |
| 二、双指针 | 4 | 283 移动零 · 11 盛最多水的容器 · 15 三数之和 · 42 接雨水 |
| 三、滑动窗口 | 2 | 3 无重复字符的最长子串 · 438 找到字符串中所有字母异位词 |
| 四、子串 | 3 | 560 和为 K 的子数组 · 239 滑动窗口最大值 · 76 最小覆盖子串 |
| 五、普通数组 | 5 | 53 最大子数组和 · 56 合并区间 · 189 轮转数组 · 238 除自身以外数组的乘积 · 41 缺失的第一个正数 |
| 六、矩阵 | 4 | 73 矩阵置零 · 54 螺旋矩阵 · 48 旋转图像 · 240 搜索二维矩阵 II |
| 七、链表 | 14 | 160 相交链表 · 206 反转链表 · 234 回文链表 · 141 环形链表 · 142 环形链表 II · 21 合并两个有序链表 · 2 两数相加 · 19 删除倒数第 N 个 · 24 两两交换 · 25 K 个一组翻转 · 138 随机链表的复制 · 148 排序链表 · 23 合并 K 个升序链表 · 146 LRU 缓存 |
| 八、二叉树 | 15 | 94 中序遍历 · 104 最大深度 · 226 翻转 · 101 对称 · 543 直径 · 102 层序遍历 · 108 有序数组转 BST · 98 验证 BST · 230 第 K 小 · 199 右视图 · 114 展开为链表 · 105 前序+中序构造 · 437 路径总和 III · 236 最近公共祖先 · 124 最大路径和 |
| 九、图论 | 4 | 200 岛屿数量 · 994 腐烂的橘子 · 207 课程表 · 208 实现 Trie |
| 十、回溯 | 8 | 46 全排列 · 78 子集 · 17 电话号码的字母组合 · 39 组合总和 · 22 括号生成 · 79 单词搜索 · 131 分割回文串 · 51 N 皇后 |
| 十一、二分查找 | 6 | 35 搜索插入位置 · 74 搜索二维矩阵 · 34 查找第一个和最后一个位置 · 33 搜索旋转排序数组 · 153 旋转数组最小值 · 4 两个正序数组的中位数 |
| 十二、栈 | 5 | 20 有效的括号 · 155 最小栈 · 394 字符串解码 · 739 每日温度 · 84 柱状图中最大的矩形 |
| 十三、堆 | 3 | 215 第 K 个最大元素 · 347 前 K 个高频元素 · 295 数据流的中位数 |
| 十四、贪心算法 | 4 | 121 买卖股票的最佳时机 · 55 跳跃游戏 · 45 跳跃游戏 II · 763 划分字母区间 |
| 十五、动态规划 | 10 | 70 爬楼梯 · 118 杨辉三角 · 198 打家劫舍 · 279 完全平方数 · 322 零钱兑换 · 139 单词拆分 · 300 最长递增子序列 · 152 乘积最大子数组 · 416 分割等和子集 · 32 最长有效括号 |
| 十六、多维动态规划 | 5 | 62 不同路径 · 64 最小路径和 · 5 最长回文子串 · 1143 最长公共子序列 · 72 编辑距离 |
| 十七、技巧 | 5 | 136 只出现一次的数字 · 169 多数元素 · 75 颜色分类 · 31 下一个排列 · 287 寻找重复数 |

---

## 一、哈希

> 核心思想：**空间换时间**。看到「两数/计数/去重/分组」先想哈希。

### 1. 两数之和 · 简单

**思路**：一遍遍历，哈希表存 `值 → 下标`。对每个 `x` 先查 `target - x` 是否已出现过；没查到就把 `x` 存进去（保证不用自己配对）。

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();          // 值 -> 下标
        for (int i = 0; i < nums.length; i++) {
            Integer j = map.get(target - nums[i]);            // 先查补数在不在
            if (j != null) return new int[]{j, i};            // 命中直接返回
            map.put(nums[i], i);                              // 没命中再把自己存进去
        }
        return new int[0];                                    // 题目保证有解，兜底
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(n)`

**例子讲解**：`nums = [2, 7, 11, 15]`，`target = 9`

- `i=0`，`x=2`：查 `9-2=7`，表里没有 → 存入 `{2:0}`
- `i=1`，`x=7`：查 `9-7=2`，命中下标 `0` → 返回 `[0, 1]`

关键在「**先查后存**」：如果先存再查，`i=0` 时查 `9-2=7` 会查到还没存的值？不会，但反过来写会出现自己配自己（如 `target = 2*x`）。先查后存天然规避。

---

### 49. 字母异位词分组 · 中等

**思路**：异位词「排序后字符串相同」→ 用排序结果当 key 分组。（更快写法：用 26 位计数编码成字符串当 key，把排序的 `O(k log k)` 降到 `O(k)`。）

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();                 // 排序后的串 -> 原词列表
        for (String s : strs) {
            char[] c = s.toCharArray();
            Arrays.sort(c);                                              // 排序得到统一指纹
            map.computeIfAbsent(new String(c), k -> new ArrayList<>())   // key 不存在就新建列表
               .add(s);                                                  // 挂到对应分组
        }
        return new ArrayList<>(map.values());                            // 所有分组即答案
    }
}
```

**复杂度**：时间 `O(n·k log k)`，空间 `O(n·k)`

**例子讲解**：`strs = ["eat","tea","tan","ate","nat","bat"]`

| 原词 | 排序后（key） |
| :-- | :-- |
| eat | `aet` |
| tea | `aet` |
| tan | `ant` |
| ate | `aet` |
| nat | `ant` |
| bat | `abt` |

`map` 最终为 `{aet:[eat,tea,ate], ant:[tan,nat], abt:[bat]}`，`values()` 就是答案。注意「字母异位词」和「子串异位词」不同，这里顺序任意，所以排序当指纹是安全的。

---

### 128. 最长连续序列 · 中等

**思路**：全部丢进 `HashSet`。**只从「没有前驱 `x-1`」的数开始**向后连续计数——这样每个数只会被访问常数次，避免重复扫描。

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int x : nums) set.add(x);                 // 去重并支持 O(1) 查询
        int ans = 0;
        for (int x : set) {
            if (set.contains(x - 1)) continue;          // 不是序列起点，跳过（保证总复杂度 O(n)）
            int y = x;
            while (set.contains(y + 1)) y++;            // 从起点一路往后数
            ans = Math.max(ans, y - x + 1);             // 更新最长长度
        }
        return ans;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(n)`

**例子讲解**：`nums = [100, 4, 200, 1, 3, 2]`，`set = {1,2,3,4,100,200}`

- `x=100`：没有 `99` → 是起点，往后 `101` 不存在 → 长度 **1**
- `x=4`：有 `3` → 跳过（它属于以 1 开头的序列）
- `x=200`：没有 `199` → 起点，长度 **1**
- `x=1`：没有 `0` → 起点，一路数到 `4` → 长度 **4** ✅
- `x=3`：有 `2` → 跳过；`x=2`：有 `1` → 跳过

答案 `4`。**为什么复杂度是 O(n)**：只有起点才会触发 `while`，每个元素最多被「数到」一次。

---

## 二、双指针

> 核心思想：**用两个指针把 O(n²) 降到 O(n)**。关键是想清楚「往哪边移动」。

### 283. 移动零 · 简单

**思路**：快慢指针。`j` 指向下一个待填充的非零位置，`i` 扫到非零就与 `nums[j]` 交换。

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int j = 0;                                        // j: 下一个非零元素的落点
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) {                           // 遇到非零才处理
                int t = nums[i]; nums[i] = nums[j]; nums[j] = t;
                j++;                                      // 落点右移
            }
        }
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`nums = [0, 1, 0, 3, 12]`

| i | 当前值 | 动作 | 数组变成 | j |
| :-- | :-- | :-- | :-- | :-- |
| 0 | 0 | 跳过 | `[0,1,0,3,12]` | 0 |
| 1 | 1 | 交换 `i=1`↔`j=0` | `[1,0,0,3,12]` | 1 |
| 2 | 0 | 跳过 | `[1,0,0,3,12]` | 1 |
| 3 | 3 | 交换 `i=3`↔`j=1` | `[1,3,0,0,12]` | 2 |
| 4 | 12 | 交换 `i=4`↔`j=2` | `[1,3,12,0,0]` | 3 |

答案 `[1,3,12,0,0]`。用「交换」而非「赋值 + 最后补零」，一次遍历就够；`j <= i` 恒成立，所以不会丢数据。

---

### 11. 盛最多水的容器 · 中等

**思路**：左右指针夹逼，面积 = `min(h[l], h[r]) * (r - l)`。**每次移动较矮的一边**——移动高边只会让宽度变小且高度仍受限于矮边，不可能更优。

```java
class Solution {
    public int maxArea(int[] h) {
        int l = 0, r = h.length - 1, ans = 0;
        while (l < r) {
            ans = Math.max(ans, Math.min(h[l], h[r]) * (r - l));   // 宽度 × 较矮的板
            if (h[l] < h[r]) l++;                                  // 移动矮的一侧才有机会变高
            else r--;
        }
        return ans;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`height = [1,8,6,2,5,4,8,3,7]`

- `l=0(1), r=8(7)`：面积 `min(1,7) × 8 = 8`，左边矮 → `l=1`
- `l=1(8), r=8(7)`：面积 `min(8,7) × 7 = 49` ✅（最优），右边矮 → `r=7`
- `l=1(8), r=7(3)`：`3 × 6 = 18`，右边矮 → `r=6`
- `l=1(8), r=6(8)`：`8 × 5 = 40`，相等走 `else` → `r=5`
- `l=1, r=5(4)`：`4 × 4 = 16` → `r=4`；`r=3`：`2 × 2 = 4`；`r=2`：`6 × 1 = 6` → 结束

答案 `49`。**为什么移动矮边不会漏解**：假设移动的是高边，新容器高度 `≤` 原高度且宽度更小，面积必然不增，所以最优解不可能只在被丢掉的那些组合里。

---

### 15. 三数之和 · 中等

**思路**：排序 + **固定第一个数 + 双指针**。注意三处去重：`i` 跳过相同值、找到答案后 `l/r` 跳过相同值、`nums[i] > 0` 提前 break。

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);                                   // 排序后才能用双指针 + 方便去重
        List<List<Integer>> res = new ArrayList<>();
        for (int i = 0; i < nums.length - 2; i++) {
            if (nums[i] > 0) break;                          // 最小的都 >0，后面不可能和为 0
            if (i > 0 && nums[i] == nums[i - 1]) continue;   // 第一层去重
            int l = i + 1, r = nums.length - 1;
            while (l < r) {
                int s = nums[i] + nums[l] + nums[r];
                if (s < 0) l++;                              // 和太小，左指针右移
                else if (s > 0) r--;                         // 和太大，右指针左移
                else {
                    res.add(Arrays.asList(nums[i], nums[l], nums[r]));
                    while (l < r && nums[l] == nums[l + 1]) l++;   // 第二层去重：跳过重复的左值
                    while (l < r && nums[r] == nums[r - 1]) r--;   // 跳过重复的右值
                    l++; r--;                                          // 同时收缩
                }
            }
        }
        return res;
    }
}
```

**复杂度**：时间 `O(n²)`，空间 `O(log n)`（排序栈深度）

**例子讲解**：`nums = [-1,0,1,2,-1,-4]` → 排序后 `[-4,-1,-1,0,1,2]`

- `i=0`（值 `-4`）：`l=1(-1), r=5(2)` 和 `-3<0` → `l=2` 和 `-3<0` → `l=3` 和 `-2<0` → `l=4` 和 `-1<0` → 越界结束
- `i=1`（值 `-1`）：`l=2(-1), r=5(2)` 和 `0` ✅ → 收 `[-1,-1,2]`，跳过重复后 `l=3, r=4`
  - `l=3(0), r=4(1)` 和 `0` ✅ → 收 `[-1,0,1]` → `l=4, r=3` 结束
- `i=2`：`nums[2] == nums[1] == -1` → **跳过**（否则重复）
- `i=3`（值 `0`）：`l=4(1), r=5(2)` 和 `3>0` → `r=4`，`l==r` 结束
- `i=4`：`l=5, r=5` 不满足 `l<r`

答案 `[[-1,-1,2], [-1,0,1]]`。**三处去重缺一不可**，这是本题最容易 WA 的地方。

---

### 42. 接雨水 · 困难

**思路**：双指针 + 左右最大高度。始终处理**较矮的一侧**：该侧能接的水只由该侧的 `max` 决定，差值即为这一格的积水。

```java
class Solution {
    public int trap(int[] h) {
        int l = 0, r = h.length - 1;
        int lm = 0, rm = 0, ans = 0;                       // lm/rm: 左右两侧见过的最大高度
        while (l < r) {
            if (h[l] < h[r]) {                             // 较矮的是左侧
                lm = Math.max(lm, h[l]);                   // 更新左侧最大高度
                ans += lm - h[l++];                        // 这一格能接 lm - 当前高
            } else {                                       // 较矮的是右侧
                rm = Math.max(rm, h[r]);
                ans += rm - h[r--];
            }
        }
        return ans;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`height = [0,1,0,2,1,0,1,3,2,1,2,1]`（答案 6）

| 步 | l | r | 较矮侧 | lm / rm | 本次加水 | 累计 |
| :-- | :-- | :-- | :-- | :-- | :-- | :-- |
| 1 | 0(0) | 11(1) | 左 | lm=0 | 0 | 0 |
| 2 | 1(1) | 11(1) | 右(相等) | rm=1 | 0 | 0 |
| 3 | 1(1) | 10(2) | 左 | lm=1 | 0 | 0 |
| 4 | 2(0) | 10(2) | 左 | lm=1 | **1** | 1 |
| 5 | 3(2) | 10(2) | 右 | rm=2 | 0 | 1 |
| 6 | 3(2) | 9(1) | 右 | rm=2 | **1** | 2 |
| 7 | 3(2) | 7(3) | 左 | lm=2 | 0 | 2 |
| 8 | 4(1) | 7(3) | 左 | lm=2 | **1** | 3 |
| 9 | 5(0) | 7(3) | 左 | lm=2 | **2** | 5 |
| 10 | 6(1) | 7(3) | 左 | lm=2 | **1** | 6 |

`l` 与 `r` 相遇时结束，答案 `6`。**为什么只看较矮侧**：较矮侧的水位由「该侧已知最大值」决定，而另一侧一定不低于它，所以不会被另一侧限制。

---

## 三、滑动窗口

> 模板：右指针扩张 → 满足/违反条件时收缩左指针 → 更新答案。

### 3. 无重复字符的最长子串 · 中等

**思路**：`map` 记录字符「最近一次出现的下标」。窗口左边界取 `max(l, 上次位置 + 1)`，防止左边界回退。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> map = new HashMap<>();          // 字符 -> 最近出现的下标
        int l = 0, ans = 0;                                     // l: 窗口左边界
        for (int r = 0; r < s.length(); r++) {
            char c = s.charAt(r);
            if (map.containsKey(c))
                l = Math.max(l, map.get(c) + 1);                // 关键：取 max 防止左边界回退
            map.put(c, r);                                      // 记录/更新最近位置
            ans = Math.max(ans, r - l + 1);                     // 当前窗口长度
        }
        return ans;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(k)`（字符集大小）

**例子讲解**：`s = "abcabcbb"`

| r | 字符 | map 查询 | l | 更新后 map | 窗口 | 长度 |
| :-- | :-- | :-- | :-- | :-- | :-- | :-- |
| 0 | a | 未出现 | 0 | {a:0} | `a` | 1 |
| 1 | b | 未出现 | 0 | {a:0,b:1} | `ab` | 2 |
| 2 | c | 未出现 | 0 | {a:0,b:1,c:2} | `abc` | **3** |
| 3 | a | 位置 0 | `max(0,1)=1` | {a:3,...} | `bca` | 3 |
| 4 | b | 位置 1 | `max(1,2)=2` | {b:4,...} | `cab` | 3 |
| 5 | c | 位置 2 | `max(2,3)=3` | {c:5,...} | `abc` | 3 |
| 6 | b | 位置 4 | `max(3,5)=5` | {b:6,...} | `cb` | 2 |
| 7 | b | 位置 6 | `max(5,7)=7` | {b:7} | `b` | 1 |

答案 `3`。**`Math.max` 的意义**：`"abba"` 到第二个 `a` 时，若不取 max，`l` 会从 2 被拉回 1，导致长度算错。

---

### 438. 找到字符串中所有字母异位词 · 中等

**思路**：**固定长度窗口** + 26 位计数数组。窗口右移一格就加一个字符、减一个字符，`Arrays.equals` 判断是否同构。

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> res = new ArrayList<>();
        int n = s.length(), m = p.length();
        if (n < m) return res;                          // 窗口放不下，直接空
        int[] need = new int[26], win = new int[26];    // 目标计数 / 窗口计数
        for (char c : p.toCharArray()) need[c - 'a']++;
        for (int i = 0; i < n; i++) {
            win[s.charAt(i) - 'a']++;                   // 右端进窗
            if (i >= m) win[s.charAt(i - m) - 'a']--;   // 左端出窗，保持窗口长度 = m
            if (Arrays.equals(need, win)) res.add(i - m + 1);   // 计数完全相同即异位词
        }
        return res;
    }
}
```

**复杂度**：时间 `O(n)`（每次比较 26 个元素，常数级），空间 `O(1)`

**例子讲解**：`s = "cbaebabacd"`，`p = "abc"`（`need`：a=1, b=1, c=1）

| 窗口右端 i | 窗口内容 | 窗口计数 | 是否匹配 | 记录起点 |
| :-- | :-- | :-- | :-- | :-- |
| 2 | `cba` | a1 b1 c1 | ✅ | 0 |
| 3 | `bae` | a1 b1 e1 | ❌ | — |
| 4 | `aeb` | a1 b1 e1 | ❌ | — |
| 5 | `eba` | a1 b1 e1 | ❌ | — |
| 6 | `bab` | a1 b2 | ❌ | — |
| 7 | `aba` | a2 b1 | ❌ | — |
| 8 | `bac` | a1 b1 c1 | ✅ | 6 |
| 9 | `acd` | a1 c1 d1 | ❌ | — |

答案 `[0, 6]`。**关键**：窗口长度固定为 `m`，所以用「进一个、出一个」就能 `O(1)` 维护计数，不必每次重新统计整个窗口。

---

## 四、子串

### 560. 和为 K 的子数组 · 中等

**思路**：**前缀和 + 哈希**。`pre[j] - pre[i] == k` 即「以 j 结尾的合法子数组数」= `map` 中 `pre - k` 的出现次数。初始放 `map[0] = 1`（空前缀）。

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();     // 前缀和 -> 出现次数
        map.put(0, 1);                                   // 空前缀，覆盖「从 0 开始」的子数组
        int pre = 0, ans = 0;
        for (int x : nums) {
            pre += x;                                    // 累加前缀和
            ans += map.getOrDefault(pre - k, 0);         // 之前有多少个 pre-k，就有多少个合法子数组
            map.merge(pre, 1, Integer::sum);             // 记录当前前缀和
        }
        return ans;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(n)`

**例子讲解**：`nums = [1,1,1]`，`k = 2`

| 元素 | pre | 查 `pre-k` | 命中次数 | 累加 ans | map 更新 |
| :-- | :-- | :-- | :-- | :-- | :-- |
| — | 0 | — | — | 0 | {0:1} |
| 1 | 1 | -1 | 0 | 0 | {0:1, 1:1} |
| 1 | 2 | 0 | **1** | 1 | {0:1, 1:1, 2:1} |
| 1 | 3 | 1 | **1** | 2 | {0:1, 1:1, 2:1, 3:1} |

答案 `2`（`[1,1]` 前两个 + 后两个）。**`map[0]=1` 的作用**：当 `pre == k` 时说明「从头开始的整段」也合法，少了这一条会漏解。

---

### 239. 滑动窗口最大值 · 困难

**思路**：**单调递减双端队列**存下标。入队前弹掉队尾所有 `≤` 当前值的元素（它们永远不可能是最大值）；队首过期就弹出。队首即窗口最大值。

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] res = new int[n - k + 1];
        Deque<Integer> q = new ArrayDeque<>();                     // 存下标，值单调递减
        for (int i = 0; i < n; i++) {
            while (!q.isEmpty() && nums[q.peekLast()] <= nums[i])
                q.pollLast();                                      // 队尾比新元素小，永远没机会当最大值
            q.offerLast(i);                                        // 新元素入队
            if (q.peekFirst() <= i - k) q.pollFirst();             // 队首滑出窗口，弹出
            if (i >= k - 1) res[i - k + 1] = nums[q.peekFirst()];   // 窗口成型，记录最大值
        }
        return res;
    }
}
```

**复杂度**：时间 `O(n)`（每个下标最多进出队一次），空间 `O(k)`

**例子讲解**：`nums = [1,3,-1,-3,5,3,6,7]`，`k = 3`

| i | 值 | 弹出队尾（≤ 当前值） | 队列（下标:值） | 滑出窗口 | 窗口最大值 |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 0 | 1 | — | [0:1] | — | — |
| 1 | 3 | 弹 0 | [1:3] | — | — |
| 2 | -1 | — | [1:3, 2:-1] | — | **3** |
| 3 | -3 | — | [1:3, 2:-1, 3:-3] | — | **3** |
| 4 | 5 | 弹 3,2,1 | [4:5] | — | **5** |
| 5 | 3 | — | [4:5, 5:3] | — | **5** |
| 6 | 6 | 弹 5,4 | [6:6] | — | **6** |
| 7 | 7 | 弹 6 | [7:7] | — | **7** |

答案 `[3,3,5,5,6,7]`。**注意顺序**：先入队、再判队首过期、最后取最大值——先弹过期再入队也可以，但取最大值必须放在最后。

---

### 76. 最小覆盖子串 · 困难

**思路**：`need`/`win` 计数 + `matched` 记录「已满足的字符总数」。右指针扩到全满足后，左指针一直缩到刚好不满足，过程中更新最短答案。

```java
class Solution {
    public String minWindow(String s, String t) {
        int[] need = new int[128], win = new int[128];   // 128 覆盖 ASCII 全部字符
        for (char c : t.toCharArray()) need[c]++;
        int matched = 0, l = 0, start = 0, len = Integer.MAX_VALUE;
        for (int r = 0; r < s.length(); r++) {
            char c = s.charAt(r);
            win[c]++;
            if (need[c] > 0 && win[c] <= need[c]) matched++;   // 本字符的配额又被满足一个
            while (matched == t.length()) {                    // 已全覆盖，尝试收缩左边界
                if (r - l + 1 < len) { len = r - l + 1; start = l; }   // 记录更优解
                char d = s.charAt(l++);
                win[d]--;                                      // 左端出窗
                if (need[d] > 0 && win[d] < need[d]) matched--; // 配额不够了，退出内层循环
            }
        }
        return len == Integer.MAX_VALUE ? "" : s.substring(start, start + len);
    }
}
```

**复杂度**：时间 `O(n + m)`，空间 `O(1)`

**例子讲解**：`s = "ADOBECODEBANC"`，`t = "ABC"`（`need`：A=1, B=1, C=1）

下标：`0:A 1:D 2:O 3:B 4:E 5:C 6:O 7:D 8:E 9:B 10:A 11:N 12:C`

| 阶段 | 触发点 | 事件 | 当前最优解 |
| :-- | :-- | :-- | :-- |
| ① | `r=5`（`C`） | `matched` 首次到 3，窗口 `ADOBEC`（l=0..5） | `ADOBEC`，长 **6** |
| ② | 收缩 | 弹掉左端 `A` 后 `matched` 掉到 2，收缩停止 | 不变 |
| ③ | `r=12`（`C`） | 窗口 `ODEBANC`（l=6..12）第三次全覆盖，长 7 > 6 | 不更新 |
| ④ | 收缩到 l=8 | 窗口 `EBANC`，长 **5** | 更新为 `EBANC` |
| ⑤ | 收缩到 l=9 | 窗口 `BANC`，长 **4** | **更新为 `BANC`** ✅ |
| ⑥ | 再弹 `B` | `matched` 掉到 2，循环结束 | 最终答案 `BANC` |

答案 `"BANC"`。注意 ② 里为什么能一路收缩到 l=5 才停：被丢掉的 `D/O/E` 都不在 `t` 里，`matched` 不受影响，所以窗口能白捡地变小。

**记忆点**：`matched` 统计的是「**单位数**」而不是「字符种类」。如果 `t = "AAB"`（`need[A]=2`），扫到第一个 `A` 时 `matched++`、扫到第二个 `A` 时 `matched` 再 `++`，目标值始终是 `t.length()`。

---

## 五、普通数组

### 53. 最大子数组和 · 中等

**思路**：`cur = max(nums[i], cur + nums[i])`——前缀和为负就重新开始。滚动变量即可，无需数组。

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int cur = nums[0], ans = nums[0];      // cur: 以当前元素结尾的最大和
        for (int i = 1; i < nums.length; i++) {
            cur = Math.max(nums[i], cur + nums[i]);   // 前面拖后腿就自立门户
            ans = Math.max(ans, cur);                 // 全局最大
        }
        return ans;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`nums = [-2,1,-3,4,-1,2,1,-5,4]`（答案 6）

| i | 值 | `cur + x` | 新的 cur | 说明 | ans |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 0 | -2 | — | -2 | 起点 | -2 |
| 1 | 1 | -1 | **1** | 甩掉 -2 | 1 |
| 2 | -3 | -2 | **-2** | 保留 `1,-3` 比单 -3 好 | 1 |
| 3 | 4 | 2 | **4** | 甩掉负数前缀 | 4 |
| 4 | -1 | 3 | 3 | 继续 | 4 |
| 5 | 2 | 5 | 5 | 继续 | 5 |
| 6 | 1 | 6 | **6** | 继续 | **6** |
| 7 | -5 | 1 | 1 | 继续 | 6 |
| 8 | 4 | 5 | 5 | 继续 | 6 |

答案 `6`，对应子数组 `[4,-1,2,1]`。**注意初始化**：`cur` 和 `ans` 都要用 `nums[0]`，不能初始化成 0（否则全负数组会输出 0）。

---

### 56. 合并区间 · 中等

**思路**：按左端点排序。遍历时若「当前左端 ≤ 结果末尾右端」则合并（右端取 max），否则另起一段。

```java
class Solution {
    public int[][] merge(int[][] a) {
        Arrays.sort(a, (x, y) -> x[0] - y[0]);          // 按左端点升序
        List<int[]> res = new ArrayList<>();
        for (int[] x : a) {
            int[] last = res.isEmpty() ? null : res.get(res.size() - 1);
            if (last != null && x[0] <= last[1])        // 与上一段有重叠（或相接）
                last[1] = Math.max(last[1], x[1]);      // 右端点取更远的
            else res.add(x);                            // 不重叠，新开一段
        }
        return res.toArray(new int[0][]);
    }
}
```

**复杂度**：时间 `O(n log n)`，空间 `O(n)`

**例子讲解**：`intervals = [[1,3],[2,6],[8,10],[15,18]]`

- 排序后顺序不变
- `[1,3]`：`res` 空 → 直接加入 → `res = [[1,3]]`
- `[2,6]`：`2 ≤ 3` → 合并 → `res = [[1,6]]`
- `[8,10]`：`8 > 6` → 新段 → `res = [[1,6],[8,10]]`
- `[15,18]`：`15 > 10` → 新段 → `res = [[1,6],[8,10],[15,18]]`

答案 `[[1,6],[8,10],[15,18]]`。**为什么按左端排序就够**：排序后所有新区间的左端单调不减，只需比较左端与「已合并段的右端」即可判断重叠。

---

### 189. 轮转数组 · 中等

**思路**：**三次翻转**——整体翻转 → 翻转前 k 个 → 翻转后 n-k 个。

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        k %= n;                          // k 可能大于 n，先取模
        rev(nums, 0, n - 1);             // ① 整体翻转
        rev(nums, 0, k - 1);             // ② 前 k 个翻转
        rev(nums, k, n - 1);             // ③ 后 n-k 个翻转
    }
    void rev(int[] a, int i, int j) {    // 双指针原地反转 [i, j]
        while (i < j) { int t = a[i]; a[i++] = a[j]; a[j--] = t; }
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`nums = [1,2,3,4,5,6,7]`，`k = 3`

1. 整体翻转 → `[7,6,5,4,3,2,1]`
2. 前 3 个翻转 → `[5,6,7,4,3,2,1]`
3. 后 4 个翻转 → `[5,6,7,1,2,3,4]` ✅

答案 `[5,6,7,1,2,3,4]`。**为什么成立**：设前 n-k 个为 A、后 k 个为 B，目标是 BA。整体翻转得 `rev(B)rev(A)`，再分别翻回 `B A` ✅。别忘了 `k %= n`，否则 `k = n` 时 `rev(0, n-1)` 会对整段反向反而算错。

---

### 238. 除自身以外数组的乘积 · 中等

**思路**：答案 = **左侧前缀积 × 右侧后缀积**。先把前缀积写入 `res`，再从右往左用一个滚动变量乘上后缀积，满足「不用除法 + O(1) 额外空间」。

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] res = new int[n];
        res[0] = 1;                                     // 下标 0 左侧没有元素
        for (int i = 1; i < n; i++)
            res[i] = res[i - 1] * nums[i - 1];          // 第一趟：res[i] = 左侧全部乘积
        for (int i = n - 1, suf = 1; i >= 0; i--) {     // suf: 右侧全部乘积
            res[i] *= suf;                              // 左 × 右 即答案
            suf *= nums[i];                             // 更新后缀积，供左一个下用
        }
        return res;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`（输出数组不计）

**例子讲解**：`nums = [1,2,3,4]`

**第一趟（左侧前缀积）**：

| i | 计算 | res[i] |
| :-- | :-- | :-- |
| 0 | 初始 | 1 |
| 1 | `1 × 1` | 1 |
| 2 | `1 × 2` | 2 |
| 3 | `2 × 3` | 6 |

`res = [1,1,2,6]`

**第二趟（乘上右侧后缀积）**：

| i | suf（进来时） | `res[i] *= suf` | 新 suf |
| :-- | :-- | :-- | :-- |
| 3 | 1 | 6×1 = **6** | 4 |
| 2 | 4 | 2×4 = **8** | 12 |
| 1 | 12 | 1×12 = **12** | 24 |
| 0 | 24 | 1×24 = **24** | 96 |

答案 `[24,12,8,6]` ✅。**注意后缀积的更新时机**：必须在乘完 `res[i]` 之后再 `suf *= nums[i]`，否则会把自身也算进去。

---

### 41. 缺失的第一个正数 · 困难

**思路**：**原地哈希**。把值 `v`（`1 ≤ v ≤ n`）换到下标 `v-1`。第一轮归位后，第一个 `nums[i] != i+1` 的位置答案就是 `i+1`；全对则是 `n+1`。

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n; i++)
            // 值在 [1,n] 且在正确的位上 → 一直换到不能换为止
            while (nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i]) {
                int j = nums[i] - 1;                  // 目标下标
                int t = nums[i]; nums[i] = nums[j]; nums[j] = t;
            }
        for (int i = 0; i < n; i++)
            if (nums[i] != i + 1) return i + 1;       // 第一个对不上的位置
        return n + 1;                                 // 1..n 全都在
    }
}
```

**复杂度**：时间 `O(n)`（每次交换都让一个元素归位），空间 `O(1)`

**例子讲解**：`nums = [3,4,-1,1]`，`n = 4`

- `i=0`，`nums[0]=3` → 目标下标 2，交换 → `[-1,4,3,1]`
- `i=0`，`nums[0]=-1` 非法 → `while` 退出
- `i=1`，`nums[1]=4` → 目标下标 3，交换 → `[-1,1,3,4]`
- `i=1`，`nums[1]=1` → 目标下标 0，交换 → `[1,-1,3,4]`；`nums[1]=-1` 退出
- `i=2`，`nums[2]=3` 已在位（`nums[2] == 2+1`）→ `while` 不执行
- `i=3`，`nums[3]=4` 已在位

归位结果 `[1,-1,3,4]` → 扫描发现 `nums[1] = -1 != 2` → 答案 **2** ✅

**记忆点**：`while` 里用 `nums[nums[i]-1] != nums[i]` 而不是 `!= i`，是为了**重复元素不死循环**（如 `[1,1]`）。

---

## 六、矩阵

### 73. 矩阵置零 · 中等

**思路**：**用第一行/第一列当标记位**，两个布尔量单独记录首行/首列本身是否需要清零。省掉 `O(m+n)` 的额外空间。

```java
class Solution {
    public void setZeroes(int[][] m) {
        int R = m.length, C = m[0].length;
        boolean r0 = false, c0 = false;                  // 首行 / 首列自身是否要清
        for (int j = 0; j < C; j++) if (m[0][j] == 0) r0 = true;
        for (int i = 0; i < R; i++) if (m[i][0] == 0) c0 = true;
        for (int i = 1; i < R; i++)                      // 第一趟：只标记，不改数据
            for (int j = 1; j < C; j++)
                if (m[i][j] == 0) { m[i][0] = 0; m[0][j] = 0; }
        for (int i = 1; i < R; i++)                      // 第二趟：按标记清零
            for (int j = 1; j < C; j++)
                if (m[i][0] == 0 || m[0][j] == 0) m[i][j] = 0;
        if (r0) Arrays.fill(m[0], 0);                    // 最后处理首行首列，避免污染标记
        if (c0) for (int i = 0; i < R; i++) m[i][0] = 0;
    }
}
```

**复杂度**：时间 `O(mn)`，空间 `O(1)`

**例子讲解**：`matrix = [[1,1,1],[1,0,1],[1,1,1]]`

1. 首行无 0、首列无 0 → `r0 = c0 = false`
2. 标记趟：`(1,1)` 为 0 → `m[1][0] = 0`、`m[0][1] = 0`
   矩阵变成 `[[1,0,1],[0,0,1],[1,1,1]]`
3. 清零趟：`(1,1)` 因 `m[1][0]==0` 置 0；`(1,2)` 因 `m[1][0]==0` 置 0；`(0,1)` 不在范围内（从 1 开始）
   → `[[1,0,1],[0,0,0],[1,1,1]]`？再检查 `(2,1)`：`m[2][0]=1`、`m[0][1]=0` → 置 0 → `[[1,0,1],[0,0,0],[1,0,1]]`
4. `r0/c0` 都是 false，不动

答案 `[[1,0,1],[0,0,0],[1,0,1]]` ✅ **顺序很重要**：必须先标记、再清零，最后处理首行首列，否则标记位会被覆写。

---

### 54. 螺旋矩阵 · 中等

**思路**：维护上 `t`、下 `b`、左 `l`、右 `r` 四条边界，按 **右→下→左→上** 遍历，每走完一条边就收缩并判越界。

```java
class Solution {
    public List<Integer> spiralOrder(int[][] m) {
        List<Integer> res = new ArrayList<>();
        int t = 0, b = m.length - 1, l = 0, r = m[0].length - 1;
        while (true) {
            for (int j = l; j <= r; j++) res.add(m[t][j]);   // ① 右：遍历上边界
            if (++t > b) break;                              // 上边界下移，越界即结束
            for (int i = t; i <= b; i++) res.add(m[i][r]);   // ② 下：遍历右边界
            if (--r < l) break;
            for (int j = r; j >= l; j--) res.add(m[b][j]);   // ③ 左：遍历下边界
            if (--b < t) break;
            for (int i = b; i >= t; i--) res.add(m[i][l]);   // ④ 上：遍历左边界
            if (++l > r) break;
        }
        return res;
    }
}
```

**复杂度**：时间 `O(mn)`，空间 `O(1)`

**例子讲解**：`matrix = [[1,2,3],[4,5,6],[7,8,9]]`，`t=0,b=2,l=0,r=2`

| 轮次 | 动作 | 产出 | 边界变化 |
| :-- | :-- | :-- | :-- |
| 1 | ① 上边界向右 | `1,2,3` | `t=1` |
| 1 | ② 右边界向下 | `6,9` | `r=1` |
| 1 | ③ 下边界向左 | `8,7` | `b=1` |
| 1 | ④ 左边界向上（`i=1..1`） | `4` | `l=1` |
| 2 | ① 上边界向右（`j=1..1`） | `5` | `t=2 > b=1` → break |

答案 `[1,2,3,6,9,8,7,4,5]` ✅ **每条边后立刻判越界**是核心，否则单行/单列矩阵会重复输出。

---

### 48. 旋转图像 · 中等

**思路**：顺时针 90° = **先沿主对角线转置，再左右翻转每一行**。

```java
class Solution {
    public void rotate(int[][] m) {
        int n = m.length;
        for (int i = 0; i < n; i++)                       // ① 转置：交换 (i,j) 与 (j,i)
            for (int j = i + 1; j < n; j++) {             // j 从 i+1 起，避免换两次
                int t = m[i][j]; m[i][j] = m[j][i]; m[j][i] = t;
            }
        for (int[] row : m)                               // ② 每行左右翻转
            for (int i = 0, j = n - 1; i < j; i++, j--) {
                int t = row[i]; row[i] = row[j]; row[j] = t;
            }
    }
}
```

**复杂度**：时间 `O(n²)`，空间 `O(1)`

**例子讲解**：`matrix = [[1,2,3],[4,5,6],[7,8,9]]`

1. 转置 → `[[1,4,7],[2,5,8],[3,6,9]]`
2. 每行左右翻转 → `[[7,4,1],[8,5,2],[9,6,3]]` ✅

**验证**：原 `(0,1)=2` 旋转后应在 `(1,2)` → 结果 `(1,2)=2` ✅。**为什么不能整体翻转再转置**：转置 + 左右翻转 = 顺时针 90°；转置 + 上下翻转 = 逆时针 90°。顺序别记混。

---

### 240. 搜索二维矩阵 II · 中等

**思路**：从**右上角**出发——比 target 大就左移，比 target 小就下移，一次排除一整行/列。

```java
class Solution {
    public boolean searchMatrix(int[][] m, int target) {
        int i = 0, j = m[0].length - 1;              // 从右上角出发
        while (i < m.length && j >= 0) {
            if (m[i][j] == target) return true;
            if (m[i][j] > target) j--;               // 太大 → 这一列都不可能，左移
            else i++;                                // 太小 → 这一行都不可能，下移
        }
        return false;
    }
}
```

**复杂度**：时间 `O(m + n)`，空间 `O(1)`

**例子讲解**：`matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]]`，`target = 5`

- `(0,4)=15 > 5` → 左移 `j=3`
- `(0,3)=11 > 5` → `j=2`
- `(0,2)=7 > 5` → `j=1`
- `(0,1)=4 < 5` → 下移 `i=1`
- `(1,1)=5` → **true** ✅

**为什么选右上角**：它同时是「本行最大」和「本列最小」，所以两个方向上的判断都能**确定性地排除一整行或一整列**。左上角做不到这一点。

---

## 七、链表

> 两把万能钥匙：**虚拟头结点 `dummy`** 和 **快慢指针**。链表题一律先画图再写指针。

### 160. 相交链表 · 简单

**思路**：双指针各走 `a+c+b` 与 `b+c+a`，总路程相同 → 有交点必然相遇，无交点则同时停在 `null`。

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode h1, ListNode h2) {
        ListNode a = h1, b = h2;
        while (a != b) {                       // 两个指针走到同一个结点才停
            a = (a == null) ? h2 : a.next;     // a 走完自己的链就跳到对方链头
            b = (b == null) ? h1 : b.next;     // b 同理
        }
        return a;                              // 有交点返回交点，无交点返回 null
    }
}
```

**复杂度**：时间 `O(m + n)`，空间 `O(1)`

**例子讲解**：`A = 4→1→8→4→5`，`B = 5→6→1→8→4→5`，公共段从 `8` 开始（`a段=2, c=3, b段=3`）

| 步 | 指针 a（值） | 指针 b（值） |
| :-- | :-- | :-- |
| 1 | 4 | 5 |
| 2 | 1 | 6 |
| 3 | **8（交点在 A 上）** | 1 |
| 4 | 4 | **8** |
| 5 | 5 | 4 |
| 6 | `null` → 跳 B 头 `5` | 5 |
| 7 | 6 | `null` → 跳 A 头 `4` |
| 8 | 1 | 1 |
| 9 | **8** | **8** ✅ 相遇 |

两个指针都走了 `a + c + b = 8` 步。**无交点的情况**：各自走完 `m + n` 步后同时变成 `null`，`a != b` 为假，循环结束返回 `null`。

---

### 206. 反转链表 · 简单

**思路**：三指针迭代，逐个把 `cur.next` 指向 `pre`。

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null, cur = head;
        while (cur != null) {
            ListNode nxt = cur.next;   // ① 先存后继，否则断链后找不到
            cur.next = pre;            // ② 反转指针
            pre = cur;                 // ③ pre 前移
            cur = nxt;                 // ④ cur 前移
        }
        return pre;                    // cur 为 null 时 pre 就是新头
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`1 → 2 → 3 → null`

| 轮 | cur | nxt | cur.next 改成 | pre | cur（下一轮） |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 1 | 1 | 2 | null | 1 | 2 |
| 2 | 2 | 3 | 1 | 2 | 3 |
| 3 | 3 | null | 2 | 3 | null |

链表变成 `1←2←3`，`pre = 3`，返回 `3 → 2 → 1 → null` ✅
**顺序不能乱**：必须先存 `nxt`，否则第 ② 步 `cur.next = pre` 之后就再也找不到后面的结点了。

---

### 234. 回文链表 · 简单

**思路**：快慢指针找中点 → **反转后半段** → 与前半段逐个比较。进阶要求 `O(1)` 空间可用此法（还可再反转回去还原链表）。

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {   // 快慢指针，slow 停在「后半段起点」
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode pre = null;
        while (slow != null) {                        // 原地反转后半段
            ListNode n = slow.next;
            slow.next = pre;
            pre = slow;
            slow = n;
        }
        while (pre != null) {                         // 两段逐个比对
            if (pre.val != head.val) return false;
            pre = pre.next;
            head = head.next;
        }
        return true;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`1 → 2 → 2 → 1`

1. 找中点：`slow` 从 `1` 出发，
   - 轮 1：`slow=2`(第 2 个结点)，`fast=2`(第 3 个结点)
   - 轮 2：`slow=2`(第 3 个)，`fast=null` → 停
   - 所以后半段 = `2 → 1`
2. 反转后半段 → `1 → 2`
3. 比对：`1` vs `head(1)` ✅ → `2` vs `head(2)` ✅ → `pre` 为 `null` → **true**

若是 `1 → 2 → 3 → 1`：比对到第 2 个结点 `2` vs `3` ❌ → **false**。
**奇数长度也正确**：如 `1→2→1`，`slow` 停在第 3 个结点（`1`），反转后比较 `1` vs `1` ✅，中间的 `2` 天然不用比。

---

### 141. 环形链表 · 简单

**思路**：快慢指针，慢走 1 快走 2，有环必相遇。

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode s = head, f = head;
        while (f != null && f.next != null) {   // 快指针能走两步才继续
            s = s.next;                         // 慢走 1
            f = f.next.next;                    // 快走 2
            if (s == f) return true;            // 追上即说明有环
        }
        return false;                           // 快指针走到尽头，无环
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`3 → 2 → 0 → -4 →` 末尾指回 `2`（环长 3，入环点 `2`）

| 轮 | slow | fast |
| :-- | :-- | :-- |
| 0 | 3 | 3 |
| 1 | 2 | 0 |
| 2 | 0 | 2 |
| 3 | -4 | -4 ✅ 相遇 |

返回 `true`。**为什么必相遇**：进入环后快指针相对慢指针每轮缩短 1 个身位，环是有限的，所以一定会追上（不会跨过）。循环条件里 `f != null && f.next != null` 缺一不可，否则对 `f.next.next` 会空指针。

---

### 142. 环形链表 II · 中等

**思路**：相遇后让一个指针回到 `head`，两指针同速前进，**再次相遇处即入环点**（推导：`2(a+b) = a+b+n(b+c)` ⇒ `a = c + (n-1)(b+c)`）。

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode s = head, f = head;
        while (f != null && f.next != null) {
            s = s.next;
            f = f.next.next;
            if (s == f) {                        // 找到相遇点
                s = head;                        // 一个指针回到起点
                while (s != f) {                 // 同速前进
                    s = s.next;
                    f = f.next;
                }
                return s;                        // 再次相遇即入环点
            }
        }
        return null;                             // 无环
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`3 → 2 → 0 → -4 →` 末尾指回 `2`

- 相遇点（同上例）：`-4`
- 令 `s = head = 3`，`f = -4`，然后同速走：
  - 轮 1：`s = 2`，`f = 2` → **相等，返回 `2`** ✅

**为什么成立**：设头到入环点距离 `a`，入环点到相遇点距离 `b`，相遇点绕回入环点距离 `c`。
快指针走了 `a + b + n(b+c)`，慢指针走了 `a + b`，而快 = 慢的 2 倍 → `a + b = n(b+c)` → `a = c + (n-1)(b+c)`。
也就是说：**从头走 `a` 步**和**从相遇点走 `c` 步（绕整数圈）**会落在同一点，即入环点。

---

### 21. 合并两个有序链表 · 简单

**思路**：`dummy` + 双指针，每次接较小的那个；循环结束把剩下的整段直接接上。

```java
class Solution {
    public ListNode mergeTwoLists(ListNode a, ListNode b) {
        ListNode d = new ListNode(0), p = d;      // d 是虚拟头，p 是尾指针
        while (a != null && b != null) {
            if (a.val <= b.val) { p.next = a; a = a.next; }   // 接 a
            else { p.next = b; b = b.next; }                  // 接 b
            p = p.next;                                       // 尾指针前移
        }
        p.next = (a == null) ? b : a;             // 谁没空就把整段挂上
        return d.next;                            // 虚拟头的下一个才是真头
    }
}
```

**复杂度**：时间 `O(m + n)`，空间 `O(1)`

**例子讲解**：`l1 = 1→2→4`，`l2 = 1→3→4`

| 轮 | a | b | 比较 | 结果链 | p |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 1 | 1 | 1 | `1<=1` 取 a | `1` | 1 |
| 2 | 2 | 1 | `2>1` 取 b | `1→1` | 1(b) |
| 3 | 2 | 3 | `2<=3` 取 a | `1→1→2` | 2 |
| 4 | 4 | 3 | `4>3` 取 b | `1→1→2→3` | 3(b) |
| 5 | 4 | 4 | `4<=4` 取 a | `1→1→2→3→4` | 4(a) |
| — | null | 4 | 退出循环 | 挂上剩余 b | — |

答案 `1→1→2→3→4→4` ✅。**为什么用 `dummy`**：第一个结点由谁充当是动态的，有了 `dummy` 就不需要特判「结果链为空」。

---

### 2. 两数相加 · 中等

**思路**：同步遍历，维护进位 `carry`；循环条件是 `l1 != null || l2 != null || carry != 0`，一次搞定长度不等和末位进位。

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode d = new ListNode(0), p = d;
        int carry = 0;                                      // 进位
        while (l1 != null || l2 != null || carry != 0) {    // 三个条件任一成立都要继续
            int s = carry;                                  // 本轮和，先带上进位
            if (l1 != null) { s += l1.val; l1 = l1.next; }
            if (l2 != null) { s += l2.val; l2 = l2.next; }
            carry = s / 10;                                 // 新的进位
            p.next = new ListNode(s % 10);                  // 本位数字
            p = p.next;
        }
        return d.next;
    }
}
```

**复杂度**：时间 `O(max(m, n))`，空间 `O(1)`

**例子讲解**：`l1 = 2→4→3`（表示 342），`l2 = 5→6→4`（表示 465），期望 807

| 轮 | l1 | l2 | s（含进位） | 本位 | 新 carry |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 1 | 2 | 5 | `0+2+5 = 7` | 7 | 0 |
| 2 | 4 | 6 | `0+4+6 = 10` | **0** | **1** |
| 3 | 3 | 4 | `1+3+4 = 8` | 8 | 0 |
| — | null | null | carry=0 | 退出 | — |

答案 `7→0→8` = 807 ✅
**为什么循环条件要带 `carry != 0`**：像 `5 + 5 = 10` 这种两条链同时走完但还有进位的情况，少了这个条件就会漏掉最高位。

---

### 19. 删除链表的倒数第 N 个结点 · 中等

**思路**：`dummy` + 快慢指针。快指针先走 `n+1` 步，然后一起走到快指针为空，慢指针正好在待删结点的前一个。

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode d = new ListNode(0, head), f = d, s = d;   // 都从虚拟头出发
        for (int i = 0; i <= n; i++) f = f.next;            // 快指针先走 n+1 步
        while (f != null) {                                  // 一起走到快指针出界
            f = f.next;
            s = s.next;
        }
        s.next = s.next.next;                                // 跳过待删结点
        return d.next;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`head = 1→2→3→4→5`，`n = 2`（删掉 4），加虚拟头后为 `d→1→2→3→4→5`

1. 快指针先走 `n+1 = 3` 步：`d → 1 → 2 → 3`（落在结点 `3`）
2. 一起走到 `f == null`：

| 轮 | f | s |
| :-- | :-- | :-- |
| 起 | 3 | d |
| 1 | 4 | 1 |
| 2 | 5 | 2 |
| 3 | null | 3 |

3. `s` 停在结点 `3`，`s.next = s.next.next` 跳过 `4`

答案 `1→2→3→5` ✅
**为什么快指针走 `n+1` 而不是 `n` 步**：我们要让 `s` 停在待删结点的**前一个**（这样才能改指针）。走 `n+1` 步正好差出这个身位。也正因为如此，必须用 `dummy`——删头结点时 `s` 需要停在一个真实存在的「前驱」上。

---

### 24. 两两交换链表中的节点 · 中等

**思路**：`dummy` + `pre` 指针，每轮把 `pre` 后面的两结点交换并接回；`pre` 前进到交换后的第二个结点。

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode d = new ListNode(0, head), pre = d;
        while (pre.next != null && pre.next.next != null) {   // 至少有两个结点才交换
            ListNode a = pre.next, b = a.next;
            a.next = b.next;      // ① a 跳过 b，指向 b 的后继
            b.next = a;           // ② b 指向 a
            pre.next = b;         // ③ 前驱指向新的第一个结点 b
            pre = a;              // ④ pre 移到本轮的第二位（a），准备下一轮
        }
        return d.next;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`1 → 2 → 3 → 4`

**第 1 轮**：`pre = d`，`a = 1`，`b = 2`
- ① `1.next = 2.next = 3`
- ② `2.next = 1`
- ③ `d.next = 2`
- ④ `pre = 1`
- 链表：`d → 2 → 1 → 3 → 4`

**第 2 轮**：`a = 3`，`b = 4`
- ① `3.next = null`
- ② `4.next = 3`
- ③ `pre(=1).next = 4`
- ④ `pre = 3`
- 链表：`d → 2 → 1 → 4 → 3`

**第 3 轮**：`pre.next = 3`，`pre.next.next = null` → 退出

答案 `2→1→4→3` ✅
**三步赋值顺序要记牢**：`a.next` → `b.next` → `pre.next`，从后往前接，中间不会断链。

---

### 25. K 个一组翻转链表 · 困难

**思路**：先探路判断剩余是否够 `k` 个；够就**翻转这一段（标准的 pre/cur 指针反转）**，再把段前 `pre` 接到新头、段尾 `start` 接到下一组头。

```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode d = new ListNode(0, head), pre = d;
        while (true) {
            ListNode end = pre;
            for (int i = 0; i < k && end != null; i++) end = end.next;
            if (end == null) break;                 // 剩余不足 k 个，保持原样，结束
            ListNode start = pre.next, cur = start, prev = null;
            for (int i = 0; i < k; i++) {           // 组内指针反转（同 206 题）
                ListNode t = cur.next;
                cur.next = prev;
                prev = cur;
                cur = t;
            }
            pre.next = prev;                        // 段前接新头
            start.next = cur;                       // 段尾接下一组
            pre = start;                            // pre 走到本段尾，准备下一组
        }
        return d.next;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`1→2→3→4→5`，`k = 2`

**第 1 组**：`pre = d`，探路 `end` 先走 2 步到 `2`（非 null，够）
- `start = 1`，反转 `1→2` 得 `2→1`，`cur` 停在 `3`
- `pre.next = 2`（`d → 2 → 1`），`start(1).next = 3` → `d → 2 → 1 → 3 → 4 → 5`
- `pre = 1`

**第 2 组**：探路 `end` 从 `1` 走 2 步到 `4`（够）
- `start = 3`，反转 `3→4` 得 `4→3`，`cur` 停在 `5`
- `pre(1).next = 4`，`start(3).next = 5` → `d → 2 → 1 → 4 → 3 → 5`
- `pre = 3`

**第 3 组**：探路 `end` 从 `3` 走 2 步 → `5` 再走一步变 `null` → **break**

答案 `2→1→4→3→5` ✅
**两个易错点**：① 探路要用临时指针，不能移动 `pre`；② 记住 `start` 在反转后是**段尾**，它必须指向下一组的头，否则整条链会断。

---

### 138. 随机链表的复制 · 中等

**思路**：哈希表存 `原结点 → 新结点`。第一趟只建点，第二趟连 `next` 和 `random`（`map.get(null)` 自然返回 `null`，省掉判空）。

```java
class Solution {
    public Node copyRandomList(Node head) {
        Map<Node, Node> map = new HashMap<>();                  // 原结点 -> 新结点
        for (Node p = head; p != null; p = p.next)
            map.put(p, new Node(p.val));                        // ① 先只建点，不连指针
        for (Node p = head; p != null; p = p.next) {
            map.get(p).next = map.get(p.next);                  // ② 连 next（null 也能安全映射）
            map.get(p).random = map.get(p.random);              // ③ 连 random
        }
        return map.get(head);
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(n)`
> `O(1)` 空间写法：把副本结点插到原结点后面（`A→A'→B→B'`），借助 `p.next.random = p.random.next` 连随机指针，最后拆成两条链。

**例子讲解**：输入 `[[7,null], [13,0], [11,4], [10,2], [1,0]]`（每个结点是 `[值, random指向的下标]`）

**第一趟**：依次创建 `7'、13'、11'、10'、1'`，`map = {7→7', 13→13', 11→11', 10→10', 1→1'}`

**第二趟**：

| 原结点 | `next` 映射 | `random` 映射 |
| :-- | :-- | :-- |
| 7 | `7'.next = 13'` | `random = null`（下标 null） |
| 13 | `13'.next = 11'` | `random = 7'`（下标 0） |
| 11 | `11'.next = 10'` | `random = 1'`（下标 4） |
| 10 | `10'.next = 1'` | `random = 11'`（下标 2） |
| 1 | `1'.next = null` | `random = 7'`（下标 0） |

返回 `map.get(head) = 7'`，得到一条完全独立的新链 ✅
**关键技巧**：`map.get(null)` 返回 `null`，把「random 为空」和「next 到末尾」两种情况统一处理，代码省掉一堆 `if`。
**为什么不能只建点再连**：`random` 可能指向**还没创建**的结点，所以必须先建完所有点，再统一连指针。

---

### 148. 排序链表 · 中等

**思路**：**归并排序**。快慢指针找中点断开，递归排两半，再合并。（也可自底向上合并实现 `O(1)` 空间。）

```java
class Solution {
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null) return head;   // 0/1 个结点天然有序
        ListNode s = head, f = head.next;                     // f 从 head.next 起，保证 s 停在中点左侧
        while (f != null && f.next != null) {
            s = s.next;
            f = f.next.next;
        }
        ListNode mid = s.next;
        s.next = null;                                        // 从中间断开
        return merge(sortList(head), sortList(mid));          // 递归排 + 合并
    }
    ListNode merge(ListNode a, ListNode b) {                  // 同 21 题
        ListNode d = new ListNode(0), p = d;
        while (a != null && b != null) {
            if (a.val <= b.val) { p.next = a; a = a.next; }
            else { p.next = b; b = b.next; }
            p = p.next;
        }
        p.next = (a != null) ? a : b;
        return d.next;
    }
}
```

**复杂度**：时间 `O(n log n)`，空间 `O(log n)`

**例子讲解**：`4 → 2 → 1 → 3`

**分割**（快指针从 `head.next` 出发，避免偶数长度时切偏）：
- 整条链：`s` 最终停在 `2`，`mid = 1` → 断成 `4→2` 和 `1→3`
- `4→2`：`s` 停在 `4`，`mid = 2` → 断成 `4` 和 `2`
- `1→3`：断成 `1` 和 `3`

**归并回溯**：
- `merge(4, 2)` → `2→4`
- `merge(1, 3)` → `1→3`
- `merge(2→4, 1→3)` → `1→2→3→4` ✅

**为什么 `f` 从 `head.next` 开始**：若 `f` 也从 `head` 起，对 `4→2` 这种两个结点的链，`s` 会走到 `2`，导致「左半为空、右半是自己」的无限递归。从 `head.next` 起能保证 `s` 落在**左中位**。

---

### 23. 合并 K 个升序链表 · 困难

**思路**：**小顶堆**存各链表当前头结点，每次弹出最小值接到结果尾部并补入它的后继。（另一种：两两分治归并，`O(N log k)`。）

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        PriorityQueue<ListNode> pq = new PriorityQueue<>((a, b) -> a.val - b.val);   // 小顶堆
        for (ListNode l : lists) if (l != null) pq.offer(l);                        // 各链头入堆
        ListNode d = new ListNode(0), p = d;
        while (!pq.isEmpty()) {
            ListNode cur = pq.poll();      // 当前全局最小
            p.next = cur;
            p = p.next;
            if (cur.next != null) pq.offer(cur.next);   // 从同一条链补上下一个
        }
        return d.next;
    }
}
```

**复杂度**：时间 `O(N log k)`（N 为总结点数，k 为链数），空间 `O(k)`

**例子讲解**：`lists = [[1,4,5], [1,3,4], [2,6]]`

初始堆：`{1(链0), 1(链1), 2(链2)}`

| 轮 | 弹出 | 结果链 | 补入堆 |
| :-- | :-- | :-- | :-- |
| 1 | 1(链0) | 1 | 4(链0) |
| 2 | 1(链1) | 1→1 | 3(链1) |
| 3 | 2(链2) | 1→1→2 | 6(链2) |
| 4 | 3(链1) | …→3 | 4(链1) |
| 5 | 4(链0) | …→4 | 5(链0) |
| 6 | 4(链1) | …→4 | null（不补） |
| 7 | 5(链0) | …→5 | null |
| 8 | 6(链2) | …→6 | null |

答案 `1→1→2→3→4→4→5→6` ✅
**为什么堆里始终只有 k 个元素**：每次弹一个才补一个，且都是从**同一条链**补，链内本身有序，所以补进来的必然不小于刚弹出的。

---

### 146. LRU 缓存 · 中等

**思路**：**哈希表 + 双向链表**。哈希做到 `O(1)` 定位，双向链表维护「最近使用」顺序——新访问的移到头部，超容量就删尾结点。

```java
class LRUCache {
    class Node {
        int k, v; Node pre, nxt;
        Node(int k, int v) { this.k = k; this.v = v; }
    }
    Map<Integer, Node> map = new HashMap<>();            // key -> 结点，O(1) 定位
    Node head = new Node(0, 0), tail = new Node(0, 0);   // 两个哨兵，省掉边界特判
    int cap;

    public LRUCache(int capacity) {
        cap = capacity;
        head.nxt = tail;                                 // 初始化空链表
        tail.pre = head;
    }
    public int get(int key) {
        Node n = map.get(key);
        if (n == null) return -1;
        moveToHead(n);                                   // 访问过 = 最近使用
        return n.v;
    }
    public void put(int key, int value) {
        Node n = map.get(key);
        if (n != null) { n.v = value; moveToHead(n); return; }   // 已存在：更新 + 提到头部
        if (map.size() == cap) {                                  // 满了：删最久未用（尾部）
            map.remove(tail.pre.k);
            remove(tail.pre);
        }
        n = new Node(key, value);
        map.put(key, n);
        addHead(n);                                               // 新结点放头部
    }
    void addHead(Node n) {                               // 头插
        n.nxt = head.nxt; n.pre = head;
        head.nxt.pre = n; head.nxt = n;
    }
    void remove(Node n) {                                // 摘链
        n.pre.nxt = n.nxt; n.nxt.pre = n.pre;
    }
    void moveToHead(Node n) { remove(n); addHead(n); }    // 先摘后插
}
```

**复杂度**：`get`/`put` 时间 `O(1)`，空间 `O(capacity)`

**例子讲解**：`capacity = 2`，操作序列 `put(1,1) → put(2,2) → get(1) → put(3,3) → get(2) → put(4,4) → get(1) → get(3) → get(4)`

| 操作 | 说明 | 链表（头=最近） | map | 返回 |
| :-- | :-- | :-- | :-- | :-- |
| `put(1,1)` | 头插 1 | `1` | {1} | — |
| `put(2,2)` | 头插 2 | `2 → 1` | {1,2} | — |
| `get(1)` | 命中，提到头 | `1 → 2` | {1,2} | **1** |
| `put(3,3)` | 满了，删尾 `2`，头插 3 | `3 → 1` | {1,3} | — |
| `get(2)` | 未命中 | `3 → 1` | {1,3} | **-1** |
| `put(4,4)` | 满了，删尾 `1`，头插 4 | `4 → 3` | {3,4} | — |
| `get(1)` | 未命中 | `4 → 3` | {3,4} | **-1** |
| `get(3)` | 命中，提到头 | `3 → 4` | {3,4} | **3** |
| `get(4)` | 命中，提到头 | `4 → 3` | {3,4} | **4** |

**两个哨兵的意义**：`head`/`tail` 让「删尾结点」和「头插」都不需要判断链表是否为空，`moveToHead` 也不用特判「结点本来就在头」。少了哨兵，代码要多出 3~4 个 `if`。

---

## 八、二叉树

> 递归三问：**递归函数做什么？终止条件是什么？单层逻辑是什么？**
> 需要「从左右子树取信息回传」时用后序；需要「带着状态往下走」时用带参 DFS。

### 94. 二叉树的中序遍历 · 简单

**思路**：递归是 左 → 根 → 右。**迭代模板**：一路向左压栈，弹出即访问，再转向右子树。

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Deque<TreeNode> st = new ArrayDeque<>();
        while (root != null || !st.isEmpty()) {
            while (root != null) {         // 一路向左压栈
                st.push(root);
                root = root.left;
            }
            root = st.pop();               // 弹出即访问（此时左子树已处理完）
            res.add(root.val);
            root = root.right;             // 转向右子树，重复上述过程
        }
        return res;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(h)`

**例子讲解**：`root = [1, null, 2, 3]`，即 `1` 的右孩子是 `2`，`2` 的左孩子是 `3`

| 步 | 操作 | 栈 | res |
| :-- | :-- | :-- | :-- |
| 1 | 压 1，转 1.left=null | [1] | [] |
| 2 | 弹 1 → 访问 | [] | [1] |
| 3 | 转 1.right=2，压 2，转 2.left=3 | [2] | [1] |
| 4 | 压 3，转 3.left=null | [2,3] | [1] |
| 5 | 弹 3 → 访问 | [2] | [1,3] |
| 6 | 转 3.right=null，弹 2 → 访问 | [] | [1,3,2] |
| 7 | 转 2.right=null，两者皆空 → 结束 | [] | [1,3,2] |

答案 `[1,3,2]` ✅
**记忆点**：`while (root != null || !st.isEmpty())` 双条件缺一不可——`root != null` 负责「继续向左探」，`!st.isEmpty()` 负责「还有祖先没访问」。

---

### 104. 二叉树的最大深度 · 简单

**思路**：后序，深度 = `max(左, 右) + 1`。

```java
class Solution {
    public int maxDepth(TreeNode root) {
        // 空结点深度为 0；否则左右取大的加 1（算上自己这层）
        return root == null ? 0 : Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(h)`

**例子讲解**：`root = [3, 9, 20, null, null, 15, 7]`

```
        3
      /   \
     9     20
          /  \
        15    7
```

自底向上算：
- `15`、`7` 是叶子 → `depth = 1`
- `20` → `max(1, 1) + 1 = 2`
- `9` 是叶子 → `1`
- `3` → `max(1, 2) + 1 = 3` ✅

答案 `3`。**注意**：叶子结点返回 1（不是 0），所以空结点必须返回 0，否则整棵树的深度会少 1。

---

### 226. 翻转二叉树 · 简单

**思路**：交换左右孩子，再递归翻转（前序/后序都行）。

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) return null;
        TreeNode t = root.left;                              // 暂存左孩子
        root.left = invertTree(root.right);                  // 翻转后的右子树成为新左
        root.right = invertTree(t);                          // 翻转后的左子树成为新右
        return root;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(h)`

**例子讲解**：`root = [4, 2, 7, 1, 3, 6, 9]`

```
原树                翻转后
    4                   4
   / \                 / \
  2   7               7   2
 / \ / \             / \ / \
1  3 6  9           9  6 3  1
```

递归过程：
- `4`：交换左右 → 左变 `invert(7)`，右变 `invert(2)`
- `invert(7)` → `7` 的左变 `invert(9)=9`，右变 `invert(6)=6` → `7(9,6)`
- `invert(2)` → `2` 的左变 `invert(3)=3`，右变 `invert(1)=1` → `2(3,1)`

答案 `[4, 7, 2, 9, 6, 3, 1]` ✅
**易错点**：不要写成 `root.left = invertTree(root.right); root.right = invertTree(root.left);` —— 第一步已经改掉了 `root.left`，第二步会把「新的左子树」再翻一遍。所以必须先用 `t` 暂存。

---

### 101. 对称二叉树 · 简单

**思路**：写辅助函数判断**两棵子树是否互为镜像**——值相等，且 `a.left` 与 `b.right`、`a.right` 与 `b.left` 分别镜像。

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return root == null || check(root.left, root.right);   // 空树也算对称
    }
    boolean check(TreeNode a, TreeNode b) {
        if (a == null || b == null) return a == b;             // 都空 → true；一空一不空 → false
        return a.val == b.val
                && check(a.left, b.right)                       // 外侧配对
                && check(a.right, b.left);                      // 内侧配对
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(h)`

**例子讲解**：`root = [1, 2, 2, 3, 4, 4, 3]`（对称）

```
      1
    /   \
   2     2
  / \   / \
 3   4 4   3
```

- `check(2左, 2右)`：值相等 ✓
  - 外侧：`check(3, 3)` ✓
  - 内侧：`check(4, 4)` ✓
- 全部通过 → **true**

反例 `[1, 2, 2, null, 3, null, 3]`：`check(2左, 2右)` 时，左 `2` 的右孩子是 `3`，右 `2` 的左孩子是 `null` → `check(3, null)` 返回 `false` ❌。
**核心**：对称 = **外侧对内侧**，写成 `check(a.left, b.left)` 就变成「两棵一样的树」而不是镜像了。

---

### 543. 二叉树的直径 · 简单

**思路**：直径 = 某个结点的「左深度 + 右深度」的最大值，**不一定经过根**。所以要在后序求深度的同时顺手更新全局答案。

```java
class Solution {
    int ans = 0;                                             // 全局最大直径（边数）
    public int diameterOfBinaryTree(TreeNode root) { depth(root); return ans; }
    int depth(TreeNode n) {
        if (n == null) return 0;
        int l = depth(n.left), r = depth(n.right);           // 先拿到左右深度
        ans = Math.max(ans, l + r);                          // 以 n 为「拐点」的路径长度
        return Math.max(l, r) + 1;                           // 向上返回单边最大深度
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(h)`

**例子讲解**：`root = [1, 2, 3, 4, 5]`

```
      1
     / \
    2   3
   / \
  4   5
```

自底向上：

| 结点 | 左深度 l | 右深度 r | `ans = max(ans, l+r)` | 返回 |
| :-- | :-- | :-- | :-- | :-- |
| 4 | 0 | 0 | 0 | 1 |
| 5 | 0 | 0 | 0 | 1 |
| 2 | 1 | 1 | **2** | 2 |
| 3 | 0 | 0 | 2 | 1 |
| 1 | 2 | 1 | **3** ✅ | 3 |

答案 `3`（路径 `4→2→1→3` 或 `5→2→1→3`，3 条边）✅
**为什么不能只在根上算 `左深+右深`**：若最长路径完全落在左子树内部（如 `[1,2,3,4,5,...]` 全在左侧），根处的 `l+r` 会偏小。所以必须在**每个结点**都试一次「拐点」。

---

### 102. 二叉树的层序遍历 · 中等

**思路**：队列 BFS，**每轮先记下当前层结点数 `sz`**，就能天然分层。

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) return res;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while (!q.isEmpty()) {
            int sz = q.size();                 // 关键：先冻结本层结点数
            List<Integer> level = new ArrayList<>();
            while (sz-- > 0) {                 // 只处理本层的这 sz 个
                TreeNode n = q.poll();
                level.add(n.val);
                if (n.left != null) q.offer(n.left);    // 下一层结点入队
                if (n.right != null) q.offer(n.right);
            }
            res.add(level);                    // 本层收集完毕
        }
        return res;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(n)`

**例子讲解**：`root = [3, 9, 20, null, null, 15, 7]`

| 轮 | 队首时的队列 | sz | 本层 | 入队的下一层 |
| :-- | :-- | :-- | :-- | :-- |
| 1 | [3] | 1 | [3] | 9, 20 |
| 2 | [9, 20] | 2 | [9, 20] | 15, 7 |
| 3 | [15, 7] | 2 | [15, 7] | 无 |

答案 `[[3],[9,20],[15,7]]` ✅
**`sz` 的作用**：如果直接用 `while (!q.isEmpty())` 当内层循环，就会把下一层的结点也一起处理掉，无法分层。

---

### 108. 将有序数组转换为二叉搜索树 · 简单

**思路**：**取中点作根**，左右区间递归建树 → 天然高度平衡。

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) { return build(nums, 0, nums.length - 1); }
    TreeNode build(int[] a, int l, int r) {
        if (l > r) return null;              // 区间为空
        int m = (l + r) >>> 1;               // 取中点作根（lower mid）
        TreeNode n = new TreeNode(a[m]);
        n.left = build(a, l, m - 1);         // 左半区间建左子树
        n.right = build(a, m + 1, r);        // 右半区间建右子树
        return n;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(log n)`

**例子讲解**：`nums = [-10, -3, 0, 5, 9]`（下标 0~4）

- `build(0,4)`：`m=2` → 根 `0`；左 `build(0,1)`，右 `build(3,4)`
- `build(0,1)`：`m=0` → 根 `-10`；右 `build(1,1)` → `-3`
- `build(3,4)`：`m=3` → 根 `5`；右 `build(4,4)` → `9`

```
        0
      /   \
   -10     5
      \      \
      -3       9
```

答案（层序）`[0, -10, 5, null, -3, null, 9]` ✅ 中序输出仍是 `[-10,-3,0,5,9]`，且高度差 ≤ 1。
**为什么取中点就平衡**：每次左右区间长度最多差 1，递归下去高度必然是 `O(log n)`。`(l+r)>>>1` 与 `(l+r)/2` 等价，但能避免 `l+r` 溢出。

---

### 98. 验证二叉搜索树 · 中等

**思路**：**中序遍历结果必须严格递增**（用一个成员变量记前驱值即可，不用数组）。另一写法是递归传上下界 `(low, high)`。

```java
class Solution {
    long pre = Long.MIN_VALUE;                 // 前驱值，用 long 避免 int 最小值误判
    public boolean isValidBST(TreeNode root) {
        if (root == null) return true;
        if (!isValidBST(root.left)) return false;      // 左子树必须合法
        if (root.val <= pre) return false;             // 必须严格大于前驱
        pre = root.val;                                // 更新前驱
        return isValidBST(root.right);                 // 右子树必须合法
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(h)`

**例子讲解**：`root = [5, 1, 4, null, null, 3, 6]`

```
      5
     / \
    1   4
       / \
      3   6
```

中序遍历：`1, 5, 3, 4, 6`

| 访问 | 值 | `pre` | 判断 |
| :-- | :-- | :-- | :-- |
| 1 | 1 | Long.MIN | `1 > MIN` ✓，pre=1 |
| 2 | 5 | 1 | `5 > 1` ✓，pre=5 |
| 3 | 3 | 5 | `3 <= 5` ❌ → **返回 false** |

答案 `false` ✅（结点 `3` 在 `5` 的右子树里却更小）
**为什么不能只比较「父 > 左 && 父 < 右」**：BST 要求的是**与整棵子树的所有结点**比较，不是只跟父结点比。`[5,1,4,null,null,3,6]` 中 `4 > 5` 的局部判断看不出问题，但 3 落在 5 的右子树就违规了。用中序递增或上下界才严谨。

---

### 230. 二叉搜索树中第 K 小的元素 · 中等

**思路**：中序遍历即升序，数到第 `k` 个就返回。数据频繁增删的场景可改成「维护每个结点的子树大小」做 `O(h)` 查询。

```java
class Solution {
    int k, ans;
    public int kthSmallest(TreeNode root, int k) { this.k = k; dfs(root); return ans; }
    void dfs(TreeNode n) {
        if (n == null || k <= 0) return;       // k<=0 说明已找到，直接剪枝返回
        dfs(n.left);                           // 左：先访问更小的
        if (--k == 0) { ans = n.val; return; } // 数到第 k 个，记录并停止
        dfs(n.right);
    }
}
```

**复杂度**：时间 `O(k)`（找到就停），空间 `O(h)`

**例子讲解**：`root = [3, 1, 4, null, 2]`，`k = 1`

```
      3
     / \
    1   4
     \
      2
```

中序访问顺序：`1 → 2 → 3 → 4`

| 访问顺序 | 值 | `--k` 后 | 判断 |
| :-- | :-- | :-- | :-- |
| 1 | 1 | 0 | **命中** → `ans = 1`，return |

答案 `1` ✅（若 `k = 3`，走到结点 `3` 时命中，答案 `3`）
**`k <= 0` 剪枝的价值**：命中后父层的 `dfs(n.right)` 会立刻返回，不用遍历剩余子树，实际是 `O(k + h)`。

---

### 199. 二叉树的右视图 · 中等

**思路**：**先右后左**的 DFS + 深度首次到达判断：`depth == res.size()` 说明这一层还没被记录过，当前就是该层最右结点。

```java
class Solution {
    List<Integer> res = new ArrayList<>();
    public List<Integer> rightSideView(TreeNode root) { dfs(root, 0); return res; }
    void dfs(TreeNode n, int d) {
        if (n == null) return;
        if (d == res.size()) res.add(n.val);   // 该深度第一次被访问 → 必是最右结点
        dfs(n.right, d + 1);                   // 先右：保证最右结点最先到
        dfs(n.left, d + 1);                    // 后左：同层的其他结点会被上面的条件挡住
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(h)`

**例子讲解**：`root = [1, 2, 3, null, 5, null, 4]`

```
        1
      /   \
     2     3
      \     \
       5     4
```

遍历顺序（先右后左）：`1(d=0) → 3(d=1) → 4(d=2) → 2(d=1) → 5(d=2)`

| 结点 | 深度 d | `res.size()` | 动作 |
| :-- | :-- | :-- | :-- |
| 1 | 0 | 0 | `0 == 0` → 加入 → res=[1] |
| 3 | 1 | 1 | `1 == 1` → 加入 → res=[1,3] |
| 4 | 2 | 2 | `2 == 2` → 加入 → res=[1,3,4] |
| 2 | 1 | 3 | `1 != 3` → 跳过 |
| 5 | 2 | 3 | `2 != 3` → 跳过 |

答案 `[1,3,4]` ✅
**为什么「先右后左」是关键**：如果先走左边，深度 1 会先被左节点 `2` 占掉，右视图就错了。

---

### 114. 二叉树展开为链表 · 中等

**思路**：**找左子树最右结点（前驱）**，把当前结点的右子树接到前驱的右边，再把左子树整体搬到右边，然后继续往右走。全程 `O(1)` 空间。

```java
class Solution {
    public void flatten(TreeNode root) {
        while (root != null) {
            if (root.left != null) {                   // 有左子树才需要处理
                TreeNode p = root.left;
                while (p.right != null) p = p.right;   // 找到左子树的最右结点（前序的后继）
                p.right = root.right;                  // 原右子树挂到前驱右下
                root.right = root.left;                // 左子树整体搬到右边
                root.left = null;                      // 左指针清空
            }
            root = root.right;                         // 继续处理下一个结点
        }
    }
}
```

**复杂度**：时间 `O(n)`（每条边最多被走两次），空间 `O(1)`

**例子讲解**：`root = [1, 2, 5, 3, 4, null, 6]`

```
        1
      /   \
     2     5
    / \     \
   3   4     6
```

- `root=1`：左子树 `2(3,4)`，最右结点是 `4` → `4.right = 5(6)`；`1.right = 2(3,4)`，`1.left = null`
  此时 `1 → 2 → 3, 4 → 5 → 6`
- `root=2`：左子树 `3`，最右结点是 `3` → `3.right = 4`；`2.right = 3`，`2.left = null`
  此时 `1 → 2 → 3 → 4 → 5 → 6` ✅
- `root=3`：`left == null` → 跳过
- `root=4`：`left == null` → 跳过
- `root=5`：`left == null` → 跳过
- `root=6`：`left == null` → 跳过

答案 `1 → 2 → 3 → 4 → 5 → 6`（全用 `right` 指针）✅
**为什么叫「前驱」**：前序遍历中，`root` 之后紧接着就是「左子树的最右结点」，把它当作链表里的前驱，接上原右子树正好符合前序顺序。

---

### 105. 从前序与中序遍历序列构造二叉树 · 中等

**思路**：前序首元素是根 → 在中序里定位根（**哈希表加速**）→ 得出左子树长度 → 划分两段区间递归。关键是区间下标的推导。

```java
class Solution {
    Map<Integer, Integer> idx = new HashMap<>();          // 值 -> 中序下标
    public TreeNode buildTree(int[] pre, int[] in) {
        for (int i = 0; i < in.length; i++) idx.put(in[i], i);
        return build(pre, 0, in.length - 1, 0);           // 参数：前序区间 [pl, pr]，中序左端 il
    }
    TreeNode build(int[] pre, int pl, int pr, int il) {
        if (pl > pr) return null;
        TreeNode root = new TreeNode(pre[pl]);            // 前序首元素即根
        int i = idx.get(pre[pl]), leftLen = i - il;       // 根在中序的位置 → 左子树长度
        root.left  = build(pre, pl + 1, pl + leftLen, il);           // 前序左段 / 中序左段
        root.right = build(pre, pl + leftLen + 1, pr, i + 1);        // 前序右段 / 中序右段
        return root;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(n)`

**例子讲解**：`preorder = [3, 9, 20, 15, 7]`，`inorder = [9, 3, 15, 20, 7]`

`idx = {9:0, 3:1, 15:2, 20:3, 7:4}`

- `build(pre, 0, 4, 0)`：根 = `pre[0] = 3`；中序位置 `i=1` → `leftLen = 1-0 = 1`
  - 左：`build(pre, 1, 1, 0)` → 根 `9`，`i=0`、`leftLen=0` → 无孩子
  - 右：`build(pre, 2, 4, 2)` → 根 `20`；`i=3` → `leftLen = 3-2 = 1`
    - 左：`build(pre, 3, 3, 2)` → 根 `15`
    - 右：`build(pre, 4, 4, 4)` → 根 `7`

```
      3
     / \
    9   20
       /  \
      15   7
```

答案 `[3, 9, 20, null, null, 15, 7]` ✅
**区间公式记法**：前序左段 = `[pl+1, pl+leftLen]`，前序右段 = `[pl+leftLen+1, pr]`；中序左段 = `[il, i-1]`，中序右段 = `[i+1, ...]`。**`leftLen` 是唯一需要算的量**，其余都是它的加减。

---

### 437. 路径总和 III · 中等

**思路**：把「任意两点间路径和」转成**前缀和之差**：`pre - target` 出现过几次，就有几条合法路径。DFS 时用 map 记当前路径前缀和，**回溯要撤销计数**。

```java
class Solution {
    Map<Long, Integer> map = new HashMap<>();     // 当前路径上的前缀和 -> 出现次数
    int target, ans = 0;
    public int pathSum(TreeNode root, int targetSum) {
        target = targetSum;
        map.put(0L, 1);                           // 空前缀，覆盖「从根开始」的路径
        dfs(root, 0L);
        return ans;
    }
    void dfs(TreeNode n, long pre) {
        if (n == null) return;
        pre += n.val;                             // 更新到当前结点的前缀和
        ans += map.getOrDefault(pre - target, 0); // 之前有多少个 pre-target 就有多少条路径
        map.merge(pre, 1, Integer::sum);          // 把当前前缀和记入路径
        dfs(n.left, pre);
        dfs(n.right, pre);
        map.merge(pre, -1, Integer::sum);         // 回溯：离开这个分支前撤销
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(h)`

**例子讲解**：`root = [10,5,-3,3,2,null,11,3,-2,null,1]`，`targetSum = 8`

```
            10
          /    \
         5      -3
        / \       \
       3   2      11
      / \   \
     3  -2   1
```

沿最左路径 `10 → 5 → 3 → 3` 的前缀和变化：

| 结点 | `pre` | 查 `pre-8` | 命中 | ans | map 中新增 |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 10 | 10 | 2 | 0 | 0 | {0:1, 10:1} |
| 5 | 15 | 7 | 0 | 0 | {15:1} |
| 3 | 18 | 10 | **1**（路径 `5→3`） | 1 | {18:1} |
| 3 | 21 | 13 | 0 | 1 | {21:1} |

其他分支同理累计，最终答案 **3**（`5→3`、`5→2→1`、`-3→11`）✅
**为什么必须回溯**：`map` 记录的是「**当前**根到结点的这一条路径」上的前缀和。如果离开子树时不撤销，左子树的计数就会污染右子树的查询，答案偏大。

---

### 236. 二叉树的最近公共祖先 · 中等

**思路**：后序遍历。结点本身是 `p` 或 `q` 就返回它；**左右子树都找到了 → 当前结点就是 LCA**；只找到一边就往上传递那一边。

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) return root;   // 终止：命中目标或走到底
        TreeNode l = lowestCommonAncestor(root.left, p, q);
        TreeNode r = lowestCommonAncestor(root.right, p, q);
        if (l != null && r != null) return root;                   // 一左一右 → root 就是 LCA
        return (l != null) ? l : r;                                // 只有一边有，往上传递
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(h)`

**例子讲解**：`root = [3,5,1,6,2,0,8,null,null,7,4]`，`p = 5`，`q = 1`

```
              3
           /     \
          5       1
        /  \     / \
       6    2   0   8
           / \
          7   4
```

- 左子树根 `5` 就是 `p` → 返回 `5`
- 右子树根 `1` 就是 `q` → 返回 `1`
- 回到 `3`：`l = 5`，`r = 1`，两者都非空 → **返回 `3`** ✅

再看 `p = 5, q = 4`：
- `4` 在 `5` 的右子树里 → 递归到 `5` 时 `root == p` 直接返回 `5`
- `3` 收到 `l = 5`、`r = null` → 返回 `5` ✅（当 p 是 q 的祖先时，p 自己就是 LCA）

---

### 124. 二叉树中的最大路径和 · 困难

**思路**：后序。函数返回「以该结点为端点、向下延伸的最大贡献」（负贡献取 0 剪掉）；同时用 `左 + 根 + 右` 更新全局答案（这条路径是「拐弯」的，不能再往上传）。

```java
class Solution {
    int ans = Integer.MIN_VALUE;                    // 用最小值初始化，兼容全负数
    public int maxPathSum(TreeNode root) { dfs(root); return ans; }
    int dfs(TreeNode n) {
        if (n == null) return 0;
        int l = Math.max(dfs(n.left), 0),           // 负贡献直接当 0（不选这段）
            r = Math.max(dfs(n.right), 0);
        ans = Math.max(ans, n.val + l + r);         // 在 n 处拐弯的路径
        return n.val + Math.max(l, r);              // 只选一边往上延伸
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(h)`

**例子讲解**：`root = [-10, 9, 20, null, null, 15, 7]`

```
        -10
       /    \
      9      20
           /   \
         15     7
```

| 结点 | l | r | `n.val + l + r` | 更新 ans | 返回 `n.val + max(l,r)` |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 15 | 0 | 0 | 15 | 15 | 15 |
| 7 | 0 | 0 | 7 | 15 | 7 |
| 20 | 15 | 7 | **42** ✅ | **42** | 35 |
| 9 | 0 | 0 | 9 | 42 | 9 |
| -10 | 9 | 35 | 34 | 42 | 25 |

答案 `42`（路径 `15 → 20 → 7`）✅
**两个函数语义要分清**：返回值是「**单边**链」的最大和（只能往一个方向延伸，否则会分叉）；`ans` 收集的是「**拐弯**链」的和（左右都用上，但它没法再往父结点延伸）。混在一起写就会得到非法路径。

---

## 九、图论

> 网格题 = 隐式图，DFS/BFS 四方向扩散；依赖关系题 = 拓扑排序；字符串前缀 = Trie。

### 200. 岛屿数量 · 中等

**思路**：遍历网格，遇到 `'1'` 计数 +1，并 DFS 把相连陆地全部「淹没」成 `'0'`，保证每块陆地只被统计一次。

```java
class Solution {
    public int numIslands(char[][] g) {
        int cnt = 0;
        for (int i = 0; i < g.length; i++)
            for (int j = 0; j < g[0].length; j++)
                if (g[i][j] == '1') {         // 发现一块新陆地
                    cnt++;                    // 岛屿数 +1
                    dfs(g, i, j);             // 把整块陆地淹没，避免重复计数
                }
        return cnt;
    }
    void dfs(char[][] g, int i, int j) {
        // 越界或不是陆地 → 返回
        if (i < 0 || i >= g.length || j < 0 || j >= g[0].length || g[i][j] != '1') return;
        g[i][j] = '0';                        // 淹没当前格子（兼作 visited 标记）
        dfs(g, i + 1, j);
        dfs(g, i - 1, j);
        dfs(g, i, j + 1);
        dfs(g, i, j - 1);
    }
}
```

**复杂度**：时间 `O(mn)`，空间 `O(mn)`（递归栈最坏情况）

**例子讲解**：`grid = [["1","1","0","0","0"],["1","1","0","0","0"],["0","0","1","0","0"],["0","0","0","1","1"]]`

```
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1
```

- `(0,0)` 是 `'1'` → `cnt = 1`，DFS 把左上 2×2 的陆地全淹没
- 扫到 `(2,2)` 是 `'1'` → `cnt = 2`，淹没它
- 扫到 `(3,3)` 是 `'1'` → `cnt = 3`，连带淹没 `(3,4)`
- 其余都是 `'0'`

答案 `3` ✅
**技巧**：直接改写原数组当 `visited`，不用额外开 `boolean[][]`，空间省一半。代价是原数据被破坏（面试时提一句即可）。

---

### 994. 腐烂的橘子 · 中等

**思路**：**多源 BFS**。所有腐烂橘子同时入队，按层扩散，层数 = 分钟数；循环结束若还有新鲜橘子则返回 -1。（注意循环条件 `fresh > 0`，避免空转多算一分钟。）

```java
class Solution {
    public int orangesRotting(int[][] g) {
        int m = g.length, n = g[0].length, fresh = 0, ans = 0;
        Queue<int[]> q = new LinkedList<>();
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++) {
                if (g[i][j] == 2) q.offer(new int[]{i, j});   // 所有腐烂橘子同时入队
                else if (g[i][j] == 1) fresh++;              // 统计新鲜橘子
            }
        int[][] dirs = {{1,0},{-1,0},{0,1},{0,-1}};
        while (fresh > 0 && !q.isEmpty()) {                  // fresh==0 立刻停，避免多算一分钟
            ans++;                                           // 进入下一分钟
            for (int sz = q.size(); sz > 0; sz--) {           // 只处理本层的结点
                int[] c = q.poll();
                for (int[] d : dirs) {
                    int x = c[0] + d[0], y = c[1] + d[1];
                    if (x >= 0 && x < m && y >= 0 && y < n && g[x][y] == 1) {
                        g[x][y] = 2;                          // 腐化
                        fresh--;
                        q.offer(new int[]{x, y});             // 加入下一层
                    }
                }
            }
        }
        return fresh == 0 ? ans : -1;                        // 还有剩的说明无法腐化完
    }
}
```

**复杂度**：时间 `O(mn)`，空间 `O(mn)`

**例子讲解**：`grid = [[2,1,1],[1,1,0],[0,1,1]]`（0 是空格，1 新鲜，2 腐烂）

初始队列 `[(0,0)]`，`fresh = 6`

| 分钟 | 出队 | 新腐化 | fresh | 队列 |
| :-- | :-- | :-- | :-- | :-- |
| 1 | (0,0) | (0,1), (1,0) | 4 | [(0,1),(1,0)] |
| 2 | (0,1),(1,0) | (0,2), (1,1) | 2 | [(0,2),(1,1)] |
| 3 | (0,2),(1,1) | (2,1) | 1 | [(2,1)] |
| 4 | (2,1) | (2,2) | 0 | [(2,2)] |

退出循环（`fresh == 0`），答案 `4` ✅
**两个易错点**：① 循环条件要带 `fresh > 0`，否则最后一层腐化完还会再加 1 分钟；② 若初始就没有新鲜橘子（`fresh == 0`），直接返回 `0`。

---

### 207. 课程表 · 中等

**思路**：**拓扑排序（Kahn 算法）**。统计入度 + 邻接表，入度为 0 的入队，逐层剥离；能剥离的结点数等于总数 → 无环。

```java
class Solution {
    public boolean canFinish(int n, int[][] pre) {
        List<Integer>[] g = new List[n];              // 邻接表：先修课 -> 后续课
        for (int i = 0; i < n; i++) g[i] = new ArrayList<>();
        int[] in = new int[n];                        // 入度
        for (int[] p : pre) { g[p[1]].add(p[0]); in[p[0]]++; }
        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < n; i++) if (in[i] == 0) q.offer(i);   // 无前置要求的课先入队
        int cnt = 0;
        while (!q.isEmpty()) {
            int u = q.poll();
            cnt++;                                    // 这门课可以被修
            for (int v : g[u]) if (--in[v] == 0) q.offer(v);      // 解锁后继课程
        }
        return cnt == n;                              // 全部剥离完 → 无环
    }
}
```

**复杂度**：时间 `O(n + e)`，空间 `O(n + e)`

**例子讲解**：`numCourses = 2`，`prerequisites = [[1,0]]`（修 1 前要先修 0）

- 邻接表：`0 → [1]`
- 入度：`in = [0, 1]`
- 初始队列：`[0]`（`in[0] == 0`）
- 循环：弹出 `0`，`cnt = 1`；它的后继 `1` 入度减为 0 → 入队
- 弹出 `1`，`cnt = 2`
- `cnt == n` → **true** ✅

反例 `prerequisites = [[1,0],[0,1]]`（互相依赖）：
- 入度 `in = [1, 1]`，队列初始为空
- 循环一次都不执行，`cnt = 0 != 2` → **false** ✅

**记忆点**：入度为 0 = 「没有前置要求」。如果所有课程都有前置（成环），就永远没有起点，`cnt` 到不了 `n`。

---

### 208. 实现 Trie (前缀树) · 中等

**思路**：每个结点持有 `Trie[26]` 子结点数组 + `isEnd` 标记。`insert` 沿途建点，`search` 走到底再看 `isEnd`，`startsWith` 只要能走通即可。

```java
class Trie {
    Trie[] ch = new Trie[26];                 // 26 个小写字母的子结点
    boolean end;                              // 是否是某个单词的结尾
    public void insert(String w) {
        Trie p = this;
        for (char c : w.toCharArray()) {
            int i = c - 'a';
            if (p.ch[i] == null) p.ch[i] = new Trie();   // 结点不存在就新建
            p = p.ch[i];                                 // 往下走
        }
        p.end = true;                                    // 最后一个结点打上结尾标记
    }
    public boolean search(String w) { Trie p = find(w); return p != null && p.end; }
    public boolean startsWith(String p) { return find(p) != null; }   // 只要有路径即可
    private Trie find(String s) {                 // 沿路径走，走不通返回 null
        Trie p = this;
        for (char c : s.toCharArray()) {
            p = p.ch[c - 'a'];
            if (p == null) return null;
        }
        return p;
    }
}
```

**复杂度**：时间 `O(L)`（L 为单词长度），空间 `O(总字符数 × 26)`

**例子讲解**：依次 `insert("apple")`

- 依次创建 `a → p → p → l → e`，最后把 `e` 结点的 `end` 置为 `true`

| 操作 | 过程 | 结果 |
| :-- | :-- | :-- |
| `search("apple")` | 路径 `a→p→p→l→e` 走得通，且 `end == true` | **true** |
| `search("app")` | 路径走得通，但 `app` 结点 `end == false`（还有 `l`） | **false** |
| `startsWith("app")` | 路径走得通即可，不看 `end` | **true** |
| `search("apply")` | 走 `l` 后没有 `y` 子结点 | **false** |

**核心区别**：`search` = 「走通 **且** 是结尾」；`startsWith` = 「走通 **即可**」。这就是 `isEnd` 存在的原因。

---

## 十、回溯

> **万能模板**：`选 → 递归 → 撤销`。三处必检查：终止条件、横向遍历范围（`start` 或全数组）、是否需要去重。
>
> 去重口诀：**同一层用 `used[i-1]`/`i > start && a[i]==a[i-1]` 跳过；同一支可以重复用**（递归传 `i` 而不是 `i+1`）。

### 46. 全排列 · 中等

**思路**：`used` 数组标记已用元素，DFS 到 `path` 长度等于 `n` 时收集。

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> permute(int[] nums) {
        dfs(nums, new boolean[nums.length], new ArrayList<>());
        return res;
    }
    void dfs(int[] a, boolean[] used, List<Integer> path) {
        if (path.size() == a.length) {                 // 终止：已经选满 n 个
            res.add(new ArrayList<>(path));            // 必须拷贝！否则后续修改会污染结果
            return;
        }
        for (int i = 0; i < a.length; i++) {           // 每层都从 0 开始（顺序有关）
            if (used[i]) continue;                     // 已用过的跳过
            used[i] = true;                            // 选
            path.add(a[i]);
            dfs(a, used, path);                        // 递归
            path.remove(path.size() - 1);              // 撤销
            used[i] = false;
        }
    }
}
```

**复杂度**：时间 `O(n · n!)`，空间 `O(n)`

**例子讲解**：`nums = [1, 2, 3]`，递归树（部分）

```
                    []
        /            |            \
      1             2             3
    /   \         /   \         /   \
   2     3       1     3       1     2
   |     |       |     |       |     |
   3     2       3     1       2     1
 [1,2,3][1,3,2] [2,1,3][2,3,1] [3,1,2][3,2,1]
```

- 走到 `path = [1,2,3]` → 收集 → 撤销 `3` → 回到 `[1,2]` → 试 `3` 后面的元素（没有）→ 撤销 `2` → …
- 最终 6 个结果

答案 `[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]` ✅
**两个易错点**：① 收集结果必须 `new ArrayList<>(path)` 拷贝；② 排列**顺序有关**，所以每层循环从 `0` 开始（不像子集从 `start` 开始）。

---

### 78. 子集 · 中等

**思路**：**每次进入递归就是一个子集**，先收集再继续选；用 `start` 参数保证只往后选，天然不重复。

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> subsets(int[] nums) {
        dfs(nums, 0, new ArrayList<>());
        return res;
    }
    void dfs(int[] a, int start, List<Integer> path) {
        res.add(new ArrayList<>(path));                // 先收集当前这个子集（含空集）
        for (int i = start; i < a.length; i++) {       // 只能从 start 往后选，避免重复
            path.add(a[i]);                            // 选
            dfs(a, i + 1, path);                       // 递归（i+1 保证不重复选自己）
            path.remove(path.size() - 1);              // 撤销
        }
    }
}
```

**复杂度**：时间 `O(n · 2ⁿ)`，空间 `O(n)`

**例子讲解**：`nums = [1, 2, 3]`，递归树

```
dfs(start=0, path=[]) → 收集 []
├─ 选 1 → dfs(1, [1]) → 收集 [1]
│   ├─ 选 2 → dfs(2, [1,2]) → 收集 [1,2]
│   │   └─ 选 3 → dfs(3, [1,2,3]) → 收集 [1,2,3]
│   └─ 选 3 → dfs(3, [1,3]) → 收集 [1,3]
├─ 选 2 → dfs(2, [2]) → 收集 [2]
│   └─ 选 3 → dfs(3, [2,3]) → 收集 [2,3]
└─ 选 3 → dfs(3, [3]) → 收集 [3]
```

答案 `[[], [1], [1,2], [1,2,3], [1,3], [2], [2,3], [3]]`（共 `2³ = 8` 个）✅
**和全排列的核心差别**：子集**顺序无关**（`[1,2]` 和 `[2,1]` 是同一个），所以用 `start` 限制「只能往后选」；排列顺序有关，所以要 `used` 标记 + 每层从 0 开始。

---

### 17. 电话号码的字母组合 · 中等

**思路**：按位处理数字，枚举该数字对应的字母，DFS 拼接。

```java
class Solution {
    String[] M = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    List<String> res = new ArrayList<>();
    public List<String> letterCombinations(String digits) {
        if (digits.isEmpty()) return res;               // 空输入返回空列表（不是 [""]）
        dfs(digits, 0, new StringBuilder());
        return res;
    }
    void dfs(String d, int i, StringBuilder sb) {
        if (i == d.length()) {                          // 终止：所有位都选完了
            res.add(sb.toString());
            return;
        }
        for (char c : M[d.charAt(i) - '0'].toCharArray()) {   // 枚举本位可用的字母
            sb.append(c);                               // 选
            dfs(d, i + 1, sb);                          // 处理下一位
            sb.deleteCharAt(sb.length() - 1);           // 撤销
        }
    }
}
```

**复杂度**：时间 `O(4ⁿ · n)`，空间 `O(n)`

**例子讲解**：`digits = "23"`

- `M[2] = "abc"`，`M[3] = "def"`

```
dfs(i=0, "")
├─ 'a' → dfs(i=1, "a")  ├─ 'd' → "ad"  ├─ 'e' → "ae"  └─ 'f' → "af"
├─ 'b' → dfs(i=1, "b")  ├─ 'd' → "bd"  ├─ 'e' → "be"  └─ 'f' → "bf"
└─ 'c' → dfs(i=1, "c")  ├─ 'd' → "cd"  ├─ 'e' → "ce"  └─ 'f' → "cf"
```

答案 `["ad","ae","af","bd","be","bf","cd","ce","cf"]`（`3 × 3 = 9` 个）✅
**注意空输入**：`digits = ""` 时按题意应返回 `[]`，所以循环体只写 `if (k == d.length())` 是不够的（会返回 `[""]`），要额外判空。

---

### 39. 组合总和 · 中等

**思路**：元素可重复使用 → **递归传 `i` 而不是 `i+1`**。排序后 `c[i] > rest` 直接 break 剪枝。

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> combinationSum(int[] c, int target) {
        Arrays.sort(c);                                 // 排序后才能用 break 剪枝
        dfs(c, 0, target, new ArrayList<>());
        return res;
    }
    void dfs(int[] c, int start, int rest, List<Integer> path) {
        if (rest == 0) {                                // 终止：刚好凑够
            res.add(new ArrayList<>(path));
            return;
        }
        for (int i = start; i < c.length && c[i] <= rest; i++) {   // c[i] > rest 后面更不可能，直接停
            path.add(c[i]);
            dfs(c, i, rest - c[i], path);               // 传 i：同一个数可以再用
            path.remove(path.size() - 1);
        }
    }
}
```

**复杂度**：时间 `O(n^(T/min))`（指数级），空间 `O(T/min)`

**例子讲解**：`candidates = [2, 3, 6, 7]`，`target = 7`

```
dfs(start=0, rest=7, [])
├─ 选 2 → dfs(0, rest=5, [2])
│   ├─ 选 2 → dfs(0, rest=3, [2,2])
│   │   ├─ 选 2 → c[0]=2 ≤ 3 → dfs(0, rest=1, [2,2,2]) → 循环里 c[0]=2 > 1 → 无解
│   │   └─ 选 3 → dfs(1, rest=0, [2,2,3]) → 收集 ✅
│   └─ 选 3 → dfs(1, rest=2, [2,3]) → 循环里 3 > 2 → 无解
├─ 选 3 → dfs(1, rest=4, [3]) → 3 > 4? 不；选 3 → rest=1 无解
├─ 选 6 → rest=1 无解
└─ 选 7 → dfs(3, rest=0, [7]) → 收集 ✅
```

答案 `[[2,2,3], [7]]` ✅
**两个关键点**：① 递归传 `i`（不是 `i+1`）才能重复用同一个数；② 传入 `start`（不是 0）保证结果内部递增、不出现 `[3,2,2]` 这种重复排列。如果题目改成「每个数只能用一次」，把 `i` 换成 `i+1` 即可。

---

### 22. 括号生成 · 中等

**思路**：无需去重，只靠**合法性约束**剪枝：`左括号 < n` 才能放 `(`；`右括号 < 左括号` 才能放 `)`。

```java
class Solution {
    List<String> res = new ArrayList<>();
    public List<String> generateParenthesis(int n) {
        dfs(n, 0, 0, new StringBuilder());
        return res;
    }
    void dfs(int n, int l, int r, StringBuilder sb) {
        if (sb.length() == 2 * n) {                     // 终止：长度够了
            res.add(sb.toString());
            return;
        }
        if (l < n) {                                    // 左括号还没用完 → 可以放 '('
            sb.append('(');
            dfs(n, l + 1, r, sb);
            sb.deleteCharAt(sb.length() - 1);
        }
        if (r < l) {                                    // 右括号比左括号少 → 可以放 ')'
            sb.append(')');
            dfs(n, l, r + 1, sb);
            sb.deleteCharAt(sb.length() - 1);
        }
    }
}
```

**复杂度**：时间 `O(卡特兰数(n) · n)`，空间 `O(n)`

**例子讲解**：`n = 2`，递归过程

```
dfs(l=0, r=0, "")
└─ l<n → '(' → dfs(1, 0, "(")
   ├─ l<n → '(' → dfs(2, 0, "((")
   │   └─ r<l → ')' → dfs(2, 1, "(()")
   │       └─ r<l → ')' → "(())" ✅ 长度 4 收集
   └─ r<l → ')' → dfs(1, 1, "()")
       └─ l<n → '(' → dfs(2, 1, "()(")
           └─ r<l → ')' → "()()" ✅
```

答案 `["(())", "()()"]` ✅（`n = 3` 时得到 5 个）
**为什么不用去重**：两个约束条件本身就把非法路径和重复路径都堵死了——任何时刻都只有「加 `(`」和「加 `)`」两个分支，且不会产生同一个串的两种生成方式。

---

### 79. 单词搜索 · 中等

**思路**：每个格子作为起点 DFS 逐字符匹配；**已走过的格子临时改成 `'#'`** 防止回头，回溯时恢复。

```java
class Solution {
    public boolean exist(char[][] b, String w) {
        for (int i = 0; i < b.length; i++)
            for (int j = 0; j < b[0].length; j++)
                if (dfs(b, w, i, j, 0)) return true;      // 任一格子出发能成即可
        return false;
    }
    boolean dfs(char[][] b, String w, int i, int j, int k) {
        if (k == w.length()) return true;                 // 终止：整个单词匹配完
        // 越界 或 字符不匹配 → 失败
        if (i < 0 || i >= b.length || j < 0 || j >= b[0].length || b[i][j] != w.charAt(k))
            return false;
        char t = b[i][j];
        b[i][j] = '#';                                    // 标记已访问，防止走回头路
        boolean ok = dfs(b, w, i + 1, j, k + 1)
                  || dfs(b, w, i - 1, j, k + 1)
                  || dfs(b, w, i, j + 1, k + 1)
                  || dfs(b, w, i, j - 1, k + 1);
        b[i][j] = t;                                      // 回溯：恢复现场
        return ok;
    }
}
```

**复杂度**：时间 `O(mn · 3^L)`（每步最多 3 个方向），空间 `O(L)`

**例子讲解**：`board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]]`，`word = "ABCCED"`

```
A B C E
S F C S
A D E E
```

- 起点 `(0,0)='A'` 匹配 `A` → 标记 `#`
- 向右 `(0,1)='B'` 匹配 → 标记
- 向右 `(0,2)='C'` 匹配 → 标记
- 向右 `(0,3)='E'` ≠ `C`，向下越界 → 回退
- 向下 `(1,2)='C'` 匹配 → 标记
- 向右 `(1,3)='S'` ≠ `E`，向左 `(1,1)='F'` ≠ `E`，向上 `(0,2)='#'` 不匹配，向下 `(2,2)='E'` 匹配 → 标记
- 向下 `(2,2)` 已用，向右越界，向左 `(2,1)='D'` 匹配最后一个字符 → `k = 6` → **true** ✅

路径：`A(0,0) → B(0,1) → C(0,2) → C(1,2) → E(2,2) → D(2,1)`
**为什么标 `'#'` 而不是用 `visited` 数组**：省空间，且回溯恢复方便。**恢复一定要写**——否则这个格子在整个搜索里就永久失效了。

---

### 131. 分割回文串 · 中等

**思路**：DFS 枚举切分点：`s[start..i]` 是回文就切一刀继续；**先用区间 DP 预处理回文表**，把每次判断降到 `O(1)`。
回文 DP 递推：`dp[l][r] = s[l]==s[r] && (r-l < 2 || dp[l+1][r-1])`。

```java
class Solution {
    List<List<String>> res = new ArrayList<>();
    boolean[][] dp;                                       // dp[l][r]：s[l..r] 是否回文
    public List<List<String>> partition(String s) {
        int n = s.length();
        dp = new boolean[n][n];
        for (int r = 0; r < n; r++)                       // 右端点递增
            for (int l = r; l >= 0; l--)                  // 左端点递减，保证用到的 dp[l+1][r-1] 已算好
                dp[l][r] = s.charAt(l) == s.charAt(r) && (r - l < 2 || dp[l + 1][r - 1]);
        dfs(s, 0, new ArrayList<>());
        return res;
    }
    void dfs(String s, int start, List<String> path) {
        if (start == s.length()) {                        // 终止：切到头了
            res.add(new ArrayList<>(path));
            return;
        }
        for (int i = start; i < s.length(); i++) {        // 枚举本段的右端点
            if (!dp[start][i]) continue;                  // O(1) 判断是否回文，不是就换下一个切点
            path.add(s.substring(start, i + 1));          // 选这一段
            dfs(s, i + 1, path);                          // 从下一刀继续
            path.remove(path.size() - 1);                 // 撤销
        }
    }
}
```

**复杂度**：时间 `O(n · 2ⁿ)`，空间 `O(n²)`

**例子讲解**：`s = "aab"`

回文表（只列 true 的）：`dp[0][0]=T, dp[1][1]=T, dp[0][1]=T("aa"), dp[2][2]=T`

```
dfs(start=0, [])
├─ 切 [0,0]="a" ✅ → dfs(1, ["a"])
│   ├─ 切 [1,1]="a" ✅ → dfs(2, ["a","a"])
│   │   └─ 切 [2,2]="b" ✅ → dfs(3) → 收集 ["a","a","b"] ✅
│   └─ 切 [1,2]="ab" ❌ 不是回文 → 跳过
└─ 切 [0,1]="aa" ✅ → dfs(2, ["aa"])
    └─ 切 [2,2]="b" ✅ → dfs(3) → 收集 ["aa","b"] ✅
```

答案 `[["a","a","b"], ["aa","b"]]` ✅
**DP 的遍历顺序**：`r` 递增、`l` 递减，这样 `dp[l+1][r-1]`（更小的区间）一定已经算过。写成 `l` 递增会用到未计算的值。

---

### 51. N 皇后 · 困难

**思路**：逐行放置，三个集合分别记录**列 `c`、主对角线 `r-c`、副对角线 `r+c`** 是否被占用，冲突就跳过（这就是剪枝）。

```java
class Solution {
    List<List<String>> res = new ArrayList<>();
    Set<Integer> col = new HashSet<>(), dg = new HashSet<>(), udg = new HashSet<>();
    int n; char[][] board;
    public List<List<String>> solveNQueens(int n) {
        this.n = n;
        board = new char[n][n];
        for (char[] r : board) Arrays.fill(r, '.');      // 初始化为空棋盘
        dfs(0);                                          // 从第 0 行开始放
        return res;
    }
    void dfs(int r) {
        if (r == n) {                                    // 每行都放好了 → 一个解
            List<String> cur = new ArrayList<>();
            for (char[] row : board) cur.add(new String(row));
            res.add(cur);
            return;
        }
        for (int c = 0; c < n; c++) {                    // 尝试本行的每一列
            // 同列 / 同主对角线(r-c) / 同副对角线(r+c) 冲突则跳过
            if (col.contains(c) || dg.contains(r - c) || udg.contains(r + c)) continue;
            board[r][c] = 'Q';                           // 放
            col.add(c); dg.add(r - c); udg.add(r + c);
            dfs(r + 1);                                  // 处理下一行
            board[r][c] = '.';                           // 撤销
            col.remove(c); dg.remove(r - c); udg.remove(r + c);
        }
    }
}
```

**复杂度**：时间 `O(n!)`，空间 `O(n)`

**例子讲解**：`n = 4`

- 第 0 行放 `c=0` → 占 `col={0}`、`dg={0}`、`udg={0}`
- 第 1 行：`c=0` 列冲突；`c=1` 副对角线冲突（`r+c=2` 还没被占？实际 `1-0=1`、`1+1=2`，`dg` 里是 `0`，`udg` 里是 `0` → 但 `col` 里没 1 → 合法）→ 放 `(1,1)`
  - 第 2 行：`c=0`（`2-0=2`、`2+0=2`）→ `dg`= {0,1}，`udg`={0,2} → 不冲突，`col` 无 0 → 放 `(2,0)`？
    - 第 3 行：`c=0`列冲突；`c=1`列冲突；`c=2` `dg=3-2=1` 冲突；`c=3` `udg=3+3=6`... `col` 无 3，`dg`={0,1,2} 里无 0，`udg`={0,2,3} 里无 6 → 检查 `dg`：`r-c = 3-3 = 0` → `dg` 里有 0 → 冲突
    - 无解 → 回溯
  - 第 2 行 `c=2`：`dg = 2-2 = 0` 冲突 → 跳过
  - 第 2 行 `c=3`：`dg = 2-3 = -1` 不冲突，`udg = 5` 不冲突，`col` 无 3 → 放 `(2,3)`
    - 第 3 行：`c=0` → `dg=3` 不冲突、`udg=3` 不冲突、`col` 无 0 → 放 `(3,0)` → 第 4 行 → **收集** `Q..Q / ...Q / Q...` 等等

`n = 4` 共有 **2** 个解：

```
.Q..        ..Q.
...Q        Q...
Q...        ...Q
..Q.        .Q..
```

**对角线编号技巧**：同一条主对角线（左上→右下）上 `r - c` 恒定；同一条副对角线（右上→左下）上 `r + c` 恒定。用两个 `Set` 就能 `O(1)` 判断，比遍历整个棋盘便宜得多。

---

## 十一、二分查找

> 两套边界写法，**背一套用到底**：
> - **左闭右开 `[l, r)`**：`while (l < r)`，`r = m`；结束时 `l` 是答案。
> - **闭区间 `[l, r]`**：`while (l <= r)`，`r = m - 1`。
>
> 找「第一个满足条件的位置」用左闭右开最不容易错。
> **中位数取法**：`(l + r) >>> 1` 避免溢出。

### 35. 搜索插入位置 · 简单

**思路**：找**第一个 ≥ target 的下标**，找不到就是 `nums.length`（正好是插入位置），左闭右开写法天然返回这个值。

```java
class Solution {
    public int searchInsert(int[] a, int t) {
        int l = 0, r = a.length;          // 左闭右开区间 [l, r)
        while (l < r) {
            int m = (l + r) >>> 1;
            if (a[m] < t) l = m + 1;      // m 太小，答案在右半
            else r = m;                   // a[m] >= t，m 可能就是答案
        }
        return l;                         // l 收敛到第一个 >= t 的位置
    }
}
```

**复杂度**：时间 `O(log n)`，空间 `O(1)`

**例子讲解**：`nums = [1, 3, 5, 6]`

**`target = 5`**（存在，期望返回 2）

| 轮 | l | r | m | a[m] | 比较 | 更新 |
| :-- | :-- | :-- | :-- | :-- | :-- | :-- |
| 1 | 0 | 4 | 2 | 5 | `5 < 5` ✗ | `r = 2` |
| 2 | 0 | 2 | 1 | 3 | `3 < 5` ✓ | `l = 2` |

`l == r == 2` → 返回 **2** ✅

**`target = 2`**（不存在，应插入下标 1）

| 轮 | l | r | m | a[m] | 更新 |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 1 | 0 | 4 | 2 | 5 | `r = 2` |
| 2 | 0 | 2 | 1 | 3 | `r = 1` |
| 3 | 0 | 1 | 0 | 1 | `1 < 2` → `l = 1` |

`l == r == 1` → 返回 **1** ✅

**`target = 7`**（比所有数都大）：最终 `l = 4 = nums.length` → 返回 **4** ✅
**记忆点**：`a[m] < t` 用严格小于，`r = m` 不是 `m - 1`。这样 `l` 最终收敛的位置就是「第一个 ≥ t 的下标」，**插入位置和查找位置统一成一个语义**。

---

### 74. 搜索二维矩阵 · 中等

**思路**：二维数组按行展开就是一维有序数组 → **把下标整体二分**，取值用 `m[mid / C][mid % C]`。

```java
class Solution {
    public boolean searchMatrix(int[][] m, int t) {
        int R = m.length, C = m[0].length;
        int l = 0, r = R * C - 1;                        // 当成一维数组 [0, R*C-1]
        while (l <= r) {                                 // 闭区间写法
            int mid = (l + r) >>> 1;
            int v = m[mid / C][mid % C];                 // 一维下标 → 二维坐标
            if (v == t) return true;
            if (v < t) l = mid + 1;
            else r = mid - 1;
        }
        return false;
    }
}
```

**复杂度**：时间 `O(log(mn))`，空间 `O(1)`

**例子讲解**：`matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]]`，`target = 3`

`R = 3, C = 4`，一维下标范围 `[0, 11]`

| 轮 | l | r | mid | `mid/C`, `mid%C` | 值 | 比较 |
| :-- | :-- | :-- | :-- | :-- | :-- | :-- |
| 1 | 0 | 11 | 5 | `1,1` | 11 | `11 > 3` → `r = 4` |
| 2 | 0 | 4 | 2 | `0,2` | 5 | `5 > 3` → `r = 1` |
| 3 | 0 | 1 | 0 | `0,0` | 1 | `1 < 3` → `l = 1` |
| 4 | 1 | 1 | 1 | `0,1` | **3** | 相等 → **true** ✅ |

**关键前提**：题目保证「每行递增」且「本行首元素 > 上一行末元素」，所以**按行拼接后整体有序**，才能直接整体二分。若只有「每行递增 + 每列递增」（如 #240），整体拼接不有序，就得用右上角那种走法。

---

### 34. 在排序数组中查找元素的第一个和最后一个位置 · 中等

**思路**：抽出「**第一个 ≥ t 的下标**」函数 `lower(t)`，答案就是 `[lower(t), lower(t+1) - 1]`。

```java
class Solution {
    public int[] searchRange(int[] a, int t) {
        int l = lower(a, t);                    // 第一个 >= t
        int r = lower(a, t + 1) - 1;            // 第一个 > t 的前一个 = 最后一个 == t
        return (l < a.length && a[l] == t) ? new int[]{l, r} : new int[]{-1, -1};
    }
    int lower(int[] a, int t) {                 // 第一个 >= t 的下标（同 #35）
        int l = 0, r = a.length;
        while (l < r) {
            int m = (l + r) >>> 1;
            if (a[m] < t) l = m + 1; else r = m;
        }
        return l;
    }
}
```

**复杂度**：时间 `O(log n)`，空间 `O(1)`

**例子讲解**：`nums = [5, 7, 7, 8, 8, 10]`，`target = 8`

- `lower(a, 8)`：找第一个 ≥ 8 的位置
  - 最终 `l = 3`（`a[3] = 8`）
- `lower(a, 9)`：找第一个 ≥ 9 的位置
  - 数组里 ≥ 9 的只有 `10`（下标 5）→ 返回 `5`
  - 所以 `r = 5 - 1 = 4`（`a[4] = 8`）
- 检查 `a[3] == 8` ✓ → 返回 `[3, 4]` ✅

**`target = 6`**：`lower(a, 6)` → 第一个 ≥ 6 的是 `7`（下标 1）；`lower(a, 7)` → 下标 1，`r = 0`。
`a[1] = 7 == 6`？否 → 返回 `[-1, -1]` ✅
**技巧**：把「找最后一个等于 t」转化为「找第一个 > t 再减 1」，只需要一个 `lower` 函数，代码短且不会写错边界。

---

### 33. 搜索旋转排序数组 · 中等

**思路**：`mid` 切下去，**左右两半必有一半是有序的**。先判断哪半有序，再看 `target` 是否落在有序那半的区间内，据此收缩。

```java
class Solution {
    public int search(int[] a, int t) {
        int l = 0, r = a.length - 1;
        while (l <= r) {
            int m = (l + r) >>> 1;
            if (a[m] == t) return m;
            if (a[l] <= a[m]) {                        // 左半 [l, m] 有序
                if (a[l] <= t && t < a[m]) r = m - 1;  // t 在有序区内 → 往左
                else l = m + 1;
            } else {                                   // 右半 [m, r] 有序
                if (a[m] < t && t <= a[r]) l = m + 1;  // t 在有序区内 → 往右
                else r = m - 1;
            }
        }
        return -1;
    }
}
```

**复杂度**：时间 `O(log n)`，空间 `O(1)`

**例子讲解**：`nums = [4, 5, 6, 7, 0, 1, 2]`，`target = 0`

| 轮 | l | r | m | a[m] | 判断哪半有序 | target 在有序区内? | 更新 |
| :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- |
| 1 | 0 | 6 | 3 | 7 | `a[0]=4 ≤ a[3]=7` → 左半 `[4,7]` 有序 | `4 ≤ 0` ✗ | `l = 4` |
| 2 | 4 | 6 | 5 | 1 | `a[4]=0 ≤ a[5]=1` → 左半 `[0,1]` 有序 | `0 ≤ 0 < 1` ✓ | `r = 4` |
| 3 | 4 | 4 | 4 | **0** | — | 命中 → **返回 4** ✅ | — |

再验证一个不存在的目标 `target = 3`：

| 轮 | l | r | m | a[m] | 判断 | 更新 |
| :-- | :-- | :-- | :-- | :-- | :-- | :-- |
| 1 | 0 | 6 | 3 | 7 | 左半 `[4,7]` 有序；`3` 不在 `[4,7)` | `l = 4` |
| 2 | 4 | 6 | 5 | 1 | 左半 `[0,1]` 有序；`3` 不在 `[0,1)` | `l = 6` |
| 3 | 6 | 6 | 6 | 2 | 左半 `[2,2]` 有序；`3` 不在 `[2,2)` | `l = 7` |

`l = 7 > r = 6` → 返回 **-1** ✅

**两个易错点**：① 判断「哪半有序」必须用 `a[l] <= a[m]`（**带等号**），否则 `l == m` 时会判错；② 有序区间的边界要写成**半开区间**（`a[l] <= t && t < a[m]`；右侧是 `a[m] < t && t <= a[r]`），两侧都用 `<=` 会漏判或判重。

---

### 153. 寻找旋转排序数组中的最小值 · 中等

**思路**：比较 `nums[m]` 与 `nums[r]`：`nums[m] < nums[r]` 说明 m 在右段（最小值在左，含 m）→ `r = m`；否则 m 在左段 → `l = m + 1`。

```java
class Solution {
    public int findMin(int[] a) {
        int l = 0, r = a.length - 1;
        while (l < r) {                      // 注意是 <，收敛到单点
            int m = (l + r) >>> 1;
            if (a[m] < a[r]) r = m;          // 右半递增 → 最小值在 [l, m]
            else l = m + 1;                  // 否则最小值在 (m, r]
        }
        return a[l];                         // l == r，即为最小值
    }
}
```

**复杂度**：时间 `O(log n)`，空间 `O(1)`

**例子讲解**：`nums = [3, 4, 5, 1, 2]`（旋转点在 `1`）

| 轮 | l | r | m | a[m] | a[r] | 比较 | 更新 |
| :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- |
| 1 | 0 | 4 | 2 | 5 | 2 | `5 < 2` ✗ | `l = 3` |
| 2 | 3 | 4 | 3 | 1 | 2 | `1 < 2` ✓ | `r = 3` |

`l == r == 3` → 返回 `a[3] = 1` ✅

再看未旋转的 `nums = [1, 2, 3]`：
- 轮 1：`m=1, a[1]=2, a[2]=3` → `2 < 3` ✓ → `r = 1`
- 轮 2：`m=0, a[0]=1, a[1]=2` → `1 < 2` ✓ → `r = 0`
- `l == r == 0` → 返回 `1` ✅（未旋转时也能正确输出首元素）

**为什么和 `a[r]` 比而不是 `a[l]`**：旋转后右段是「较小的一段」，与 `a[r]` 比较能明确判断 `m` 落在哪段。与 `a[l]` 比会有 `a[l] <= a[m]` 恒成立（左段内部递增）的歧义，无法区分。

---

### 4. 寻找两个正序数组的中位数 · 困难

**思路**：在**较短的数组**上二分划分点 `i`（`j = half - i`），保证左右两半元素个数相等。合法条件是 `aLeft ≤ bRight && bLeft ≤ aRight`；中位数由划分线两侧的四个边界值算出。
（另一思路：转化为「找第 k 小」逐步二分排除。）

```java
class Solution {
    public double findMedianSortedArrays(int[] a, int[] b) {
        if (a.length > b.length) return findMedianSortedArrays(b, a);   // 保证在短数组上二分
        int m = a.length, n = b.length, half = (m + n + 1) / 2;         // 左半需要的元素个数
        int l = 0, r = m;
        while (l <= r) {
            int i = (l + r) >>> 1, j = half - i;         // a 切 i 个，b 切 j 个
            int aL = (i == 0) ? Integer.MIN_VALUE : a[i - 1];   // 哨兵处理边界
            int aR = (i == m) ? Integer.MAX_VALUE : a[i];
            int bL = (j == 0) ? Integer.MIN_VALUE : b[j - 1];
            int bR = (j == n) ? Integer.MAX_VALUE : b[j];
            if (aL <= bR && bL <= aR) {                  // 找到合法划分
                int left = Math.max(aL, bL);             // 左半最大值
                if ((m + n) % 2 == 1) return left;       // 奇数：就是它
                return (left + Math.min(aR, bR)) / 2.0;  // 偶数：和右半最小值取平均
            }
            if (aL > bR) r = i - 1;                      // a 的左半太大 → 划分点左移
            else l = i + 1;
        }
        return 0;
    }
}
```

**复杂度**：时间 `O(log min(m, n))`，空间 `O(1)`

**例子讲解**：`nums1 = [1, 3]`，`nums2 = [2]`（期望中位数 `2.0`）

`m = 2, n = 3, half = (2+3+1)/2 = 3`（左半要 3 个元素）

| 轮 | i | j=3-i | aL | aR | bL | bR | 合法性 | 动作 |
| :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- |
| 1 | 1 | 2 | 1 | 3 | 2 | +∞ | `1 ≤ +∞` ✓ 且 `2 ≤ 3` ✓ | **合法** |

- `left = max(1, 2) = 2`
- 总长 5 为奇数 → 返回 `2` → **2.0** ✅

再看 `nums1 = [1, 2]`，`nums2 = [3, 4]`：`half = 2`

- 第 1 轮 `i=1, j=1` → `aL=1, aR=2, bL=3, bR=4` → `bL=3 > aR=2` ❌ 不合法 → `l = 2`
- 第 2 轮 `i=2, j=0` → `aL=2, aR=+∞, bL=-∞, bR=3` → `2 ≤ 3` ✓ 且 `-∞ ≤ +∞` ✓ 合法
- `left = max(2, -∞) = 2`，`right = min(+∞, 3) = 3`，偶数个 → 中位数 `(2+3)/2 = 2.5` ✅

**关键领悟**：不需要预先算出正确的划分点，**二分会自动收敛到合法划分**——只要 `aL > bR` 就往左收、否则往右推，代码不用讨论任何边界特例。哨兵值 `±∞` 则把「切在最左/最右」这两种情况统一掉了。

---

## 十二、栈

> 识别信号：**括号匹配、最近的更大/更小元素、嵌套结构解析** → 单调栈 / 辅助栈。

### 20. 有效的括号 · 简单

**思路**：遇左括号入栈，遇右括号检查栈顶是否是其对应的左括号；最后栈必须为空。

```java
class Solution {
    public boolean isValid(String s) {
        char[] pair = new char[128];                  // 右括号 -> 对应左括号
        pair[')'] = '('; pair[']'] = '['; pair['}'] = '{';
        Deque<Character> st = new ArrayDeque<>();
        for (char c : s.toCharArray()) {
            if (c == ')' || c == ']' || c == '}') {   // 右括号：必须匹配栈顶
                if (st.isEmpty() || st.pop() != pair[c]) return false;
            } else st.push(c);                        // 左括号：入栈等待匹配
        }
        return st.isEmpty();                          // 还有剩的左括号 → 不合法
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(n)`

**例子讲解**：

| 输入 | 过程 | 结果 |
| :-- | :-- | :-- |
| `"()[]{}"` | `(`入栈 → `)`弹栈顶 `(` 匹配 ✓ → `[`入栈 → `]`匹配 ✓ → `{`入栈 → `}`匹配 ✓ → 栈空 | **true** |
| `"([)]"` | `(`入栈 → `[`入栈 → 遇 `)`，栈顶是 `[` ≠ `(` | **false** |
| `"(("` | 两个都入栈，结束时栈非空 | **false** |
| `")"` | 遇 `)` 时栈为空 | **false** |

**为什么用栈**：括号匹配是**后进先出**的嵌套结构——最近一个还没闭合的左括号，正应该被当前这个右括号闭合。用「计数左右括号数量」是经典错解，因为无法区分 `"([)]"` 这种交叉情形。

---

### 155. 最小栈 · 中等

**思路**：**辅助栈同步记录「当前最小值」**，压入时压 `min(新值, 辅助栈顶)`，弹出时一起弹，`getMin` 就是辅助栈顶。

```java
class MinStack {
    Deque<Integer> st = new ArrayDeque<>(), mn = new ArrayDeque<>();   // mn: 历史最小值栈
    public void push(int v) {
        st.push(v);
        mn.push(mn.isEmpty() ? v : Math.min(v, mn.peek()));   // 同步压入「当前最小值」
    }
    public void pop() { st.pop(); mn.pop(); }                 // 同步弹出，历史最小值自动回退
    public int top() { return st.peek(); }
    public int getMin() { return mn.peek(); }                 // O(1) 拿到最小值
}
```

**复杂度**：各操作时间 `O(1)`，空间 `O(n)`

**例子讲解**：依次 `push(-2) → push(0) → push(-3) → getMin() → pop() → top() → getMin()`

| 操作 | st | mn | 返回 |
| :-- | :-- | :-- | :-- |
| `push(-2)` | [-2] | [-2] | — |
| `push(0)` | [-2, 0] | [-2, -2] | — |
| `push(-3)` | [-2, 0, -3] | [-2, -2, -3] | — |
| `getMin()` | — | — | **-3** |
| `pop()` | [-2, 0] | [-2, -2] | — |
| `top()` | — | — | **0** |
| `getMin()` | — | — | **-2** ✅ |

**为什么 `push` 压的是 `min(新值, 栈顶)` 而不是「只压更小的值」**：这样两个栈**长度永远相等**，`pop` 时不用判断「弹出的到底是不是当前最小值」，代码最简。弹出 `-3` 后 `mn` 栈顶自然回到 `-2`，最小值就正确恢复了。

---

### 394. 字符串解码 · 中等

**思路**：**双栈（数字栈 + 字符串栈）**。遇数字累加（可能是多位数）；遇 `[` 把「当前倍数」和「当前已拼串」入栈后清空；遇 `]` 弹栈相乘拼接；遇字母直接追加。

```java
class Solution {
    public String decodeString(String s) {
        Deque<Integer> num = new ArrayDeque<>();            // 括号外的倍数
        Deque<StringBuilder> str = new ArrayDeque<>();      // 括号外的已拼串
        StringBuilder cur = new StringBuilder();            // 当前层的串
        int k = 0;                                          // 当前累积的数字
        for (char c : s.toCharArray()) {
            if (Character.isDigit(c)) k = k * 10 + c - '0';  // 处理多位数，如 "12[a]"
            else if (c == '[') {                            // 进入新一层
                num.push(k);                                // 保存倍数
                str.push(cur);                              // 保存外层已拼串
                k = 0; cur = new StringBuilder();           // 重置，开始新一层
            } else if (c == ']') {                          // 退出当前层
                StringBuilder t = str.pop();                // 取回外层串
                int cnt = num.pop();                        // 取回倍数
                while (cnt-- > 0) t.append(cur);            // 当前层内容重复 cnt 次拼到外层
                cur = t;
            } else cur.append(c);                           // 字母直接追加
        }
        return cur.toString();
    }
}
```

**复杂度**：时间 `O(输出长度)`，空间 `O(n)`

**例子讲解**：`s = "3[a2[c]]"`

| 字符 | 动作 | num | str | cur |
| :-- | :-- | :-- | :-- | :-- |
| `3` | `k = 3` | [] | [] | `` |
| `[` | 压 `k=3`、压 `cur=""`，重置 | [3] | [""] | `` |
| `a` | 追加 | [3] | [""] | `a` |
| `2` | `k = 2` | [3] | [""] | `a` |
| `[` | 压 `k=2`、压 `cur="a"`，重置 | [3,2] | ["", "a"] | `` |
| `c` | 追加 | [3,2] | ["", "a"] | `c` |
| `]` | 弹 str=`"a"`、弹 num=`2` → `"a" + "c"×2` = `acc` | [3] | [""] | `acc` |
| `]` | 弹 str=`""`、弹 num=`3` → `"" + "acc"×3` = `accaccacc` ✅ | [] | [] | `accaccacc` |

答案 `"accaccacc"` ✅
**两个细节**：① 数字要写成 `k = k*10 + d` 才能处理 `"12[a]"`；② 遇 `[` 时把 `cur` 存栈后必须 **`new` 一个新的 StringBuilder**（而不是 `setLength(0)` 清空），否则栈里的引用会被一起清掉。

---

### 739. 每日温度 · 中等

**思路**：**单调递减栈**存下标（栈内温度递减）。当前温度比栈顶高时，栈顶元素就找到了它的「下一个更高温度」，弹栈并记录天数差；否则压栈。

```java
class Solution {
    public int[] dailyTemperatures(int[] t) {
        int n = t.length;
        int[] res = new int[n];                  // 默认 0（后面没有更高温度）
        Deque<Integer> st = new ArrayDeque<>();  // 存下标，栈内温度单调递减
        for (int i = 0; i < n; i++) {
            while (!st.isEmpty() && t[i] > t[st.peek()]) {   // 当前温度比栈顶高
                int j = st.pop();                             // j 的答案找到了
                res[j] = i - j;                               // 等了多少天
            }
            st.push(i);                                       // 自己入栈等待
        }
        return res;
    }
}
```

**复杂度**：时间 `O(n)`（每个下标最多进出栈一次），空间 `O(n)`

**例子讲解**：`temperatures = [73,74,75,71,69,72,76,73]`

| i | 温度 | 弹出 | 记录 | 栈（下标:温度） |
| :-- | :-- | :-- | :-- | :-- |
| 0 | 73 | — | — | [0:73] |
| 1 | 74 | 0 | `res[0]=1` | [1:74] |
| 2 | 75 | 1 | `res[1]=1` | [2:75] |
| 3 | 71 | — | — | [2:75, 3:71] |
| 4 | 69 | — | — | [2:75, 3:71, 4:69] |
| 5 | 72 | 4, 3 | `res[4]=1, res[3]=2` | [2:75, 5:72] |
| 6 | 76 | 5, 2 | `res[5]=1, res[2]=4` | [6:76] |
| 7 | 73 | — | — | [6:76, 7:73] |

栈里剩下下标 6、7（后面没有更高温度，保持默认 0）

答案 `[1,1,4,2,1,1,0,0]` ✅
**为什么存下标而不是温度**：答案要的是**天数差** `i - j`，只存温度就没法算出距离了。

---

### 84. 柱状图中最大的矩形 · 困难

**思路**：**单调递增栈**存下标。当遇到比自己矮的柱子时弹栈，此时弹出柱子的左右边界都已确定：**高 = 弹出柱高，宽 = 右边界 - 左边界 - 1**。两边各加一个高度 0 的**哨兵**，省掉收尾与判空。

```java
class Solution {
    public int largestRectangleArea(int[] h) {
        int n = h.length;
        int[] a = new int[n + 2];
        System.arraycopy(h, 0, a, 1, n);      // 两端补 0 哨兵：保证所有柱子最终都被弹出结算
        Deque<Integer> st = new ArrayDeque<>();
        int ans = 0;
        for (int i = 0; i < a.length; i++) {
            while (!st.isEmpty() && a[st.peek()] > a[i]) {   // 遇到更矮的 → 弹出结算
                int cur = st.pop();                          // cur 是「高」
                // 左边界 = 弹出后新的栈顶，右边界 = i
                ans = Math.max(ans, a[cur] * (i - st.peek() - 1));
            }
            st.push(i);
        }
        return ans;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(n)`

**例子讲解**：`heights = [2,1,5,6,2,3]`，加哨兵后 `a = [0,2,1,5,6,2,3,0]`（下标整体右移 1）

| i | `a[i]` | 动作 | 面积 = 高 × 宽 | ans | 栈 |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 0 | 0 | push | — | 0 | [0] |
| 1 | 2 | push | — | 0 | [0,1] |
| 2 | 1 | 弹 1 | `2 × (2-0-1) = 2` | 2 | [0,2] |
| 3 | 5 | push | — | 2 | [0,2,3] |
| 4 | 6 | push | — | 2 | [0,2,3,4] |
| 5 | 2 | 弹 4 | `6 × (5-3-1) = 6` | 6 | [0,2,3] |
| 5 | 2 | 弹 3 | `5 × (5-2-1) = 10` | **10** ✅ | [0,2] |
| 5 | 2 | push | — | 10 | [0,2,5] |
| 6 | 3 | push | — | 10 | [0,2,5,6] |
| 7 | 0 | 弹 6 | `3 × (7-5-1) = 3` | 10 | [0,2,5] |
| 7 | 0 | 弹 5 | `2 × (7-2-1) = 8` | 10 | [0,2] |
| 7 | 0 | 弹 2 | `1 × (7-0-1) = 6` | 10 | [0] |
| 7 | 0 | push | — | 10 | [0,7] |

答案 `10`（高度 5、宽度 2，对应 `heights[2..3] = [5, 6]`）✅
**为什么宽是 `i - st.peek() - 1`**：弹出 `cur` 后，新栈顶是「左边第一个比 `cur` 矮的柱子」，`i` 是「右边第一个比 `cur` 矮的柱子」，中间这段就是 `cur` 能作为最矮柱子撑起的最大宽度。**两端哨兵高度 0** 保证最后所有柱子都会被弹出结算。

---

## 十三、堆

> 识别信号：**Top-K / 中位数 / 多路归并**。
> **第 K 大用「小顶堆」，第 K 小用「大顶堆」**（堆里留 K 个，堆顶正好是门槛）。

### 215. 数组中的第K个最大元素 · 中等

**思路**：小顶堆维护最大的 k 个，超出就弹堆顶；堆顶即第 k 大。
（更优：**快速选择**，平均 `O(n)`；也可手写堆。）

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();   // 小顶堆
        for (int x : nums) {
            pq.offer(x);
            if (pq.size() > k) pq.poll();                    // 超出 k 个就丢掉最小的
        }
        return pq.peek();                                    // 堆顶 = 第 k 大
    }
}
```

**复杂度**：时间 `O(n log k)`，空间 `O(k)`

**例子讲解**：`nums = [3,2,1,5,6,4]`，`k = 2`（期望第 2 大 = 5）

| 元素 | offer 后堆内容 | 是否超 k | poll | 堆顶 |
| :-- | :-- | :-- | :-- | :-- |
| 3 | [3] | 否 | — | 3 |
| 2 | [2,3] | 否 | — | 2 |
| 1 | [1,2,3] | 是 | 丢 1 | 2 |
| 5 | [2,3,5] | 是 | 丢 2 | 3 |
| 6 | [3,5,6] | 是 | 丢 3 | 5 |
| 4 | [4,5,6] | 是 | 丢 4 | 5 |

返回 **5** ✅（降序排列 `6,5,4,3,2,1`，第 2 个是 5）
**为什么第 K 大要用小顶堆**：堆里始终留「当前最大的 k 个」，堆顶是这 k 个里最小的——也就是门槛。比门槛小的元素进来就会被立刻踢出，最终留下的正是前 k 大。

---

### 347. 前 K 个高频元素 · 中等

**思路**：哈希统计频次 → 小顶堆按频次保留前 k 个。
（更优：**桶排序**按频次分桶，从高频桶往低扫，做到 `O(n)`。）

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : nums) cnt.merge(x, 1, Integer::sum);     // 统计频次
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] - b[1]);   // 按频次的小顶堆
        for (Map.Entry<Integer, Integer> e : cnt.entrySet()) {
            pq.offer(new int[]{e.getKey(), e.getValue()});
            if (pq.size() > k) pq.poll();                     // 只留频次最高的 k 个
        }
        int[] res = new int[k];
        for (int i = 0; i < k; i++) res[i] = pq.poll()[0];    // 取出元素值
        return res;
    }
}
```

**复杂度**：时间 `O(n log k)`，空间 `O(n)`

**例子讲解**：`nums = [1,1,1,2,2,3]`，`k = 2`

频次统计：`{1:3, 2:2, 3:1}`

| 入堆元素 | 堆内容 `[值,频次]` | 是否超 k | poll |
| :-- | :-- | :-- | :-- |
| `[1,3]` | [[1,3]] | 否 | — |
| `[2,2]` | [[2,2], [1,3]] | 否 | — |
| `[3,1]` | [[3,1], [1,3], [2,2]] | 是 | 丢 `[3,1]` |

堆里剩下 `[2,2]` 和 `[1,3]` → 输出 `[2, 1]`（或 `[1, 2]`，题目不要求顺序）

答案 `[1, 2]` ✅
**桶排序思路**（可以口头答，写代码也行）：频次最大不超过 `n`，于是开 `n+1` 个桶，把元素按频次丢进对应桶，再从高频桶往低扫，凑够 k 个就返回——`O(n)` 时间。

---

### 295. 数据流的中位数 · 困难

**思路**：**对顶堆**。`small`（大顶堆）存较小的一半，`large`（小顶堆）存较大的一半，且保持 `small.size()` 始终 ≥ `large.size()` 且差距 ≤ 1。中位数：奇数取 `small` 堆顶，偶数取两堆顶平均。

```java
class MedianFinder {
    PriorityQueue<Integer> small = new PriorityQueue<>((a, b) -> b - a);  // 较小一半（大顶堆）
    PriorityQueue<Integer> large = new PriorityQueue<>();                 // 较大一半（小顶堆）

    public void addNum(int num) {
        if (small.size() == large.size()) {   // 两堆等长：应该让 small 多 1 个
            large.offer(num);                 // 先丢进 large
            small.offer(large.poll());        // 再把 large 里最小的挪给 small
        } else {                              // small 已多 1 个：应该补齐 large
            small.offer(num);                 // 先丢进 small
            large.offer(small.poll());        // 再把 small 里最大的挪给 large
        }
    }
    public double findMedian() {
        return small.size() == large.size()
                ? (small.peek() + large.peek()) / 2.0   // 偶数个：中间两个取平均
                : small.peek();                          // 奇数个：small 堆顶
    }
}
```

**复杂度**：`addNum` 时间 `O(log n)`，`findMedian` 时间 `O(1)`，空间 `O(n)`

**例子讲解**：依次 `addNum(1) → addNum(2) → findMedian() → addNum(3) → findMedian()`

| 操作 | 分支 | 过程 | small（大顶堆） | large（小顶堆） | 返回 |
| :-- | :-- | :-- | :-- | :-- | :-- |
| `addNum(1)` | 等长(0=0) | `large=[1]` → 挪给 small | [1] | [] | — |
| `addNum(2)` | 不等长(1≠0) | `small=[1,2]` → 挪最大值 2 给 large | [1] | [2] | — |
| `findMedian()` | 等长(1=1) | `(1+2)/2` | [1] | [2] | **1.5** |
| `addNum(3)` | 等长(1=1) | `large=[2,3]` → 挪最小值 2 给 small | [1,2] | [3] | — |
| `findMedian()` | 不等长(2≠1) | `small.peek()` | [1,2] | [3] | **2** ✅ |

**「先丢进去再挪回来」的妙处**：不用比较 `num` 和两个堆顶的大小，插入逻辑统一成两行，不会有边界 bug。挪回来的那一步保证了「small 的所有元素 ≤ large 的所有元素」这个核心不变式。

---

## 十四、贪心算法

> 识别信号：**局部最优能推出全局最优**。写之前先问自己「这一步贪心会不会破坏后面的解」。

### 121. 买卖股票的最佳时机 · 简单

**思路**：一次遍历，维护**历史最低价**，用「当前价 - 历史最低价」不断更新最大利润。

```java
class Solution {
    public int maxProfit(int[] p) {
        int min = Integer.MAX_VALUE, ans = 0;   // min: 历史最低买入价
        for (int x : p) {
            min = Math.min(min, x);             // 先更新最低价（今天也能买）
            ans = Math.max(ans, x - min);       // 再算今天卖出能赚多少
        }
        return ans;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`prices = [7, 1, 5, 3, 6, 4]`

| i | 价格 | min | `x - min` | ans |
| :-- | :-- | :-- | :-- | :-- |
| 0 | 7 | 7 | 0 | 0 |
| 1 | 1 | **1** | 0 | 0 |
| 2 | 5 | 1 | 4 | 4 |
| 3 | 3 | 1 | 2 | 4 |
| 4 | 6 | 1 | **5** | **5** ✅ |
| 5 | 4 | 1 | 3 | 5 |

答案 `5`（第 1 天买、第 4 天卖）✅
**顺序不能反**：必须先 `min = min(min, x)` 再算 `x - min`。如果先算利润再更新 `min`，就会出现「用今天的价格买入又今天卖出」的情况（利润为 0 倒也无所谓，但逻辑上应该允许当天买）——实际上两种写法答案一致，但先更新更直观。

---

### 55. 跳跃游戏 · 中等

**思路**：维护**能到达的最远下标 `reach`**。遍历中若 `i > reach` 说明中间断了 → false；否则用 `i + nums[i]` 更新 `reach`。

```java
class Solution {
    public boolean canJump(int[] a) {
        int reach = 0;                          // 当前能到达的最远下标
        for (int i = 0; i < a.length; i++) {
            if (i > reach) return false;        // 这个位置都到不了，后面更别提
            reach = Math.max(reach, i + a[i]);  // 从 i 起跳能覆盖到哪
        }
        return true;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：

**`nums = [2,3,1,1,4]` → true**

| i | `nums[i]` | `i > reach`? | reach |
| :-- | :-- | :-- | :-- |
| 0 | 2 | 否（0 ≤ 0） | `max(0, 2) = 2` |
| 1 | 3 | 否（1 ≤ 2） | `max(2, 4) = 4` |
| 2 | 1 | 否 | `max(4, 3) = 4` |
| 3 | 1 | 否 | `max(4, 4) = 4` |
| 4 | 4 | 否（4 ≤ 4） | `max(4, 8) = 8` |

到最后都没断 → **true** ✅

**`nums = [3,2,1,0,4]` → false**

| i | `nums[i]` | `i > reach`? | reach |
| :-- | :-- | :-- | :-- |
| 0 | 3 | 否 | 3 |
| 1 | 2 | 否 | `max(3, 3) = 3` |
| 2 | 1 | 否 | 3 |
| 3 | 0 | 否（3 ≤ 3） | `max(3, 3) = 3` |
| 4 | 4 | **4 > 3 → return false** ❌ | — |

**贪心的正确性**：如果下标 `i` 可达，那么 `0..i` 之间所有位置都可达（跳跃可以停在任意中间格），所以我们只需要关心「最远能到哪」，不需要记录具体路径。

---

### 45. 跳跃游戏 II · 中等

**思路**：**隐式 BFS 分层**。`end` 是当前这一跳能覆盖的右边界，在 `[i, end]` 内记录下一步能到的最远处 `far`；走到 `end` 就步数 +1 并把 `end` 更新为 `far`。
（注意循环到 `n-1` 即可，最后一格不用再跳。）

```java
class Solution {
    public int jump(int[] a) {
        int steps = 0, end = 0, far = 0;        // end: 当前这跳的覆盖右边界；far: 下一跳能达到的最远
        for (int i = 0; i < a.length - 1; i++) {  // 注意是 n-1，站到最后一格就不用再跳了
            far = Math.max(far, i + a[i]);      // 在当前覆盖范围内，收集下一跳的最远点
            if (i == end) {                     // 走到本层边界 → 必须跳一次
                steps++;
                end = far;                      // 下一跳的边界
            }
        }
        return steps;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`nums = [2,3,1,1,4]`

| i | `nums[i]` | far | `i == end`? | steps | end |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 0 | 2 | `max(0, 2) = 2` | 是（0 == 0） | **1** | 2 |
| 1 | 3 | `max(2, 4) = 4` | 否 | 1 | 2 |
| 2 | 1 | `max(4, 3) = 4` | 是（2 == 2） | **2** | 4 |
| 3 | 1 | `max(4, 4) = 4` | 否 | 2 | 4 |

循环到 `i = 3` 结束（`n-1 = 4` 不处理）→ 返回 **2** ✅
路径：`0 →（跳 2 步到下标 1 或 2）→` 取下标 1，从下标 1 跳 3 步直接到终点。

**为什么是「最少步数」**：`end` 把数组切成若干段，每段代表「跳同样多步能覆盖的范围」。走到段末才计数，等于按层推进——这就是 BFS 的层数。**循环到 `n-1`** 而不是 `n` 很关键：否则站在终点还会被多加一步。

---

### 763. 划分字母区间 · 中等

**思路**：先扫一遍记录**每个字符最后出现的位置**；再扫第二遍，维护当前片段的最远边界 `end`，扫到 `i == end` 说明这段里所有字符都不会再出现在后面，切割。

```java
class Solution {
    public List<Integer> partitionLabels(String s) {
        int[] last = new int[26];                        // 每个字母最后出现的下标
        for (int i = 0; i < s.length(); i++) last[s.charAt(i) - 'a'] = i;
        List<Integer> res = new ArrayList<>();
        int start = 0, end = 0;
        for (int i = 0; i < s.length(); i++) {
            end = Math.max(end, last[s.charAt(i) - 'a']);  // 当前片段至少要延伸到这
            if (i == end) {                               // 走到边界 → 可以切了
                res.add(end - start + 1);
                start = i + 1;                            // 下一段起点
            }
        }
        return res;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`s = "ababcbacadefegdehijhklij"`

字符最后位置：`a:8, b:5, c:7, d:14, e:15, f:11, g:13, h:19, i:22, j:23, k:20, l:21`

| i | 字符 | `last[c]` | end | `i == end`? | 动作 |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 0 | a | 8 | 8 | 否 | — |
| 1 | b | 5 | 8 | 否 | — |
| 2 | a | 8 | 8 | 否 | — |
| … | … | … | 8 | 否 | — |
| 8 | a | 8 | 8 | **是** | 切出长度 `8-0+1 = 9`，start=9 |
| 9 | d | 14 | 14 | 否 | — |
| 10 | e | 15 | 15 | 否 | — |
| … | … | … | 15 | 否 | — |
| 15 | e | 15 | 15 | **是** | 切出长度 `15-9+1 = 7`，start=16 |
| 16 | h | 19 | 19 | 否 | — |
| 17 | i | 22 | 22 | 否 | — |
| 18 | j | 23 | 23 | 否 | — |
| … | … | … | 23 | 否 | — |
| 23 | j | 23 | 23 | **是** | 切出长度 `23-16+1 = 8` |

答案 `[9, 7, 8]` ✅
**为什么贪心正确**：片段一旦开始，就必须包含片段内每个字符的**最后一次出现**（否则同一个字母会跨片段出现，违反「同一字母最多出现在一个片段中」）。取所有 `last` 的最大值作边界，正好是能切的最早位置——切得越早片段越多，满足「尽可能多的片段」。

---

## 十五、动态规划

> **四步定式**：① 定义 `dp[i]` 的含义 → ② 写递推式 → ③ 初始化边界 → ④ 确定遍历顺序 + 能否滚动压缩。
> **背包模型**：
> - 组合/求最小个数（零钱兑换、完全平方数）：`for 物品 for 容量正序`
> - 0/1 背包（分隔等和子集）：`for 物品 for 容量倒序`（每件只用一次）

### 70. 爬楼梯 · 简单

**思路**：`f(n) = f(n-1) + f(n-2)`，滚动两个变量即可（本质是斐波那契）。

```java
class Solution {
    public int climbStairs(int n) {
        int a = 1, pre = 1;                  // a = f(1) = 1, pre = f(0) = 1
        for (int i = 2; i <= n; i++) {       // 从 f(2) 递推到 f(n)
            int t = a + pre;                 // f(i) = f(i-1) + f(i-2)
            pre = a;                         // 前移：pre 变成 f(i-1)
            a = t;                           // a 变成 f(i)
        }
        return a;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`n = 5`（期望 8）

| i | `t = a + pre` | pre | a |
| :-- | :-- | :-- | :-- |
| 初始 | — | 1 (=f0) | 1 (=f1) |
| 2 | `1+1 = 2` | 1 | **2** |
| 3 | `2+1 = 3` | 2 | **3** |
| 4 | `3+2 = 5` | 3 | **5** |
| 5 | `5+3 = 8` | 5 | **8** ✅ |

答案 `8`。验证：5 级的走法有 `11111 / 1112 / 1121 / 1211 / 2111 / 122 / 212 / 221` 共 8 种 ✅
**为什么 `pre` 也初始化成 1**：`f(0) = 1` 是人为定义的空走法（一次不走），有了它 `i = 2` 时 `f(2) = f(1) + f(0) = 2` 才成立。若写成 `pre = 0`，`n = 1` 时返回值会错。

---

### 118. 杨辉三角 · 简单

**思路**：逐行构造，首尾为 1，中间 = 上一行相邻两项之和。

```java
class Solution {
    public List<List<Integer>> generate(int n) {
        List<List<Integer>> res = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            List<Integer> row = new ArrayList<>();
            for (int j = 0; j <= i; j++)
                // 首尾（j==0 或 j==i）为 1，其余为上一行左 + 上一行右
                row.add(j == 0 || j == i ? 1
                        : res.get(i - 1).get(j - 1) + res.get(i - 1).get(j));
            res.add(row);
        }
        return res;
    }
}
```

**复杂度**：时间 `O(n²)`，空间 `O(n²)`

**例子讲解**：`numRows = 5`

| 行 i | 计算过程 | 结果 |
| :-- | :-- | :-- |
| 0 | 首尾合一 | `[1]` |
| 1 | 首 1，尾 1 | `[1, 1]` |
| 2 | 首 1，中间 `1+1=2`，尾 1 | `[1, 2, 1]` |
| 3 | 首 1，中间 `1+2=3`、`2+1=3`，尾 1 | `[1, 3, 3, 1]` |
| 4 | 首 1，中间 `1+3=4`、`3+3=6`、`3+1=4`，尾 1 | `[1, 4, 6, 4, 1]` |

答案 `[[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]` ✅
**下标对应关系**：第 `i` 行的第 `j` 个数（`0 < j < i`）= 上一行第 `j-1` 个 + 上一行第 `j` 个，逐一对应即可，不用管首尾特判（用三目运算符一次搞定）。

---

### 198. 打家劫舍 · 中等

**思路**：`dp[i] = max(dp[i-1]（不偷）, dp[i-2] + nums[i]（偷）)`，滚动两个变量。

```java
class Solution {
    public int rob(int[] nums) {
        int pre = 0, cur = 0;                // pre = dp[i-2], cur = dp[i-1]
        for (int x : nums) {
            int t = Math.max(cur, pre + x);  // 不偷当前（cur）vs 偷当前（pre + x）
            pre = cur;                       // 整体前移
            cur = t;
        }
        return cur;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`nums = [2, 7, 9, 3, 1]`（最优 2+9+1 = 12）

| x | `cur`（=dp[i-1]） | `pre + x`（=dp[i-2]+x） | 新的 cur | 新的 pre |
| :-- | :-- | :-- | :-- | :-- |
| 2 | 0 | `0 + 2 = 2` | **2** | 0 |
| 7 | 2 | `0 + 7 = 7` | **7** | 2 |
| 9 | 7 | `2 + 9 = 11` | **11** ← 偷 9 更划算 | 7 |
| 3 | 11 | `7 + 3 = 10` | **11** | 11 |
| 1 | 11 | `11 + 1 = 12` | **12** ✅ | 11 |

答案 `12`（偷第 1、3、5 家：2 + 9 + 1）✅
**状态含义要记牢**：`cur` 是「考虑前 i 家能偷到的最大金额」，`pre` 是「前 i-1 家」。如果偷第 i 家，第 i-1 家就不能偷，所以要配上 `pre`；不偷的话直接继承 `cur`。

---

### 279. 完全平方数 · 中等

**思路**：**完全背包求最少个数**。`dp[i]` = 组成 `i` 的最少平方数个数，枚举最后一个平方数 `j²`：`dp[i] = min(dp[i], dp[i - j²] + 1)`。

```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        Arrays.fill(dp, Integer.MAX_VALUE);   // 先置为「不可达」
        dp[0] = 0;                            // 组 0 需要 0 个
        for (int i = 1; i <= n; i++)
            for (int j = 1; j * j <= i; j++)  // 枚举所有 ≤ i 的平方数
                dp[i] = Math.min(dp[i], dp[i - j * j] + 1);
        return dp[n];
    }
}
```

**复杂度**：时间 `O(n√n)`，空间 `O(n)`

**例子讲解**：`n = 12`（期望 3，即 `4 + 4 + 4`）

| i | 可选平方数 | 候选值 `dp[i-j²]+1` | dp[i] |
| :-- | :-- | :-- | :-- |
| 0 | — | — | 0 |
| 1 | 1 | `dp[0]+1 = 1` | 1 |
| 2 | 1 | `dp[1]+1 = 2` | 2 |
| 3 | 1 | `dp[2]+1 = 3` | 3 |
| 4 | 1, **4** | `dp[3]+1 = 4`；`dp[0]+1 = **1**` | **1** |
| 5 | 1, 4 | `dp[4]+1 = 2`；`dp[1]+1 = 2` | 2 |
| 8 | 1, 4 | `dp[7]+1 = 5`；`dp[4]+1 = **2**` | **2** |
| 9 | 1, 4, **9** | …；`dp[0]+1 = **1**` | **1** |
| 10 | 1, 4, 9 | `dp[9]+1 = **2**` | 2 |
| 11 | 1, 4, 9 | `dp[10]+1 = 3`；`dp[7]+1 = 5`；`dp[2]+1 = 3` | 3 |
| 12 | 1, 4, 9 | `dp[11]+1 = 4`；**`dp[8]+1 = 3`**；`dp[3]+1 = 4` | **3** ✅ |

答案 `3`（`12 = 4 + 4 + 4`）✅
**和 #322 零钱兑换的关系**：两题结构完全一样——#322 的「硬币」是给定数组，本题的「硬币」是所有平方数。所以内层循环 `for (int j = 1; j*j <= i; j++)` 等价于「遍历所有面额的硬币」。**容量正序**是因为每种硬币可以用无限次（完全背包）。

---

### 322. 零钱兑换 · 中等

**思路**：完全背包求最少硬币数。`dp[j] = min(dp[j], dp[j - coin] + 1)`。**容量正序遍历**（保证同一硬币可重复用）；用 `amount + 1` 当无穷大，最后判不可达。

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);              // 用「答案不可能达到的大值」表示不可达
        dp[0] = 0;                                // 凑 0 元需要 0 枚
        for (int c : coins)                       // 外层：物品
            for (int j = c; j <= amount; j++)     // 内层：容量正序 → 完全背包
                dp[j] = Math.min(dp[j], dp[j - c] + 1);
        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

**复杂度**：时间 `O(amount × k)`，空间 `O(amount)`

**例子讲解**：`coins = [1, 2, 5]`，`amount = 11`（期望 3 = 5+5+1）

按物品逐个更新（每轮在上一轮基础上继续叠加，所以是「可重复使用」）：

| 处理完 | dp[0] | dp[1] | dp[2] | dp[3] | dp[4] | dp[5] | dp[6] | … | dp[11] |
| :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- |
| 初始 | 0 | ∞ | ∞ | ∞ | ∞ | ∞ | ∞ | ∞ | ∞ |
| 硬币 1 | 0 | . 1 | 2 | 3 | 4 | 5 | 6 | … | 11 |
| 硬币 2 | 0 | 1 | **1** | 2 | **2** | 3 | **3** | … | 6 |
| 硬币 5 | 0 | 1 | 1 | 2 | 2 | **1** | **2** | … | **3** ✅ |

答案 `3`（`5 + 5 + 1`）✅
**两个关键点**：
① **容量正序** = 完全背包（物品可重复）。如果写成倒序 `for (int j = amount; j >= c; j--)`，就变成 0/1 背包（每枚硬币只能用一次），本题会算错。
② **不可达值不要用 `Integer.MAX_VALUE`**：`dp[j - c] + 1` 会整型溢出。用 `amount + 1`（答案最多就是 `amount` 枚 1 元）既安全又能用 `> amount` 判不可达。

---

### 139. 单词拆分 · 中等

**思路**：`dp[i]` 表示前 `i` 个字符能否被拆分。枚举切分点 `j`：只要 `dp[j]` 为真且 `s[j..i)` 在字典中，`dp[i]` 即为真。

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> set = new HashSet<>(wordDict);     // 改成 Set，查询 O(1)
        int n = s.length();
        boolean[] dp = new boolean[n + 1];
        dp[0] = true;                                  // 空串天然可拆
        for (int i = 1; i <= n; i++)                   // 枚举前缀长度 i
            for (int j = 0; j < i; j++)                // 枚举最后一个单词的起点 j
                if (dp[j] && set.contains(s.substring(j, i))) {
                    dp[i] = true;
                    break;                             // 找到一种拆法即可
                }
        return dp[n];
    }
}
```

**复杂度**：时间 `O(n² · L)`（L 为子串哈希耗时），空间 `O(n)`

**例子讲解**：`s = "leetcode"`，`wordDict = ["leet", "code"]`

`n = 8`，`dp[0] = true`

| i | j 循环 | 判断 | dp[i] |
| :-- | :-- | :-- | :-- |
| 1 | j=0 | `dp[0]` ✓ 但 `"l"` 不在字典 ✗ | false |
| 2 | j=0,1 | `"le"`、`"e"` 都不在 ✗ | false |
| 3 | j=0..2 | `"lee"` 等都不在 ✗ | false |
| **4** | j=0 | `dp[0]` ✓ 且 `"leet"` **在字典** ✓ | **true** |
| 5 | j=0..3 | `"leetc"`、`"eetc"`… 都不在 | false |
| 6 | j=0..5 | 都不在 | false |
| 7 | j=0..6 | 都不在 | false |
| **8** | j=4 | `dp[4]` ✓ 且 `"code"` **在字典** ✓ | **true** ✅ |

答案 `true` ✅
**`dp` 的含义要精确**：`dp[i]` 说的是「`s` 的**前 i 个字符**能拆」，不是「以第 i 个字符结尾」。所以循环边界是 `i <= n`、子串是 `s.substring(j, i)`（左闭右开，长度正好 `i - j`）。

---

### 300. 最长递增子序列 · 中等

**思路**：朴素 `dp[i]` = 以 `i` 结尾的 LIS 长度，`O(n²)`。
**优化（会写这个就够）**：维护 `tails` 数组，`tails[len]` = 长度为 `len+1` 的 LIS 的最小结尾值。遍历时**二分找第一个 ≥ x 的位置并替换**（相当于「把牌接在最合适的位置上」），`tails` 的增长长度就是答案。
> 注意：`tails` 本身不是某个真实 LIS，只是长度正确。

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] tails = new int[nums.length];      // tails[k] = 长度 k+1 的 LIS 的最小结尾
        int size = 0;                            // 当前 LIS 长度
        for (int x : nums) {
            int l = 0, r = size;
            while (l < r) {                      // 二分找第一个 >= x 的位置
                int m = (l + r) >>> 1;
                if (tails[m] < x) l = m + 1; else r = m;
            }
            tails[l] = x;                        // 替换（或追加）
            if (l == size) size++;               // 追加到末尾 → 长度 +1
        }
        return size;
    }
}
```

**复杂度**：时间 `O(n log n)`，空间 `O(n)`

**例子讲解**：`nums = [10, 9, 2, 5, 3, 7, 101, 18]`（答案 4，如 `2,3,7,18`）

| x | 二分结果（第一个 ≥ x 的位置） | 动作 | tails | size |
| :-- | :-- | :-- | :-- | :-- |
| 10 | l=0 | 替换 tails[0] | `[10]` | 1 |
| 9 | l=0 | 替换 tails[0] | `[9]` | 1 |
| 2 | l=0 | 替换 tails[0] | `[2]` | 1 |
| 5 | l=1 | 追加 | `[2, 5]` | 2 |
| 3 | l=1 | 替换 tails[1] | `[2, 3]` | 2 |
| 7 | l=2 | 追加 | `[2, 3, 7]` | 3 |
| 101 | l=3 | 追加 | `[2, 3, 7, 101]` | 4 |
| 18 | l=3 | 替换 tails[3] | `[2, 3, 7, 18]` | **4** ✅ |

答案 `4` ✅
**替换而不是插入的含义**：把 `x` 放到「第一个 ≥ 它的位置」，是让**同样长度下的结尾尽可能小**——结尾越小，后面越容易接上更长的序列。这就是贪心思想，配合二分把 `O(n²)` 降到 `O(n log n)`。
**为什么 `tails` 不是真实的 LIS**：上例最后 `tails = [2,3,7,18]` 恰好是真实 LIS；但换成 `[3,4,1,2]`，`tails` 最终是 `[1,2]`，长度 2 正确，内容却和前两个元素 `3,4` 无关。**只用它的长度**。

---

### 152. 乘积最大子数组 · 中等

**思路**：同时维护**以 i 结尾的最大积 `max` 和最小积 `min`**（最小值乘上负数会翻成最大）。遇负数时先交换 `max` 与 `min`。

```java
class Solution {
    public int maxProduct(int[] a) {
        int max = a[0], min = a[0], ans = a[0];   // 都以 a[0] 结尾
        for (int i = 1; i < a.length; i++) {
            if (a[i] < 0) { int t = max; max = min; min = t; }   // 负数会让大小关系翻转
            max = Math.max(a[i], max * a[i]);     // 要么自立门户，要么乘上之前的
            min = Math.min(a[i], min * a[i]);
            ans = Math.max(ans, max);
        }
        return ans;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`nums = [2, 3, -2, 4]`（答案 6，子数组 `[2,3]`）

| i | a[i] | 是否交换 | max | min | ans |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 0 | 2 | — | 2 | 2 | 2 |
| 1 | 3 | 否 | `max(3, 2×3)=6` | `min(3, 2×3)=3` | **6** |
| 2 | -2 | **是**（先换：max=3, min=6） | `max(-2, 3×-2=-6) = -2` | `min(-2, 6×-2=-12) = -12` | 6 |
| 3 | 4 | 否 | `max(4, -2×4=-8) = 4` | `min(4, -12×4=-48) = -48` | 6 ✅ |

**为什么必须维护 `min`**：单独看 `[2, 3, -2, 4]` 里的 `-12`（来自 `2×3×-2`）虽然现在最小，但只要后面再乘一个负数（比如 `-2 × -12 = 24`）就能变成最大。这正是 `[−2,3,−4]` 这类用例答案落在负数乘积上的原因。**遇到负数先交换**，等价于「用上一轮的最小值去乘负数」，逻辑上是同一件事。

---

### 416. 分割等和子集 · 中等

**思路**：转换成 **0/1 背包**：能否选出子集和恰为 `sum/2`。`dp[j]` 表示能否凑出和 `j`；**容量必须倒序遍历**（每件物品只用一次）。

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int x : nums) sum += x;
        if ((sum & 1) == 1) return false;          // 和为奇数不可能平分
        int target = sum / 2;
        boolean[] dp = new boolean[target + 1];
        dp[0] = true;                              // 凑 0 永远可以（一个都不选）
        for (int x : nums)                         // 外层：物品
            for (int j = target; j >= x; j--)      // 内层：容量倒序 → 0/1 背包
                dp[j] = dp[j] || dp[j - x];        // 不选 x 或 选 x
        return dp[target];
    }
}
```

**复杂度**：时间 `O(n × target)`，空间 `O(target)`

**例子讲解**：`nums = [1, 5, 11, 5]`，`sum = 22`，`target = 11`

按物品逐个处理（`dp` 只记录 true 的位置）：

| 处理物品 | 倒序更新得到的 true 位置 | 含义 |
| :-- | :-- | :-- |
| 初始 | `{0}` | — |
| 1 | `{0, 1}` | 能凑出 0、1 |
| 5 | `{0, 1, 5, 6}` | 新增 5 和 1+5 |
| 11 | `{0, 1, 5, 6, 11, 12, 16, 17}` | **出现 11** → 可提前返回 ✅ |

答案 `true`（分成 `[11]` 和 `[1,5,5]`，两边和都是 11）✅
**为什么必须倒序**：如果正序更新，`dp[j - x]` 可能已经是「本轮刚被 `x` 更新过」的值，等于同一个 `x` 被用了两次（退化成完全背包）。倒序能保证 `dp[j - x]` 还是上一轮（没选 `x` 时）的状态。
**先判奇偶**：`sum` 是奇数直接 false，这是最省事也最容易拿分的剪枝。

---

### 32. 最长有效括号 · 困难

**思路**：**栈存下标，栈底永远放「最后一个未匹配的右括号位置」**（初始为 -1）。遇 `(` 压下标；遇 `)` 先弹，若栈空说明当前 `)` 无法匹配，把它作为新基准压入；否则 `i - stack.peek()` 就是以 `i` 结尾的最长有效长度。

```java
class Solution {
    public int longestValidParentheses(String s) {
        Deque<Integer> st = new ArrayDeque<>();
        st.push(-1);                          // 基准：假装在 -1 处有个「无法匹配的右括号」
        int ans = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') st.push(i);   // 左括号：压下标等匹配
            else {
                st.pop();                         // 右括号：先弹掉一个左括号（或基准）
                if (st.isEmpty()) st.push(i);     // 弹空了 → 当前 ) 无法匹配，成为新基准
                else ans = Math.max(ans, i - st.peek());   // 栈顶是上一个未匹配位置
            }
        }
        return ans;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(n)`
> 另有 `O(1)` 空间做法：从左到右、从右到左各扫一遍，用左右括号计数平衡求解。

**例子讲解**：`s = ")()())"`（期望 4，即 `"()()"`）

| i | 字符 | 动作 | 栈 | ans |
| :-- | :-- | :-- | :-- | :-- |
| 起 | — | 初始化 | [-1] | 0 |
| 0 | `)` | pop → 空 → push 0 | [0] | 0 |
| 1 | `(` | push 1 | [0, 1] | 0 |
| 2 | `)` | pop 1 → 非空 → `2 - 0 = 2` | [0] | **2** |
| 3 | `(` | push 3 | [0, 3] | 2 |
| 4 | `)` | pop 3 → 非空 → `4 - 0 = 4` | [0] | **4** ✅ |
| 5 | `)` | pop 0 → 空 → push 5 | [5] | 4 |

答案 `4` ✅

另一个例子 `s = "(()"`：

| i | 字符 | 动作 | 栈 | ans |
| :-- | :-- | :-- | :-- | :-- |
| 起 | — | 初始化 | [-1] | 0 |
| 0 | `(` | push 0 | [-1, 0] | 0 |
| 1 | `(` | push 1 | [-1, 0, 1] | 0 |
| 2 | `)` | pop 1 → 非空 → `2 - 0 = 2` | [-1, 0] | **2** |

答案 `2` ✅
**核心不变式**：**栈顶始终是「上一个无法被匹配的位置」**。所以 `i - st.peek()` 就是「以 i 结尾的有效括号串长度」。理解这一点，整题就通了——`-1` 和「无法匹配的 `)` 的下标」都是同一类东西。

---

## 十六、多维动态规划

> 二维 DP 通用套路：**先定 `dp[i][j]` 的含义 → 看 `(i,j)` 能从哪几个格子转移过来 → 处理第一行/第一列边界 → 尝试滚动成一维**。
> 字符串双序列问题（LCS、编辑距离）几乎都是「两个字符串各取前 i / 前 j 个」的表格。

### 62. 不同路径 · 中等

**思路**：`dp[i][j] = dp[i-1][j] + dp[i][j-1]`（只能从上方或左方过来）。滚动成一维后：`dp[j] += dp[j-1]`。

```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[] dp = new int[n];
        Arrays.fill(dp, 1);          // 第一行：每个格子只有「一路向右」一种走法
        for (int i = 1; i < m; i++)          // 从第 1 行开始
            for (int j = 1; j < n; j++)      // 第 0 列永远是 1，不用改
                dp[j] += dp[j - 1];          // 旧 dp[j]=上方, dp[j-1]=左方（本轮已更新）
        return dp[n - 1];
    }
}
```

**复杂度**：时间 `O(mn)`，空间 `O(n)`

**例子讲解**：`m = 3`，`n = 7`（期望 28）

初始 `dp = [1, 1, 1, 1, 1, 1, 1]`

| 行 i | 逐列更新后 dp |
| :-- | :-- |
| 1 | `[1, 2, 3, 4, 5, 6, 7]`（每个 `dp[j] += 左边`） |
| 2 | `[1, 3, 6, 10, 15, 21, 28]` |
| — | 返回 `dp[6] = 28` ✅ |

验证：`3 × 7` 的网格从左上到右下要走 6 步向右、2 步向下，组合数 `C(8,2) = 28` ✅
**为什么 `dp[j - 1]` 已经是「左方」**：内层 `j` 从小到大，`dp[j-1]` 在这一行**刚刚被更新过**，代表「左方（同一行的前一个）」。而 `dp[j]` 本身还没更新，代表「上方（上一行同列）」。这就是滚动数组能成立的原因。

---

### 64. 最小路径和 · 中等

**思路**：`dp[j] = grid[i][j] + min(dp[j]（上方）, dp[j-1]（左方）)`；第一列只能从上方来，单独处理。

```java
class Solution {
    public int minPathSum(int[][] g) {
        int n = g[0].length;
        int[] dp = new int[n];
        dp[0] = g[0][0];
        for (int j = 1; j < n; j++) dp[j] = dp[j - 1] + g[0][j];   // 第一行只能一路从左边来
        for (int i = 1; i < g.length; i++)
            for (int j = 0; j < n; j++)
                // j==0 时只能从上方来，所以只有一个候选
                dp[j] = g[i][j] + (j == 0 ? dp[j] : Math.min(dp[j], dp[j - 1]));
        return dp[n - 1];
    }
}
```

**复杂度**：时间 `O(mn)`，空间 `O(n)`

**例子讲解**：`grid = [[1,3,1],[1,5,1],[4,2,1]]`（期望 7，路径 `1→3→1→1→1`）

初始化第一行：`dp = [1, 1+3=4, 4+1=5]`

| 行 i | 列 j | `g[i][j]` | 候选 | 新 dp[j] | dp |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 1 | 0 | 1 | `dp[0]=1`（上方） | `1+1 = 2` | `[2, 4, 5]` |
| 1 | 1 | 5 | `min(4, 2) = 2` | `5+2 = 7` | `[2, 7, 5]` |
| 1 | 2 | 1 | `min(5, 7) = 5` | `1+5 = 6` | `[2, 7, 6]` |
| 2 | 0 | 4 | `dp[0]=2` | `4+2 = 6` | `[6, 7, 6]` |
| 2 | 1 | 2 | `min(7, 6) = 6` | `2+6 = 8` | `[6, 8, 6]` |
| 2 | 2 | 1 | `min(6, 8) = 6` | `1+6 = 7` | `[6, 8, 7]` |

返回 `dp[2] = 7` ✅ 路径：`1 → 3 → 1 → 1 → 1`（和 = 7）
**为什么第 0 列要特判**：`dp[0]` 的含义是「走到本行第 0 列的最小路径和」，而第 0 列没有左方格子，只能从上方来，写成 `min(dp[j], dp[j-1])` 会把上一行的 `dp[n-1]` 误当左方。

---

### 5. 最长回文子串 · 中等

**思路**：**中心扩展**。枚举每个中心（`d=0` 奇数长度、`d=1` 偶数长度），向两边扩到不相等为止，长度 = `r - l - 1`。
（区间 DP 写法：`dp[l][r] = s[l]==s[r] && (r-l<2 || dp[l+1][r-1])`。）

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length(), start = 0, max = 1;
        for (int i = 0; i < n; i++)
            for (int d = 0; d < 2; d++) {           // d=0 奇数中心，d=1 偶数中心
                int l = i, r = i + d;               // 偶数中心时 r 从 i+1 开始
                while (l >= 0 && r < n && s.charAt(l) == s.charAt(r)) {
                    l--; r++;                       // 向两边扩
                }
                // 退出时 l/r 已越界一格，实际回文区间是 [l+1, r-1]，长度 r-l-1
                if (r - l - 1 > max) { max = r - l - 1; start = l + 1; }
            }
        return s.substring(start, start + max);
    }
}
```

**复杂度**：时间 `O(n²)`，空间 `O(1)`
> 进阶 Manacher 算法可做到 `O(n)`（Hot 100 不要求）。

**例子讲解**：`s = "babad"`（答案 `"bab"`）

以 `i = 1`（字符 `b`）为中心：

- `d = 0`（奇数中心）：`l = r = 1`
  - `s[1]==s[1]` ✓ → `l=0, r=2`
  - `s[0]='b' == s[2]='b'` ✓ → `l=-1, r=3`
  - `l < 0` → 停
  - 长度 `= 3 - (-1) - 1 = 3` > `max(1)` → `max = 3, start = 0` → `"bab"` ✅
- `d = 1`（偶数中心）：`l = 1, r = 2`
  - `s[1]='a' != s[2]='b'` ✗ → 立即停
  - 长度 `= 2 - 1 - 1 = 0`

以 `i = 2`（字符 `a`）为中心：`d=0` 时扩到 `l=0, r=4`，`s[0]='b' != s[4]='d'` → 长度 `4-0-1 = 3`，不 `>` `max` → 不更新。

最终 `max = 3, start = 0` → 返回 `"bab"` ✅（`"aba"` 同样合法，题目任选其一）
**长度公式推导**：`while` 退出时 `l` 和 `r` 都已经「多走一格」（一个越界或字符不相等），所以真实回文是 `[l+1, r-1]`，长度 = `(r-1) - (l+1) + 1 = r - l - 1`。这个公式不用记，画一次就记住了。

---

### 1143. 最长公共子序列 · 中等

**思路**：`dp[i][j]` = `a` 前 i 个与 `b` 前 j 个的 LCS 长度。
字符相等 → `dp[i-1][j-1] + 1`；不等 → `max(dp[i-1][j], dp[i][j-1])`。

```java
class Solution {
    public int longestCommonSubsequence(String a, String b) {
        int m = a.length(), n = b.length();
        int[][] dp = new int[m + 1][n + 1];      // dp[i][j] 对应 a 前 i 个、b 前 j 个
        for (int i = 1; i <= m; i++)
            for (int j = 1; j <= n; j++)
                dp[i][j] = (a.charAt(i - 1) == b.charAt(j - 1))
                        ? dp[i - 1][j - 1] + 1                       // 字符相同 → 接在斜上方
                        : Math.max(dp[i - 1][j], dp[i][j - 1]);      // 不同 → 取两者较优
        return dp[m][n];
    }
}
```

**复杂度**：时间 `O(mn)`，空间 `O(mn)`（可滚动压到 `O(n)`）

**例子讲解**：`text1 = "abcde"`，`text2 = "ace"`（LCS = `"ace"`，长度 3）

`dp` 表格（行 = `a` 的前 i 个，列 = `b` 的前 j 个）：

| `dp[i][j]` | j=0 `""` | j=1 `a` | j=2 `c` | j=3 `e` |
| :-- | :-- | :-- | :-- | :-- |
| i=0 `""` | 0 | 0 | 0 | 0 |
| i=1 `a` | 0 | **1**（a==a → `dp[0][0]+1`） | 1 | 1 |
| i=2 `ab` | 0 | 1 | 1 | 1 |
| i=3 `abc` | 0 | 1 | **2**（c==c → `dp[2][1]+1`） | 2 |
| i=4 `abcd` | 0 | 1 | 2 | 2 |
| i=5 `abcde` | 0 | 1 | 2 | **3**（e==e → `dp[4][2]+1`） ✅ |

返回 `dp[5][3] = 3` ✅
**`i-1` / `j-1` 的对应**：`dp[i][j]` 是「前 i 个」，所以第 i 个字符是 `a.charAt(i-1)`。这个偏移是最容易写错的地方。
**为什么不同时取三个候选**：`max(dp[i-1][j], dp[i][j-1])` 已经涵盖了 `dp[i-1][j-1]`（因为 `dp` 单调不减），写三目更简洁。

---

### 72. 编辑距离 · 困难（中等难度实感）

**思路**：`dp[i][j]` = `a` 前 i 个变成 `b` 前 j 个的最少操作数。
- 字符相同：`dp[i][j] = dp[i-1][j-1]`（不用操作）
- 不同：`1 + min(删除 dp[i-1][j], 插入 dp[i][j-1], 替换 dp[i-1][j-1])`

下面是**滚动数组版**：`pre` 保存 `dp[i-1][j-1]`，`dp[j]` 在更新前保存的旧值就是 `dp[i-1][j]`。

```java
class Solution {
    public int minDistance(String a, String b) {
        int m = a.length(), n = b.length();
        int[] dp = new int[n + 1];
        for (int j = 0; j <= n; j++) dp[j] = j;      // a 为空：需要插入 j 个字符
        for (int i = 1; i <= m; i++) {
            int pre = dp[0];                          // pre 保存 dp[i-1][j-1]
            dp[0] = i;                                // b 为空：需要删除 i 个字符
            for (int j = 1; j <= n; j++) {
                int t = dp[j];                        // 先存下 dp[i-1][j]（旧值 = 上方）
                dp[j] = (a.charAt(i - 1) == b.charAt(j - 1))
                        ? pre                         // 相同 → 继承斜上方
                        : Math.min(Math.min(dp[j], dp[j - 1]), pre) + 1;   // 删 / 增 / 换
                pre = t;                              // 为下一个 j 准备斜上方
            }
        }
        return dp[n];
    }
}
```

**复杂度**：时间 `O(mn)`，空间 `O(n)`

**例子讲解**：`word1 = "horse"`，`word2 = "ros"`（答案 3：`horse → rorse → rose → ros`）

初始 `dp = [0, 1, 2, 3]`（`word2` 为空时依次插入）

| i | `a[i-1]` | 更新后 dp | 说明 |
| :-- | :-- | :-- | :-- |
| 1 | h | `[1, 1, 2, 3]` | h vs r 不同 → `min(1,1,0)+1 = 1`；h vs o/s 都要 2、3 次 |
| 2 | o | `[2, 2, 1, 2]` | o == o → `dp[2] = pre = 1`（"ho"→"ro" 只需替换 h） |
| 3 | r | `[3, 2, 2, 2]` | r == r → `dp[1] = pre = 2` |
| 4 | s | `[4, 3, 3, 2]` | s == s → `dp[3] = pre = 2` |
| 5 | e | `[5, 4, 4, 3]` | e 与 e 不匹配（b 第 3 位是 s）→ 取 min 加 1 → 3 ✅ |

返回 `dp[3] = 3` ✅
**三种操作对应哪一格**（记住这张图就够）：
- **删除** `a` 的字符 → 看**上方** `dp[i-1][j]`
- **插入** `b` 的字符 → 看**左方** `dp[i][j-1]`
- **替换** → 看**左上** `dp[i-1][j-1]`（字符相同时则免费继承）

---

## 十七、技巧

### 136. 只出现一次的数字 · 简单

**思路**：**异或**。`a ^ a = 0`、`a ^ 0 = a`、满足交换律 → 成对的数全抵消，剩下即答案。

```java
class Solution {
    public int singleNumber(int[] nums) {
        int x = 0;
        for (int v : nums) x ^= v;    // 成对的数两两抵消为 0，最后只剩单个的
        return x;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`nums = [4, 1, 2, 1, 2]`

| 步 | 累加值 | 说明 |
| :-- | :-- | :-- |
| 起 | 0 | — |
| `^4` | `0^4 = 4` | 4 |
| `^1` | `4^1 = 5` | — |
| `^2` | `5^2 = 7` | — |
| `^1` | `7^1 = 6` | 抵消掉了 1 |
| `^2` | `6^2 = 4` | 抵消掉了 2 ✅ |

答案 `4` ✅
**三个异或性质**：① `a ^ a = 0`（自己抵消）；② `a ^ 0 = a`（单位元）；③ 满足交换律和结合律（顺序无关）。三者合起来，就相当于「把所有成对的数划掉」。
**推广**：若其他数字都出现 3 次、只有一个出现 1 次，可以按二进制位统计「每位 1 的个数 mod 3」。

---

### 169. 多数元素 · 简单

**思路**：**摩尔投票**。候选者 `cnt` 为 0 就换人；相同 `+1`，不同 `-1`。票数超过一半的元素一定活到最后。

```java
class Solution {
    public int majorityElement(int[] nums) {
        int cand = 0, cnt = 0;
        for (int x : nums) {
            if (cnt == 0) cand = x;               // 上一个候选人被抵消光了，换人
            cnt += (x == cand) ? 1 : -1;          // 同票 +1，异票 -1
        }
        return cand;
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`nums = [2, 2, 1, 1, 1, 2, 2]`

| x | `cnt == 0`? | cand | cnt 变化 | cnt |
| :-- | :-- | :-- | :-- | :-- |
| 2 | 是 | **2** | `+1` | 1 |
| 2 | 否 | 2 | `+1` | 2 |
| 1 | 否 | 2 | `-1` | 1 |
| 1 | 否 | 2 | `-1` | **0** |
| 1 | 是 | **1** | `+1` | 1 |
| 2 | 否 | 1 | `-1` | **0** |
| 2 | 是 | **2** | `+1` | 1 |

返回 **2** ✅
**为什么一定正确**：设多数元素出现 `k` 次、其余共 `m` 次，且 `k > m`。把「多数元素」记为 +1、「其他」记为 -1，总和 `k - m > 0`。每次 `cnt` 归零相当于丢掉一段「数量相等」的区间，剩余数组里多数元素仍是多数——所以最后留下的一定是它。

---

### 75. 颜色分类 · 中等

**思路**：**三指针一趟扫描**。`p0` 是 0 区右边界，`p2` 是 2 区左边界，`i` 扫描：
- `0` → 与 `p0` 交换，`p0++`、`i++`（换过来的必是 1）
- `2` → 与 `p2` 交换，`p2--`（换过来的是未处理值，**`i` 不能动**）
- `1` → `i++`

```java
class Solution {
    public void sortColors(int[] a) {
        int p0 = 0, i = 0, p2 = a.length - 1;
        while (i <= p2) {                            // i 越过 p2 就结束（2 区已排好）
            if (a[i] == 0) {
                int t = a[i]; a[i] = a[p0]; a[p0] = t;   // 0 丢到左区
                p0++; i++;                               // 两个指针都前进
            } else if (a[i] == 2) {
                int t = a[i]; a[i] = a[p2]; a[p2] = t;   // 2 丢到右区
                p2--;                                    // 只动 p2，i 不动（新换来的值还没看）
            } else i++;                                  // 1 留在中间，直接前进
        }
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`nums = [2, 0, 2, 1, 1, 0]`

| 轮 | i | `a[i]` | 动作 | 数组 | p0 | p2 |
| :-- | :-- | :-- | :-- | :-- | :-- | :-- |
| 1 | 0 | 2 | 与 `p2=5` 交换，`p2--` | `[0,0,2,1,1,2]` | 0 | 4 |
| 2 | 0 | 0 | 与 `p0=0` 交换（自身），`p0++, i++` | `[0,0,2,1,1,2]` | 1 | 4 |
| 3 | 1 | 0 | 与 `p0=1` 交换（自身），`p0++, i++` | `[0,0,2,1,1,2]` | 2 | 4 |
| 4 | 2 | 2 | 与 `p2=4` 交换，`p2--` | `[0,0,1,1,2,2]` | 2 | 3 |
| 5 | 2 | 1 | `i++` | `[0,0,1,1,2,2]` | 2 | 3 |
| 6 | 3 | 1 | `i++` | `[0,0,1,1,2,2]` | 2 | 3 |
| — | 4 | — | `i(4) > p2(3)` → 结束 | — | — | — |

答案 `[0,0,1,1,2,2]` ✅
**为什么 `a[i]==0` 时 `i` 可以前进**：`p0 <= i` 恒成立，`a[p0]` 要么是 1（被换到 `i` 位置，仍是「1」需要保留在中间），要么 `p0 == i`（自身交换）。两种情况新 `a[i]` 都确定是 1，可以放心 `i++`。
**为什么 `a[i]==2` 时 `i` 不能动**：从 `p2` 换过来的值是**还没检查过**的，可能是 0/1/2，必须留在原位下一轮再看。

---

### 31. 下一个排列 · 中等

**思路**：三步走。
① **从右往左找第一个升序对** `a[i] < a[i+1]`（`i` 就是要变大的那位）；
② 再从右往左找**第一个比 `a[i]` 大的数**，与之交换；
③ 把 `i+1` 之后的部分**反转成升序**（原本是降序，反转即最小的排列）。
若找不到升序对（整体降序），直接反转整个数组。

```java
class Solution {
    public void nextPermutation(int[] a) {
        int i = a.length - 2;
        while (i >= 0 && a[i] >= a[i + 1]) i--;        // ① 找「峰顶左边」的位置
        if (i >= 0) {                                  // 找到才交换（i<0 说明整体降序）
            int j = a.length - 1;
            while (a[j] <= a[i]) j--;                  // ② 从右找第一个 > a[i] 的数
            int t = a[i]; a[i] = a[j]; a[j] = t;
        }
        for (int l = i + 1, r = a.length - 1; l < r; l++, r--) {   // ③ 后缀反转成升序
            int t = a[l]; a[l] = a[r]; a[r] = t;
        }
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：

**`nums = [1, 2, 3]` → `[1, 3, 2]`**
1. 从右找升序对：`a[1]=2 < a[2]=3` → `i = 1`
2. 从右找第一个 `> 2` 的：`3`（下标 2）→ 交换 → `[1, 3, 2]`
3. 反转 `i+1 = 2` 之后的部分（只有一个元素，无变化）

答案 `[1, 3, 2]` ✅

**`nums = [3, 2, 1]`（最大排列）→ `[1, 2, 3]`**
1. `i` 从 1 递减：`a[1]=2 >= a[2]=1` → `i=0`；`a[0]=3 >= a[1]=2` → `i=-1`，退出
2. `i < 0` → 跳过交换
3. 反转 `i+1 = 0` 之后的部分 → 整个数组反转 → `[1, 2, 3]` ✅

**`nums = [1, 1, 5]` → `[1, 5, 1]`**
1. `i = 1`（`1 < 5`）
2. `j = 2`（`5 > 1`）→ 交换 → `[1, 5, 1]`
3. 反转 `[2, 2]` 无变化 ✅

**为什么第 ③ 步是「反转」**：`i+1` 之后的部分必然是**降序**（因为 `i` 是第一个升序对的位置）。降序的下一排列就是它自己的反序，也就是最小的升序排列。**注意不要漏掉 `i < 0` 时也要反转整个数组**，这是最容易漏的一个边界。

---

### 287. 寻找重复数 · 中等

**思路**：把 `i → nums[i]` 看成**函数图**，因为值域是 `[1, n]`，重复的数就是**入环点**——于是转化为「环形链表 II」，用快慢指针 `O(1)` 空间求解。
（替代解法：二分答案 / 原地哈希标记。）

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int s = 0, f = 0;
        do {                                  // 阶段一：找环内相遇点
            s = nums[s];                      // 慢指针走一步
            f = nums[nums[f]];                // 快指针走两步
        } while (s != f);
        s = 0;                                // 阶段二：一个指针回到起点
        while (s != f) {                      // 同速前进
            s = nums[s];
            f = nums[f];
        }
        return s;                             // 再次相遇即入环点 = 重复的数
    }
}
```

**复杂度**：时间 `O(n)`，空间 `O(1)`

**例子讲解**：`nums = [1, 3, 4, 2, 2]`（重复数 2，下标 0~4）

映射关系：`0→1, 1→3, 2→4, 3→2, 4→2`

**阶段一（找相遇点）**：

| 轮 | s | f |
| :-- | :-- | :-- |
| 起 | 0 | 0 |
| 1 | `nums[0]=1` | `nums[nums[0]]=nums[1]=3` |
| 2 | `nums[1]=3` | `nums[nums[3]]=nums[2]=4` |
| 3 | `nums[3]=2` | `nums[nums[4]]=nums[2]=4` |
| 4 | `nums[2]=4` | `nums[nums[4]]=nums[2]=4` → 相等 ✅ |

**阶段二**：`s = 0, f = 4`

| 轮 | s | f |
| :-- | :-- | :-- |
| 1 | `nums[0]=1` | `nums[4]=2` |
| 2 | `nums[1]=3` | `nums[2]=4` |
| 3 | `nums[3]=2` | `nums[4]=2` → 相等 → **返回 2** ✅ |

答案 `2` ✅
**为什么重复数就是入环点**：值域是 `[1, n]` 但有 `n+1` 个数，由抽屉原理必有一个值 `v` 被两个下标指向 → 沿 `i → nums[i]` 走，必然有两条边汇入 `v` → `v` 就是环的入口（唯一被多条边指向的结点）。
**三个前提**：① 不能修改原数组（所以不能用排序或原地哈希）；② 值域必须是 `[1, n]`（否则映射不成立）；③ 只有一个数重复，且重复**两次以上**也可以（入环点性质不变）。

---

## 附录 A：一页纸复习路线

| 阶段 | 专题 | 复习重点 |
| :-- | :-- | :-- |
| 第 1 遍（打底） | 哈希 → 双指针 → 滑动窗口 → 前缀和 | 建立「哈希 / 双指针 / 窗口」的条件反射 |
| 第 2 遍（结构） | 链表 → 二叉树 → 图论 | 画图能力 + 递归三问 + BFS/DFS 模板 |
| 第 3 遍（搜索） | 回溯 → 二分 → 栈 → 堆 | 回溯模板、二分边界、单调栈/堆的使用信号 |
| 第 4 遍（进阶） | 贪心 → 动态规划 → 多维 DP → 技巧 | DP 四步定式 + 背包模型 + 位运算技巧 |

## 附录 B：高频易错点清单

- **二分**：统一用左闭右开 `[l, r)` + `r = m`，别混用两种写法；求中点用 `(l + r) >>> 1`。
- **滑动窗口**：左边界要用 `max(l, ...)` 防止回退（#3）；缩窗时记得同步更新计数（#76）。
- **回溯**：`path` 加入结果必须 `new ArrayList<>(path)` 拷贝；递归传 `i` 还是 `i+1` 决定元素能否重复用。
- **链表**：只要涉及「首结点可能被删/被改」，一律上 `dummy`；改指针前先保存 `next`。
- **二叉树**：求「全局最优但路径可拐弯」的题（直径 #543、最大路径和 #124）必须在后序里顺手更新全局变量。
- **DP**：完全背包容量**正序**，0/1 背包容量**倒序**；不可达状态用「比答案大」的哨兵值而不是 `MAX_VALUE`（避免 +1 溢出）。
- **单调栈**：存**下标**而不是值，方便算宽度（#84）和天数（#739）。
- **堆**：第 K 大用小顶堆，第 K 小用大顶堆——**堆顶是门槛**。

## 附录 C：各专题一句话记忆锚点

| 专题 | 锚点 |
| :-- | :-- |
| 哈希 | 要 O(1) 查「有没有 / 出现过几次」 |
| 双指针 | 有序 + 两端夹逼，比 O(n²) 更聪明的枚举 |
| 滑动窗口 | 连续子数组/子串 + 单调性 |
| 子串 | 前缀和之差 = 区间和 |
| 矩阵 | 原地标记 / 边界收缩 / 按对角线变换 |
| 链表 | dummy + 快慢指针 + 画图 |
| 二叉树 | 后序回传信息，前序带入状态 |
| 图论 | 网格 = 图；依赖 = 拓扑；前缀 = Trie |
| 回溯 | 选 → 递归 → 撤销 |
| 二分 | 找边界：第一个满足条件的位置 |
| 栈 | 最近的更大/更小元素、嵌套解析 |
| 堆 | Top-K 与动态中位数 |
| 贪心 | 每一步都拿最好的，且能证明不亏 |
| 动态规划 | 定义状态 → 递推 → 边界 → 压缩 |
| 多维 DP | 两串取前缀，表格填转移 |
| 技巧 | 异或消对、摩尔投票、三指针、函数图找环 |

