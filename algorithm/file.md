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

## ==最长连续序列==

**难度指数：♥ ♥** 



**题目描述：**

​    给定一个未排序的整数数组  **nums** ，找出数字连续的最长序列（<u>不要求序列元素在原数组中连续</u>）的长度。

**示例 2：**

```
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```



- #### 解法一：哈希集合



| 时间复杂度 | 空间复杂度 |   优点   |     缺点     |
| :--------: | :--------: | :------: | :----------: |
|    O(n)    |    O(n)    | 简单直观 | 需要额外内存 |

*代码实现：*

```c++
int longestConsecutive(vector<int>& nums) {
        unordered_set<int> a(nums.begin(), nums.end());
        int maxx = 0;
        for (auto it : a) {
            if (!a.count(it - 1)) {
                int curNum = it;
                int curLen = 1;
                while (a.count(curNum + 1)) {
                    curNum++;
                    curLen++;
                }
                maxx = max(maxx, curLen);
            }
        }
        return maxx;
    }
```

*心得：*

​    这题用set去重的性质以及哈希表的O(1)映射，能很高效率解决问题。



- #### 解法二：哈希表记录区间边界



| 时间复杂度 | 空间复杂度 |      优点      |   缺点   |
| :--------: | :--------: | :------------: | :------: |
|    O(n)    |    O(n)    | 可动态添加数据 | 逻辑复杂 |

*代码实现：*

```c++
int longestConsecutive(vector<int>& nums) {
        unordered_map<int, int> mp;
        int maxx = 0;
        for(auto& num : nums) {
            if (!mp.count(num)) {
                int l = mp.count(num - 1) ? mp[num - 1] : 0;
                int r = mp.count(num + 1) ? mp[num + 1] : 0;
                int len = l + r + 1;
                mp[num] = len;
                if (l > 0) mp[num - l] = len;
                if (r > 0) mp[num + r] = len;
                maxx = max(maxx, len);
            }
        }
        return maxx;
    }
```

*心得：*

​    这个解法的有动态规划的思想。关键在两个if语句，帮助端点实时更新数据。



---

---

## ==移动零==

**难度指数：♥** 



**题目描述：**

​    给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**请注意** ，<u>必须在不复制数组的情况下原地对数组进行操作</u>。

**示例 1:**

```
输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]
```



- #### 解法一：双指针



| 时间复杂度 | 空间复杂度 |  优点  |            缺点            |
| :--------: | :--------: | :----: | :------------------------: |
|    O(n)    |    O(1)    | 效率高 | 有自身交换的情况，有些浪费 |

代码实现：

```c++
void moveZeroes(vector<int>& nums) {
        for (int i = 0, j = 0; j < nums.size(); ++j) {
            if (nums[j] != 0) {
                swap(nums[i++], nums[j]);
            }
        }
    }
```

*心得：*

​    简单的双指针方法。



- #### 解法二：STL方法



| 时间复杂度 | 空间复杂度 |   优点   |            缺点            |
| :--------: | :--------: | :------: | :------------------------: |
|    O(n)    |   不确定   | 极其简洁 | 空间复杂度可能O(1)可能O(n) |

*代码实现：*

```c++
void moveZeroes(vector<int>& nums) {
        stable_partition(nums.begin(), nums.end(), [](int x) { return x != 0; });
    }
```

*心得：*

​    太冷门了，不适用。



---

---

## ==盛最多水的容器==

**难度指数：♥ ♥**



**题目描述：**

​    给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。返回容器可以储存的最大水量。

**说明：**你不能倾斜容器。

**示例 1：**

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```



- #### 解法一：双指针



| 时间复杂度 | 空间复杂度 |     优点     |     缺点     |
| :--------: | :--------: | :----------: | :----------: |
|    O(n)    |    O(1)    | 效率高，简洁 | 要用贪心思想 |

*代码实现：*

```c++
int maxArea(vector<int>& height) {
        int maxx = 0;
        int n = height.size();
        int l = 1, r = n;
        while (l < r) {
            if (height[l - 1] > height[r - 1]) {
                maxx = max(maxx, (r - l) * height[r - 1]);
                r--;
            }
            else {
                maxx = max(maxx, (r - l) * height[l - 1]);
                l++;
            }
        }
        return {maxx};
    }
```

*心得：*

​    实际上需要盛水最多，一开始的起点直接从最有可能盛水最多的两端开始。由于短板原理，每次只需向最短的那根木棍的中心方向移动。



---

---

## ==三数之和==

**难度指数：♥ ♥ ♥**



**题目描述：**

​    给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请你返回所有和为 **0** 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```



- #### 解法一：排序 + 双指针



| 时间复杂度 | 空间复杂度 |     优点     |   缺点   |
| :--------: | :--------: | :----------: | :------: |
|   O(n²)    |    O(1)    | 无需额外空间 | 不容易想 |

