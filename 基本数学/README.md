# 基础数学
- 乱序

| 类型 |        标题        | 分数 | 标签 |
| :--: | :----------------: | :--: | :--: |
| 跟随 |      gcd 模板      |  1   |      |
| 跟随 |    质数筛选模板    |  1   |      |
| 跟随 |    欧拉函数模板    |  1   |      |
| 跟随 |  扩展欧几里得模板  |  1   |      |
| 跟随 |   二分快速幂模板   |  1   |      |
| 跟随 | 矩阵二分快速幂模板 |  1   |      |

## gcd 的使用

- gcd

### 文件名
gcd.cpp

### 分数
1

---
### 初始化代码
```c++
#include <iostream>
using namespace std;

int main() {

    return 0;
}
```

---
### 第一步
#### 讲解
这一节我们学习如何求两个整数的最大公因数。
最大公因数，也称最大公约数、最大公因子，指两个或多个整数共有约数中最大的一个。
先定义两个整数`n`和`m`并从键盘输入。在`main`函数里写下
```c++
int n, m;
cin >> n >> m;
```

#### 代码
```c++
#include <iostream>
using namespace std;

int main() {
    int n, m;
    cin >> n >> m;

    return 0;
}
```

---
### 第二步
#### 讲解
首先我们定义一个`gcd`函数，这个函数用来用来求传入的两个参数的最大公约数。

在`main`函数上方写下
```c++
int gcd(int a, int b) {

}
```

#### 提示
```c++
#include <iostream>
using namespace std;
int gcd(int a, int b) {

}
int main() {
    int n, m;
    cin >> n >> m;

    return 0;
}
```

#### 代码
```c++
#include <iostream>
using namespace std;
int gcd(int a, int b) {

}
int main() {
    int n, m;
    cin >> n >> m;

    return 0;
}
```

---
### 第三步
#### 讲解
接下来我们要实现`gcd`的具体过程。
我们要用到辗转相除的方法来计算两个数的最大公约数。
过程如下：
1、a % b得余数c
2、若c = 0,则b即为两数的最大公约数
3、若c != 0,则a = b, b = c,再回去执行过程1
根据上面的思路我们可以实现代码如下
```c++
int gcd(int a, int b) {
    if (b == 0) {
        return a;
    }
    return gcd(b, a % b);
}
```

#### 提示
```c++
#include <iostream>
using namespace std;
int gcd(int a, int b) {
    if (b == 0) {
        return a;
    }
    return gcd(b, a % b);
}
int main() {
    int n, m;
    cin >> n >> m;

    return 0;
}
```


#### 代码
```c++
#include <iostream>
using namespace std;
int gcd(int a, int b) {
    if (b == 0) {
        return a;
    }
    return gcd(b, a % b);
}
int main() {
    int n, m;
    cin >> n >> m;

    return 0;
}
```

---
### 第四步
#### 讲解
最后输出所求的`gcd`的值。在`main`里继续写下
```c++
cout << gcd(n, m) << endl;
```

#### 提示
```c++
#include <iostream>
using namespace std;
int gcd(int a, int b) {
    if (b == 0) {
        return a;
    }
    return gcd(b, a % b);
}
int main() {
    int n, m;
    cin >> n >> m;
    cout << gcd(n, m) << endl;
    return 0;
}
```


#### 代码
```c++
#include <iostream>
using namespace std;
int gcd(int a, int b) {
    if (b == 0) {
        return a;
    }
    return gcd(b, a % b);
}
int main() {
    int n, m;
    cin >> n >> m;
    cout << gcd(n, m) << endl;
    return 0;
}
```



---
### 完成讲解
终于完成了，点击运行，输入下面的数据看看效果吧。
```
8 12
```

***


## prime 的使用
- prime

### 文件名
prime.cpp

### 分数
1

---
### 初始化代码
```c++
#include <iostream>
using namespace std;

int main() {

    return 0;
}
```

---
### 第一步
#### 讲解
这一节我们学习如何素数打表，如求100以内的素数有哪些。
素数又称为质数，质数定义为在大于1的自然数中，除了1和它本身以外不再有其他因数。
先定义一个布尔类型的标记数组`is_prime`。在`main`函数上方写下
```c++
bool is_prime[100];
```

#### 代码
```c++
#include <iostream>
using namespace std;
bool is_prime[100];
int main() {

    return 0;
}
```

