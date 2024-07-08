---
layout: post
title: Day 3
subtitle: ASP.Net Core MVC
tags: []
comments: true
---

請先閱讀 <a href="../2024-04-17-descriptor/">描述器 descriptor</a>

使用 Python 的 property ( )，我們可以在類別建立 managed attribute，managed attribute 也就是所謂的 property，用於在不破壞 public API 下，可以變更內部實作方式。

舉例來說，使用者讀取 A.p 這裡的 p 就是 public API 給外界讀取，我們可以變更 property p 的實作方式，而使用者呼 p 的方式保持不變。

<br/>

### 服務的生命週期

```csharp
// program.cs
builder.Services.AddTransient<IMyService, MyService>();
builder.Services.AddScoped<IMyService, MyService>();
builder.Services.AddSingleton<IMyService, MyService>();
```

AddTransient 每次執行都會生成，同個 request 在不同地方呼叫‧舉例來說，同時有在 controller 或 view 的 DI 生成時，會建立不同的物件
AddScoped 一個 request 只會生成一次，一個網頁(request)會生成一次，同時有在 controller 或 view 的 DI 生成時，會使用不同的物件
AddSingleton 綁定 exe，關閉程式時才會結束

AddSingleton 的特別之處在於可以在 program.cs 選擇要注入的服務，兩種寫法如下

```csharp
// program.cs
MyService myService = new MyService();
builder.Services.AddSingleton<IMyService>(myService);
```
第二種寫法

```cshart
// program.cs
IMyService myService = new MyService();
builder.Services.AddSingleton(myService);
```  

### IDisposable

如果是自己生成的服務物件，則不會自己 Dispose，需要使用 using

```csharp
using MyService myService = new MyService();
builder.Services.AddSingleton<IMyService>(myService);
```

### ILDASM

Tools > Command Line > Developer Command Prompt > 輸入指令 ildasm > 
將 StarterM.dll 拖入 > MANIFEST > 拉到最後可以看到 EmbeddedFile 的 aaa.txt 與 bbb.txt 的內容


{:.note}

<br>

###

MVC 的 controller 回傳值只會有 Helper，WEB API 較為複雜，有三種不同的可能回傳值

### 測試 WEB API

Visual Studio 2022 17.8 以上有提供 Views > Other Windows > Endpoint Exporer
第一行跟第二行中間不能有空白，第二跟三中間要有空白
這裡的 x-www-form-urlencoded 對應到 Controller 方法的是 [FromForm]，如果是 json，則 Controller 方法對應的是 [FromBody]

```json
POST {{StarterM_HostAddress}}/api/customers
Content-Type:application/x-www-form-urlencoded

CustomerID=windhaven&CompanyName=nvda&Country=USA
```

```json
POST {{StarterM_HostAddress}}/api/customers
Content-Type:application/json

{
  "CustomerID":"leo",
  "CompanyName":"tsb",
  "Country":"Taiwan"
}
```

###Swagger

一定要加描述 [HttpGet]

###

```javascript
@model StarterM.Models.Customer
@{
    Layout = null;
}

<!DOCTYPE html>

<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>Home</title>
</head>
<body>
    <h1>Home</h1>
    <form id="form1">
        @Html.EditorForModel()
    </form>
    <hr />
    <div>
        <input type="button" id="btnAll" value="List" />
        <input type="button" id="btnGet" value="Get" />
        <input type="button" id="btnInsert" value="Insert" />
        <input type="button" id="btnUpdate" value="Update" />
        <input type="button" id="btnDelete" value="Delete" />
    </div>
    <ul id="list"></ul>
    <script src="~/lib/jquery/jquery.js"></script>
    <script>
        $(() => {
            async function getAll() {
                $("#list").empty();
                let respose = await fetch("@Url.Content("~/api/customers")");
                let result = await respose.json();
                console.log(result);//js array
                result.forEach(item => {
                    $("#list").append(`<li>${item.customerID} - ${item.companyName} - ${item.companyName}</li>`);
                });
            }

            $("#btnAll").on("click", getAll);

        });
    </script>
</body>
</html>
```

javascript 反引號(backtick) 允許ES6模板字串的基本語法是使用反引號（``）將字串包圍起來，在字串中可以使用${}來插入變數或表達式。


javascript 中需用 Url.Content 來處理 ~

因為只有一行，await respose.json(); 省略了 return


### Get

```javascript
$("#btnGet").on("click", () => {
    fetch(`@Url.Content("~/api/customers")/${$("#CustomerID").val()}`)
        .then(response => {
            if (response.ok)
                return response.json();
            else
                throw response.statusText
        })
        .then(item => {
            console.log(item);
            $("#CompanyName").val(item.companyName);
            $("#Country").val(item.country);
        })
        .catch(alert);
});
```

### Insert

```javascript
$("#btnInsert").on("click", () => {
    fetch("@Url.Content("~/api/customers")", {
    method: "POST",
    headers: {
            "Content-Type": "application/x-www-form-urlencoded"
    },
    body:$("#form1").serialize()
    })
        .then(getAll);
});
```

fetch 後面是類似 C# 的選項參數概念，另外 $("#form1").serialize() 會組成 CustomerID=aaa&CompanyName=bbb&Country=ccc

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
