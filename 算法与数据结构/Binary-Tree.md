---
title: 【 算法 】Binary Tree
date: 2015-10-08 15:01:13
tags: 算法
---

## 二叉树的先序、中序、后序遍历

### 先序遍历 - 递归与非递归
```
public void preOrderRecur (Node head) {
	if(head == null){
		return;
	}
	System.out.println(head.value + " ");
	preOrderRecur(head.left);
	preOrderRecur(head.right);
}
```
<br/>

**先序遍历非递归方法思路：**
1）申请一个新的栈，记为 stack。然后将头节点 head 压入 stack 中；
2）从 stack 中弹出栈顶节点，记为 cur，打印cur节点的值，再将节点 cur 的右孩子（不为空的话）压入 stack 中，最后将节点 cur 的左孩子（不为空的话）压入 stack 中；
3）不断重复步骤2，直到 stack 为空。
```
public void preOrderUnRecur (Node head) {
	if(head != null){
		Stack<Node> stack = new Stack<Node>();
		stack.push(head);
		while(!stack.isEmpty()){
			head = stack.pop();
			System.out.println(head.value + " ");
			if(head.right != null){
				stack.push(head.right);
			}
			if(head.left != null){
				stack.push(head.left);
			}
		}
	}
}
```

<br/>

### 中序遍历 - 递归与非递归
```
public void inOrderRecur (Node head) {
	if(head == null){
		return;
	}
	inOrderRecur(head.left);
	System.out.println(head.value + " ");
	inOrderRecur(head.right);
}
```

<br/>

**中序遍历非递归方法思路：**
1）申请一个新的栈，记为 stack。初始时，令变量 cur = head；
2）先把 cur 节点压入栈中，对以 cur 节点为头的整棵子树来说，依次把左边界压入栈中，即不停地令 cur = cur.left，然后重复步骤2；
3）不断重复步骤2，直到发现 cur 为空，此时从 stack 中弹出一个节点，记为 node。打印 node 的值，并且让 cur = node.right，然后继续重复步骤2；
4）当 stack 为空且 cur 为空时，停止整个过程。
```
public void inOrderUnRecur (Node head) {
	if(head == null){
		return;
	}
	while(!stack.isEmpty() || head != null){
		if(head != null){
			stack.push(head);
			head = head.left;
		}else{
			head = stack.pop();
			System.out.println(head.value + " ");
			head = head.right;
		}
	}
}
```

<br/>

### 后序遍历 - 递归与非递归
```
public void posOrderRecur (Node head) {
	if(head == null){
		return;
	}
	posOrderRecur(head.left);
	posOrderRecur(head.right);
	System.out.println(head.value + " ");
}
```

<br/>

**后序遍历非递归方法思路 -- 稍微复杂 **
思路一：使用两个栈实现后序遍历
1）申请一个栈，记为 s1，然后将头节点 head 压入 s1 中；
2）从 s1 中弹出的节点记为 cur，然后依次将 cur 的左孩子和右孩子压入 s1 中；
3）在整个过程中，每一个从 s1 中弹出的节点都放进 s2 中；
4）不断重复步骤2和步骤3，直到 s1 为空，过程停止；
5）从 s2 中依次弹出节点并打印，打印的顺序就是后序遍历的顺序。
```
public void posOrderUnRecur (Node head) {
	if(head == null){
		return;
	}
	Stack<Node> s1 = new Stack<Node>();
	Stack<Node> s2 = new Stack<Node>();

	s1.push(head);
	while(!s1.isEmpty()){
		head = s1.pop();
		s2.push(head);
		if(head.left != null){
			s1.push(head.left);
		}
		if(head.right != null){
			s1.push(head.right);
		}
	}
	while(!s2.isEmpty()){
		System.out.println(s2.pop().value + " ");
	}
}
```

<br/>

## 打印二叉树的边界节点
逆时针打印二叉树的边界节点，打印顺序为：最左节点、非最左最右的叶子节点、最右节点。
```
// 主函数
public void printEdge1 (Node head) {
	if(head == null){
		return;
	}
	int height = getHeight(head, 0);

 	// 定义二维数组，保存每层树的最左最右节点
	Node[][] edgeMap = new Node[height][2];
	// 初始化保存节点的数组
	setEdgeMap(head, 0, edgeMap);

	// 打印左边界
	for(int i = 0; i != edgeMap.length; i++) {
		System.out.println(edgeMap[i][0].value + " ");
	}
	// 打印既不是左节点，也不是右节点的叶子节点
	printLeafNotInMap(head, 0, edgeMap);
	// 打印右边界
	for(int i = edgeMap.length - 1; i != -1; i--) {
		if(edgeMap[i][0] != edgeMap[i][1]) {
			System.out.println(edgeMap[i][1].value + " ");
		}
	}
	System.out.println();
}

// 先序遍历二叉树，保存每层的最左及最右节点
pubilc void setEdgeMap (Node h, int l, Node[][] edgeMap) {
	if(h == null){
		return;
	}
	edgeMap[l][0] = edgeMap[l][0] == null ? h : edgeMap[l][0];
	edgeMap[l][1] = h;

	setEdgeMap(h.left, l+1, edgeMap);
	setEdgeMap(h.right, l+1, edgeMap);
}

// 先序遍历二叉树，打印既不是最左节点也不是最右节点的叶子节点
public void printLeafNotInMap(Node h, int l, Node[][] m) {
	if(h == null){
		return;
	}
	if(h.left == null && h.right == null && h != m[l][0] && h != m[l][1]) {
		System.out.println(h.value + " ");
	}
	printLeafNotInMap(h.left, l+1, m);
	printLeafNotInMap(h.right, l+1, m);
}

// 获取节点 h 子树的高度
public int getHeight (Node h, int l) {
	// 左子树或右子树为空
	if(h == null){
		return l;
	}
	return Math.max(getHeight(h.left, l+1), getHeight(h.right, l+1));
}
```

<br/>