---
### 第二步
#### 讲解
我们要用埃式筛法来快速筛选素数。
算法思路：我们从最小的素数2开始，把2的倍数的数排除，然后把3的倍数的数排除，因为4是2的倍数已经被排除了，所以再把5的倍数的数排除，一直这样把所有的素数的倍数排除，剩下的就只有素数了。
我们先初始化数组，在`main`函数里面写下
```c++
for (int i = 2; i < 100; ++i) {
    is_prime[i] = 1;
}
```

#### 提示
```c++
#include <iostream>
using namespace std;
bool is_prime[100];
int main() {
    for (int i = 2; i < 100; ++i) {
        is_prime[i] = 1;
    }

    return 0;
}
```

#### 代码
```c++
#include <iostream>
using namespace std;
bool is_prime[100];
int main() {
    for (int i = 2; i < 100; ++i) {
        is_prime[i] = 1;
    }

    return 0;
}
```

---
### 第三步
#### 讲解
然后实现上面的埃式筛法。注意我们只需遍历到sqrt(100)，因为后面是重复的。
根据上面的思路我们可以实现代码如下
```c++
for (int i = 2; i * i < 100; ++i) {
    if (is_prime[i]) {
        for (int j = i * i; j < 100; j +=i) {
            is_prime[j] = 0;
        }
    }
}
```

#### 提示
```c++
#include <iostream>
using namespace std;
bool is_prime[100];
int main() {
    for (int i = 2; i < 100; ++i) {
        is_prime[i] = 1;
    }
    for (int i = 2; i * i < 100; ++i) {
        if (is_prime[i]) {
            for (int j = i * i; j < 100; j +=i) {
                is_prime[j] = 0;
            }
        }
    }

    return 0;
}
```


#### 代码
```c++
#include <iostream>
using namespace std;
bool is_prime[100];
int main() {
    for (int i = 2; i < 100; ++i) {
        is_prime[i] = 1;
    }
    for (int i = 2; i * i < 100; ++i) {
        if (is_prime[i]) {
            for (int j = i * i; j < 100; j +=i) {
                is_prime[j] = 0;
            }
        }
    }

    return 0;
}
```

---
### 第四步
#### 讲解
最后输出我们计算出来的100以内的所有素数的值。在`main`里继续写下
```c++
for (int i = 2; i < 100; ++i) {
    if (is_prime[i] == 1) {
        cout << i << " ";
    }
}
cout << endl;
```

#### 提示
```c++
#include <iostream>
using namespace std;
bool is_prime[100];
int main() {
    for (int i = 2; i < 100; ++i) {
        is_prime[i] = 1;
    }
    for (int i = 2; i * i < 100; ++i) {
        if (is_prime[i]) {
            for (int j = i * i; j < 100; j +=i) {
                is_prime[j] = 0;
            }
        }
    }
    for (int i = 2; i < 100; ++i) {
        if (is_prime[i] == 1) {
            cout << i << " ";
        }
    }
    cout << endl;

    return 0;
}
```


#### 代码
```c++
#include <iostream>
using namespace std;
bool is_prime[100];
int main() {
    for (int i = 2; i < 100; ++i) {
        is_prime[i] = 1;
    }
    for (int i = 2; i * i < 100; ++i) {
        if (is_prime[i]) {
            for (int j = i * i; j < 100; j +=i) {
                is_prime[j] = 0;
            }
        }
    }
    for (int i = 2; i < 100; ++i) {
        if (is_prime[i] == 1) {
            cout << i << " ";
        }
    }
    cout << endl;

    return 0;
}
```



---
### 完成讲解
这一节已经完成了，赶紧运行看看效果。

聪明的你一定学会了怎么素数打表了。

***


## euler 的使用
- euler

### 文件名
euler.cpp

### 分数
1

---
### 初始化代码
```c++
#include <iostream>
using namespace std;

int main() {

    return 0;
}
```

---
### 第一步
#### 讲解
这一节我们学习欧拉函数。
在数论，对正整数n，欧拉函数是小于n的正整数中与n互质的数的数目（φ(1)=1）
互质是公约数只有1的两个整数，叫做互质整数。
先输入一个整数`n`，我们来求`n`的欧拉函数值。在`main`函数里面写下
```c++
int n;
cin >> n;
```

#### 代码
```c++
#include <iostream>
using namespace std;
int main() {
    int n;
    cin >> n;

    return 0;
}
```

