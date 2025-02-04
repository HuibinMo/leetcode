# 灵茶山艾府的基础算法精讲笔记和leetcode答案

# 15. 三数之和
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
