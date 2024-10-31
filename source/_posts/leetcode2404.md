---
title: 2404. 出现最频繁的偶数元素
date: 2023-03-24 23:26:45
---

很简单 可以用hash表进行模拟，也可以用数组来做，因为题目已经给了数的范围
```
class Solution {
    public int mostFrequentEven(int[] nums) {
        int[] count = new int[100001];
        int res = -1;
        int resCount = 0;
        for(int x: nums){
            if((x % 2) != 0) {
                continue;
            }
            int c = ++count[x];
            if(c > resCount || c == resCount && x < res){
                resCount = c;
                res = x;
            }
        }
        return res;

    }
}