---
### 第二步
#### 讲解
先来看一个欧拉函数的性质
对于一个正整数N的素数幂分解N = P1^q1 * P2^q2 * ... * Pn^qn。
φ(N) = N * (1 - 1/P1) * (1 - 1/P2) * ... * (1 - 1/Pn)
于是我们可以在O(sqrt(n))的时间内求出一个数的欧拉函数值
我们先初始化数组，在`main`函数里面写下
```c++
int res = n;
for (int i = 2; i * i <= n; ++i) {
    if (n % i == 0) {
        res = res / i * (i - 1); //先进行除法是为了防止中间数据溢出
        while (n % i == 0) {
            n /= i;
        }
    }
}
```

#### 提示
```c++
#include <iostream>
using namespace std;
int main() {
    int n;
    cin >> n;
    int res = n;
    for (int i = 2; i * i <= n; ++i) {
        if (n % i == 0) {
            res = res / i * (i - 1); //先进行除法是为了防止中间数据溢出
            while (n % i == 0) {
                n /= i;
            }
        }
    }

    return 0;
}
```

#### 代码
```c++
#include <iostream>
using namespace std;
int main() {
    int n;
    cin >> n;
    int res = n;
    for (int i = 2; i * i <= n; ++i) {
        if (n % i == 0) {
            res = res / i * (i - 1); //先进行除法是为了防止中间数据溢出
            while (n % i == 0) {
                n /= i;
            }
        }
    }

    return 0;
}
```

---
### 第三步
#### 讲解
可以发现上面的遍历是经过优化的，只遍历到sqrt(n),但是有可能还剩一个较大的质数因子。
所以还需加上一下的代码
```c++
if (n > 1) {
    res = res / n * (n - 1);
}
```

#### 提示
```c++
#include <iostream>
using namespace std;
int main() {
    int n;
    cin >> n;
    int res = n;
    for (int i = 2; i * i <= n; ++i) {
        if (n % i == 0) {
            res = res / i * (i - 1); //先进行除法是为了防止中间数据溢出
            while (n % i == 0) {
                n /= i;
            }
        }
    }
    if (n > 1) {
        res = res / n * (n - 1);
    }

    return 0;
}
```


#### 代码
```c++
#include <iostream>
using namespace std;
int main() {
    int n;
    cin >> n;
    int res = n;
    for (int i = 2; i * i <= n; ++i) {
        if (n % i == 0) {
            res = res / i * (i - 1); //先进行除法是为了防止中间数据溢出
            while (n % i == 0) {
                n /= i;
            }
        }
    }
    if (n > 1) {
        res = res / n * (n - 1);
    }

    return 0;
}
```

---
### 第四步
#### 讲解
最后输出我们计算出来的`n`的欧拉函数的值。在`main`里继续写下
```c++
cout << res << endl;
```

#### 提示
```c++
#include <iostream>
using namespace std;
int main() {
    int n;
    cin >> n;
    int res = n;
    for (int i = 2; i * i <= n; ++i) {
        if (n % i == 0) {
            res = res / i * (i - 1); //先进行除法是为了防止中间数据溢出
            while (n % i == 0) {
                n /= i;
            }
        }
    }
    if (n > 1) {
        res = res / n * (n - 1);
    }
    cout << res << endl;
    return 0;
}
```


#### 代码
```c++
#include <iostream>
using namespace std;
int main() {
    int n;
    cin >> n;
    int res = n;
    for (int i = 2; i * i <= n; ++i) {
        if (n % i == 0) {
            res = res / i * (i - 1); //先进行除法是为了防止中间数据溢出
            while (n % i == 0) {
                n /= i;
            }
        }
    }
    if (n > 1) {
        res = res / n * (n - 1);
    }
    cout << res << endl;
    return 0;
}
```



---
### 完成讲解
终于完成了，点击运行，输入下面的数据看看效果吧。
```
18
```
聪明的你一定学会了如何求一个数的欧拉函数值了吧。

***

## exgcd 的使用

- exgcd

### 文件名
exgcd.cpp

### 分数
1

---
### 初始化代码
```c++
#include <iostream>
using namespace std;

int main() {

    return 0;
}
```

---
### 第一步
#### 讲解
这一节我们学习`exgcd`(扩展GCD)算法。
首先来看一个结论：对于不完全为`0`的非负整数`a`，`b`，`gcd（a，b）`表示 `a`，`b` 的最大公约数，必然存在整
数对 `x`，`y` ，使得 `gcd（a，b）= ax + by`。
`exgcd`就是在已知`a`,`b`的情况下求解`x`和`y`。
先输入`a`和`b`两个整数。在`main`函数里面写下
```c++
int a, b, x, y;
cin >> a >> b;
```

#### 代码
```c++
#include <iostream>
using namespace std;
int main() {
    int a, b, x, y;
    cin >> a >> b;

    return 0;
}
```

