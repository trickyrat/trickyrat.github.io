---
title: LeetCode-468 Valid IP Address
date: 2020-08-11 00:25:14
categories: [Algorithms, LeetCode]
tags: Algorithms
banner_img: /img/bg/LeetCode-468/sunset.jpg
---

## 验证 IP 地址（Valid IP Address）

### 题目描述

编写一个函数来验证输入的字符串是否是有效的 IPv4 或  IPv6 地址。

IPv4  地址由十进制数和点来表示，每个地址包含 4 个十进制数，其范围为  0 - 255，  用`(".")`分割。比如，`172.16.254.1`；

同时，IPv4 地址内的数不会以 `0` 开头。比如，地址  `172.16.254.01` 是不合法的。

IPv6  地址由 8 组 16 进制的数字来表示，每组表示  16 比特。这些组数字通过 `(":")`分割。  
比如,  `2001:0db8:85a3:0000:0000:8a2e:0370:7334` 是一个有效的地址。而且，我们可以加入一些以 0 开头的数字，字母可以使用大写，也可以是小写。所以， `2001:db8:85a3:0:0:8A2E:0370:7334 `也是一个有效的 IPv6 address 地址 (即，忽略 0 开头，忽略大小写)。

然而，我们不能因为某个组的值为 `0`，而使用一个空的组，以至于出现 `(::)` 的情况。  比如， `2001:0db8:85a3::8A2E:0370:7334` 是无效的 IPv6 地址。

同时，在 IPv6 地址中，多余的 `0` 也是不被允许的。比如， `02001:0db8:85a3:0000:0000:8a2e:0370:7334` 是无效的。

### 示例

#### 示例 1

> 输入: "172.16.254.1"  
> 输出: "IPv4"  
> 解释: 这是一个有效的 IPv4 地址, 所以返回 "IPv4"。

#### 示例 2

> 输入: "2001:0db8:85a3:0:0:8A2E:0370:7334"  
> 输出: "IPv6"  
> 解释: 这是一个有效的 IPv6 地址, 所以返回 "IPv6"。

#### 示例 3

> 输入: "256.256.256.256"  
> 输出: "Neither"  
> 解释: 这个地址既不是 IPv4 也不是 IPv6 地址。

### 分析过程：

看到题目，首先想到就是正则表达式，无论是 IPv4 还是 IPv6 其实都是分组后每一组的规则是相同的按照相应的规则进行匹配。  
对于 IPv4 来说，其结构为`A.B.C.D`，由三个`.`和四个`0-255`数字组成，因此存在五种情景

| 情景  |        说明        |  范围   |
| :---: | :----------------: | :-----: |
|   0   | 一位，首位可以为 0 |   0-9   |
|  10   | 两位，首位不能为 0 |  10-99  |
|  100  | 三位，首位不能为 0 | 100-199 |
|  200  |    三位 2 开头     | 200-249 |
|  250  |    三位 25 开头    | 250-255 |

这样一来正则表达式就很简单了，对于前三组为`([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5].){3}`，第四组就少一个`.`即可。

同理，对于 IPv6 来说，结构为`A:B:C:D:E:F:G:H`，由七组`:`和八组 16 进制数组成，也没有限制首位不能为 0，所以 IPv6 的正则表达式为`(([0-9a-fA-F]{1,4}):)`，第八组少一个`:`。

C# 代码

```csharp
public class Solution
{
    public string ValidIPAddress(string IP)
    {
        string ipv4_chunk = "([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])";
        Regex ipv4Pattern = new Regex("^(" + ipv4_chunk + ".){3}" + ipv4_chunk + "$");
        string ipv6_chunk = "([0-9a-fA-F]{1,4})";
        Regex ipv6Pattern = new Regex(@"^(" + ipv6_chunk + ":){7}" + ipv6_chunk + "$");
        if(IP.Contains('.'))
        {
            return ipv4Pattern.IsMatch(IP) ? "IPv4" : "Neither";
        }
        if(IP.Contains(':'))
        {
            return ipv6Pattern.IsMatch(IP) ? "IPv6" : "Neither";
        }
        return "Neither";
    }
}
```

试一下，能跑，但是耗时 140ms,打败 1%的用户，emmmmmm....

换一个思路，其实验证每个组的逻辑是相同，我们完全可以不使用正则表达式去匹配，而是每一组去验证即可。

C# 代码

```csharp
public class Solution
{
    public string ValidIPAddress(string IP)
    {

        if (IP.Count(c => c == '.') == 3)
        {
            return ValidIPv4(IP);
        }
        else if(IP.Count(c => c == ':') == 7)
        {
            return ValidIPv6(IP);
        }
        else
        {
            return "Neither";
        }
    }

    private string ValidIPv4(string IP)
    {
        string[] chunks = IP.Split('.');
        foreach (var chunk in chunks)
        {
            if(chunk.Length == 0 || chunk.Length > 3)
            {
                return "Neither";
            }
            if (chunk[0] == '0' && chunk.Length != 1)
            {
                return "Neither";
            }
            foreach (var c in chunk)
            {
                if(!char.IsNumber(c))
                {
                    return "Neither";
                }
            }
            if(System.Convert.ToInt32(chunk) > 255)
            {
                return "Neither";
            }
        }
        return "IPv4";
    }
    private string ValidIPv6(string IP)
    {
        string[] chunks = IP.Split(':');
        string hexDigits = "0123456789abcdefABCDEF";
        foreach (var chunk in chunks)
        {
            if (chunk.Length == 0 || chunk.Length > 4)
            {
                return "Neither";
            }
            foreach (var c in chunk)
            {
                if(hexDigits.IndexOf(c) == -1)
                {
                    return "Neither";
                }
            }

        }
        return "IPv6";
    }
}
```

嗯，104ms，提升不少。

Python 代码

```python
    def is_valid_ip_address(self, IP: str) -> str:
        def valid_ipv4(IP: str) -> str:
            chunks = IP.split('.')
            for chunk in chunks:
                length = len(chunk)
                if length == 0 or length > 3:
                    return "Neither"
                if chunk[0] == '0' and length != 1:
                    return "Neither"
                if not chunk.isnumeric():
                    return "Neither"
                if int(chunk) > 255:
                    return "Neither"
            return "IPv4"

        def valid_ipv6(IP: str) -> str:
            chunks = IP.split(':')
            hex_digits = "0123456789abcdefABCDEF"
            for chunk in chunks:
                length = len(chunk)
                if length == 0 or length > 4:
                    return "Neither"
                for c in chunk:
                    if hex_digits.find(c) == -1:
                        return "Neither"
            return "IPv6"

        if IP.count(".") == 3:
            return valid_ipv4(IP)
        elif IP.count(":") == 7:
            return valid_ipv6(IP)
        else:
            return "Neither"
```
