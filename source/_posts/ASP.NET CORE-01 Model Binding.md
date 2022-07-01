---
title: ASP.NET CORE-01 Model Binding
date: 2020-04-06 16:01:07
categories: [Web, ASP.NET CORE]
tags: Web
---

## 模型绑定到底用来做什么的

简而言之，就是消费者和服务提供者沟通的中间载体。以`Web Api`为例，就是当 Api 消费者发送 http 请求数据时，与 Controller 交互时的一种数据载体，`Asp.Net Core`中是将请求的数据转换承一个键值对的数据结构存起来。根据键名称去和`Asp.Net Core`中的 Model 进行匹配并赋值。

## ASP.NET CORE 中数据的流向

读取数据：数据库 => ORM 框架 => Controller => Pages/Api  
写入数据：Api/Pages => Controller => ORM 框架 =>数据库

## 页面和 Controller 怎么进行数据交互

接口消费者（可能是页面或者 web api）通过 http 请求（http request）将请求的参数传递给`Asp.Net Core`框架，然后框架通过请求管道将请求参数处理为相应的模型，然后根据处理好的参数模型以及路由，决定访问哪一个 Controller 数据库数据与接口消费者需求的数据可能存在一些敏感数据（如，密码等）我们一般会在模型中再加一层 ViewModel 或者 Dto。这样就可以方便地控制返回给接口消费者的数据。

## 控制器请求参数来源

| 来源 Attribute |           来源说明           |                          示例                          |                                  说明                                  |
| :------------: | :--------------------------: | :----------------------------------------------------: | :--------------------------------------------------------------------: |
| `[FromQuery]`  |     从查询字符串获取值。     | `https://test.com/employees?name='trickyrat'&age='12'` | 框架会访问 employees 接口，并且传递 name 和 age 到后台模型的两个属性上 |
| `[FromRoute]`  |     从路由数据中获取值。     |       `https://test.com/employees/{employeesid}`       |                          获取路由中提供的数据                          |
|  `[FromForm]`  | 从已发布的表单字段中获取值。 |              `https://test.com/employees`              |                         获取提交的表单中的数据                         |
|  `[FromBody]`  |     从请求正文中获取值。     |              `https://test.com/employees`              |                    获取 http 请求 body 中提供的数据                    |
| `[FromHeader]` |    从 HTTP 标头中获取值。    |              `https://test.com/employees`              |                   获取 http 请求的 header 里面的数据                   |

```csharp
public IActionResult Hello([FromQuery]HelloRequest request)
{
    ...
}

public IActionResult Hello([FromRoute]HelloRequest request)
{
    ...
}

public IActionResult Hello([FromForm]HelloRequest request)
{
    ...
}

public IActionResult Hello([FromBody]HelloRequest request)
{
    ...
}

public IActionResult Hello([FromHeader]HelloRequest request)
{
    ...
}
```

这些 Attribute 也可以用于修饰 Model 中的属性，意思也很明了就是说这个属性只能通过特定传参方式传递数据。

```csharp
class Person
{
    [FromQuery]
    public int Age { get; set;}
}

class PersonController
{
    public IActionResult GetPerson([FromBody]Request request)
    {
        ...
    }
}
```

此示例中的 Age 会忽略参数中`[FromBody]`Attribute，Age 只能通过 Query 字符串中获取数据。

`Asp.Net Core`也提供自定义数据来源，只需要实现`IValueProvider`和`IValueProviderFactory`两个接口的类，并且在 startup 中通过依赖注入注入便可以实现。  
控制器中求情参数可以是简单的内置类型，也可以是自定义的类型。一般简单的接口，只需要几个简单的参数就可以满足，当需求复杂后，便可以自定义一个类型来作为请求参数的载体。