---
### 第二步
#### 讲解
然后将`exgcd`的函数框架写好。在`main`函数上面写下
```c++
int exgcd(int a, int b, int &x, int &y) {

}
```

#### 提示
```c++
#include <iostream>
using namespace std;
int exgcd(int a, int b, int &x, int &y) {

}
int main() {
    int a, b, x, y;
    cin >> a >> b;

    return 0;
}
```

#### 代码
```c++
#include <iostream>
using namespace std;
int exgcd(int a, int b, int &x, int &y) {

}
int main() {
    int a, b, x, y;
    cin >> a >> b;

    return 0;
}
```

---
### 第三步
#### 讲解
接下来我们来简单整理一下思路。
1、当 b = 0，gcd（a，b）= a，此时 x = 1，y = 0。
2、对于方程ax + by = gcd(a, b)；我们设解为x1,  y1
我们令a = b, b = a % b;
得到方程bx + a % by = gcd(b， a % b);
由欧几里得算法可以得到gcd(a, b) = gcd(b, a % b);
代入可得：bx + a % b y = gcd(a, b)
设此方程解为x2, y2；
将模运算转换可得： a % b = a - (a / b) * b;
代入方程化解得：
ay2 + b(x2 - (a / b) y2) = gcd(a, b);
与ax1 + by1 = gcd(a, b) 联立，我们很容易得：
x1 = y2, y1 = x2 - (a / b)y2;
然后就可以递归求解。根据这个思路我们可以实现第一步代码如下
```c++
int exgcd(int a, int b, int &x, int &y) {
    if(b == 0) {
        x = 1;
        y = 0;
        return a;
    }
}
```

#### 提示
```c++
#include <iostream>
using namespace std;
int exgcd(int a, int b, int &x, int &y) {
    if(b == 0) {
        x = 1;
        y = 0;
        return a;
    }
}
int main() {
    int a, b, x, y;
    cin >> a >> b;

    return 0;
}
```


#### 代码
```c++
#include <iostream>
using namespace std;
int exgcd(int a, int b, int &x, int &y) {
    if(b == 0) {
        x = 1;
        y = 0;
        return a;
    }
}
int main() {
    int a, b, x, y;
    cin >> a >> b;

    return 0;
}
```

---
### 第四步
#### 讲解
接着实现第二步的代码。在`exgcd`里继续写下
```c++
int exgcd(int a, int b, int &x, int &y) {
    if(b == 0) {
        x = 1;
        y = 0;
        return a;
    }
    int r = exgcd(b, a % b, x, y);
    int t = x;
    x = y;
    y = t - a / b * y;
    return r;
}
```

#### 提示
```c++
#include <iostream>
using namespace std;
int exgcd(int a, int b, int &x, int &y) {
    if(b == 0) {
        x = 1;
        y = 0;
        return a;
    }
    int r = exgcd(b, a % b, x, y);
    int t = x;
    x = y;
    y = t - a / b * y;
    return r;
}
int main() {
    int a, b, x, y;
    cin >> a >> b;

    return 0;
}
```


#### 代码
```c++
#include <iostream>
using namespace std;
int exgcd(int a, int b, int &x, int &y) {
    if(b == 0) {
        x = 1;
        y = 0;
        return a;
    }
    int r = exgcd(b, a % b, x, y);
    int t = x;
    x = y;
    y = t - a / b * y;
    return r;
}
int main() {
    int a, b, x, y;
    cin >> a >> b;

    return 0;
}
```



---
### 第五步
#### 讲解
然后我们调用已经写好的`exgcd`函数。在`main`里继续写下
```c++
int d = exgcd(a, b, x, y);
```

#### 提示
```c++
#include <iostream>
using namespace std;
int exgcd(int a, int b, int &x, int &y) {
    if(b == 0) {
        x = 1;
        y = 0;
        return a;
    }
    int r = exgcd(b, a % b, x, y);
    int t = x;
    x = y;
    y = t - a / b * y;
    return r;
}
int main() {
    int a, b, x, y;
    cin >> a >> b;
    int d = exgcd(a, b, x, y);

    return 0;
}
```


#### 代码
```c++
#include <iostream>
using namespace std;
int exgcd(int a, int b, int &x, int &y) {
    if(b == 0) {
        x = 1;
        y = 0;
        return a;
    }
    int r = exgcd(b, a % b, x, y);
    int t = x;
    x = y;
    y = t - a / b * y;
    return r;
}
int main() {
    int a, b, x, y;
    cin >> a >> b;
    int d = exgcd(a, b, x, y);

    return 0;
}
```



