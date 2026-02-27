# 算法日记

---

*——力扣 HOT 100——*

---



## ==两数之和==

**难度指数：♥**



##### 题目描述：

​    一个整数数组nums和一个整数目标值target，找出 **和为target** 的那 **两个** 整数，并返回它们数组的下标。

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```



- #### 解法一：哈希表

  

| 时间复杂度 | 空间复杂度 |   优点   |     缺点     |
| :--------: | :--------: | :------: | :----------: |
|    O(n)    |    O(n)    | 效率最高 | 需要额外内存 |

*代码实现：*

```C++
vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map <int, int> num_map;
        int len = nums.size();
        for(int i = 0 ; i < len ; ++i){
           int num = target - nums[i];
           if (num_map.find(num) != num_map.end()) {
               return {num_map[num], i};  
           }
           num_map[nums[i]] = i;
        }
    return {};
    }
```

















