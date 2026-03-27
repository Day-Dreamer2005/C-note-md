
### 罗马数字转整数
```C
//罗马数字转整数
//遍历比较，若该值小于下一个值则取负（减去自身），否则取正（加上自身）
//使用switch语句映射每个罗马字符对应的整数值
int roman_char_val(char s)
{
	switch (s)
	{
		case 'I': return 1;
		case 'V': return 5;
		case 'X': return 10;
		case 'L': return 50;
		case 'C': return 100;
		case 'D': return 500;
		case 'M': return 1000;
		default: return 0;
	}
}
int romanToint(const char* s)
{
	int len = strlen(s);
	int total = 0;
	int current = 0;

	for (int i = 0; i < len; i++)
	{
		current = roman_char_val(s[i]);

		if (current < roman_char_val(s[i + 1]) && i+1<len)
		{
			total -= current;
		}
		else
		{
			total += current;
		}
	}
	return total;
}

int main()
{
	char roman[16];
	fgets(roman, sizeof(roman), stdin);
	roman[strcspn(roman, "\n")] = '\0';

	int ret = romanToint(roman);

	printf("%s to int: %d\n", roman, ret);

	return 0;
}
```

---

### 最长公共前缀
多种算法：
- 横向扫描，比较相邻两个字符串的公共前缀然后更新
- 纵向扫描，以一个字符串为基准，纵向比较每一个字符

```c
char* longestCommonPrefix(char** strs, int strsSize) {

    //以第一个字符串为比较基准
    //从第一个字符串的第一个开始和其它字符串比较

    //若字符串为空
    if(0 == strsSize)
    {
        char* empty = (char*)malloc(1);

        empty = '\0';

        return empty;
    }

    //字符串不为空

    char* first = strs[0];//以第一个字符串为比较基准

    int len = strlen(first);

    int i = 0;

    for(i = 0;i<len;i++)//；轮流比较字符
    {
        for(int j = 1;j<strsSize;j++)//轮流比较字符串
        {
            if(first[i] != strs[j][i] || strs[j][i] == '\0')
            {
                char* ret = (char*)malloc(i+1);

                strncpy(ret,first,i);

                ret[i] = '\0';

                return ret;
            }
        }
    }

    //第一个字符串全部匹配，即为子字符串
    if(first[i] == '\0')
    {
        char* ret = (char*)malloc(i+1);

        strcpy(ret,first);

        return ret;
    }

    return '\0';
}
```

---

### 括号匹配（栈实现）
```c
bool isValid(char* s) {

    //栈实现
    //左括号入栈，右括号匹配栈顶
    //右括号匹配前判断栈是否为空
    //所有字符均比较匹配过且最后栈为空则返回top == 0 (true)

    int len = strlen(s);
    char stack[10001];//创建栈
    int top = 0;//指向栈顶

    for(int i = 0;i<len;i++)
    {
        //左括号入栈
        if(s[i] == '(' ||
           s[i] == '[' ||
           s[i] == '{'  )
        {
            stack[top++] = s[i];
        }
        //右括号匹配
        else
        {
            //判断栈是否空
            if(0 == top)
            {
                //栈为空匹配失败
                return false;
            }
            else
            {
                if((s[i] == ')' && stack[top-1] != '(') ||
                   (s[i] == ']' && stack[top-1] != '[') ||
                   (s[i] == '}' && stack[top-1] != '{'))
                {
                    //括号匹配失败
                    return false;
                }
                else
                {
                    //匹配成功
                    top--;
                }
            }
        }
    }
    //遍历字符串后若栈空则全部匹配
    return 0 == top;
}
```