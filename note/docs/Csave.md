# code save

# rethink

1. 先整理清思路，分模块步步完成。
2. 循环内重复使用的数据要在下一次循环前格式化
3. if( == ) not if( = ) ,scanf(”%lf”,&(double)x),不要忘了 &和lf!!!
- 1.**出栈序列的合法性**
    
    给定一个最大容量为 *M* 的堆栈，将 *N* 个数字按 1, 2, 3, ..., *N* 的顺序入栈，允许按任何顺序出栈，则哪些数字序列是不可能得到的？例如给定 *M*=5、*N*=7，则我们有可能得到{ 1, 2, 3, 4, 5, 6, 7 }，但不可能得到{ 3, 2, 1, 7, 5, 6, 4 }。
    
    ### **输入格式：**
    
    输入第一行给出 3 个不超过 1000 的正整数：*M*（堆栈最大容量）、*N*（入栈元素个数）、*K*（待检查的出栈序列个数）。最后 *K* 行，每行给出 *N* 个数字的出栈序列。所有同行数字以空格间隔。
    
    ### **输出格式：**
    
    对每一行出栈序列，如果其的确是有可能得到的合法序列，就在一行中输出`YES`，否则输出`NO`。
    
    ### **输入样例：**
    
    ```
    5 7 5
    1 2 3 4 5 6 7
    3 2 1 7 5 6 4
    7 6 5 4 3 2 1
    5 6 4 3 7 2 1
    1 7 6 5 4 3 2
    
    ```
    
    ### **输出样例：**
    
    ```
    YES
    NO
    NO
    YES
    NO
    ```
    
```c
#include<stdio.h>
int push(int a[],int top,int x)
{
    a[top] = x;
    top++;
    return top;
}
int main()
{
    int m,n,k;
    scanf("%d %d %d",&m,&n,&k);
    int a[m];
    int b[n]; //save scanf
    for(int i = 0;i < k;i++)
    {
        int x = 1;
        int top = 0;
        int cnt = 0;
        int end = 1; //if 1,printf No
        for(int j = 0;j<m;j++)
        {
            a[j] = 0;
        }
        for(int j = 0;j<n;j++)
        {
            scanf("%d",&b[j]);
        }
        while(top < 5) //empty max = 5
        {
            top = push(a,top,x);
            x++;
            while(a[top-1] == b[cnt] && top>0 )
            {
                cnt++;
                top--;
            }
            if(cnt == 7)
            {
                printf("Yes\n");
                end = 0;
                break;
            }
        }
        if(end)
        {
            printf("No\n");
        }
    }
}
```
    
