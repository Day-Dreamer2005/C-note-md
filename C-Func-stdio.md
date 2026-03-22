
## C输入函数  

`fgets()`函数可以用来读取一行字符串（包括空格）  
语法：
	`char *fgets(char *str,int n,FILE *stream);`  
其中
- `str`为指向数组的指针
- `n`为要读取的最大字符数（包括字符`\0`）
- `stream`表示文件流，通常使用`stdin`表示标准输入

实例  
```C
#include <stdio.h>

int main() {
    char str[100];
    printf("Enter a string: ");
    fgets(str, sizeof(str), stdin);
    printf("You entered: %s", str);
    return 0;
}
```

## C输出函数  

`puts()`函数将一个字符串输出到标准输出设备，并自动在末尾添加换行符  
语法：
		`int puts(const char *str);`  
其中  
- `str`为要输出的字符串  

实例  
```C
#include <stdio.h>

int main() {
    char str[] = "Hello, World!";
    puts(str);
    return 0;
}
```

---
```C
#include <stdio.h>
#include <string.h>

int main()
{
	char str[100];
	char ch;
	char* ret;

	puts("search what:");//puts函数会在末尾自动添加换行符
	ch = getchar();
	getchar();//清空缓存区

	puts("Please input a str:");

	fgets(str, sizeof(str), stdin);
	//使用标准输入方式输入字符串到str，最多输入sizeof(str)个字符（包括'\0'）

	ret = (char*)memchr(str,ch,strlen(str));
	//搜索字符ch在字符串str第一次出现的位置，搜索范围前strlen(str)个字节，若找到返回该位置地址，否则返回NULL

	fputs(ret, stdout);//标准输出ret所指向的字符串


	return 0;
}
```

---
```C
#include <stdio.h>
#include <string.h>

int main()
{
	char arr1[20];
	char arr2[20];

	strcpy(arr1, "Hello ");
	strcpy(arr2, "World!");

	printf("Before strcat:%s\n",arr1);

	strcat(arr1, arr2);//将arr2内容追加到arr1后（但要注意arr1溢出风险）

	printf("\nAfter strcat:%s\n",arr1);
	
	return 0;
}
```

---
## 自定义实现`strlen()`函数功能  

### 1.计数器方法  
```C
int my_strlen(const char* str)
{
	int count = 0;
	assert(str);//判断所传指针不为空

	while (*str)
	{
		count++;
		str++;
	}
	return count;
}
int main()
{
	int len = my_strlen("Asleep");

	printf("The str's lenth:%d\n", len);

	return 0;
}
```