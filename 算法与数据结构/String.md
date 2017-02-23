---
title: 【 算法 】String
date: 2015-11-18 16:09:28
tags: 算法
---

## 判断一个字符串是否是子串
**Implement strStr()**

```
public int strStr(String haystack, String needle) {
    for(int i = 0; ; i++){
        for(int j = 0; ; j++){
            if(j == needle.length()) return i;
            if(i + j == haystack.length()) return -1;
            if(needle.charAt(j) != haystack.charAt(i+j)) break;
        }
    }
}
```

<br/>

## 判断一个字符串是否由一个子串不断重复得到
**Repeated Substring Pattern**

思路：子串的长度一定是字符串长度的约数
```
public boolean repeatedSubstringPattern(String str) {
    int len = str.length();
    for(int i = len/2; i >= 1; i--){
        if(len % i == 0){
            // 子字符串的个数为 m 个
            int m = len/i;
            int j = 1;
           
            for(j = 1; j < m; j++){
                if(!str.substring(0,i).equals(str.substring(j*i,j*i+i))){
                    break;
                }
            }
            if(j == m){
                return true;
            }
        }
    }
    return false;
}
```

<br/>

## 将字符串转换成整数
**String to Integer (atoi)**

注意：-/+/空格/整型范围
```
public int myAtoi(String str) {
        char[] chas = str.toCharArray();
        int sign = 1, base = 0, i = 0;
        
        while (i < chas.length && chas[i] == ' ') { i++; }
        if (i < chas.length && (chas[i] == '-' || chas[i] == '+')) {
            sign = 1 - 2 * (chas[i++] == '-' ? 1 : 0); 
        }
        while (i < chas.length && chas[i] >= '0' && chas[i] <= '9') {
            if (base > Integer.MAX_VALUE / 10 || (base == Integer.MAX_VALUE / 10 && chas[i] - '0' > 7)) {
                if (sign == 1) return Integer.MAX_VALUE;
                else return Integer.MIN_VALUE;
            }
            base  = 10 * base + (chas[i++] - '0');
        }
        return base * sign;
    }
```
<br/>

## 第一个字符串是否能由第二个字符串的字符构成
**Ransom Note**

```
public boolean canConstruct(String ransomNote, String magazine) {
    char[] chasM = magazine.toCharArray();
    char[] chasN = ransomNote.toCharArray();
    int lenM = chasM.length;
    int lenN = chasN.length;
    if(lenN == 0){
        return true;
    }
    int[] map = new int[26];
    for(int i = 0; i < chasM.length; i++){
        map[chasM[i]-'a']++;
    }
    for(int i = 0; i < chasN.length; i++){
        if(--map[chasN[i]-'a'] < 0){
            return false;
        }
    }
    return true;
}
```

<br/>

## 判断一个字符串中有几个单词
**Number of Segments in a String**

单词之间以空格间隔，注意：空格可能有多个，可能出现在字符串前后。
```
public int countSegments(String s) {
    char[] chas = s.toCharArray();
    int res = 0;
    int count = 0;
    for(int i = 0; i < chas.length; i++){
        if(chas[i] != ' '){
            res = count++ == 0 ? res + 1 : res;
        }else{
            count = 0;
        }
    }
    return res;
}
```

<br/>

## 求字符串数组的最长前缀
**Longest Common Prefix**

思路：从头依次求字符串数组的前缀
```
public String longestCommonPrefix(String[] strs) {
    if(strs == null || strs.length == 0) return ""; 
    String pre = strs[0];
    for(int i = 0; i < strs.length; i++){
        while(strs[i].indexOf(pre) != 0){
         	// !! 注意
            pre = pre.substring(0,pre.length()-1);
        }
    }
    return pre;
}
```

<br/>

## 有规律字符串
**Count and Say**

按照一定规律排列的字符串，1、11、21、1211、111221、312211、……，规律为前面为 1 个 1、2 个 1、1 个 2 和 1 个 1、……，求此字符串数组第 n 项为什么。
```
public String countAndSay(int n) {
    if(n < 0) return "";
    StringBuilder sb = new StringBuilder("1");
    
    for(int i = 0; i < n-1; i++){
        String str = sb.toString();
        sb = new StringBuilder();
        int count = 0;
        char c = str.charAt(0);
        
        for(int j = 0; j < str.length(); j++){
            if(str.charAt(j) == c){
                count++;
            }else{
                sb.append(count).append(c);
                count = 1;
            }
            c = str.charAt(j);
        }
        // ！！注意
        sb.append(count).append(c);
    }
    return sb.toString();
}
```

