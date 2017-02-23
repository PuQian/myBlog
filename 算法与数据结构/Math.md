---
title: 【 算法 】Math-E
date: 2015-10-30 17:25:05
tags: 算法
---

## 求 n! 末尾有几个零
**Factorial Trailing Zeroes**

思路：首先把阶乘求出来再找到有几个零是非常不现实的，因为阶乘很大，没法儿保存。
`n!` 的末尾的 `0` 是由因子 `2` 和 `5` 相乘得到的，而在 1 到 n 这个范围内，因子 `2` 的个数要远多于 `5`，因此只需计算 1 到 n 的范围内有多少因子 `5` 就可以了。
例1：`n = 15`，那么在 `15!` 中有 3 个 5 （其中来自`5`、`10`、`15`），所以计算 `n/5` 就可以；
例2：`n = 25`，与例1相同，计算 `n/5`，可以得到 5 个 5，分别来自其中的 `5，10，15，20，25`，但是在 `25` 中包含 2 个 5，所以除了计算`n/5`，还要计算`n/5/5，n/5/5/5，n/5/5/5/5，...` 直到商为0，然后求和。
 ```
public int trailingZeroes(int n) {
    if(n < 1) return 0;
    int res = 0;
    while(n/5 != 0){
        n /= 5;
        res += n;
    }
    return res;
}
 ```
<br/>

## 判断一个整数是否回文 
**Palindrome Number**

思路：用一个变量 `rev` 从末尾保存倒序的数 `x` 到中间，比较 `x == rev`（偶数），或者 `x == rev / 10`（奇数）。

``` 
public boolean isPalindrome(int x) {
    if(x < 0 || (x != 0 && x % 10 == 0)) return false;
    int rev = 0;
    while(x > rev){
        rev = rev * 10 + x % 10;
        x = x/10;
    }
    return (x == rev || x == rev / 10);
}
```

<br/>

## 使整型数组中所有值相等的最少步数
**Minimum Moves to Equal Array Elements**

思路：每次数组 `n-1` 项加 1，`n-1` 项加 1，相当于每次一项减 1，因此最后需要的步数为数组中所有项与最小值差值之和。
```
public int minMoves(int[] nums) {
    if(nums.length == 0) return 0;
    int min = nums[0];
    for(int n : nums) min = Math.min(min, n);
    int res = 0;
    for(int n : nums) res += n -min;
    return res;
}
```
<br/>

## 判断整数 n 最多能由 1 加到几
**Arranging Coins**

注意：不能使用 1 一直加与 n 比较大小，因为这样会超出整型范围，方法是减。
```
public int arrangeCoins(int n) {
    if(n < 0) return -1;
    int i = 1;
    int minus = n;
    while(minus >= i){
        minus -= i++;
    }
    return --i;
}
```

<br/>

## 判断一个数是否是 m 的 n 次幂
**Power of Two / Three / Four**

不能使用循环或者递归。
如果 num 是 m 的 n 次方，那么以 m 为底的对数一定是一个整数。

```
// 最小浮点数
private static final double epsilon = 10e-15;

public boolean isPowerOfThree(int n) {
    if(0 == n)
        return false;
    double res = Math.log(n)/Math.log(3);
    // !! 注意判断条件为 结果 res 为整数，而不是其他浮点数
    return Math.abs(res - Math.round(res)) < epsilon;
}
```

<br/>

## 求两个矩形覆盖的面积
**Rectangle Area**

思路：求矩形的面积之和减去矩形相交的面积。注意：不能直接使用 length = Math.min(D,H) - Math.max(B,F)，可能会超过整型范围。
```
public int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
    int length = Math.min(D,H) > Math.max(B,F) ? Math.min(D,H) - Math.max(B,F) : 0;
    int width = Math.min(C,G) > Math.max(A,E) ? Math.min(C,G) - Math.max(A,E) : 0;
    return (C-A)*(D-B) + (G-E)*(H-F) - length*width;
}
```

<br/>

## 有规律整数 1
**Happy Number**

例如：19 符合条件：1^2 + 9^2 = 82，8^2 + 2^2 = 68，6^2 + 8^2 = 100，1^2 + 0^2 + 0^2 = 1
注意：符合条件的数最后都是1，不符合条件的数最后会出现几个数之间的循环。
```
public boolean isHappy(int n) {
    HashSet<Integer> hashSet = new HashSet<Integer>();
    int squareSum = n;
    
    // !!注意循环条件，HashSet 返回 false，说明集合中已经存在该整数。
    while(hashSet.add(n)){
        squareSum = 0;
        while(n != 0){
            squareSum += (n % 10) * (n % 10);
            n /= 10;
        }
        n = squareSum;
    }
    if(n == 1){
        return true;
    }else{
        return false;   
    }
}
```

<br/>

## 有规律整数 2
**Ugly Number**

条件：判断一个整数是否只包含 `2`、 `3`、 `5`，3 个质因数。
思路：将整数 n 依次无限次除以2、3、5，结果为 1 则结论成立。
```
public boolean isUgly(int num) {
    for (int i = 2; i < 6 && num > 0; i++){
        while (num % i == 0){
            num /= i;
        }
    }
    return num == 1;
}
```

