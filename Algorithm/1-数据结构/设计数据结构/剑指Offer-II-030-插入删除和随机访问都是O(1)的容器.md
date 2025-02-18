题目：[剑指 Offer II 030. 插入、删除和随机访问都是 O(1) 的容器](https://leetcode.cn/problems/FortPu/)

题解：https://leetcode.cn/problems/FortPu/solution/by-ac_oier-rls4/

## 变长数组 + 哈希表

对于 `insert` 和 `remove` 操作容易想到使用「哈希表」来实现 O(1) 复杂度，但对于 `getRandom` 操作，比较理想的情况是能够在一个数组内随机下标进行返回。

将两者结合，我们可以将哈希表设计为：以入参 `val` 为键，数组下标 `loc` 为值。

**为了确保严格 O(1)，我们不能「使用拒绝采样」和「在数组非结尾位置添加/删除元素」。**

因此我们需要申请一个足够大的数组 `nums`（利用数据范围为 2* 10^5），并使用变量 `idx` 记录当前使用到哪一位（即下标在 `[0, idx]` 范围内均是存活值）。

对于几类操作逻辑：

- `insert` 操作：使用哈希表判断 `val` 是否存在，存在的话返回 `false`，否则将其添加到 `nums`，更新 `idx`，同时更新哈希表；
- `remove` 操作：使用哈希表判断 `val` 是否存在，不存在的话返回 `false`，否则从哈希表中将 `val` 删除，同时取出其所在 `nums` 的下标 `loc`，然后将 `nums[idx]` 赋值到 `loc` 位置，并更新 `idx`（含义为将原本处于 `loc` 位置的元素删除），同时更新原本位于 `idx` 位置的数在哈希表中的值为 `loc`（若 `loc` 与 `idx` 相等，说明删除的是最后一个元素，这一步可跳过）；
- `getRandom` 操作：由于我们人为确保了 `[0, idx]` 均为存活值，因此直接在 `[0, idx + 1)` 范围内进行随机即可。

时间复杂度：所有操作均为 O(1)

空间复杂度：O(n)

```python
class RandomizedSet:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.nums = [0 for _ in range(int(2 * 1e5))]
        self.idx = -1 # 记录当前使用到哪一位
        self.hashmap = collections.defaultdict(int)


    def insert(self, val: int) -> bool:
        """
        Inserts a value to the set. Returns true if the set did not already contain the specified element.
        """
        if val in self.hashmap: return False
        self.idx += 1
        self.hashmap[val] = self.idx
        self.nums[self.idx] = val
        return True


    def remove(self, val: int) -> bool:
        """
        Removes a value from the set. Returns true if the set contained the specified element.
        """
        if val not in self.hashmap: return False
        if self.hashmap[val] == self.idx:
            del self.hashmap[val]
            self.idx -= 1
        else:
            iloc = self.hashmap[val];
            del self.hashmap[val]

            swap_val = self.nums[self.idx]
            self.nums[iloc] = swap_val
            self.hashmap[swap_val] = iloc

            self.idx -= 1
        return True


    def getRandom(self) -> int:
        """
        Get a random element from the set.
        """
        rdm = random.randint(0, self.idx)
        return self.nums[rdm]


# Your RandomizedSet object will be instantiated and called as such:
# obj = RandomizedSet()
# param_1 = obj.insert(val)
# param_2 = obj.remove(val)
# param_3 = obj.getRandom()
```