<br/>

## 版本比较
**Compare Version Numbers**

注意测试例子：1 与 1.0、 1.0.1 与 1
```
public int compareVersion(String version1, String version2) {
    // !! 注意
    String[] strV1 = version1.split("\\.");
    String[] strV2 = version2.split("\\.");

    int len = Math.max(strV1.length, strV2.length);   
    for(int i = 0; i < len; i++){
        Integer num1 = i < strV1.length ? Integer.parseInt(strV1[i]) : 0;
        Integer num2 = i < strV2.length ? Integer.parseInt(strV2[i]) : 0;
        int compare = num1.compareTo(num2);
        if(compare != 0){
            return compare;
        }
    }
    return 0;
}
```

<br/>

## 二进制加法
**Add Digits**

思路：从字符串末尾相加，保存进位位
```
public String addBinary(String a, String b) {
    String res = "";  
    int over = 0, iA = a.length() - 1, iB = b.length() - 1;

    while(iA >=0 || iB >=0 || over == 1){
        over += iA >= 0 ? a.charAt(iA--) - '0' : 0;
        over += iB >= 0 ? b.charAt(iB--) - '0' : 0;
        res = over % 2 + res;
        over /= 2;
    }
    return res;
}
```

<br/>

## 字符串加法
**Add Strings**

求两个长度不超过 5100 的字符串的算术和，方法和求二进制加法完全相同。
```
public String addStrings(String num1, String num2) {
    String res = "";
    int iNum1 = num1.length() - 1, iNum2 = num2.length() - 1, over = 0;
    
    while(iNum1 >= 0 || iNum2 >= 0 || over >= 1){
        over += iNum1 >= 0 ? num1.charAt(iNum1--) - '0' : 0;
        over += iNum2 >= 0 ? num2.charAt(iNum2--) - '0' : 0;
        res = over % 10 + res;
        over = over / 10;
    }
    return res;
}
```

<br/>

## 判断是否是 IPv4 或 IPv6 地址
**Validate IP Address 有待研究**
```
    public String validIPAddress(String IP) {
    	if(isValidIPv4(IP)) return "IPv4";
    	else if(isValidIPv6(IP)) return "IPv6";
    	else return "Neither";
    }
    
    public boolean isValidIPv4(String ip) {
    	if(ip.length() < 7) return false;
    	if(ip.charAt(0) == '.') return false;
    	if(ip.charAt(ip.length() - 1) == '.') return false;
    	String[] tokens = ip.split("\\.");
    	if(tokens.length != 4) return false;
    	for(String token:tokens) {
    		if(!isValidIPv4Token(token)) return false;
    	}
    	return true;
    }
    
    public boolean isValidIPv4Token(String token) {
    	if(token.startsWith("0") && token.length() > 1) return false;
    	try {
    		int parsedInt = Integer.parseInt(token);
    		if(parsedInt < 0 || parsedInt > 255) return false;
    		if(parsedInt == 0 && token.charAt(0) != '0') return false;
    	} catch(NumberFormatException nfe) {
    		return false;
    	}
    	return true;
    }
    	
    public boolean isValidIPv6(String ip) {
    	if(ip.length() < 15) return false;
    	if(ip.charAt(0) == ':') return false;
    	if(ip.charAt(ip.length() - 1) == ':') return false;
    	String[] tokens = ip.split(":");
    	if(tokens.length != 8) return false;
    	for(String token: tokens) {
    		if(!isValidIPv6Token(token)) return false;
    	}
    	return true;
    }
    
    public boolean isValidIPv6Token(String token) {
    	if(token.length() > 4 || token.length() == 0) return false;
    	char[] chars = token.toCharArray();
    	for(char c:chars) {
    		boolean isDigit = c >= 48 && c <= 57;
    		boolean isUppercaseAF = c >= 65 && c <= 70;
    		boolean isLowerCaseAF = c >= 97 && c <= 102;
    		if(!(isDigit || isUppercaseAF || isLowerCaseAF)) 
    			return false;
    	}
    	return true;
    }
```

