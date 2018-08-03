---
title: 数据种子设定的 EF Core
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 7028e1923152b27f56721dab75aae8b9c2f5ad75
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163195"
---
# <a name="data-seeding"></a>数据种子设定

> [!NOTE]  
> 此功能是在 EF Core 2.1 的新增功能。

数据种子设定允许开发者提供初始数据填充数据库。 与ef6版本不同，在 EF Core 中，种子数据与相应的实体类型相关联，并作为模型配置的一部分。 之后 EF Core [迁移](xref:core/managing-schemas/migrations/index)将在数据库执行迁移升级到新版本时，自动计算所需的插入、 更新或删除操作并应用。

例如，可配置`Blog`的种子数据在`OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

若要添加实体的关系的外键值需要指定。 经常外键属性是在卷影状态下，因此，若要能够设置匿名类的值应使用：

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

一旦已添加实体，则建议使用[迁移](xref:core/managing-schemas/migrations/index)以应用更改。 

或者，可以使用`context.Database.EnsureCreated()`以创建新的数据库包含种子数据，例如对于一个测试数据库，或者在使用内存中提供程序。 请注意，如果数据库已存在，`EnsureCreated()`将不会更新架构或数据库中的种子数据。