<br/>

## 有规律数字 3
**Ugly Number II**

求第 n 个 Ugly数字，用动态规划，**有待研究**
```
public int nthUglyNumber(int n) {
    int[] ugly = new int[n];
    ugly[0] = 1;
    int index2 = 0, index3 = 0, index5 = 0;
    int factor2 = 2, factor3 = 3, factor5 = 5;
    for(int i=1; i < n;i++){
        int min = Math.min(Math.min(factor2,factor3),factor5);
        ugly[i] = min;
        if(factor2 == min)
            factor2 = 2*ugly[++index2];
        if(factor3 == min)
            factor3 = 3*ugly[++index3];
        if(factor5 == min)
            factor5 = 5*ugly[++index5];
    }
    return ugly[n-1];
}
```

<br/>

## 反转一个 int 型数 
**Reverse Integer**

注意：1. 反转之后可能超出范围；2. 整数可能为负数
```
public int reverse(int x) {
    int res = 0, temp = 0;
    while(x != 0){
        res = res * 10 + x % 10;
        // !! 注意
        if((res - x % 10)/10 != temp){
            return 0;
        }
        x /= 10;
        temp = res;
    }
    return res;
}
```

<br/>

## 英文字母计数
**Excel Sheet Column Title**

思路：ABA = (26^0 * 1) + (26^1 * 2) + (26^2 * 1)，注意 Z = 26
1 -- A、2 -- B、3 -- C、...、26 -- Z、27 -- AA、28 -- AB
```
// AA --> 27
public int titleToNumber(String s) {
    int res = 0;
    int count = 0;
    for(int i = s.length() - 1; i >= 0; i--){
        res += Math.pow(26,count++) * (s.charAt(i) - 'A' + 1);
    }
    return res;
}

// 27 --> AA
public String convertToTitle(int n) {
	StringBuilder result = new StringBuilder();

	while(n > 0){
		// ！！n-- 
	    n--;
	    result.insert(0, (char)('A' + n % 26));
	    n /= 26;
	}

	return result.toString();
}
```

<br/>

## 计算小于 n 的素数的个数
**Count Primes**

采用普通方式一个一个计算会超时
```
public int countPrimes(int n) {
    boolean[] notPrime = new boolean[n];
    int count = 0;
    for (int i = 2; i < n; i++) {
        if (notPrime[i] == false) {
            count++;
            for (int j = 2; i*j < n; j++) {
                notPrime[i*j] = true;
            }
        }
    }
    
    return count;
}
```

<br/>

## 整型数组每项移动后求最大值
**Rotota Function**

```
例子：A = [4, 3, 2, 6]

F(0) = (0 * 4) + (1 * 3) + (2 * 2) + (3 * 6) = 0 + 3 + 4 + 18 = 25
F(1) = (0 * 6) + (1 * 4) + (2 * 3) + (3 * 2) = 0 + 4 + 6 + 6 = 16
F(2) = (0 * 2) + (1 * 6) + (2 * 4) + (3 * 3) = 0 + 6 + 8 + 9 = 23
F(3) = (0 * 3) + (1 * 2) + (2 * 6) + (3 * 4) = 0 + 2 + 12 + 12 = 26

So the maximum value of F(0), F(1), F(2), F(3) is F(3) = 26.
```

<br/>

思路：**F(n) 与 F(n-1) 的关系： ** F(n) = F(n - 1) + sum - n * end;
```
public int maxRotateFunction(int[] A) {
    int pre = 0, sum = 0, k = 1;
    int max = Integer.MIN_VALUE;
    for(int i = 0; i < A.length; i++){
        pre += i * A[i];
        sum += A[i];
    }
    max = Math.max(pre, max);
    while(k < A.length){
        pre = pre + sum - A.length * A[A.length - k];
        max = Math.max(pre, max);
        k++;
    }
    return max;
}
```

<br/>

## 计算自然数第 n 位为数字几
**Nth Digit ~~~**
例子："1234567891011121314……"，第 10 位为 1，第 11 位为0
```
public int findNthDigit(int n) {
    int len = 1;
    long count = 9;
    int start = 1;
    
    while(n > len * count){
        n -= len * count;
        len++;
        count *= 10;
        start *= 10;
    }
    
    start += (n - 1) / len;
	String s = Integer.toString(start);
	return Character.getNumericValue(s.charAt((n - 1) % len));
}
```

## 整型数组值加一
**Plus One**

例子：[0] -- [1]，[9,7] -- [9,8]，[9,9] -- [1,0,0]
```
public int[] plusOne(int[] digits) {
    int over = 1, len = digits.length;
    for(int i = len - 1; i >= 0; i--){
        digits[i] += over;
        if(digits[i] < 10){
            return digits;
        }else{
            digits[i] = 0;
        }
    }
    int[] newdigits = new int[len + 1];
    newdigits[0] = 1;
    for (int k = 1; k < len + 1; k++){
        newdigits[k] = digits[k - 1];   
    }
    return newdigits;
}
```