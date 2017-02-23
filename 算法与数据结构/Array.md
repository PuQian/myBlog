---
title: 【 算法 】Array
date: 2015-10-05 10:20:39
tags: 算法
---

## 求整型数组中第 K 大的数
**Kth Largest Element in an Array**

思路：使用有限队列
```
public int findKthLargest(int[] nums, int k) {
    Comparator<Integer> cmp = new Comparator<Integer>(){
         public int compare(Integer e1, Integer e2) {
	        return e2 - e1;
	     }
    };
    Queue<Integer> pq = new PriorityQueue<Integer>(cmp);
    for(int i = 0; i < nums.length; i++){
        pq.add(nums[i]);
    }
    while(k-- > 1){
        pq.poll();
    }
    return pq.peek();
}
```

<br/>

## 求长度为 n 的数组缺少的数
**Find All Numbers Disappeared in an Array~~~**

例子：[3,2,1,2,1]，输出[4,5]，数组长度为5，数组中缺少数字 4 和 5。
```
public List<Integer> findDisappearedNumbers(int[] nums) {
    List<Integer> ret = new ArrayList<Integer>();
    
    for(int i = 0; i < nums.length; i++) {
        int val = Math.abs(nums[i]) - 1;
        if(nums[val] > 0) {
            nums[val] = -nums[val];
        }
    }
    
    for(int i = 0; i < nums.length; i++) {
        if(nums[i] > 0) {
            ret.add(i+1);
        }
    }
    return ret;
}
```
