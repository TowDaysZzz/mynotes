题目：[剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode.cn/problems/que-shi-de-shu-zi-lcof/)

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int left = 0;
        int right = nums.size() - 1;
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            if (nums[mid] == mid) { // 左区间正确
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }
};
```



```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();
        int pre = nums[0];
        for (int i = 1; i < n; i++) {
            if (nums[i] != pre + 1) {
                return pre + 1;
            }
            pre++;
        }
        if (nums[0] == 0) return *nums.rbegin() + 1;
        return nums[0] - 1;
    }
};
```

