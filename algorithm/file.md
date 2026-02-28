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

---

---



## ==字母异位词分组==

**难度指数：♥ ♥**



**题目描述：**

​    给你一个字符串数组，请你将 **字母异位词** 组合在一起。可以按任意顺序返回结果列表。

**示例 1:**

- ```
  输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
  
  输出: [["bat"],["nat","tan"],["ate","eat","tea"]]
  
  解释：
  
  - 在 strs 中没有字符串可以通过重新排列来形成 "bat"。
  - 字符串 "nat" 和 "tan" 是字母异位词，因为它们可以重新排列以形成彼此。
  - 字符串 "ate" ，"eat" 和 "tea" 是字母异位词，因为它们可以重新排列以形成彼此。
  ```



- #### 解法一：哈希表 + 排序

  

| 时间复杂度 | 空间复杂度 |   优点   |    缺点    |
| :--------: | :--------: | :------: | :--------: |
| O(n*klogk) |   O(n*k)   | 通用性强 | 排序开销大 |

*代码实现：*

```c++
vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> wordMap;
        for (auto& s : strs) {
            string key = s;
            sort(key.begin(), key.end());
            wordMap[key].push_back(s);
        }
        vector<vector<string>> result;
        for (auto it : wordMap) {
            result.push_back(it.second);
        }
        return result;
    }
```

*心得：*

​    哈希表能处理各种字符集，通用性极强。但每次排key的时候都要排序一遍，比较耗时间。



- #### 解法二：字符计数法



| 时间复杂度 | 空间复杂度 |        优点        |          缺点          |
| :--------: | :--------: | :----------------: | :--------------------: |
|   O(n*k)   |   O(n*k)   | 数据很多时优于排序 | 数据少时慢且内存消耗大 |

*代码实现：*

```c++
vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map <string, vector<string>> wordMap;
        for (auto& s : strs) {
            array<int, 26> count = {0};
            string key = "";
            for (auto c : s) {
                count[c - 'a']++;
            }
            for (int i = 0; i < 26; ++i) {
                key += to_string(count[i]) + '#';
            }
            wordMap[key].push_back(s);
        }
        vector<vector<string>> result;
        for (auto& p : wordMap) {
            result.push_back(p.second);
        }
        return result;
    }
```

*心得：*

​    本质上与排序法一样，都是想找一个特定的key，来对应一种字母异位词组。排序在字符串很长时sort要耗时间，在此情况下该方法占优。



---

---

