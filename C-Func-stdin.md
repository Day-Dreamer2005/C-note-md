
# C输入函数  

`fgets()`函数可以用来读取一行字符串（包括空格）  
语法：
	> `char *fgets(char *str,int n,FILE *stream);`  
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

