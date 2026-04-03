## 标准文件

**C 语言把所有的设备都当作文件**。所以设备（比如显示器）被处理的方式与文件相同。以下三个文件会在程序执行时自动打开，以便访问键盘和屏幕。

| 标准文件 | 文件指针   | 设备   |
| ---- | ------ | ---- |
| 标准输入 | stdin  | 键盘   |
| 标准输出 | stdout | 屏幕   |
| 标准错误 | stderr | 您的屏幕 |

---

## C输入函数  
### `fgets()`
`fgets()`函数可以用来读取一行字符串（包括空格）  
语法：
	`char *fgets(char *str,int n,FILE *stream);`  
其中
- `str`为指向数组的指针
- `n`为要读取的最大字符数（包括字符`\0`）
- `stream`表示文件流，通常使用`stdin`表示标准输入

注意⚠️
>`fgets()`会读取末尾的换行符`\n`

实例  
```C
#include <stdio.h>

int main() {
    char str[100];
    
    printf("Enter a string: ");
    fgets(str, sizeof(str), stdin);
    
    //fgets()会读取换行符\n，可能会导致意外，建议将\n用\0替换掉
    str[strcspn(str,"\n")] = '\0';
    
    printf("You entered: %s", str);
    return 0;
}
```

---

### `fprintf()`
将第三个参数的数据以第二个参数的格式输出到第一个参数
```c
int main()
{
    FILE* pf = fopen("test.txt","w");
    if(pf == NULL)
    {
        printf("%s\n",strerror(errno));
        return 0;
    }

    struct data
    {
        char name[20];
        int age;
        char career[50];
    };

    struct data example = {"AsleepDragon",20,"AI full stack engineer"};

    fprintf(pf,"name:%s\nage:%d\ncareer:%s\n",example.name,example.age,example.career);

    fclose(pf);
    pf = NULL;

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