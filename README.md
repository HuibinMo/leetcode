# 灵茶山艾府的基础算法精讲笔记和leetcode答案

## 15. 三数之和
```python
# 时间复杂度为O(n^2), 空间复杂度O(1)
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        ans = []
        n = len(nums)
        for i in range(n - 2):
            x = nums[i]
            if i > 0 and x == nums[i - 1]:  # 跳过重复数字. 三元组不能有重复
                continue
            if x + nums[i + 1] + nums[i + 2] > 0:  # 优化一: 数字自小到大排列, 若x与后面2数之和大于0, 后面的组合不可能等于0
                break
            if x + nums[-2] + nums[-1] < 0:  # 优化二: 数字自小到大排列, 若x与前面2数之和小于0, 后面的组合有可能等于0
                continue
            j = i + 1
            k = n - 1
            while j < k:
                s = x + nums[j] + nums[k] # 三数之和
                if s > 0:
                    k -= 1
                elif s < 0:
                    j += 1
                else:  # 三数之和为 0
                    ans.append([x, nums[j], nums[k]])
                    j += 1
                    while j < k and nums[j] == nums[j - 1]:  # 跳过j的重复数字
                        j += 1
                    k -= 1
                    while k > j and nums[k] == nums[k + 1]:  # 跳过k的重复数字
                        k -= 1
        return ans
```

## 11. 盛最多水的容器
```python
# 时间复杂度O(n), 空间复杂度O(1)
class Solution:
    def maxArea(self, height: List[int]) -> int:
        ans = left = 0  # 初始化左柱在最左侧
        right = len(height) - 1  # 初始化右柱在最右侧
        while left < right:
            area = (right - left) * min(height[left], height[right])
            ans = max(ans, area)
            if height[left] < height[right]: # 左柱比右柱短
                # height[left] 与右边的任意线段都无法组成一个比 ans 更大的面积
                left += 1
            else:
                # height[right] 与左边的任意线段都无法组成一个比 ans 更大的面积
                right -= 1
        return ans
```

## 42. 接雨水
```python
# 方法一：前后缀分解
# 时间复杂度O(n), 空间复杂度O(n)
class Solution:
    def trap(self, height: List[int]) -> int:
        n = len(height)

        # 前缀
        pre_max = [0] * n  # pre_max[i] 表示从 height[0] 到 height[i] 的最大值
        pre_max[0] = height[0]
        for i in range(1, n):
            pre_max[i] = max(pre_max[i - 1], height[i])

        # 后缀
        suf_max = [0] * n  # suf_max[i] 表示从 height[i] 到 height[n-1] 的最大值
        suf_max[-1] = height[-1]
        for i in range(n - 2, -1, -1):
            suf_max[i] = max(suf_max[i + 1], height[i])

        # 求和
        ans = 0
        for h, pre, suf in zip(height, pre_max, suf_max):
            ans += min(pre, suf) - h  # 累加每个水桶能接多少水
        return ans


# 方法二：相向双指针
# 时间复杂度O(n), 空间复杂度O(1)
class Solution:
    def trap(self, height: List[int]) -> int:
        ans = left = pre_max = suf_max = 0
        right = len(height) - 1
        while left < right:
            pre_max = max(pre_max, height[left])  # 更新前缀最大值
            suf_max = max(suf_max, height[right])  # 更新后缀最大值
            if pre_max < suf_max:
                ans += pre_max - height[left]
                left += 1  # 指针右移
            else:
                ans += suf_max - height[right]
                right -= 1  # 指针左移
        return ans
```
