---
title: 【 算法 】Hash Table
date: 2015-10-14 13:56:19
tags: 算法
---

## 求连在一起的正方形的周长
**Island Perimeter**

例子：[[0,1,0,0], [1,1,1,0], [0,1,0,0], [1,1,0,0]]，结果 16。
思路：计算所有正方形的个数以及正方形相临边的个数。
```
public int islandPerimeter(int[][] grid) {
    int islands = 0, neighbours = 0;
    for(int i = 0; i < grid.length; i++){
        for(int j = 0; j < grid[0].length; j++){
            if(grid[i][j] == 1){
                islands++;
                if(i !=0 && grid[i-1][j] == 1) neighbours++;
                if(j !=0 && grid[i][j-1] == 1) neighbours++;
            }
        }
    }
    return islands * 4 - neighbours * 2;
}
```

<br/>

## 求两个数组的重合部分
**Intersection of Two Arrays**

例子：nums1 = [1,1,2,2,3]，nums2 = [1,2,2,3,4,5]，res = [1,2,3]
注意：熟练使用 HashSet结构。
```
public int[] intersection(int[] nums1, int[] nums2) {
    HashSet<Integer> set = new HashSet<Integer>();
    HashSet<Integer> resSet = new HashSet<Integer>();
    for(int i = 0; i < nums1.length; i++){
        set.add(nums1[i]);
    }
    for(int i = 0; i < nums2.length; i++){
        if(set.contains(nums2[i])){
            resSet.add(nums2[i]);
        }
    }
    int i = 0;
    int[] res = new int[resSet.size()];
    for(Integer num : resSet){
        res[i++] = num;
    }
    return res;
}
```

<br/>

## 求数组中任意两数相加等于 k
**Two Sum**

注意：熟练使用 HashMap 结构
```
public int[] twoSum(int[] nums, int target) {
    HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
    int[] res = new int[2];
    for(int i = 0; i < nums.length; i++){
        if(map.containsKey(target - nums[i])){
            res[0] = map.get(target - nums[i]);
            res[1] = i;
            break;
        }
        map.put(nums[i],i);
    }
    return res;
}
```
<br/>

**Two Sum II - Input array is sorted**
**二分查找方法 ~~~ 返回数组下标从 1 开始 **
```
public int[] twoSum(int[] numbers, int target) {
    int[] indice = new int[2];
    if (num == null || num.length < 2) return indice;
    int left = 0, right = num.length - 1;
    while (left < right) {
        int v = num[left] + num[right];
        if (v == target) {
            indice[0] = left + 1;
            indice[1] = right + 1;
            break;
        } else if (v > target) {
            right --;
        } else {
            left ++;
        }
    }
    return indice;
}
```

<br/>

## 验证数独是否满足条件
**Valid Sudoku**

注意：有数的才需要验证
```
public boolean isValidSudoku(char[][] board) {
    List<Set<Character>> row = new ArrayList<Set<Character>>();
    List<Set<Character>> line = new ArrayList<Set<Character>>();
    List<Set<Character>> square = new ArrayList<Set<Character>>();
    
    for(int i = 0; i < 9; i++){
        row.add(new HashSet<Character>());
        line.add(new HashSet<Character>());
        square.add(new HashSet<Character>());
    }
    
    for(int i = 0; i < 9; i++){
        for(int j = 0; j < 9; j++){
            char c = board[i][j];
            if(c != '.'){
                if(row.get(i).contains(c) || line.get(j).contains(c) || square.get(i/3*3+j/3).contains(c)){
                    return false;
                }
                row.get(i).add(c);
                line.get(j).add(c);
                square.get(i/3*3+j/3).add(c);
            }
        }
    }
    return true;
}
```

<br/>

## 字符数组是否满足一个模式
**Word Pattern**

```
public boolean wordPattern(String pattern, String str) {
    HashMap<Character,String> map = new HashMap<Character,String>();
    String[] strArray = str.split(" "); 
    
    if(strArray.length != pattern.length()) return false;
    for(int i = 0; i < pattern.length(); i++){
        char c = pattern.charAt(i);
        String s = strArray[i];
        if(map.containsKey(c)){
            if(!map.get(c).equals(s)){
                return false;
            }
        }else{
            if(map.containsValue(s)){
                return false;
            }
            map.put(c,s);
        }
    }
    return true;
}
```

<br/>

## 猜数-输出猜对位置的个数以及猜对数字的个数
**Bulls and Cows**

例子：secret = "1807"，guess = "7810"，输出 "1A3B"；sercet = "1122"，guess = "1222"，输出"3A0B"。
```
public String getHint(String secret, String guess) {
    int bull = 0, cows = 0;
    int[] map = new int[10];
    
    for(int i = 0; i < secret.length(); i++){
        if(secret.charAt(i) == guess.charAt(i)){
            bull++;
        }else{
            map[secret.charAt(i) - '0']++;   
        }
    }
    for(int i = 0; i < guess.length(); i++){
        if(secret.charAt(i) != guess.charAt(i) && map[guess.charAt(i) - '0']-- > 0){
            cows++;
        }
    }
    String res = bull + "A" + cows + "B";
    return res;
}
```

<br/>

## 求满足条件的平面内点的个数
**Number of Boomerangs~~~**

找出 (i, j, k) 三个点中，i、j 间距离等于 i、k 间距离的点
```
public int numberOfBoomerangs(int[][] points) {
    int res = 0;

    Map<Integer, Integer> map = new HashMap<>();
    for(int i=0; i<points.length; i++) {
        for(int j=0; j<points.length; j++) {
            if(i == j)
                continue;
            
            int d = getDistance(points[i], points[j]);                
            map.put(d, map.getOrDefault(d, 0) + 1);
        }
        
        for(int val : map.values()) {
            res += val * (val-1);
        }            
        map.clear();
    }
    
    return res;
}

private int getDistance(int[] a, int[] b) {
    int dx = a[0] - b[0];
    int dy = a[1] - b[1];
    
    return dx*dx + dy*dy;
}
```

