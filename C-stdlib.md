### `malloc()`动态内存分配
大多数情况下数据所需内存的空间大小是不固定，若固定分配内存空间，可能会导致内存偏小过内存过剩，这时就需要动态为数据开辟空间
注意：
- <mark style="background: #FFF3A3A6;">要验证是否成功开辟内存空间，避免对空指针的解引用</mark>（**警告 C6011** 表示代码可能在**未检查指针是否为 NULL** 的情况下直接解引用它，这会导致**未定义行为**甚至程序崩溃。常见于 _malloc_、_new_ 等内存分配失败后未做空指针判断的场景）
```c
int main()
{
	int n;
	puts("Input size:");
	scanf("%d", &n);

	int* p = (int*)malloc(n * sizeof(int));

	if (p == NULL)//要验证是否成功分配内存空间，避免空指针的解引用
	{
		printf("%s\n", strerror(errno));
	}
	else
	{
		int i = 0;
		for (i; i < n; i++)
		{
			*(p + i) = i;
		}
		for (i = 0; i < n; i++)
		{
			printf("%d ", *(p + i));
		}
	}
	free(p);//用完之后释放p指向的内存空间
	p = NULL;//取消p的指向
	
	return 0;
}
```

⚠️⚠️⚠️注意：
![](./C-stdlib-1774704261697.webp)
<mark style="background: #FF5582A6;">致命错误</mark> ：循环里使用 `*p++ = i++` 直接移动了 p，循环结束时 p 已不再指向 malloc 分配的起始地址；根据 C 标准，free 必须传入 **malloc 返回的指针或 NULL**

1111111