*代码实现：*

```c++
vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> result;
        for (int i = 0; i < nums.size() - 2; ++i) {
            if (i > 0 && nums[i - 1] == nums[i]) continue;
            int l = i + 1;
            int r = nums.size() - 1;
            while (l < r) {
                if (nums[i] + nums[l] + nums[r] == 0) {
                    result.push_back({nums[i], nums[l], nums[r]});
                    while (l < r && nums[l] == nums[l + 1]) l++;
                    while (l < r && nums[r] == nums[r - 1]) r--;
                    r--;
                    l++;
                }
                else if (nums[i] + nums[l] + nums[r] < 0) {
                    l++;
                }
                else {
                    r--;
                }
            }
        }
        return result;
    }
```

*心得：*

​    排序和双指针天生一对。排序不仅可以间接帮助去重，还为双指针铺垫。双循环的优势在于，外循环走过的路，内循环不必再走。



- #### 解法二：排序 + 哈希表去重



| 时间复杂度 | 空间复杂度 |   优点   |            缺点            |
| :--------: | :--------: | :------: | :------------------------: |
|   O(n²)    |    O(n)    | 容易想到 | 哈希额外需要内存，效率也低 |

*代码实现：*

```c++
vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> result;
        for (int i = 0; i < nums.size() - 2; ++i) {
            if (i > 0 && nums[i - 1] == nums[i]) continue;
            unordered_set <int> found;
            for (int j = i + 1; j < nums.size(); ++j) {
                int target = -nums[i] - nums[j];
                if (found.count(target)) {
                    result.push_back({nums[i], nums[j], target});
                    while (j + 1 < nums.size() && nums[j] == nums[j + 1]) j++;
                }
                else {
                    found.insert(nums[j]);
                }
            }
        }
        return result;
    }
```

*心得：*

​    类似于两数之和，自然好想。但其用时和内存消耗很大，不推荐。



---

---

## ==接雨水==

**难度指数：♥ ♥ ♥ ♥**



**题目描述：**

给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

 

**示例 1：**

![img](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png)

```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
```



- #### 解法一：暴力求解



| 时间复杂度 | 空间复杂度 |    优点    | 缺点  |
| :--------: | :--------: | :--------: | :---: |
|   O(n²)    |    O(1)    | 想法最简单 | 会TLE |

*代码实现：*

```c++
    int trap(vector<int>& height) {
        int len = height.size();
        int all = 0;
        for (int i = 0; i < len; ++i) {
            int lmax = 0;
            int rmax = 0;
            for (int j = i; j < len; ++j) rmax = max(rmax, height[j]);
            for (int j = 0; j <= i; j++) lmax = max(lmax, height[j]);
            all += min(rmax, lmax) - height[i];
        }
        return all;
    }
```

*心得：*

​    好想但是过不了leetcode最后一个测试点，毕竟时间复杂度是O(n²)。



- #### 解法二：双指针



| 时间复杂度 | 空间复杂度 |   优点   | 缺点 |
| :--------: | :--------: | :------: | :--: |
|    O(n)    |    O(1)    | 效率很高 | 难想 |

*代码实现：*

```c++
int trap(vector<int>& height) {
        int all = 0;
        int lmax = 0, rmax = 0;
        int l = 0, r = height.size() - 1;
        while (l < r) {
            if (height[l] < height[r]) {
                if (height[l] >= lmax) {
                    lmax = height[l];
                }
                else all += lmax - height[l];
                l++;
            }
            else {
                if (height[r] >= rmax) {
                    rmax = height[r];
                }
                else all += rmax -  height[r];
                r--;
            }
        } 
        return all;
    }
```

*心得：*

​    可以很好的维护当前最高值，双指针还得多练，感觉没理解透彻不能以举一反三。



---

---

## ==无重复字符的最长字串==

**难度指数：♥ ♥**



**题目描述：**

​    给定一个字符串 `s` ，请你找出其中**不含有重复字符**的 **最长 子串** 的长度。

 

**示例 1:**

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。注意 "bca" 和 "cab" 也是正确答案。
```



- #### 解法一：暴力循环 + 哈希表



| 时间复杂度 | 空间复杂度 |  优点  |       缺点        |
| :--------: | :--------: | :----: | :---------------: |
|   O(n²)    |    O(1)    | 容易想 | 在数据多的时候TLE |

*代码实现：*

```c++
int lengthOfLongestSubstring(string s) {
        int n = s.size(), ans = 0;
        for (int i = 0; i < n; ++i) {
            unordered_set<char> seen;
            for (int j = i; j < n; ++j) {
                if (seen.count(s[j])) break;     
                seen.insert(s[j]);
                ans = max(ans, j - i + 1);        
            }
        }
        return ans;
    }
