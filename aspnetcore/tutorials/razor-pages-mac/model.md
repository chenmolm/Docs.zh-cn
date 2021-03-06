---
title: 使用 Visual Studio for Mac 将模型添加到 ASP.NET Core Razor 页面应用
author: rick-anderson
description: 了解如何使用 Visual Studio for Mac 将模型添加到 ASP.NET Core 中的 Razor 页面应用。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 97bc9f14b8d6da958a7f587e54a37d2d0e0aabd4
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483656"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a>使用 Visual Studio for Mac 将模型添加到 ASP.NET Core Razor 页面应用

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>添加数据模型

* 在解决方案资源管理器中，右键单击“RazorPagesMovie”项目，然后选择“添加” > “新建文件夹”。 将文件夹命名为“Models”。
* 右键单击“Models”文件夹，然后选择“添加” > “新建文件”。
* 在“新建文件”对话框中：

  * 在左侧窗格中，选择“常规”。
  * 在中间窗格中，选择“空类”。
  * 将此类命名为“Movie”，然后选择“新建”。

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

右键单击红色波浪线，例如，行 `services.AddDbContext<MovieContext>(options =>` 中的 `MovieContext`。 选择“快速修复”>“使用RazorPagesMovie.Models;”。 Visual Studio 添加 using 语句。

生成项目以验证有没有任何错误存在。

![创建页面](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>用于进行迁移的 Entity Framework Core NuGet 包

[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) 中提供了适用于命令行接口 (CLI) 的 EF 工具。 单击 [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) 链接以获取要使用的版本号。 若要安装此包，请将它添加到 .csproj 文件中的 `DotNetCliToolReference` 集合。 注意：必须通过编辑 .csproj 文件来安装此包；不能使用 `install-package` 命令或程序包管理器 GUI。

若要编辑“.csproj”文件：

* 选择“文件” > “打开”，然后选择“.csproj”文件。
* 选择“选项”。
* 将“打开方式”更改为“源代码编辑器”。

![编辑 csproj 文件](model/csproj.png)

将 `Microsoft.EntityFrameworkCore.Tools.DotNet` 工具引用添加至第二个 \<ItemGroup>：

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

以下代码中显示的版本号在编写时是正确的。

[!INCLUDE [model3](../../includes/RP/model3.md)]

[!INCLUDE [model 4x](../../includes/RP/model4x.md)]

[!INCLUDE [model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE [model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a>将页面/电影文件添加到项目

* 在 Visual Studio 中，右键单击“页面”文件夹，然后选择“添加”>“添加现有文件夹”。
* 选择“电影”文件夹。
* 在“选择要包含在项目中的文件”对话框中，选择“包括所有”。

下一个教程介绍由基架创建的文件。

> [!div class="step-by-step"]
> [上一篇：入门](xref:tutorials/razor-pages-mac/razor-pages-start)
> [下一篇：已搭建基架的 Razor 页面](xref:tutorials/razor-pages-mac/page)
