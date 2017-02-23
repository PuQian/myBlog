---
title: 【 算法 】java类
date: 2015-12-29 15:20:54
categories: 算法
---

## java 常用关键字
```
1. Integer.MAX_VALUE、Integer.MIN_VALUE // 最大（小）整型数
   Float.MAX_VALUE、FLoat.MIN_VALUE     // 最大（小）浮点数

2. 判断一个浮点数 num 是否为整数
   double epsilon = 10e-15;
   Math.abs(num - Math.round(num)) < epsilon
```

<br/>

## java 类型转换
```
1. int --> String
   <1> String.valueOf(int)
   <2> Integer.toString(int)
   <3> int + ""

2. String --> int
   Integer.parseInt(String);


3. int --> char （一般情况下自动转换）
   char c = (int)(1 + 'A');

4. char --> int（一般情况下自动转换）
   int n = (char)('B'); 

```

<br/>

## java Stack 类
**方法：** `size()`、`isEmpty()`、`push()`、`pop()`、`peek()`、`contains()`、`remove()`、`clear()`
```
--- 定义 ---
Stack<Object> stack = new Stack<Object>();

--- 注意 ---
1. int search(Object element); // 返回对象在堆栈中的位置，以 1 为基数
```

<br/>

## java 集合框架

### Collection - Queue - LinkedList 类

**方法：** `size()`、`isEmpty()`、`add()`、`poll()`、`peek()`、`contains()`、`remove()`、`clear()`

```
--- 定义 Queue 类 ---
Queue<Object> queue = new LinkedList<Object>();

--- 注意 ---
1. 定义优先队列，并设置优先队列排队顺序
   Comparator<Integer> cmp = new Comparator<Integer>(){
        public int compare(Integer e1, Integer e2) {
            return e2 - e1;
        }
   };
   Queue<Object> pq = new PriorityQueue<Object>(cmp);
```

<br/>

### Collection - List - LinkedList 类
**方法：** `isEmpty()`、`add()`、`poll()`、`peek()`、`addFirst()`、`addLast()`、`pollFirst()`、`pollLast()`、`peekFirst`、`peekLast()`
```
--- 定义 LinkedList 类 ---
Queue<Object> queue = new LinkedList<Object>();
LinkedList<Object> linkList = new LinkedList<Object>();
```

<br/>

### Collection - List - ArrayList 类
**方法：** `size()`、`isEmpty()`、`add()`、`get()`、`remove()`、`clear()`
```
--- 定义 ArrayList 类 ---
List<Object> arrayList = new ArrayList();
ArrayList<Object> arrayList = new ArrayList();
```

<br/>

### Collection - Set - HashSet 类
**方法：** `size()`、`boolean add()`、`remove()`、`contains()`
```
--- 定义 HashSet 类 ---
HashSet<Object> hashSet = new HashSet<Object>();
Set<Object> hashSet = new HashSet<Object>();

--- 注意 ---
1. Set 元素不能重复，添加重复元素 add 返回 false
2. 遍历写法 for(Object obj : hashSet){ obj... }
```

<br/>

### Map - HashMap 类
**方法：** `put(key,value)`、`get(key)`、`containsKey(key)`、`containsValue(value)`
```
--- 定义 HashMap 类 ---
Map<Object,Object> map = new HashMap<Object,Object>();

```

<br/>

## java String 类
**方法：** `length()`、`charAt()`
```
int indexOf(String str)           // 返回指定子字符串第一次出现的索引值
String concat(String str)         // 将指定字符串连接到此字符串的结尾
String substring(int beginIndex)  // 返回子串

--- 注意 ---
1. String substring(int beginIndex, int endIndex) 包括 beginIndex，不包括 endIndex
2. String[] split(separator, howmany)  分割字符串，返回分割后的字符串数组
   由于 seperator 可以采用正则写法，因此 譬如以 "." 分割的写法为 "\\."，要转义。

```

<br/>

## java Number 类
```
Math.max()    // 返回两个参数中的最大值
Math.min()    // 返回两个参数中的最小值
Math.abs()    // 返回参数的绝对值
Math.pow()    // 返回第一个参数的第二个参数次方
Math.sqrt()   // 返回参数的算术平方根
Math.ceil()   // 向下取整，返回 double 类型
Math.floor()  // 向上取整，返回 double 类型
Math.round()  // 四舍五入，返回 int 型
Math.random() // 返回一个随机数，[0,1)

--- 注意 ---
1. Object.compareTo(Object);
   比较 Integer ：前者大于后者，返回 1；前者小于后者，返回 -1；两者相等，返回 0；
   比较 String：返回第一个不同的字符的 ASCII 码的差值，a 与 abc 这种返回长度的差值，相等返回0

```

<br/>

## Java StringBuffer / Java StringBuilder类

```
--- 定义 ---
StringBuffer sb = new StringBuffer();

--- 注意 ---
1. StringBuffer append(String s);  // 将指定的字符串追加到此字符序列后
2. insert(index, String)；         // 这字符串指定位置添加字符串

3. 在对字符串进行修改时，需要使用 StringBuffer 和 StringBuilder 类。
   与 String 类不同的是，StringBuffer 和 StringBuilder 类的对象能够被多次的修改，而不产生新的未使用对象。

4. StringBuilder 相较于 StringBuffer 有速度优势，所以多数情况下建议使用 StringBuilder 类。
   然而在应用程序要求线程安全的情况下，则必须使用 StringBuffer 类。
```