```

*心得：*

​    这里使用了哈希表的count功能，而不是vector的find功能，使得能跑过力扣的测试，然而这种方法效率依然地下，毕竟是O(n²)的复杂度。



- #### 解法二：滑动窗口



| 时间复杂度 | 空间复杂度 |   优点   |       缺点       |
| :--------: | :--------: | :------: | :--------------: |
|    O(1)    |    O(1)    | 非常高效 | 无法直接返回字串 |

*代码实现：*

```c++
int lengthOfLongestSubstring(string s) {
        int ans = 0;
        int r = -1;
        int len = s.size();
        unordered_set<char> us;
        for (int i = 0; i < len; ++i) {
            if (i != 0) {
                us.erase(s[i - 1]);
            }
            while (r + 1 < len && !us.count(s[r + 1])) {
                us.insert(s[r + 1]);
                r++;
            }
            int l = us.size();
            ans = max(ans, l);
        }
        return ans;
    }
```

*心得：*

​    经典的窗口滑动问题，算是模板了。



---

---

## ==找字符串中所有异位词==

**题目难度：♥ ♥ ♥**



**题目描述：**

​    给定两个字符串 `s` 和 `p`，找到 `s` 中所有 `p` 的 **异位词** 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

 

**示例 1:**

```
输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
```



- #### 解法一：字符计数 + 滑动窗口



| 时间复杂度 | 空间复杂度 |       优点       |             缺点              |
| :--------: | :--------: | :--------------: | :---------------------------: |
|    O(n)    |    O(1)    | 效率高，实现简单 | *字符集之间若差的过大则消耗多 |

*代码实现*：

```c++
vector<int> findAnagrams(string s, string p) {
        int len1 = p.size(), len2 = s.size();
        vector<int> ans;
        vector<int> sCnt(26, 0), pCnt(26, 0);
        if (len1 > len2) return ans;
        for (auto& it : p) pCnt[it - 'a']++;
        for (int i = 0; i < len1; ++i) sCnt[s[i] - 'a']++;
        if (sCnt == pCnt) ans.push_back(0);
        for (int i = len1; i < len2; ++i) {
            sCnt[s[i - len1] - 'a']--;
            sCnt[s[i] - 'a']++;
            if (sCnt == pCnt) {
                ans.push_back(i - len1 + 1);
            }
        }
        return ans;
    }
```

*心得：*

​    每次比对时只用比较当前各个字母出现的次数即可，无需单独提出来专门sort一遍进行比较。



---

---

## ==和为k的子数组==

**题目难度：♥ ♥ ♥**



**题目描述：**

​    给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 *该数组中和为 `k` 的子数组的个数* 。

子数组是数组中元素的连续非空序列。

 

**示例 1：**

```
输入：nums = [1,1,1], k = 2
输出：2
```



- #### 解法一：暴力枚举



| **时间复杂度** | **空间复杂度** |  **优点**  | **缺点**  |
| :------------: | :------------: | :--------: | :-------: |
|     O(n²)      |      O(1)      | 直观且好想 | 可能会TLE |

*代码实现：*

```c++
int subarraySum(vector<int>& nums, int k) {
        int ans = 0;
        int len = nums.size();
        for (int i = 0; i < len; ++i) {
            int sum = 0;
            for (int j = i; j >= 0; j--) {
                sum += nums[j];
                if (sum == k) ans++;
            }
        }
        return ans;
    }
```

*心得：*

​    这种方法暴力遍历所有子数组，代码很简单，然而暴力总是达不到效果的。



- #### 解法二：哈希表 + 前缀和



| 时间复杂度 | 空间复杂度 |   优点   |     缺点     |
| :--------: | :--------: | :------: | :----------: |
|    O(n)    |    O(n)    | 效率很高 | 不具有通用性 |

*代码实现：*

```c++
int subarraySum(vector<int>& nums, int k) {
        int ans = 0;
        int cur = 0;
        int len = nums.size();
        unordered_map <int, int> mp;
        mp[0] = 1;           //为了记录和刚好等于k的情况
        for (int i = 0; i < len; ++i) {
            cur += nums[i];
            if (mp.find(cur - k) != mp.end()) {
                ans += mp[cur - k];
            }
            mp[cur]++;
        }
        return ans;
    }