---
### 第六步
#### 讲解
最后输出结果。在`main`里继续写下
```c++
cout << a << " * " << x << " + " << b << " * " << y << " = " << d << endl;
```

#### 提示
```c++
#include <iostream>
using namespace std;
int exgcd(int a, int b, int &x, int &y) {
    if(b == 0) {
        x = 1;
        y = 0;
        return a;
    }
    int r = exgcd(b, a % b, x, y);
    int t = x;
    x = y;
    y = t - a / b * y;
    return r;
}
int main() {
    int a, b, x, y;
    cin >> a >> b;
    int d = exgcd(a, b, x, y);
    cout << a << " * " << x << " + " << b << " * " << y << " = " << d << endl;
    return 0;
}
```


#### 代码
```c++
#include <iostream>
using namespace std;
int exgcd(int a, int b, int &x, int &y) {
    if(b == 0) {
        x = 1;
        y = 0;
        return a;
    }
    int r = exgcd(b, a % b, x, y);
    int t = x;
    x = y;
    y = t - a / b * y;
    return r;
}
int main() {
    int a, b, x, y;
    cin >> a >> b;
    int d = exgcd(a, b, x, y);
    cout << a << " * " << x << " + " << b << " * " << y << " = " << d << endl;
    return 0;
}
```

---
### 完成讲解
终于完成了，点击运行，输入下面的数据看看效果吧。
```
7 9
```
提示：当gcd(a, b) != 1时，我们很容易发现方程是无解的。

***


## binary_pow 的使用
- binary_pow

### 文件名
binary_pow.cpp

### 分数
1

---
### 初始化代码
```c++
#include <iostream>
using namespace std;

int main() {

    return 0;
}
```

---
### 第一步
#### 讲解
这一节我们学习`binary_pow`(二分快速幂)算法。
我们要求a ^ b % mod的值，可以发现当b非常大的时候，我们的需要将a乘以b次，O(b)的时间复杂度是让人难以接受的。
思考一下：
对于任意一个正整数b是否可以用1,2,4,8,16,32,....,2^n这样的数来组成。
比如7 = 4 + 2 + 1
19 = 16 + 2 + 1
聪明的你是否发现了其中的规律。
先输入`a`和`b`和`mod`三个整数。在`main`函数里面写下
```c++
int a, b, mod;
cin >> a >> b >> mod;
```

#### 代码
```c++
#include <iostream>
using namespace std;
int main() {
    int a, b, mod;
    cin >> a >> b >> mod;

    return 0;
}
```

---
### 第二步
#### 讲解
根据刚才我们发现的规律，任何一个`b`都可以由2^i次幂的数构成，而且这些数不会有重复的。
于是我们可以用二分的思想来快速将`b`的值分解。
我们先将`Pow_mod`函数框架写好。在`main`函数上面写下
```c++
int Pow_mod(int a, int b, int mod){

}
```

#### 提示
```c++
#include <iostream>
using namespace std;
int Pow_mod(int a, int b, int mod){

}
int main() {
    int a, b, mod;
    cin >> a >> b >> mod;

    return 0;
}
```

#### 代码
```c++
#include <iostream>
using namespace std;
int Pow_mod(int a, int b, int mod){

}
int main() {
    int a, b, mod;
    cin >> a >> b >> mod;

    return 0;
}
```

---
### 第三步
#### 讲解
上面我们可以二分的方法快速将`b`分解成2^i次幂组合成的数。
例如a^7 = a^4 * a^2 * a^1
a^19 = a^16 * a^2 * a^1
我们将`b`转化为二进制之后可以发现7是111,19是10011。
且a^2 = a^1 * a^1
a^4 = a^2 * a^2
于是我们要快速计算一个数a^b，只需要看b的二进制位是0还是1。
根据这个思路我们可以实现代码如下
```c++
int Pow_mod(int a, int b, int mod){
    int res = 1, temp = a;
    for (; b; b /= 2) {
        if (b & 1) {
            res = res * temp % mod;
        }
        temp = temp * temp % mod;
    }
    return res;
}
```

#### 提示
```c++
#include <iostream>
using namespace std;
int Pow_mod(int a, int b, int mod){
    int res = 1, temp = a;
    for (; b; b /= 2) {
        if (b & 1) {
            res = res * temp % mod;
        }
        temp = temp * temp % mod;
    }
    return res;
}
int main() {
    int a, b, mod;
    cin >> a >> b >> mod;

    return 0;
}
```


