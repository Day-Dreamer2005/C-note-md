**C语言的文件操作**

打开文件：`FILE *fopen( const char *filename, const char *mode );`
在这里，**filename** 是字符串，用来命名文件，访问模式 **mode** 的值可以是下列值中的一个：
- **r**，打开一个已有*文本文件*，并且只允许读取，若文件不存在则会报错
- **r+**，打开一个*文本文件*，允许读写文件
- **w**，打开一个*文本文件*，允许写入。若文件不存在，则会创建一个新文件。<mark style="background: #FF5582A6;">若文件存在，则会清空原文件内容并打开（或者理解为重新创建一个空文件覆盖原文件，总之原文件内容将不复存在）</mark>
- **w+**，
- **a**，打开一个*文本文件*，以追加模式写入文件。若文件不存在，则会创建一个新文件
- 
```c
//C文件操作
int main()
{
	FILE* pf = fopen("test.txt", "r");//只读文件

	//打开失败
	if (pf == NULL)
	{
		printf("%s\n", strerror(errno));//输出错误信息
		return 0;
	}

	//打开成功
	//读文件
	
	//关闭文件
	fclose(pf);
	pf = NULL;

	return 0;
}
```