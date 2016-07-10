---
layout: post
title: "ReplaceBlank"
---
好久没有更新博客，不过之前也是胡诌的啦。由于最近在看何大牛的剑指Offer，于是就想在这里重新记录一下。  
这里的面试题目为：请实现一个函数，把字符中的每个空格替换成“%20”。例如输入“We are happy.”，则输出为“We%20are%20happy.”。
要求时间复杂度为O（n）的解法，搞定offer就靠它了。
源代码如下：

~~~
#include <iostream>
#include <string>
#include <vector>

using namespace std;

void replace_blank(char strs[], int length)
{
	int i, j, count = 0, original_length = 0;

	if (strs == NULL || length <= 0)
		return ;


	for (i = 0; strs[i] != '\0'; i ++)
	{
		original_length ++;
		if (strs[i] == ' ')
			count ++;
	}
	//cout << length << endl;

	int newlength = count * 2 + original_length;//计算最新字符串的长度，也即把空格替换成'%20'之后的长度
	if (newlength > length)
		return ;


	i = original_length - 1;
	j = newlength - 1;
	strs[newlength] = '\0';
	while (i <= j && i >= 0)
	{
		if (strs[i] == ' ')
		{
			strs[j --] = '0';
			strs[j --] = '2';
			strs[j --] = '%';
		}
		else
			strs[j --] = strs[i];
		i --;
	}
	cout << strs << endl;
	return;
}


int main(void)
{
	const size_t array_size = 1000;
	char strs[array_size];
	//gets_s(strs, array_size - 1);
	int i = 0;
	char tmp;
	tmp = cin.get();
	while(tmp != '\n')
	{
		strs[i] = tmp;
		tmp = cin.get();
		i ++;
	}
	strs[i] = '\0';
	replace_blank(strs, array_size);

	return 0;
}
~~~

这个代码的思想是利用两个指针进行字符串的复制和替换，一个用来指向原始字符串的末尾P1，一个用来指向替换之后的字符串的末尾P2。接下来是向前移动指针P1，逐个把它指向的字符复制到P2指向的位置，直到碰到第一个空格为止。  
该题目可以进行拓展，比如：有两个排序的数组A1和A2，内存在A1的末尾有足够多的空余空间容纳A2.请实现一个函数，把A2中的所有数字插入到A1中并且所有的数字是排序的。  
举一反三：  
合并两个数组（包括字符串）时，如果从前往后复制每个数字（或字符）需要重复移动数字（或字符）多次，那么我们可以考虑从后往前复制，这样就能减少移动次数，从而提高效率。