#### 代码
```c++
#include <iostream>
using namespace std;
int Pow_mod(int a, int b, int mod){
    int res = 1, temp = a;
    for (; b; b /= 2) {
        if (b & 1) {
            res = res * temp % mod;
        }
        temp = temp * temp % mod;
    }
    return res;
}
int main() {
    int a, b, mod;
    cin >> a >> b >> mod;

    return 0;
}
```

---
### 第四步
#### 讲解
最后输出我们所求的结果。在`main`里继续写下
```c++
cout << Pow_mod(a, b, mod) << endl;
```

#### 提示
```c++
#include <iostream>
using namespace std;
int Pow_mod(int a, int b, int mod){
    int res = 1, temp = a;
    for (; b; b /= 2) {
        if (b & 1) {
            res = res * temp % mod;
        }
        temp = temp * temp % mod;
    }
    return res;
}
int main() {
    int a, b, mod;
    cin >> a >> b >> mod;
    cout << Pow_mod(a, b, mod) << endl;
    return 0;
}
```


#### 代码
```c++
#include <iostream>
using namespace std;
int Pow_mod(int a, int b, int mod){
    int res = 1, temp = a;
    for (; b; b /= 2) {
        if (b & 1) {
            res = res * temp % mod;
        }
        temp = temp * temp % mod;
    }
    return res;
}
int main() {
    int a, b, mod;
    cin >> a >> b >> mod;
    cout << Pow_mod(a, b, mod) << endl;
    return 0;
}
```

---
### 完成讲解
终于完成了，点击运行，输入下面的数据看看效果吧。
```
100 100 999
```

***


## matrix_pow 的使用
- matrix_pow

### 文件名
matrix_pow.cpp

### 分数
1

---
### 初始化代码
```c++
#include <iostream>
using namespace std;

int main() {

    return 0;
}
```

---
### 第一步
#### 讲解
这一节我们学习`matrix_pow`(矩阵快速幂)算法。
提示：矩阵快速幂和上一节讲的二分快速幂思路是一样的。
我们要求矩阵A ^ y % mod的值，可以发现当y非常大的时候，我们的需要将A乘以y次，O(y)的时间复杂度是让人难以接受的。
思考一下：
对于任意一个正整数y是否可以用1,2,4,8,16,32,....,2^n这样的数来组成。
比如7 = 4 + 2 + 1
19 = 16 + 2 + 1
聪明的你是否发现了其中的规律。
先用结构体类型来定义矩阵`A`。在`main`函数上面写下
```c++
int n;
struct matrix {
   int a[100][100];
};
```

#### 代码
```c++
#include <iostream>
using namespace std;
int n;
struct matrix {
   int a[100][100];
};
int main() {

    return 0;
}
```

---
### 第二步
#### 讲解
根据刚才我们发现的规律，任何一个`y`都可以由2^i次幂的数构成，而且这些数不会有重复的。
于是我们可以用二分的思想来快速将`b`的值分解。
我们先输入矩阵`A`的值。在`main`函数里面写下
```c++
matrix A;
n = 2;
for (int i = 0; i < n; ++i) {
    for (int j = 0; j < n; ++j) {
        cin >> A.a[i][j];
    }
}
```

#### 提示
```c++
#include <iostream>
using namespace std;
int n;
struct matrix {
   int a[100][100];
};
int main() {
    matrix A;
    n = 2;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            cin >> A.a[i][j];
        }
    }

    return 0;
}
```

#### 代码
```c++
#include <iostream>
using namespace std;
int n;
struct matrix {
   int a[100][100];
};
int main() {
    matrix A;
    n = 2;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            cin >> A.a[i][j];
        }
    }

    return 0;
}
```

---
### 第三步
#### 讲解
接下来输入`y`和`mod`。在`main`函数里继续写下
```c++
int y, mod;
cin >> y >> mod;
```

#### 提示
```c++
#include <iostream>
using namespace std;
int n;
struct matrix {
   int a[100][100];
};
int main() {
    matrix A;
    n = 2;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            cin >> A.a[i][j];
        }
    }
    int y, mod;
    cin >> y >> mod;

    return 0;
}
```


#### 代码
```c++
#include <iostream>
using namespace std;
int n;
struct matrix {
   int a[100][100];
};
int main() {
    matrix A;
    n = 2;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            cin >> A.a[i][j];
        }
    }
    int y, mod;
    cin >> y >> mod;

    return 0;
}
```

---
### 第四步
#### 讲解
接下来初始化一个单位矩阵`E`，单位矩阵主对角线上是1，其他位置是0。在`main`函数上方写下
```c++
matrix unit() {
    matrix res;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            if (i == j) {
                res.a[i][j] = 1;
            } else {
                res.a[i][j] = 0;
            }
        }
    }
    return res;
}
```

