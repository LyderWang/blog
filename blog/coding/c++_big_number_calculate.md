# 高精度运算

## 高精度输入输出

==倒序存储，第0位存位数==

```c++
void s2BIG(string s,int a[])
{
	int len=s.length();;
	a[0]=len;
	for(int i=1;i<=len;i++)
	{
		a[i]=s[len-i]-'0';
	}
}

void printBIG(int a[])
{
	int len=a[0];
	for(int i=len;i>=1;i--)
	{
		cout<<a[i];
	}
	cout<<endl;
}
```

## 高精度加法

模拟竖式加法

每次两个数同一位相加，处理进位

~~~c++
void add(int a[],int b[],int c[])
{
	int lenc=max(a[0],b[0]),u=0;
	for(int i=1;i<=lenc;i++)
	{
		int t=u;
		if(i<=a[0]) t+=a[i];
		if(i<=b[0]) t+=b[i];
		c[i]=t%10;
		u=t/10;
	}
	if(u>0) c[++lenc]=u;
	c[0]=lenc;
}
~~~

## 高精度减法

跟加法类似

```c++
void subBIG(int a[],int b[],int c[])
{
    int lenc=max(a[0],b[0]),u=0;
    for(int i=1;i<=lenc;i++)
    {
        int t=a[i]-u;
        if(i<=b[0]) t-=b[i];
        if(t<0)
        {
            u=1;
            c[i]=t+10;
        }
        else
        {
            u=0;
            c[i]=t;
        }
    }
    while(c[lenc]==0&&lenc>1) lenc--;
    c[0]=lenc;
}
```

## 高精度比较

先比较位数，位数大的更大，让后从高位开始比较。

```c++
int cmpBIG(int a[],int b[])
{
    if(a[0]>b[0]) return 1;
    if(a[0]<b[0]) return -1;
    for(int i=a[0];i>=1;i--)
    {
        if(a[i]>b[i]) return 1;
        if(a[i]<b[i]) return -1;
    }
    return 0;
}
```

## 高精度乘单精度

~~~c++
void mulBIG(int a[],int b,int c[])
{
    int lenc=a[0],u=0;
    for(int i=1;i<=lenc;i++)
    {
        int t=a[i]*b+u;
        c[i]=t%10;
        u=t/10;
    }
    while(u>0)
    {
        c[++lenc]=u%10;
        u/=10;
    }
    c[0]=lenc;
}
~~~

## 高精度除以单精度

~~~c++
void divBIG(int a[],int b,int c[])
{
    int lenc=a[0],r=0;
    for(int i=lenc;i>=1;i--)
    {
        int t=r*10+a[i];
        c[i]=t/b;
        r=t%b;
    }
    while(c[lenc]==0&&lenc>1) lenc--;
    c[0]=lenc;
}
~~~
