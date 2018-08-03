---
title: 关系的 EF 设计器的 EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
caps.latest.revision: 3
ms.openlocfilehash: f924945b19dd6d73847ff3ec52c0b5a286c591bb
ms.sourcegitcommit: 9ae4473425c5e76337c9d032b0e5dbfedf1fcf57
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2018
ms.locfileid: "39120537"
---
# <a name="relationships---ef-designer"></a>关系的 EF 设计器
> [!NOTE]
> 此页提供了有关设置关系在模型中使用 EF 设计器的信息。 有关在 EF 和如何访问和处理数据使用关系的关系的常规信息，请参阅[关系和导航属性](~/ef6/fundamentals/relationships.md)。

关联定义一个模型中的实体类型之间的关系。 本主题演示如何将映射与实体框架设计器 （EF 设计器） 之间的关联。 下图显示了使用 EF 设计器时使用的主窗口。

![EFDesigner](~/ef6/media/efdesigner.png)

> [!NOTE]
> 生成概念模型时，可能会在错误列表中出现有关未映射的实体和关联的警告。 可以忽略这些警告，因为您选择从模型生成数据库后，这些错误将会消失。

## <a name="associations-overview"></a>关联概述

在设计时使用 EF 设计器模型，.edmx 文件表示您的模型。 在.edmx 文件中，**关联**元素定义两个实体类型之间的关系。 关联必须指定关系中涉及的实体类型和关系的每一端可能的实体类型数量（也称为重数）。 关联端的多重性可以具有的一 (1)、 零个或一个值 (0..1) 或许多 (\*)。 此信息指定在两个子**最终**元素。

在运行时，关联的一端的实体类型实例可以通过访问导航属性或外键 （如果您选择公开您的实体中的外键）。 通过公开的外键，实体之间的关系进行管理**ReferentialConstraint**元素 (的子元素**关联**元素)。 建议始终在你的实体中公开外的键关系。

> [!NOTE]
> 在多对多 (\*:\*) 不能向实体添加外键。 在中\*:\*关系中，关联信息通过独立对象进行管理。

有关 CSDL 元素 (**ReferentialConstraint**，**关联**等) 请参阅[CSDL 规范](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md)。

## <a name="create-and-delete-associations"></a>创建和删除关联

创建与 EF 设计器更新的关联模型的.edmx 文件的内容。 之后创建的关联，必须创建 （在本主题后面讨论） 的关联的映射。

> [!NOTE]
> 本部分假设你已添加你想要创建到模型之间的关联的实体。

### <a name="to-create-an-association"></a>创建关联

1.  右键单击设计图面上的空白区域，指向**新添**，然后选择**关联...**.
2.  中的关联的设置中填充**添加关联**对话框。

    ![AddAssociation](~/ef6/media/addassociation.png)

    > [!NOTE]
    > 您可以选择不添加导航属性或外键属性向位于关联各端实体通过清除 * * 导航属性 * * 和 * * 添加到外的键属性&lt;实体类型名称&gt;实体 * *复选框。 如果只添加一个导航属性，则将只能在一个方向遍历关联。 如果不添加导航属性，则必须选择添加外键属性才能访问位于关联各端的实体。
    
3.  单击 **“确定”**。

### <a name="to-delete-an-association"></a>删除关联

若要删除关联执行下列任一操作：

-   右键单击在 EF 设计器图面上，然后选择关联**删除**。

- OR-

-   选择一个或多个关联并按 Delete 键。

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a>实体 （引用约束） 中包括外键属性

建议始终在你的实体中公开外的键关系。 实体框架使用引用约束来标识一个属性可作为一种关系的外键。

如果您选中***添加到外的键属性&lt;实体类型名称&gt;实体***复选框为你创建关系时，添加此引用约束。

当使用 EF 设计器添加或编辑引用约束时，EF 设计器添加或修改**ReferentialConstraint** .edmx 文件的 CSDL 内容中的元素。

-   双击要编辑的关联。
    **引用约束**对话框随即出现。
-   从**主体**下拉列表中，选择引用约束中的主体实体。
    实体的键属性添加到**主体键**对话框列表中的。
-   从**依赖**下拉列表中，选择依赖实体的引用约束中。
-   对于具有依赖键每个主体键，从下拉列表中选择对应的依赖键**依赖键**列。

    ![RefConstraint](~/ef6/media/refconstraint.png)

-   单击 **“确定”**。

## <a name="create-and-edit-association-mappings"></a>创建和编辑关联映射

您可以指定如何关联映射到数据库中**映射详细信息**的 EF 设计器窗口。

> [!NOTE]
> 你只能映射不具有指定的引用约束的关联的详细信息。 如果指定引用约束的实体中包含外键属性，并可用于控制哪一列的外键映射到实体映射详细信息。

### <a name="create-an-association-mapping"></a>创建关联映射

-   右键单击设计图面并选择中的关联**表映射**。
    这将显示在关联映射**映射的详细信息**窗口。
-   单击**添加一个表或视图**。
    此时将显示一个下拉列表，其中包含存储模型中的所有表。
-   选择关联要映射到的表。
    **映射详细信息**窗口将显示在每个关联和键属性的实体类型的两端**最终**。
-   每个键属性，请单击**列**字段，然后选择该属性将映射到其中的列。

    ![MappingDetails4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a>编辑关联映射

-   右键单击设计图面并选择中的关联**表映射**。
    这将显示在关联映射**映射的详细信息**窗口。
-   单击**映射到&lt;表名&gt;**。
    此时将显示一个下拉列表，其中包含存储模型中的所有表。
-   选择关联要映射到的表。
    **映射详细信息**窗口将显示在每一端的关联和键属性的实体类型的两端。
-   每个键属性，请单击**列**字段，然后选择该属性将映射到其中的列。

## <a name="edit-and-delete-navigation-properties"></a>编辑和删除导航属性

导航属性是快捷方式属性，用于定位位于关联各端的实体模型中。 在创建两个实体类型之间的关联时可以创建导航属性。

#### <a name="to-edit-navigation-properties"></a>若要编辑导航属性

-   选择 EF 设计器图面上的导航属性。
    有关导航属性信息显示在 Visual Studio**属性**窗口。
-   更改中的属性设置**属性**窗口。

#### <a name="to-delete-navigation-properties"></a>若要删除导航属性

-   如果概念模型中的实体类型上没有公开外键，则删除导航属性可能导致仅在一个方向遍历相应的关联，或者根本不遍历关联。
-   右键单击 EF 设计器图面上，然后选择上的一个导航属性**删除**。