#### 提示
```c++
#include <iostream>
using namespace std;
int n;
struct matrix {
   int a[100][100];
};
matrix unit() {
    matrix res;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            if (i == j) {
                res.a[i][j] = 1;
            } else {
                res.a[i][j] = 0;
            }
        }
    }
    return res;
}
int main() {
    matrix A;
    n = 2;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            cin >> A.a[i][j];
        }
    }
    int y, mod;
    cin >> y >> mod;

    return 0;
}
```


#### 代码
```c++
#include <iostream>
using namespace std;
int n;
struct matrix {
   int a[100][100];
};
matrix unit() {
    matrix res;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            if (i == j) {
                res.a[i][j] = 1;
            } else {
                res.a[i][j] = 0;
            }
        }
    }
    return res;
}
int main() {
    matrix A;
    n = 2;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            cin >> A.a[i][j];
        }
    }
    int y, mod;
    cin >> y >> mod;

    return 0;
}
```

---
### 第五步
#### 讲解
然后实现矩阵乘法`matrix_mul`函数。在`unit`函数上方写下
```c++
matrix matrix_mul(matrix A, matrix B, int mod) {
    matrix C;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            C.a[i][j] = 0;
            for (int k = 0; k < n; ++k) {
                C.a[i][j] += A.a[i][k] * B.a[k][j] % mod;
                C.a[i][j] %= mod;
            }
        }
    }
    return C;
}
```

#### 提示
```c++
#include <iostream>
using namespace std;
int n;
struct matrix {
   int a[100][100];
};
matrix matrix_mul(matrix A, matrix B, int mod) {
    matrix C;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            C.a[i][j] = 0;
            for (int k = 0; k < n; ++k) {
                C.a[i][j] += A.a[i][k] * B.a[k][j] % mod;
                C.a[i][j] %= mod;
            }
        }
    }
    return C;
}
matrix unit() {
    matrix res;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            if (i == j) {
                res.a[i][j] = 1;
            } else {
                res.a[i][j] = 0;
            }
        }
    }
    return res;
}
int main() {
    matrix A;
    n = 2;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            cin >> A.a[i][j];
        }
    }
    int y, mod;
    cin >> y >> mod;

    return 0;
}
```


#### 代码
```c++
#include <iostream>
using namespace std;
int n;
struct matrix {
   int a[100][100];
};
matrix matrix_mul(matrix A, matrix B, int mod) {
    matrix C;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            C.a[i][j] = 0;
            for (int k = 0; k < n; ++k) {
                C.a[i][j] += A.a[i][k] * B.a[k][j] % mod;
                C.a[i][j] %= mod;
            }
        }
    }
    return C;
}
matrix unit() {
    matrix res;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            if (i == j) {
                res.a[i][j] = 1;
            } else {
                res.a[i][j] = 0;
            }
        }
    }
    return res;
}
int main() {
    matrix A;
    n = 2;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            cin >> A.a[i][j];
        }
    }
    int y, mod;
    cin >> y >> mod;

    return 0;
}
```

---
### 第六步
#### 讲解
上面我们可以二分的方法快速将`y`分解成2^i次幂组合成的数。
例如A^7 = A^4 * A^2 * A^1
A^19 = A^16 * A^2 * A^1
我们将`y`转化为二进制之后可以发现7是111,19是10011。
且A^2 = A^1 * A^1
A^4 = A^2 * A^2
于是我们要快速计算一个A^y，只需要看y的二进制位是0还是1。
根据这个思路我们可以实现代码如下。
在`main`函数上方写下
```c++
matrix matrix_pow(matrix A, int y, int mod) {
    matrix res = unit(), temp = A;
    for (; y; y /= 2) {
        if (y & 1) {
            res = matrix_mul(res, temp, mod);
        }
        temp = matrix_mul(temp, temp, mod);
    }
    return res;
}
```

#### 提示
```c++
#include <iostream>
using namespace std;
int n;
struct matrix {
   int a[100][100];
};
matrix matrix_mul(matrix A, matrix B, int mod) {
    matrix C;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            C.a[i][j] = 0;
            for (int k = 0; k < n; ++k) {
                C.a[i][j] += A.a[i][k] * B.a[k][j] % mod;
                C.a[i][j] %= mod;
            }
        }
    }
    return C;
}
matrix unit() {
    matrix res;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            if (i == j) {
                res.a[i][j] = 1;
            } else {
                res.a[i][j] = 0;
            }
        }
    }
    return res;
}
matrix matrix_pow(matrix A, int y, int mod) {
    matrix res = unit(), temp = A;
    for (; y; y /= 2) {
        if (y & 1) {
            res = matrix_mul(res, temp, mod);
        }
        temp = matrix_mul(temp, temp, mod);
    }
    return res;
}
int main() {
    matrix A;
    n = 2;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            cin >> A.a[i][j];
        }
    }
    int y, mod;
    cin >> y >> mod;

    return 0;
}
```


