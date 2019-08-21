# Sliding Window  

滑动窗口是**高频题**，主要是利用**双指针**移动控制窗口的位置和大小变化。  

该题型有固定解题**模板**，参考[面试官，你再问我滑动窗口问题试试？我有解题模板，不怕！](https://mp.weixin.qq.com/s?__biz=MzUyNjQxNjYyMg==&mid=2247485620&idx=1&sn=ca4cd61e99b76f98f49c7ba356a40f6f&chksm=fa0e6735cd79ee2333f789305193d7973cb173501fa38c7c0c5f956982b27c48999acff519dc&mpshare=1&scene=1&srcid=&key=64b5b2c11d4b7b9a10d91f13211d11bbc2f872a49b1874a00f2dea011f6f66acf9d82763f45ddea4b631446b7ac532758c211808578e763cd176b8d52d090e2e3ce839c2ed6be123e31dba47004448d0&ascene=1&uin=MTE4MDMwOTg0NA%3D%3D&devicetype=Windows+10&version=62060833&lang=zh_CN&pass_ticket=e4pWvEK1f6MftuL6kn%2BFqTlokdUJu2salxw7qgIXDUe8fIaSeec59ZOefrBaea%2Bn)

滑动窗口这类问题一般需要用到**双指针**来进行求解，另外一类比较特殊则是需要用到特定的数据结构，像是 sorted_map。

后者有特定的题型，后面会列出来，但是，对于前者，题形变化非常的大，一般都是基于字符串和数组的，所以我们重点总结这种基于双指针的滑动窗口问题。

题目问法大致有这几种：

- 给两个字符串，一长一短，问其中短的是否在长的中满足一定的条件存在，例如：

- 求长的的最短子串，该子串必须涵盖短的的所有字符

- 短的的 anagram 在长的中出现的所有位置

- …

- 给一个字符串或者数组，问这个字符串的子串或者子数组是否满足一定的条件，例如：

- 含有少于 k 个不同字符的最长子串

- 所有字符都只出现一次的最长子串

- …

除此之外，还有一些其他的问法，但是不变的是，这类题目脱离不开主串（主数组）和子串（子数组）的关系，要求的时间复杂度往往是**O(n)**，空间复杂度往往是**常数级**的。

之所以是滑动窗口，是因为，遍历的时候，两个指针一前一后夹着的子串（子数组）类似一个窗口，这个窗口大小和范围会随着前后指针的移动发生变化。

![双指针确定一个窗口](https://mmbiz.qpic.cn/mmbiz_png/D67peceibeIQQFkNpGt1WeJBpCpdPRDTMd8HvYApLiaMDUV8fUjVpibIGichzx8bqbD9Q8NhPgARiaebECib8Tqgo3VA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 解题思路与模板

根据前面的描述，滑动窗口就是这类题目的重点，换句话说**窗口的移动**就是重点！

我们要控制前后指针的移动来控制窗口，这样的移动是有条件的，也就是要想清楚在什么情况下移动，在什么情况下保持不变。

我的思路是**保证右指针每次往前移动一格**，每次移动都会有新的一个元素进入窗口，这时条件可能就会发生改变，然后根据当前条件来**决定左指针是否移动**，以及移动多少格。

参考模板：
```java
public int slidingWindowTemplate(String[] a, ...) {
    // 输入参数有效性判断
    if (...) {
        ...
    }

    // 申请一个散列，用于记录窗口中具体元素的个数情况
    // 这里用数组的形式呈现，也可以考虑其他数据结构
    int[] hash = new int[...];

    // 预处理(可省), 一般情况是改变 hash
    ...

    // l 表示左指针
    // count 记录当前的条件，具体根据题目要求来定义
    // result 用来存放结果
    int l = 0, count = ..., result = ...;
    for (int r = 0; r < A.length; ++r) {
        // 更新新元素在散列中的数量
        hash[A[r]]--;

        // 根据窗口的变更结果来改变条件值
        if (hash[A[r]] == ...) {
            count++;
        }

        // 如果当前条件不满足，移动左指针直至条件满足为止
        while (count > K || ...) {
            ...
            if (...) {
                count--;
            }
            hash[A[l]]++;
            l++;
        }

        // 更新结果
        results = ...
    }

    return results;
}
```
这里面的 “移动左指针直至条件满足” 部分，需要具体题目具体分析，其他部分的变化不大。
