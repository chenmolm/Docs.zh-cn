---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: 修改动画从服务器端 (C#) |Microsoft 文档
author: wenz
description: ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 动画也可能会...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 6946875552c885ffb1f2a2eb7e728b85d7dd3973
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869005"
---
<a name="modifying-animations-from-the-server-side-c"></a>修改动画从服务器端 (C#)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip)或[下载 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)

> ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 也可能在服务器端发生更改动画


## <a name="overview"></a>概述

ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 也可能在服务器端发生更改动画

## <a name="steps"></a>步骤

首先，包括`ScriptManager`在页中; 然后，ASP.NET AJAX 库加载后，使其可以使用该控件工具包：

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

动画将应用于如下所示的文本的一个面板中：

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

在关联 CSS 类中的面板，定义一种很好的背景色，并且还设置面板的宽度固定:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

其余代码在服务器端上运行，并且不使用标记;相反，它使用代码来创建`AnimationExtender`控件：

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

但是，该控件工具包当前不提供的 API 访问权限，以创建单独的动画。 不过，它是可以设置`AnimationExtender`的动画属性设置为一个字符串包含以声明方式分配动画时使用的 XML 标记。 若要创建的 XML 不能包含`<Animations>`可以使用.NET Framework 的 XML 元素支持，或按照以下代码，只需提供的字符串：

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

最后，添加`AnimationExtender`内控制转移到当前页上，`<form runat="server">`元素，并确保动画附带，运行：

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


[![使用服务器端 C# /VB 代码创建动画](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

使用服务器端 C# /VB 代码创建动画 ([单击以查看实际尺寸的图像](modifying-animations-from-the-server-side-cs/_static/image3.png))

> [!div class="step-by-step"]
> [上一页](triggering-an-animation-in-another-control-cs.md)
> [下一页](executing-animations-using-client-side-code-cs.md)
