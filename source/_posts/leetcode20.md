---
title: 20 子集
date: 2023-03-26 22:21:23
---

```class Solution {
    public boolean findSubarrays(int[] nums) {
        Map<Integer,Integer> allSets = new HashMap<>();
        for(int i = 0; i < nums.length - 1; i++){
            int res= nums[i] + nums[i+1];
            if(allSets.containsKey(res)){
                return true;
            }
            allSets.put(res, allSets.getOrDefault(res,0)+1);
        }
        return false;
    }
}


这题没什么好说的 ，集合也可以，很简单
