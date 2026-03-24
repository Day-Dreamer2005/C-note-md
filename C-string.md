
## 实现C库函数`strcpy()`  
首先引出库函数`strcpy`
`char *strcpy(char *dest, const char *src)` 把 **src** 所指向的字符串复制到 **dest**
<mark style="background: #FFF3A3A6;">需要注意的是如果目标数组 dest 不够大，而源字符串的长度又太长，可能会造成缓冲溢出的情况</mark>
```C
#include <stdio.h>
#include <string.h>
 
int main ()
{
  char str1[]="Sample string";
  char str2[40];
  char str3[40];
  strcpy (str2,str1);
  strcpy (str3,"copy successful");
  printf ("str1: %s\nstr2: %s\nstr3: %s\n",str1,str2,str3);
  return 0;
}
```
注意⚠️
- `strcpy()`直接将整个源字符串拷贝到目标空间，**包括`\0`**❗❗❗

实现自己的**strcpy()** !!!
```C
//design my copy function
char* my_strcpy(char* str1, const char* str2)
{
	assert(str1 != NULL);
	assert(str2 != NULL);//提高阅读性

	while (*str1++ = *str2++)
	{

	}
	return str1;
}
int main()
{
	char str1[100];
	char str2[100];

	puts("Please input str1:");
	fgets(str1, sizeof(str1), stdin);

	puts("Please input str2:");
	fgets(str2, sizeof(str2), stdin);

	//use my copy function
	my_strcpy(str1, str2);

	printf("\nAfter copy:\n%s\n", str1);

	return 0;
}
```
使用`assert()`函数断言传入指针不为空时，`assert(str1 != NULL)`是一种高阅读性写法，尽管和`assert(str1)`作用一样，但后者不便于阅读和后期调试代码，**<mark style="background: #D2B3FFA6;">所以更推荐第一种写法</mark>**  

<mark style="background: #FF5582A6;">拷贝字符串注意事项</mark>
- 源字符串必须以`\0`结束，否则拷贝后字符串失去结束标志
- 保证将源字符串的`\0`也拷贝到目标空间
- 目标空间必须大于所要拷贝的源字符串
- 目标空间必须可变，能被拷贝

---

## 实现C库函数`strcat()`
首先引出库函数`strcat()`
`char *strcat(char *dest, const char *src)`把 **src** 所指向的字符串追加到 **dest** 所指向的字符串的结尾
```C
#include <stdio.h>
#include <string.h>
 
int main ()
{
   char src[50], dest[50];
 
   strcpy(src,  "This is source");
   strcpy(dest, "This is destination");
 
   strcat(dest, src);
 
   printf("最终的目标字符串： |%s|", dest);
   
   return(0);
}
```


实现自己的**strcat()** !!!
```C
void my_strcat(char* dest, const char* src)
{
	assert(dest != NULL);
	assert(src != NULL);

	//找到目标字符串的\0
	while (*dest != '\0')
	{
		dest++;
	}

	//从目标字符串的\0处开始追加
	while (*dest++ = *src++)//追加到源字符串的\0停止
	{

	}
}
int main()
{
	char dest[20] = "Hello ";
	char src[] = "World";

	my_strcat(dest, src);

	printf("After strcat:\n%s", dest);

	return 0;
}
```
`strcat()`实现原理：
		源字符串从目标字符串的`\0`开始追加，到源字符串的`\0`结束  

<mark style="background: #FF5582A6;">注意事项</mark>：
- 源字符串必须以`\0`结束
- 被追加空间必须足够大，能容纳源字符串内容
- 目标空间可修改
- **当追加自身时要保证最后有结束标志`\0`**

---

## 实现C库函数`strcmp()`
库函数`strcmp()`把 **str1** 所指向的字符串和 **str2** 所指向的字符串进行比较
```C
int main()
{
	char* str1 = "abc";
	char* str2 = "bbc";

	int ret = strcmp(str1, str2);

	if (ret > 0)
	{
		printf("str1 > str2\n");
	}
	else if (0 == ret)
	{
		printf("str1 = str2\n");
	}
	else if (ret < 0)
	{
		printf("str1 < str2\n");
	}

	return 0;
}
```

