---
title: 剑指Offer 082. 含有重复元素集合的组合
date: 2023-01-13 09:11:27
---

给定一个可能有重复数字的整数数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的每个数字在每个组合中只能使用一次，解集不能包含重复的组合。

```输入: candidates = [10,1,2,7,6,1,5], target = 8,
输出:
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]


输入: candidates = [2,5,2,1,2], target = 5,
输出:
[
[1,2,2],
[5]
]
```
这一题总体思路与组合类似 但是更与不重复子集相似 相当于再加上一个条件 在本题中 target就是这个条件

```class Solution {
    List<List<Integer>> res = new LinkedList<List<Integer>>();
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        int candi_len = candidates.length;
        Arrays.sort(candidates);
        LinkedList<Integer> subset = new LinkedList<>();
        backtrack(candidates, 0, candi_len, target, subset);
        return res;
    }

    public void backtrack(int[] nums, int start, int len, int target, LinkedList<Integer> track){
        if (target == 0) {
            res.add(new ArrayList<>(track));
            return;
        }
        for(int i = start; i < len; i++){
          //剪枝 将target值减去 并且带入到下次回溯中 因为之前已经给数组排序过了 如果大于了 就说明超过了 就不用再进行回溯了
            if(nums[i] > target){
                break;
            }
            if(i != start && nums[i] == nums[i-1]){
                continue;
            }
            track.addLast(nums[i]);
            backtrack(nums, i+1, len, target - nums[i], track);
            track.removeLast();
        }
    }
}
```
