C语言中**结构体类型**可以包含多种数据类型，我们可以自由定义
结构体的声明，创建变量和访问
```C
//声名结构体
struct Stu
{
	char name[20];
	int age;
	char phone_num[12];
};

int main()
{
	//创建结构体变量
	struct Stu stu = {"Asleep",20,"18339001877"};

	//访问结构体成员
	printf("name:%s , age:%d , phone_num:%s\n", stu.name, stu.age, stu.phone_num);

	return 0;
}
```

结构体的声明多种多样：
```c
//匿名结构体
struct
{
	//member-list;
}//variabe-list;
```


```C
//重命名结构体
typedef struct Stu
{
	//member-list;
}stu;//struct Stu类型被命名为stu，即可以使用stu创建该类型结构体变量

stu s1;

```