<mark style="background: #FF5582A6;">注意！！！</mark>
- 将两个字符串对应的下标从左至右依次比较，只要有一个不同则比较结束
- 按ASCLL码值进行比较
- 返回值小于0，str1 < str2
- 返回值等于0，str1 = str2
- 返回值大于0，str1 > str2
- **小于和大于时并没有规定返回值是具体值！**

实现自己的`strcmp()` !!!
```C
int my_strcmp(const char* str1, const char* str2)
{
	assert(str1 != NULL);
	assert(str2 != NULL);

	while (*str1 == *str2)
	{
		if ((*str1 && *str2) == '\0')
		{
			return 0;//相等
		}
		str1++;
		str2++;
	}

	return (*str1 - *str2);
}
int main()
{
	char str1[] = "abcz";
	char str2[] = "abqd";

	int ret = my_strcmp(str1, str2);

	if (ret > 0)
	{
		printf("str1 > strt2\n");
	}
	else if (0 == ret)
	{
		printf("str1 == str2\n");
	}
	else if (ret < 0)
	{
		printf("str1 < str2\n");
	}

	return 0;
}
```

---

## C库函数`strncat()`
相比较`strcat()`，`strncat()`能指定追加的字符长度
`char *strncat(char *dest, const char *src, size_t n)`
```C
int main()
{
	char str1[20] = "Hello";
	char str2[10] = "World";

	strncat(str1, str2, 3);

	printf("%s\n", str1);

	return 0;
}
```
**注意**⚠️
- 追加后的字符串会自动补上`\0`，以确保有结束标志
- 当指定的追加长度大于源字符串长度时，只追加源字符串内容，最后补上`\0`
- 要确保被追加的目标空间能够容纳追加的字符串和`\0`

---

## C库函数`strncpy()`
把 **src** 所指向的字符串复制到 **dest**，最多复制 **n** 个字符。<mark style="background: #FFF3A3A6;">当 src 的长度小于 n 时，dest 的剩余部分将用空字节填充</mark>

```C
int main()
{
	char str1[20] = "Who am I?";
	char str2[10] = "Asleep";

	strncpy(str1, str2, 6);//输出Asleep I?
	strncpy(str1, str2, 7);//输出Asleep
	strncpy(str1, str2, 10);//输出Asleep



	printf("%s\n", str1);

	return 0;
}
```
注意⚠️
- `strncpy()`拷贝后不会自动补`\0`❗❗❗
- 当指定的拷贝字符数量大于源字符串时，超过的部分以`\0`代替
- 确保目标空间足够容纳

---

## C库函数`strncmp()`
`int strncmp(const char *str1, const char *str2, size_t n)`把 **str1** 和 **str2** 进行比较，最多比较前 **n** 个字符
```C
int main()
{
	char str1[10] = "Asleep";
	char str2[10] = "AslEEp";

	int ret = strncmp(str1, str2, 3);//返回等于0
	int ret = strncmp(str1, str2, 6);//返回大于0

	printf("%d\n", ret);

	return 0;
}
```

---

## C库函数`strstr()`
`char *strstr(const char *str1, const char *str2);`在字符串`str1`中查找子字符串`str2`第一次出现的位置。如果找到，返回指向该位置的指针；若未找到，返回NULL
```C
int main()
{
	char str1[10] = "Asleep";
	char str2[10] = "sleep";

	printf("%s\n", strstr(str1, str2));
	
	return 0;
}
```