```

*心得：*

​    哈希表可以及时记录当前下标为i时，包括i在内之前有多少cur - k的存在。





---

## ==滑动窗口最大化==

**题目难度：♥ ♥ ♥**



**题目描述：**

​    给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。

返回 *滑动窗口中的最大值* 。

 

**示例 1：**

```
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```



- #### 解法一：优先队列



| 时间复杂度 | 空间复杂度 | 优点 |            缺点            |
| :--------: | :--------: | :--: | :------------------------: |
|  O(nlogn)  |    O(n)    | 直观 | 时间比较慢，内存消耗比较多 |

*代码实现：*

```c++
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> ans;
        priority_queue<pair<int, int>> pq;
        for (int i = 0; i < k; ++i) {
            pq.emplace(nums[i], i);
        }
        ans.push_back(pq.top().first);
        for (int i = k; i < n; ++i) {
            pq.emplace(nums[i], i);
            while (pq.top().second <= i - k) {
                pq.pop();
            }
            ans.push_back(pq.top().first);
        }
        return ans;
    }
```

*心得：*

​    第一次学优先队列，梳理一下用法。优先队列会自动从大到小排序，top()为最大值，pop()为删除最大值，emplace可将数据列入队列。



- #### 解法二：单调队列



| 时间复杂度 | 空间复杂度 |    优点    |      缺点      |
| :--------: | :--------: | :--------: | :------------: |
|    O(n)    |    O(k)    | 高效消耗少 | 感觉没什么缺点 |

*代码实现：*

```c++
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> ans;
        deque<int> dq;
        for (int i = 0; i < k; ++i) {
            while (!dq.empty() && nums[i] > nums[dq.back()]) {
                dq.pop_back();
            }
            dq.push_back(i);
        }
        ans.push_back(nums[dq.front()]);
        for (int i = k; i < n; ++i) {
            while (!dq.empty() && nums[i] > nums[dq.back()]) {
                dq.pop_back();
            }
            dq.push_back(i);
            while (dq.front() <= i - k) {
                dq.pop_front();
            }
            ans.push_back(nums[dq.front()]);
        }
        return ans;
    }
```

*心得：*

​    deque的语法也是很少接触。front()和back()为左边到右边，放入数据为push_back()，删除最右边是pop_back()。



---

---

## ==相交链表==

**题目难度：♥** 



**题目描述：**

给你两个单链表的头节点 `headA` 和 `headB` ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 `null` 。

图示两个链表在节点 `c1` 开始相交**：**

[![img](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

题目数据 **保证** 整个链式结构中不存在环。

**注意**，函数返回结果后，链表必须 **保持其原始结构** 。



- #### 解法一：哈希表



| 时间复杂度 | 空间复杂度 |       优点       |     缺点     |
| :--------: | :--------: | :--------------: | :----------: |
|  O(m + n)  |    O(m)    | 实现非常简单好想 | 不是最高效的 |

*代码实现：*

```c++
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        unordered_set <ListNode* > v;
        struct ListNode* temp = headA;
        while (temp != NULL) {
            v.insert(temp);
            temp = temp -> next;
        }
        temp = headB;
        while (temp != NULL) {
            if (v.count(temp)) return temp;
            temp = temp -> next;
        }
        return NULL;
    }
```

*心得：*

​    以后做查找用哈希表不用vector的find。



- #### 解法二：双指针



| 时间复杂度 | 空间复杂度 |  优点  | 缺点 |
| :--------: | :--------: | :----: | :--: |
|  O(n + m)  |    O(1)    | 最高效 | 难想 |

*代码实现：*

```c++
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* temp1 = headA;
        ListNode* temp2 = headB;
        if (temp1 == NULL || temp2 == NULL) {
            return NULL;
        }
        while (temp1 != temp2) {
            temp1 = temp1 ? temp1 -> next : headB;
            temp2 = temp2 ? temp2 -> next : headA;
        }
        return temp1;
    }
```

*心得：*

​    巧妙根据数学规律解决问题。



---

---

## ==最大子数组和==

**题目难度：♥ ♥**



**题目描述：**

​    给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组**是数组中的一个连续部分。

 

**示例 1：**

```
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```



- #### 解法一：前缀和 + 贪心



| 时间复杂度 | 空间复杂度 |   优点   |          缺点          |
| :--------: | :--------: | :------: | :--------------------: |
|    O(n)    |    O(n)    | 简洁简单 | 空间复杂度还可以再优化 |

*代码实现：*

```c++
int maxSubArray(vector<int>& nums) {
        int len = nums.size();
        if (len == 1) return nums[0];
        vector<int> pre(len, 0);
        pre[0] = nums[0];
        int ans = nums[0];
        int minn = nums[0];
        for (int i = 1; i < len; ++i) {
            pre[i] = pre[i - 1] + nums[i];
            if (pre[i] - minn > pre[i]) {
                ans = ans > pre[i] - minn ? ans : pre[i] - minn;
            }
            else {
                ans = ans > pre[i] ? ans : pre[i];
            }
            minn = pre[i] < minn ? pre[i] : minn;
        }
        return ans;
    }
```

*心得：*

​    这里的最小值更新一定要放在答案更新后面，否则当前前缀和为最小值的时候会出现自己减自己的情况。
