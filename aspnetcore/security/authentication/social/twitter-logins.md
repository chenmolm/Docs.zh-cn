---
title: Twitter 外部登录名与 ASP.NET 核心的安装程序
author: rick-anderson
description: 本教程演示的集成到现有的 ASP.NET Core 应用的 Twitter 帐户用户身份验证。
ms.author: riande
ms.date: 11/01/2016
uid: security/authentication/twitter-logins
ms.openlocfilehash: 81c19105e4cda932db3302a5df343322fb85abef
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278487"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a>Twitter 外部登录名与 ASP.NET 核心的安装程序

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

本教程演示如何启用到你的用户[使用 Twitter 帐户登录](https://dev.twitter.com/web/sign-in/desktop-browser)上使用示例 ASP.NET 核心 2.0 项目创建[上一页](xref:security/authentication/social/index)。

## <a name="create-the-app-in-twitter"></a>在 Twitter 中创建应用程序

* 导航到[ https://apps.twitter.com/ ](https://apps.twitter.com/)并登录。 如果你没有 Twitter 帐户，使用**[立即注册](https://twitter.com/signup)** 链接创建一个。 登录后，**应用程序管理**页所示：

![在 Microsoft Edge 中打开的 twitter 应用程序管理](index/_static/TwitterAppManage.png)

* 点击**创建新的应用程序**并填写应用程序**名称**，**说明**和公共**网站**（可以是临时直到你的 URI注册的域名）：

![创建的应用程序页](index/_static/TwitterCreate.png)

* 输入你的开发 URI 与`/signin-twitter`追加到**有效的 OAuth 重定向 Uri**字段 (例如： `https://localhost:44320/signin-twitter`)。 本教程中稍后配置的 Twitter 身份验证方案将自动处理请求在`/signin-twitter`要实现的 OAuth 流路由。

> [!NOTE]
> URI 段`/signin-twitter`被设置为 Twitter 身份验证提供程序的默认回调。 你可以在配置通过继承 Twitter 身份验证中间件同时更改默认的回调 URI [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)属性[TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions)类。

* 填写表单的其余部分并点击**创建 Twitter 应用程序**。 显示新的应用程序详细信息：

![应用程序页上的详细信息选项卡](index/_static/TwitterAppDetails.png)

* 部署站点时你将需要重新访问**应用程序管理**页上，并注册一个新的公共 URI。

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>存储 Twitter ConsumerKey 和 ConsumerSecret

链接敏感设置，例如 Twitter`Consumer Key`和`Consumer Secret`到你应用程序配置中使用[机密 Manager](xref:security/app-secrets)。 对于此教程的目的，命名为令牌`Authentication:Twitter:ConsumerKey`和`Authentication:Twitter:ConsumerSecret`。

可以在上找到这些标记**密钥和访问令牌**选项卡后创建新的 Twitter 应用程序：

![密钥和访问令牌的选项卡](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>配置 Twitter 身份验证

在本教程使用的项目模板可确保[Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter)已安装包。

* 若要使用 Visual Studio 2017 安装此包，请右键单击项目并选择**管理 NuGet 包**。
* 若要使用.NET 核心 CLI 安装，请在项目目录中执行以下命令：

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

添加中的 Twitter 服务`ConfigureServices`中的方法*Startup.cs*文件：

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

添加在 Twitter 中间件`Configure`中的方法*Startup.cs*文件：

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

请参阅[TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) Twitter 身份验证支持的配置选项的详细信息的 API 参考。 这可以用于请求有关用户的不同信息。

## <a name="sign-in-with-twitter"></a>使用 Twitter 登录

运行你的应用程序，然后单击**登录**。 将显示一个选项以使用 Twitter 登录：

![Web 应用程序： 用户未经过身份验证](index/_static/DoneTwitter.png)

单击**Twitter**将重定向到 Twitter 进行身份验证：

![Twitter 身份验证页](index/_static/TwitterLogin.png)

在输入你的 Twitter 凭据后，你将重定向回 web 站点，你可以设置你的电子邮件。

现在已在使用你的 Twitter 凭据进行登录：

![Web 应用程序： 身份验证的用户](index/_static/Done.png)

## <a name="troubleshooting"></a>疑难解答

* **ASP.NET 核心 2.x 仅：** 如果标识未通过调用配置`services.AddIdentity`中`ConfigureServices`，尝试进行身份验证将导致*ArgumentException： 必须提供 SignInScheme 选项*。 在本教程使用的项目模板可确保，这完成的。
* 如果尚未通过应用初始迁移创建站点数据库，则会出现*处理请求时，数据库操作失败*错误。 点击**应用迁移**创建数据库和刷新可跳过错误。

## <a name="next-steps"></a>后续步骤

* 本文介绍了你使用 Twitter 可以进行的验证。 你可以遵循类似的方法进行身份验证使用其他提供商上列出[上一页](xref:security/authentication/social/index)。

* 一旦您的网站发布到 Azure web 应用时，您应重置`ConsumerSecret`Twitter 开发人员门户中。

* 设置`Authentication:Twitter:ConsumerKey`和`Authentication:Twitter:ConsumerSecret`作为在 Azure 门户中的应用程序设置。 配置系统设置以从环境变量中读取项。