- 2.**Gram-Schmidt算法**
    
    Gram-Schmidt算法用来确定一组 *n* 向量 *a*1,⋯,*ak* 是否为线性无关的。
    
    给定 *n* 向量 *a*1,⋯,*ak*，令 *q*0 为 *n* 维零向量，对于 *i*=1,⋯,*k*，
    
    1. 正交化：*q*~*i*=*ai*−(*q*1*T**ai*)*q*1−⋯−(*qi*−1*T**ai*)*qi*−1
    2. 检验线性相关性：若 *q*~*i*=0，则算法结束，当前向量和之前的向量线性相关
    3. 规范化：*qi*=∣∣*q*~*i*∣∣*q*~*i*
    
    对于 *i*=1，由于 *q*0为零，步骤1简化为 *q*~1=*a*1。如果算法没有提前退出，则向量组 *a*1,⋯,*ak* 是线性无关的；否则，若算法在 *i*=*j* 时退出，即 *q*~*j*=0，即意味着 *aj* 是 *a*1,⋯,*aj*−1 的线性组合，向量组是线性相关的。
    
    注意，考虑到浮点计算误差，取*ϵ*=10−6，即当两数距离小于`1e-6`时视作相等。
    
    又：
    
    ### **Euclid范数**
    
    一个 *n* 向量 *x* 的Euclid范数记为 ∣∣*x*∣∣ ：
    
    ∣∣*x*∣∣=*x*12+*x*22+⋯+*xn*2
    
    也可以表示为向量与其自身做内积的平方根 ∣∣*x*∣∣=*xTx*。
    
    ### **向量的内积（点积）**
    
    两个 *n* 向量的内积（也称作点积）定义为如下的标量：
    
    *aTb*=*a*1*b*1+*a*2*b*2+⋯+*an**bn*
    
    即对应元素乘积的和。有的书上记做 *a*⋅*b* 。
    
    本题由**张庆春**制作数据并提供标程。
    
    ### **输入格式:**
    
    在第一行分别给出两个整数N，M。N表示向量的维度，M表示向量的个数。
    
    接下来M行给出M个向量，用浮点数表示，每个数字之间用空格分隔。
    
    （对于区分`float`和`double`的语言，建议使用`double`类型）
    
    ### **输出格式:**
    
    如果发现输入向量组是向量线性无关的，则输出 `NO`，并在第二行输出最终的 *q*，输出使用保留两位小数，每个数字之间一个空格。
    
    如果发现输入向量组是向量线性相关的，则输出`YES`，并接着输出算法运行到第几个向量（从1开始计数）时停止的。
    
    ### **输入样例1:**
    
    ```
    5 4
    1 2 -1 -2 -3
    2 -1 3 -2 7
    -1 3 2 4 28
    3 10 -2 1 1
    
    ```
    
    ### **输出样例1:**
    
    ```
    NO
    0.52 0.36 0.49 0.59 -0.14
    
    ```
    
    ### **输入样例2:**
    
    ```
    3 4
    0.1 1 2
    -0.3 0 -5
    3 0 -0.3
    0.2 -1 3
    
    ```
    
    ### **输出样例2:**
    
    在这里给出相应的输出。例如：
    
    ```
    YES 4
    ```
    
    ```c
    #include<stdio.h>
    #include<math.h>
    int check(double p[],int n)
    {
        int ret = 0;
        int cnt = 0;
        for(int i = 0;i < n;i++)
        {
            if(p[i] < 1e-6){cnt++;}
        }
        if(cnt == n){ret = 1;}
        return ret;
    }
    double euclid(double a[],int n)  //get Euclid
    {
        double product = 0;
        for(int i = 0;i<n;i++) 
        {
            product += a[i] * a[i];
        }
        double ret = sqrt(product);
        return ret;
    }
    double inner_product(double q[],double a[],int n)
    {
        double ret = 0;
        for(int i = 0;i<n;i++)
        {
            ret += a[i] * q[i];
        }
        return ret;
    }
    int main()
    {
        int n,m;
        scanf("%d %d",&n,&m);
        double a[n],q[m][n];
        for(int i = 0;i < n;i++)
        {
            scanf("%lf",&a[i]);
        }
        double d = euclid(a,n);
        if(d < 1e-6)
        {
            printf("YES 1");
            goto end;
        }
        for(int i = 0;i < n;i++)
        {
            q[0][i] = a[i]/d;
        }
        for(int j = 1; j < m;j++)
        {
            for(int i = 0;i < n;i++)
            {
                scanf("%lf",&a[i]);
            }
            for(int i = 0;i < n;i++)
            {
                q[j][i] = a[i];
            }
            for(int k = 0;k < j;k++) //正交化
            {
                double tem = inner_product(q[k],a,n);
                for(int i = 0;i < n;i++)
                {
                    q[j][i] -= tem * q[k][i];
                }
            }
            if(check(q[j],n))
            {
                printf("YES %d",j+1);
    end:
                break;
            }
            double d = euclid(q[j],n);
            for(int i = 0;i < n;i++)  //q[i]规范化
            {
                q[j][i] = q[j][i]/d;
            } 
            if(j == m - 1)
            {
                printf("NO\n");
                printf("%.2f",q[j][0]);
                for(int i = 1;i<n;i++)
                {
                    printf(" %.2f",q[j][i]);
                }
            }
        }
    }
    ```