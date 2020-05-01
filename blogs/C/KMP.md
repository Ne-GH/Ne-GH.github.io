## KMP
---
KMP的代码实现:

查找目标串在主串中的重复次数:
```C
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

char W[10005], T[1000005];
int len_W, len_T;

void next_(int* next, int len_W) {
	int i = 0, j = -1;
	next[0] = -1;
	while (i < len_W - 1) {
		if (W[i] == W[j] || j == -1) {
			++i;
			++j;
			next[i] = j;

		}
		else {
			j = next[j];
		}
	}
	return;
}

int KMP_C(int len_W, int len_T) {
	int i = 0, j = 0, C = 0;
	int* next = (int*)malloc(len_W * sizeof(int));

	next_(next, len_W);
	while (i < len_T) {
		if (T[i] == W[j] || j == -1) {
			++i;
			++j;
		}

		else
			j = next[j];

		if (j == len_W) {
			j = next[j - 1];
			--i;
			++C;
		}



	}

	free(next);
	return C;
}

int main() {
	int n;
	scanf("%d\n", &n);
	while (n--) {
		scanf("%s%s", T, W);
		len_W = strlen(W);
		len_T = strlen(T);
		printf("%d\n", KMP_C(len_W, len_T));

	}
	return 0;
}
```

查找目标串在主串中第一次出现的位置(只改变kmp函数即可):
```C
int KMP（char* S,char* T）
{
    GetNext(T);
    int slen=strlen(S),tlen=strlen(T);
    int j=0,i=0;
    while(i<slen&&j<tlen)
    {
        if(j==-1||S[i]==T[j])
        {
            i++;
            j++;
        }
        else
        {
            j=next[j];//当匹配失效时，j值回退到next[j]的位置
        }
    }
    if(j==tlen)
        return i-j;
    else
        return -1;
}
```
第一个代码可以直接运行,第二个片段与第一个不很兼容,需要稍加修改