# varible function

- fabs(double x)求浮点数绝对值
- **math.h**
    - 幂运算: `int mask = pow(10,cnt-1);`
    - 根号:`sqrt(n)`
- **string.h**
    
    *提醒: 下边的前4个函数最后的n不是必需的*
    
    - strlen(const char *s);返回s的字符串长度（不包含结尾的0）
    - strcmp(const char *s1,const char *s2,n);比较两个字符串，返回：
        - 0: s1==s2
        - 1: s1>s2
        - -1: s1<s2
    - strcpy(char *restrict dst,const char *restrict src,n); 把src的字符串copy到dst
        - restrict 表示src和dst不重叠
        - 返回dst
        - 复制一个字符串
        
        ```c
        char *dst = (char*)malloc(strlen(src)+1);
        strcpy(dst,src);
        ```
        
    - strcat(char *restrict s1,const char *restrict s2,n); 把s2拷贝到s1后面。
        - 返回s1
        - s1必须有足够空间
    - char * strchr(const char *s,int c);  从左边search
    char * strrchr(const char *s,int c); 从右边search
        - 返回NULL表示没有找到
        - 寻找第二个字符的操作:
            
            ```c
            
            char s[] = "Hello";
            char *p = strchr(s,'l');
            p = strchr(p+1,'l');
            ```
            
        - 复制某个字符前的内容
            
            ```c
            char s[] = "Hello";
            char *p = strchr(s,'l');
            char c = *p;
            *p = '\0';
            char *t = (char*)malloc(strlen(s)+1);
            strcpy(t,s);
            printf("%s",t);
            free(t);
            *p = c;
            
            ```
            
    - char * strstr(const char *s1,const char *s2); 字符串中寻找字符串
    char * strcasestr(const char *s1,const char * s2);忽略大小写
    
- **stdlib.h**
    - *qsort(排序)*
        - 1.数组名，元素个数，数组元素所占字节（int，double），排序原则（递增，递减，奇偶交叉）
        
        ```c
        #include<stdlib.h>
        int cmp(const void *a,const void *b)
        {
        	return *(int*)a - *(int*)b;
        }
        //此时表示递增，若想递减只需将a，b换位
        
        qsort(num,n,sizeof(int),cmp)
        ```
        
        ```c
        //浮点数（double） 
        
        //需要注意浮点数会存在精度损失的问题，所以我们需要通过比较，来返回1或-1，以确定是增序还是降序。
        int cmp(const void *a,const void *b) {
        	return *(double*)a>*(double*)b?1:-1;
        }
        ```
        
        ```c
        //字符
        int cmp(const void *a,const void *b) {
        	return *(char*)a-*(char*)b;
        }
        ```
        
        ```c
        //结构体
        struct node{
        	int i;
        	double j;
        	char k;
        };
        int cmp(const void *a,const void *b) {
        	return (*(node*)a).i-(*(node*)b).i;
        }
        ```
        
    - *malloc（动态内存）free()*
        - 返回的结果是void* ,需要类型转换为所需的
        - (int*)malloc(n*sizeof(int))
        - 最后需要
        - 申请失败则返回0，或者叫做NULL
        - free()将申请的来的空间还给系统
        
        ```c
        void *p;
        int i;
        p = malloc(3);
        free(p); //right
        p = &i;
        free(p); //wrong
        ```
        
- `atoi(const char *str)` 将字符串转化为整数型，字符串中必须要有终止符