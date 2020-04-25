ANSI C的printf的实现
=
---
参靠《C陷阱与缺陷》一书
```C
#include<stdio.h>
#include<stdlib.h>
#include<stdarg.h>//ANSI C可变参数的头文件

int print(char* format, ...) {
    //为了区分C语言中的printf,将函数命名为print
	va_list ap;
	int n;
	va_start(ap, format);
	n = vprintf(format, ap);
	va_end(ap);
	return 0;
}

int main() {
    int ch1=10,ch2=20;
	print("%d\t%d\n",ch1,ch2);
	return 0;
}
```
可在复制粘贴后直接运行




