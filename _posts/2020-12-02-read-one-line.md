---
layout: post
title: 读取一行数字（数字之间用空格隔开）
subtitle: 
tags: [c]
---



## 读取一行数字（数字之间用空格隔开）

```c++
#include <iostream>

using namespace std;

int main() {
	int a[100];
	char c;
	int k = 0;
	while((c=getchar()) != '\n') {
	 if (isdigit(c)) { // 检测该字符是否为数字
	 	ungetc(c,stdin); // 把字符退回到输入流
		cin >> a[k++];
   }
	}
	for (int i = 0;i < k;i++) {
	  cout << a[i] << ' ';
	}
	return 0;
}
```

