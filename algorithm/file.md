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

```c++
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

*心得：*

​    哈希表在 **快速查找元素** 和 **统计元素频率** 上有着天然的优势。



- #### 解法二：排序 + 双指针



| 时间复杂度 | 空间复杂度 |          优点          |       缺点       |
| :--------: | :--------: | :--------------------: | :--------------: |
|  O(nlogn)  |    O(n)    | 若不返回下标，空间更小 | 时间长，代码复杂 |

*代码实现：*

```c++
vector<int> twoSum(vector<int>& nums, int target) {
        vector<pair<int, int>> saveIndex;
        int len = nums.size();
        for (int i = 0; i < len; ++i) {
            saveIndex.emplace_back(nums[i], i);
        }
        sort(saveIndex.begin(), saveIndex.end());
        int l = 0, r = len - 1;
        while (l < r) {
            int sum = saveIndex[l].first + saveIndex[r].first;
            if (sum == target) {
                return {saveIndex[l].second, saveIndex[r].second};
            }
            else if(sum < target) {
                l++;
            }
            else {
                r--;
            }
        }
        return {};
    }
```

*心得：*

​    这也是一种经典的思路。但这里要返回下标，所以用pair保存了原数据的下标，内存上多了一点消耗，否则空间复杂度为O(1)。



