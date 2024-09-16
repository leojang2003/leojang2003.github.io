---
layout: post
title: Dependency Injection
subtitle: 
tags: []
comments: true
---

### 主控台程式使用 DI

```csharp
// .NET 8
// Program.cs
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using NmipErrorMoniter;
using System.Security.Authentication.ExtendedProtection;

// 建立依賴注入的容器
var serviceCollection = new ServiceCollection();
var builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
var _config = builder.Build();

// 註冊服務
serviceCollection.AddTransient<NmipMonitorService>();
serviceCollection.AddSingleton<IConfiguration>(_config);

// 建立依賴服務提供者
var serviceProvider = serviceCollection.BuildServiceProvider();

// 執行主服務
serviceProvider.GetRequiredService<NmipMonitorService>().CheckErrors();

```

NmipMonitorService 類別如下

```csharp
internal class NmipMonitorService
{
	private readonly IConfiguration _configuration;

	public NmipMonitorService(IConfiguration configuration)
	{
		_configuration = configuration;            
	}
	
	public void CheckErrors()
	{
	    // 讀取 appsettings.json 的設定
		var files = Directory.GetFiles(_configuration["error"]!);      
	}
}
```

### Worker Service 使用 DI

```csharp
// .NET 8
// Program.cs
var config = new ConfigurationBuilder().AddJsonFile("appsettings.json").Build();

var builder = Host.CreateApplicationBuilder();
builder.Services.AddHostedService<Worker>();
builder.Services.Configure<IConfiguration>(config);

var host = builder.Build();
host.Run();

```  

Worker 類別如下

```csharp

public class Worker : BackgroundService
{
	private readonly ILogger<Worker> _logger;
	private readonly IConfiguration _configuration;

	public Worker(ILogger<Worker> logger, IConfiguration configuration)
	{
		_logger = logger;
		_configuration = configuration;
	}

	protected override async Task ExecuteAsync(CancellationToken stoppingToken)
	{
		while (!stoppingToken.IsCancellationRequested)
		{
			if (_logger.IsEnabled(LogLevel.Information))
			{
				_logger.LogInformation("Worker running at: {time}", DateTimeOffset.Now);
				Process();
				ReUploadFiles();
			}
			await Task.Delay(300000, stoppingToken);
		}
	}
	
	private void Process()
	{
		// do something
	}	
}

```

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
