
# <mark style="background: #FF5582A6;">C指针与陷阱</mark>
### 关于数组指针
若定义一个指针变量p，==`int *p = NULL`==，只能存放数组的某个元素地址  
若要存放整个数组的地址，则应该使用指针数组  
> `int arr[10] = {0};`
> `int (*ptr)[10] = &arr;`  

