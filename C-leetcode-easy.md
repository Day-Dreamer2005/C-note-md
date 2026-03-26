
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