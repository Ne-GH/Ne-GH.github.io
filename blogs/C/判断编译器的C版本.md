判断编译器的C版本
=
---
众所周知,可以通过宏``__STDC_VERSION__``判断C的版本

各版本__STDC_VERSION__值如下表

|版本|``__STDC_VERSION__``|
|:-:|:-:|
|C99前|不支持该宏|
|C99|199901L|
|C11|201112L|
|C18(C17)|201710L|

可以用``printf("%ld\n",__STDC_VERSION__);``打印该值