#### 代码
```c++
#include <iostream>
using namespace std;
int n;
struct matrix {
   int a[100][100];
};
matrix matrix_mul(matrix A, matrix B, int mod) {
    matrix C;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            C.a[i][j] = 0;
            for (int k = 0; k < n; ++k) {
                C.a[i][j] += A.a[i][k] * B.a[k][j] % mod;
                C.a[i][j] %= mod;
            }
        }
    }
    return C;
}
matrix unit() {
    matrix res;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            if (i == j) {
                res.a[i][j] = 1;
            } else {
                res.a[i][j] = 0;
            }
        }
    }
    return res;
}
matrix matrix_pow(matrix A, int y, int mod) {
    matrix res = unit(), temp = A;
    for (; y; y /= 2) {
        if (y & 1) {
            res = matrix_mul(res, temp, mod);
        }
        temp = matrix_mul(temp, temp, mod);
    }
    return res;
}
int main() {
    matrix A;
    n = 2;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            cin >> A.a[i][j];
        }
    }
    int y, mod;
    cin >> y >> mod;

    return 0;
}
```

---
### 第七步
#### 讲解
调用`matrix_pow`函数，最后输出我们所求的矩阵A的结果。在`main`里继续写下
```c++
matrix res = matrix_pow(A, y, mod);
for (int i = 0; i < n; ++i) {
    for (int j = 0; j < n; ++j) {
        cout << res.a[i][j] << " ";
    }
    cout << endl;
}
```

#### 提示
```c++
#include <iostream>
using namespace std;
int n;
struct matrix {
   int a[100][100];
};
matrix matrix_mul(matrix A, matrix B, int mod) {
    matrix C;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            C.a[i][j] = 0;
            for (int k = 0; k < n; ++k) {
                C.a[i][j] += A.a[i][k] * B.a[k][j] % mod;
                C.a[i][j] %= mod;
            }
        }
    }
    return C;
}
matrix unit() {
    matrix res;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            if (i == j) {
                res.a[i][j] = 1;
            } else {
                res.a[i][j] = 0;
            }
        }
    }
    return res;
}
matrix matrix_pow(matrix A, int y, int mod) {
    matrix res = unit(), temp = A;
    for (; y; y /= 2) {
        if (y & 1) {
            res = matrix_mul(res, temp, mod);
        }
        temp = matrix_mul(temp, temp, mod);
    }
    return res;
}

int main() {
    matrix A;
    n = 2;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            cin >> A.a[i][j];
        }
    }
    int y, mod;
    cin >> y >> mod;
    matrix res = matrix_pow(A, y, mod);
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            cout << res.a[i][j] << " ";
        }
        cout << endl;
    }
    return 0;
}
```


#### 代码
```c++
#include <iostream>
using namespace std;
int n;
struct matrix {
   int a[100][100];
};
matrix matrix_mul(matrix A, matrix B, int mod) {
    matrix C;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            C.a[i][j] = 0;
            for (int k = 0; k < n; ++k) {
                C.a[i][j] += A.a[i][k] * B.a[k][j] % mod;
                C.a[i][j] %= mod;
            }
        }
    }
    return C;
}
matrix unit() {
    matrix res;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            if (i == j) {
                res.a[i][j] = 1;
            } else {
                res.a[i][j] = 0;
            }
        }
    }
    return res;
}
matrix matrix_pow(matrix A, int y, int mod) {
    matrix res = unit(), temp = A;
    for (; y; y /= 2) {
        if (y & 1) {
            res = matrix_mul(res, temp, mod);
        }
        temp = matrix_mul(temp, temp, mod);
    }
    return res;
}

int main() {
    matrix A;
    n = 2;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            cin >> A.a[i][j];
        }
    }
    int y, mod;
    cin >> y >> mod;
    matrix res = matrix_pow(A, y, mod);
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            cout << res.a[i][j] << " ";
        }
        cout << endl;
    }
    return 0;
}
```

---
### 完成讲解
终于完成了，点击运行，输入下面的数据看看效果吧。
```
1 2
3 4
100 999
```

***


