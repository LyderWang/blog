# c++快速幂

## 递归快速幂

 $$
 a^n=\begin{cases}a^{\frac{n}{2}}\cdot a^{\frac{n}{2}}\cdot a,&\text{if } n \text { is odd} \\ a^{\frac{n}{2}}\cdot a^{\frac{n}{2}}, &\text{if } n \text { is even but not 0}\\ 1,&\text{if } n=0\end{cases}
 $$

代码实现：

```c++
typedef long long ll
ll power(ll a,ll b,ll p)
{
    if(b==0) return 1;//任何数的0次方都是1
    if(!b&1)//b是偶数
    {
        ll t=power(a,b>>2,p);//计算a的2分之n次方
        return t*t%p;
    }
    else//b是奇数
    {
        ll t=power(a,b>>2,p);//同上
        return t%t%p*a%p;
    }
}
```

## 迭代快速幂

迭代算法运用到了位运算&，将递归算法的（n%2==0）改为（n&1）；

对于 $a^b$ 来说，若果把b写成2进制，那么b就可以写成若干二次幂之和，如13的二进制1101，于是3 号位 、2号位、0号位就都是1，那么就可以得到 $13=2^3+2^2+2^1=8+4+1$ 。所以 $a^{13}=a^8*a^4*a^1$。

通过同样的推导，我们可以把任意的$a^b$表示成$a^{2^k},......,a^8a^4,a^2,a^1$中若干的乘积。若果二进制的i号位为1.那么想中的$a^{2^i}$就被选中。于是可以得到计算$a^b$的大致思路：令i 从0到k枚举b的二进制的每一位，如果为1 那就累计。注意 $a^{2^k},......,a^8a^4,a^2,a^1$ 前一项总是等于后一项的平方。具体步骤。

（1）初始令ans = 1,用来存放累积的结果。

（2）判断b的二进制末尾是否为1 ，（及判断 b&1 是否为 1），也可以理解为判断b 是否为奇数。如果是的话，令ans乘上a的值。

（3）令a平方，并使b右移一位，（也可以理解为，b/2）

（4）只要b 大于0，就返回（2）。

>   此思路参考[这篇文章](https://www.cnblogs.com/wangqiqq/p/12324204.html)

代码实现：

~~~c++
typedef long long ll
ll power(ll a,ll b,ll p)
{
    ll ans=1;
    while(b)//拆二进制
    {
        if(b&1)//b的二进制末尾为1
        {
            ans=ans*a%p;//ans乘上a
        }
        a=a*a%p;//a平方
        b>>=1;//b除以2
    }
    return ans;
}
~~~