实现自己的`strstr()`💡
```C
char* my_strstr(const char* str1, const char* str2)
{
	assert(str1 != NULL);
	assert(str2 != NULL);

	char* p1 = str1;
	char* p2 = str2;
	char* cur = str1;

	if (*str2 == '\0')
	{
		return str1;
	}
	while (*cur)
	{
		p1 = cur;//定位子字符串起始位置
		p2 = str2;

		while ((*p1 != '\0') && (*p2 != '\0') && (*p1 == *p2))
		{
			//有相同的字符进入循环开始向后比较
			p1++;
			p2++;
		}
		if (*p2 == '\0')//向后比较结束且子字符串比较完毕
		{
			return cur;//找到子字符串
		}
		cur++;//有相同字符但并未完全匹配，向后为起始位置重新比较
	}
	return NULL;//Not found，遍历整个主字符串仍未找到

}
int main()
{
	char str1[10] = "Asleep";
	char str2[10] = "sleep";

	printf("%s\n", my_strstr(str1, str2));
	
	return 0;
}
```

---

## C库函数`strtok()`
`char *strtok(char *str, const char *delim)`**strtok()** 是 C 标准库中的一个字符串处理函数，用于将字符串分割成一系列子字符串（即"标记"或"token"），分解字符串 **str** 为一组字符串，**delim** 为分隔符
```C
int main()
{
	char str[100] = "Asleep@github.com";
	const char ch[10];

	puts(str);

	puts("请输入所要分割的字符:");
	fgets(ch, sizeof(ch), stdin);

	char *token;

	for (token = strtok(str, ch); token != NULL; token = strtok(NULL, ch))
	{
		printf("%s\n", token);
	}

	return 0;
}
```
注意⚠️
- 第一次传入所需分割的字符串指针，后续只需传入`NULL`
- `strtok()` 会修改传入的字符串，将分隔符替换为 `\0`（空字符）。因此，原始字符串会被破坏

---

## C库函数`strerror()`
`char *strerror(int errnum)`从内部数组中搜索错误号 **errnum**，并返回一个指向错误消息字符串的指针。**strerror** 生成的错误字符串取决于开发平台和编译器  
>**errnum** -- 错误号，通常是 **errno**
该函数返回一个指向错误字符串的指针，该错误字符串描述了错误 errnum

示例
```C
#include <stdio.h>
#include <string.h>
#include <errno.h>

int main ()
{
   FILE *fp;

   fp = fopen("file.txt","r");
   if( fp == NULL ) 
   {
      printf("Error: %s\n", strerror(errno));
   }
   
  return(0);
}
```
让我们编译并运行上面的程序，这将产生以下结果，因为我们尝试打开一个不存在的文件：
***Error: No such file or directory***


## 字符串综合练习1

```C
//在句子中匹配给定的单词
int main()
{
	char sch[10];//查找对象
	char sent[1024];//源文章
	char cp_sent[1024];//查找文章（副本）
	char* p_first;//第一次出现的位置

	char* token;
	char* space = " ";//strtok分割标记
	int i = 0;
	int j = 0;
	int count = 0;

	printf("Please input sch:\n");
	fgets(sch, sizeof(sch), stdin);
	sch[strcspn(sch, "\r\n")] = '\0';//清除缓存区

	printf("\nPlease input sent:\n");
	fgets(sent, sizeof(sent), stdin);
	sent[strcspn(sent, "\r\n")] = '\0';//清除缓存区

	strcpy(cp_sent, sent);//拷贝副本

	//将查找对象转换为小写
	while (sch[j])
	{
		sch[j] = tolower(sch[j]);
		j++;
	}

	//将副本全部转化为小写
	while (cp_sent[i])
	{
		cp_sent[i] = tolower(cp_sent[i]);
		i++;
	}

	p_first = strstr(cp_sent, sch);//查找第一次出现位置

	token = strtok(cp_sent, space);//分割单词

	if (p_first != NULL)
	{
		while (token != NULL)
		{
			if (!strcmp(token,sch))
			{
				count++;//计数
			}
			token = strtok(NULL, space);
		}
		printf("%d\n", count);//出现次数
		printf("%d\n", (int)(p_first - cp_sent));//第一次出现位置

	}
	else
	{
		printf("-1");//未找到
	}

	return 0;
}
```

---


