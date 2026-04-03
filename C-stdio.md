## 标准文件

**C 语言把所有的***设备***都当作文件**。所以设备（比如显示器）被处理的方式与文件相同。以下三个文件会在程序执行时自动打开，以便访问键盘和屏幕。

| 标准文件 | 文件指针   | 设备   |
| ---- | ------ | ---- |
| 标准输入 | stdin  | 键盘   |
| 标准输出 | stdout | 屏幕   |
| 标准错误 | stderr | 您的屏幕 |

---

- **`scanf`** 从 **标准输入流 `stdin`** 读取数据。  
- **`printf`**（以及 `fprintf`、`fputs`、`puts` 等）把输出写入 **标准输出流 `stdout`**。  

在默认情况下，`stdin` 与 **键盘**（或管道/文件重定向）相连，`stdout` 与 **屏幕/终端**（或文件重定向）相连。因此在普通的交互式程序里，`scanf` 看起来是“从键盘读”，`printf` 看起来是“向屏幕打印”。  

---

### 1️⃣ 标准 I/O 流的概念

| 宏名       | 对应的`FILE*`     | 典型设备          | 说明                                        |
| -------- | -------------- | ------------- | ----------------------------------------- |
| `stdin`  | `FILE *stdin`  | 键盘（或文件重定向）    | **输入**流，`scanf`、`getchar`、`fgets` 等默认读取它。 |
| `stdout` | `FILE *stdout` | 屏幕/终端（或文件重定向） | **输出**流，`printf`、`puts`、`write` 等默认写入它。   |
| `stderr` | `FILE *stderr` | 屏幕/终端（通常不重定向） | 错误信息的默认输出流，`fprintf(stderr, …)` 常用于报错。    |

这些流在程序启动时已经 **打开并与操作系统的文件描述符 0、1、2 关联**，所以不需要显式 `fopen`。

---

### 2️⃣ `scanf` 与 `printf` 的默认行为

```c
int n;
scanf("%d", &n);      // 读取键盘输入（stdin）
printf("你输入的数是 %d\n", n);   // 向终端输出（stdout）
```

- **`scanf`**：  
  - 参数列表中不需要再提供文件指针，因为它默认使用 **`stdin`**。  
  - 如果标准输入被重定向（例如 `myprog < input.txt`），`scanf` 会从那个文件读取。  
  - 也可以显式指定流：`vscanf`、`fscanf(fp, …)`，但普通的 `scanf` 永远默认 `stdin`。

- **`printf`**：  
  - 同样默认使用 **`stdout`**。  
  - 若把 `stdout` 重定向到文件（`myprog > out.txt`），输出会写入那个文件。  
  - 也可以显式指定流：`fprintf(fp, …)`、`fputs(fp, …)`、`puts(fp, …)`。

---

### 3️⃣ 为什么会有“默认键盘/屏幕”的感觉？

1. **交互式运行**：在终端里直接敲代码、回车，程序的 `stdin` 与键盘相连，`stdout` 与终端相连，所以看起来像是“键盘 → 输入”， “屏幕 ← 输出”。  
2. **标准库的默认实现**：`scanf`、`printf` 并不做任何文件打开的工作，它们只是调用底层的 **文件描述符 0**（stdin）和 **1**（stdout）。  
3. **可重定向**：只要把标准流换成别的文件，行为立刻改变，说明它们并不是硬绑定在“键盘/屏幕”，而是绑定在 **流** 上。

---

### 4️⃣ 常见的流相关操作示例

```c
/* 1. 把 stdin 从键盘改成读取文件 */
FILE *fp = fopen("data.txt", "r");
if (fp) {
    dup2(fileno(fp), fileno(stdin));   // 现在 scanf 会从 data.txt 读
    fclose(fp);
}

/* 2. 把 stdout 重定向到文件 */
FILE *out = fopen("log.txt", "w");
if (out) {
    dup2(fileno(out), fileno(stdout)); // printf 现在写入 log.txt
    fclose(out);
}

/* 3. 只把错误信息发送到标准错误流 */
fprintf(stderr, "出错了！错误码=%d\n", errno);
```

- `dup2`（POSIX）或 `SetStdHandle`（Windows）可以把 **文件描述符** 复制到 `0、1、2`，从而改变 `stdin/stdout/stderr` 的目标。  
- 在 Windows 上，C 运行时也提供 `_dup2`、`_setmode` 等函数，原理相同。

---

### 5️⃣ 小结

- **`scanf`** → 默认读取 **`stdin`**（键盘或被重定向的输入）。  
- **`printf`** → 默认写入 **`stdout`**（屏幕或被重定向的输出）。  
- 它们的行为取决于 **当前的流状态**，而不是硬编码的“键盘/屏幕”。  
- 通过文件重定向、dup2/SetStdHandle 等系统调用，可以把这些流指向任何文件、管道或网络套接字，从而实现更灵活的 I/O 处理。

如果你想让 `scanf` 读取文件或让 `printf` 把结果写入文件，只需要 **改变对应流的指向**；不需要在 `scanf`/`printf` 的调用方式上做任何修改。  

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
### `fscanf`
将  **第一个参数流中的数据**  以  **第二个参数的格式**  输入到  **第三个参数指定的内存中**
```c
int main()
{
    struct data
    {
        char name[20];
        int age;
        char career[50];
    };

    struct data example = {0};

    FILE* pf = fopen("test.txt","r");
    if(pf == NULL)
    {
        printf("%s\n",strerror(errno));
        return 0;
    }
    fscanf(pf,"%s%d%s",&(example.name),&(example.age),&(example.career));

    printf("%s\n%d\n%s\n",example.name,example.age,example.career);
    return 0;

}
```



## C输出函数  

### `puts()`
将一个字符串输出到标准输出设备，并自动在末尾添加换行符  
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

### `fprintf()`
将  **第三个参数的数据**  以  **第二个参数的格式**  输出到  **第一个参数对应的流中(FILE*)**  

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
---

### `sprintf()`
将数据格式化输出到字符串中
```c
int main()
{
    char str[100];

    sprintf(str,"name:%s\nage:%d","AsleepDragon",20);

    puts(str);

    return 0;
}
```
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