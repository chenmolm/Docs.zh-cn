---
title: ASP.NET 核心中基于角色的授权
author: rick-anderson
description: 了解如何通过将角色传递给 Authorize 属性限制 ASP.NET Core 控制器和操作访问。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: 0d39a457782061a57779bacb0d3a255be352bd2d
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276427"
---
# <a name="role-based-authorization-in-aspnet-core"></a>ASP.NET 核心中基于角色的授权

<a name="security-authorization-role-based"></a>

当创建标识它可能属于一个或多个角色。 例如，Tracy 可能属于管理员和用户角色，同时 Scott 可能仅属于用户角色。 如何创建和管理这些角色取决于授权过程的后备存储。 角色公开给开发人员通过[IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole)方法[ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal)类。

## <a name="adding-role-checks"></a>添加角色检查

基于角色的授权检查均为声明性&mdash;开发人员将它们嵌入在其代码中，针对控制器或者控制器中的某个操作指定当前用户必须是的成员来访问请求的资源的角色。

例如，下面的代码可访问任何操作限制上`AdministrationController`谁是其成员的用户到`Administrator`角色：

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

以逗号分隔的列表，可以指定多个角色：

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

此控制器将作为仅可访问的成员的用户的`HRManager`角色或`Finance`角色。

如果您将应用多个属性，则访问用户必须是指定; 的所有角色的成员下面的示例要求用户必须是的成员`PowerUser`和`ControlPanelUser`角色。

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

通过应用在操作级别的其他角色授权属性，可以进一步限制访问：

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

中的上一个代码段成员`Administrator`角色或`PowerUser`角色可以访问控制器和`SetTime`操作，但只有的成员`Administrator`角色可以访问`ShutDown`操作。

您可以锁定控制器，但允许匿名、 未经身份验证访问各项操作。

```csharp
[Authorize]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [AllowAnonymous]
    public ActionResult Login()
    {
    }
}
```

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a>基于策略角色检查

此外可以使用新的策略语法中，开发人员将在启动策略注册为授权服务配置的一部分的其中表示角色的要求。 这通常发生在`ConfigureServices()`中你*Startup.cs*文件。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole", policy => policy.RequireRole("Administrator"));
    });
}
```

策略使用应用`Policy`属性`AuthorizeAttribute`属性：

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

如果你想要指定多个允许的角色中一项要求，则可作为参数来指定这些`RequireRole`方法：

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

此示例授权属于用户`Administrator`，`PowerUser`或`BackupAdministrator`角色。
