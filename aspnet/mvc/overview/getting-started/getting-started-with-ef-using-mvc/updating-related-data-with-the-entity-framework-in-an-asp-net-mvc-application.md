---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 使用实体框架中的 ASP.NET MVC 应用程序更新相关的数据 |Microsoft 文档
author: tdykstra
description: Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 的 ASP.NET MVC 5 应用程序...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2015
ms.topic: article
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: cf4a6183e068e8668eb706d9a9e311616649e863
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875612"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>使用实体框架中的 ASP.NET MVC 应用程序更新相关的数据
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下载 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 应用程序。 若要了解系列教程，请参阅[本系列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


在前面的教程中显示相关的数据;在本教程中，你将更新相关的数据。 对于大多数关系，这可以通过更新外键字段或导航属性。 对于多对多关系，实体框架不联接表直接公开，因此添加和删除实体到和从相应的导航属性。

下图显示了一些将使用的页面。

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![对于课程教师编辑](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>自定义课程创建页和编辑页

创建新的课程实体时，该实体必须与现有系相关联。 为了便于实现这种关联，基架代码包含了控制器方法以及创建和编辑视图，其中包括用于选择系的下拉列表。 下拉列表设置`Course.DepartmentID`外键属性，而这正是实体框架需要以便加载`Department`使用相应的导航属性`Department`实体。 你将使用基架代码，但需要对其略作更改以添加错误处理并对下拉列表排序。

在*CourseController.cs*，删除四个`Create`和`Edit`方法，并用它们替换下面的代码：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

添加以下`using`语句开头的文件：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`PopulateDepartmentsDropDownList`方法获取按名称排序的所有部门的列表，创建`SelectList`集合有关的下拉列表，并将集合传递到该视图`ViewBag`属性。 该方法接受可选`selectedDepartment`参数，调用代码可通过该参数指定呈现该下拉列表时选定的项。 该视图将传递名称`DepartmentID`到[DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md)帮助器，然后帮助器就会知道要查找的`ViewBag`对象`SelectList`名为`DepartmentID`。

`HttpGet` `Create`方法调用`PopulateDepartmentsDropDownList`而无需设置选定的项，因为新课程部门不尚未建立的方法：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpGet` `Edit`方法设置选定的项，基于部门已分配给过程中正在编辑的 ID:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

`HttpPost`方法同时`Create`和`Edit`还包括当它们在错误之后重新显示页时设置选定的项的代码：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

此代码可确保当重新显示页以显示错误消息，选择任何部门保持所选。

课程视图已基架使用下拉列表的部门字段中，但不希望 DepartmentID 标题为此字段中，因此请以下突出显示将更改为*Views\Course\Create.cshtml*到文件更改的标题。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

进行中相同的更改*Views\Course\Edit.cshtml*。

通常 scaffolder 不搭建基架为主键由于密钥的值由数据库生成和不能更改，而不是有意义的值，向用户显示。 基架的课程实体包含为文本框`CourseID`字段它理解，因为`DatabaseGeneratedOption.None`属性意味着用户应该可以输入的主键值。 但它不理解，因为该号码是有意义你想要在其他视图中查看它，因此你需要手动添加它。

在*Views\Course\Edit.cshtml*，添加课程数字字段之前**标题**字段。 因为它是主键，它会显示，但不能更改。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

已存在一个隐藏的字段 (`Html.HiddenFor`帮助程序) 编辑视图中的过程编号。 添加*Html.LabelFor*帮助器不会消除隐藏字段的需要，因为它不会导致课程编号，以包括在已发布数据中，当用户单击**保存**编辑页上。

在*Views\Course\Delete.cshtml*和*Views\Course\Details.cshtml*、 更改部门名称的标题从"名称"到"部门"以及添加课程数字字段之前**标题**字段。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

运行**创建**页 (显示课程索引页，然后单击**新建**) 和新的课程中输入数据：

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

单击“创建” 。 课程索引页会显示新添加到列表中的过程。 “索引”页列表中的院系名称来自导航属性，表明已正确建立关系。

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

运行**编辑**页 (显示课程索引页，然后单击**编辑**课程上)。

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

更改页面上的数据，然后单击“保存”。 课程索引页会显示已更新的课程数据。

## <a name="adding-an-edit-page-for-instructors"></a>添加教师编辑页

编辑讲师记录时，有时希望能更新讲师的办公室分配。 `Instructor`实体具有对零或一一个关系`OfficeAssignment`实体，这意味着必须处理的以下情况：

- 如果用户清除 office 分配，并且最初具有一个值，你必须删除并删除`OfficeAssignment`实体。
- 如果用户输入 office 分配值，并且它最初为空，则必须创建一个新`OfficeAssignment`实体。
- 如果用户更改一个办公室分配的值，则必须更改中的现有值`OfficeAssignment`实体。

打开*InstructorController.cs*并查看`HttpGet``Edit`方法：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

此处的基架的代码不符合要求。 设置数据的下拉列表中，但您需要的是文本框。 此方法替换为以下代码：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

此代码将删除`ViewBag`语句，并将添加为关联的预先加载`OfficeAssignment`实体。 无法执行与预先加载`Find`方法，因此`Where`和`Single`方法用于改为选择 instructor。

替换`HttpPost``Edit`方法替换为以下代码。 可处理 office 分配更新：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

对引用`RetryLimitExceededException`需要`using`语句; 若要将其添加，请右键单击`RetryLimitExceededException`，然后单击**解决** - **使用 System.Data.Entity.Infrastructure**.

![解决重试异常](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

该代码执行以下操作：

- 方法名称更改为`EditPost`因为签名现在为相同`HttpGet`方法 (`ActionName`属性指定 /Edit/ URL 仍使用)。
- 使用 `OfficeAssignment` 导航属性的预先加载从数据库获取当前的 `Instructor` 实体。 这是你未与相同`HttpGet``Edit`方法。
- 用模型绑定器中的值更新检索到的 `Instructor` 实体。 [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx)使用重载使你能够*允许列表*你想要包括的属性。 这可以防止过度发布中所述[第二个教程](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)。

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- 如果 office 位置为空，设置`Instructor.OfficeAssignment`为 null 的属性，以便中的相关的行`OfficeAssignment`表将被删除。

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- 将更改保存到数据库。

在*Views\Instructor\Edit.cshtml*后面`div`元素**雇用日期**字段中，添加以进行编辑的办公地点的新字段：

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

运行页面 (选择**教师**选项卡，然后单击**编辑**教师上)。 更改“办公室位置”，然后单击“保存”。

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>添加课程分配到教师编辑页

讲师可能教授任意数量的课程。 现在可以通过使用一组复选框来更改课程分配，从而增强“讲师编辑”页面的性能，如以下屏幕截图所示：

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

之间的关系`Course`和`Instructor`实体是多对多，这意味着你没有直接访问联接表中的外键属性。 相反，你添加和删除实体，并从`Instructor.Courses`导航属性。

用于更改讲师所对应的课程的 UI 是一组复选框。 该复选框中会显示数据库中的所有课程，选中讲师当前对应的课程即可。 用户可以通过选择或清除复选框来更改课程分配。 如果课程数大得多，你将可能想要使用不同的方法来在视图中，显示数据的但你将使用相同的方法的操作才能创建或删除关系的导航属性。

若要为复选框列表的视图提供数据，可使用视图模型类。 创建*AssignedCourseData.cs*中*Viewmodel*文件夹和替换现有代码替换为以下代码：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

在*InstructorController.cs*，替换`HttpGet``Edit`方法替换为以下代码。 突出显示所做的更改。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

该代码为 `Courses`导航属性添加了预先加载，并调用新的`PopulateAssignedCourseData`方法使用`AssignedCourseData`视图模型类为复选框数组提供信息。

中的代码`PopulateAssignedCourseData`方法读取通过所有`Course`才能加载课程使用视图的列表的实体模型类。 对每门课程而言，该代码都会检查讲师的 `Courses` 导航属性中是否存在该课程。 若要检查是否将某一课程分配给教师时，请创建高效的查找，分配给教师的课程已放入[HashSet](https://msdn.microsoft.com/library/bb359438.aspx)集合。 `Assigned`属性设置为`true`教师分配课程。 视图将使用此属性来确定应将哪些复选框显示为选中状态。 最后，该列表传递给中的视图`ViewBag`属性。

接下来，添加用户单击“保存”时执行的代码。 替换`EditPost`方法替换为以下代码，调用更新的新方法`Courses`导航属性`Instructor`实体。 突出显示所做的更改。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

方法签名现在是不同于`HttpGet``Edit`方法，以便从更改方法名称`EditPost`回`Edit`。

由于该视图不包含一套`Course`实体，不能自动更新模型联编程序`Courses`导航属性。 而不是使用模型联编程序更新`Courses`导航属性，则将执行该操作在新`UpdateInstructorCourses`方法。 为此，需要从模型绑定中排除 `Courses` 属性。 这不需要对调用代码的任何更改[TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx)因为您正在使用*允许列表*重载和`Courses`不在包括列表中。

如果没有复选框已选中中的代码`UpdateInstructorCourses`初始化`Courses`对空集合的导航属性：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

之后，代码会循环访问数据库中的所有课程，并逐一检查当前分配给讲师的课程和视图中处于选中状态的课程。 为便于高效查找，后两个集合存储在 `HashSet` 对象中。

如果某课程的复选框处于选中状态，但该课程不在 `Instructor.Courses` 导航属性中，则会将该课程添加到导航属性中的集合中。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

如果某课程的复选框未处于选中状态，但该课程存在 `Instructor.Courses` 导航属性中，则会从导航属性中删除该课程。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

在*Views\Instructor\Edit.cshtml*，添加**课程**字段与非数组的复选框，通过添加以下代码后立即`div`元素`OfficeAssignment`字段和之前`div`元素**保存**按钮：

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

粘贴完毕后的代码，如果换行和缩进看起来像它们在此处执行操作，以便它如下所示这里看到手动修复所有内容。 缩进不一定要完美，但 `@</tr><tr>`、`@:<td>`、`@:</td>` 和 `@</tr>` 行必须各成一行（如下所示），否则会出现运行时错误。

此代码将创建一个具有三列的 HTML 表。 每个列中都有一个复选框，随后是一段由课程编号和标题组成的描述文字。 所有复选框都具有相同的名称，即 selectedCourses，以告知模型绑定器将它们视为一组。 `value`的每个复选框的属性设置的值为`CourseID.`发页面，当模型联编程序会将数组传递给组成的控制器`CourseID`仅复选框进行选择的值。

当最初呈现复选框时，为分配给教师的课程的那些具有`checked`属性，后者选择它们 （它们检查的显示）。

更改课程作业之后, 你将需要能够验证所做的更改，当站点恢复时`Index`页。 因此，你需要将列添加到该页面中的表。 在这种情况下不需要使用`ViewBag`对象，因为已经在你想要显示的信息`Courses`导航属性`Instructor`您传递到页其作为模型的实体。

在*Views\Instructor\Index.cshtml*，添加**课程**标题紧跟**Office**标题下，如下面的示例中所示：

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

然后添加新的详细信息单元紧跟 office 位置详细信息单元：

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

运行**教师索引**页后，可以查看分配给每个教师课程：

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

单击**编辑**教师若要查看编辑页上。

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

更改某些课程作业，然后单击**保存**。 所做的更改将反映在“索引”页上。

 注意： 采取此处编辑教师课程数据的做法适用于没有有限的数量的课程。 若是远大于此的集合，则需要使用不同的 UI 和不同的更新方法。  
 

## <a name="update-the-deleteconfirmed-method"></a>更新 DeleteConfirmed 方法

在*InstructorController.cs*，删除`DeleteConfirmed`方法并插入以下代码在其位置。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

此代码做出以下更改：

- 如果教师被指定为任何部门的管理员，移除该部门教师分配。 不使用此代码，如果你试图删除一个教师已指定为某个部门的管理员将得到一个引用完整性错误。

此代码不会处理作为多个部门的管理员分配一个教师的情形。 在最后一个教程中，你将添加代码，以防止发生这种情况。

## <a name="add-office-location-and-courses-to-the-create-page"></a>向“创建”页添加办公室位置和课程

在*InstructorController.cs*，删除`HttpGet`和`HttpPost``Create`方法，然后在它们的位置添加以下代码：


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

此代码将类似于你看到的针对编辑方法只不过最初没有课程均被选中。 `HttpGet` `Create`方法调用`PopulateAssignedCourseData`方法不是因为可能存在但在选择的课程，以便提供为空集合`foreach`（否则查看代码将引发空引用异常的视图中的循环).

HttpPost 创建方法将添加到模板代码检查有验证错误，向数据库添加新教师之前的课程导航属性的每个所选的过程。 课程会添加即使有模型错误以便模型错误 （例如，用户键控无效的日期） 时，以便当页则重新显示具有一条错误消息，将自动还原所做的任何课程选择。

请注意，为了能够向 `Courses` 导航属性添加课程，必须将该属性初始化为空集合：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

除了在控制器代码中进行此操作之外，还可以在“讲师”模型中进行此操作，方法是将属性 getter 更改为不存在集合时自动创建集合，如以下示例所示：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

如果通过这种方式修改 `Courses` 属性，则可以删除控制器中的显式属性初始化代码。

在*Views\Instructor\Create.cshtml*、 office 位置中添加文本框和复选框的课程，雇佣日期字段后，和之前**提交**按钮。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

粘贴代码后，可以修复换行符和缩进，如你此前编辑页。

运行创建页，然后添加一个教师。

![使用课程创建教师](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<a id="transactions"></a>
## <a name="handling-transactions"></a>处理事务

中所述[基本 CRUD 功能教程](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)，默认情况下实体框架隐式实现事务。 有关方案，你需要更多的控制-例如，如果你想要包括在实体框架外部事务-中执行的操作，请参阅[使用事务](https://msdn.microsoft.com/data/dn456843)MSDN 上。

## <a name="summary"></a>总结

你现在已经完成此简介使用相关数据。 到目前为止这些教程中使用过执行同步 I/O 代码。 你可以进行应用程序通过实现异步代码中，更有效地使用 web 服务器资源和这便是你将在下一步的教程。

请在如何喜欢本教程的方式，我们可以提高上，留下反馈。 你还可以请求新主题[教我编写代码](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。

在找不到其他实体框架资源的链接[ASP.NET 数据访问的推荐资源](../../../../whitepapers/aspnet-data-access-content-map.md)。

> [!div class="step-by-step"]
> [上一页](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一页](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
