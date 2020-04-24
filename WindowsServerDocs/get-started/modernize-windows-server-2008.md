---
title: Mettre à niveau Windows Server 2008 et Windows Server 2008 R2
description: Windows Server 2008 et 2008 R2 approchent de leur fin de service. Découvrez comment effectuer une mise à niveau locale ou un réhébergement sur Azure.
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: mikeblodge
ms.author: mikeblodge
ms.date: 07/12/2018
ms.topic: get-started-article
ms.localizationpriority: high
ms.openlocfilehash: 3d2c55430a78eaabfe55b764275c6e61fa80368a
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826212"
---
# <a name="upgrade-windows-server-2008-and-windows-server-2008-r2"></a>Mettre à niveau Windows Server 2008 et Windows Server 2008 R2

Le support étendu de Windows Server 2008 et de Windows Server 2008 R2 prend fin le 14 janvier 2020. Il existe deux chemins de modernisation possibles : une mise à niveau locale ou une migration avec un réhébergement dans Azure. **Si vous réhébergez dans Azure, vous pouvez migrer vos images Server existantes gratuitement.**

![Organigramme décrivant les chemins de mise à niveau à partir de Windows Server 2008](media/WS08_upgrade_paths.png)


## <a name="on-premises-upgrade"></a>Mise à niveau locale
Si vous voulez conserver vos serveurs locaux et que vous exécutez Windows Server 2008 ou Windows Server 2008 R2, vous devez [effectuer une mise à niveau vers Windows Server 2012/2012 R2](installation-and-upgrade.md#upgrading-to-windows-server-2012-r2) avant de pouvoir [effectuer une mise à niveau vers Windows Server 2016](installation-and-upgrade.md#upgrading-to-windows-server-2016). Lors de la mise à niveau, vous avez toujours la possibilité d’effectuer une migration vers Azure avec un réhébergement.

Pour plus d’informations sur vos options de mise à niveau locale, consultez [Mise à niveau de Windows Server 2008 R2 ou de Windows Server 2008](installation-and-upgrade.md#upgrading-from-windows-server-2008-r2-or-windows-server-2008).

Si vous exécutez Windows Server 2003, vous devrez [effectuer une mise à niveau vers Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff972408(v%3dws.10)). Pour plus d’informations sur vos options de mise à niveau locale, consultez les [chemins de mise à niveau pour Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd979563(v=ws.10)).


## <a name="migrate-to-azure"></a>Migrer vers Azure
Vous pouvez migrer vos serveurs locaux de Windows Server 2008 et Windows Server 2008 R2 vers Azure, dans lequel vous pouvez continuer à les exécuter sur des machines virtuelles. Dans Azure, vous restez en conformité, bénéficiez d’une plus grande sécurité et profitez de l’innovation du cloud dans votre travail. Avantages de la migration vers Azure :

- Mises à jour de sécurité dans Azure.
- Bénéficiez de trois de mises à jour de sécurité critiques et importantes de plus pour Windows Server 2008 R2 ou 2008, inclus sans frais supplémentaires. 
- Mises à niveau sans frais dans Azure.
- Adoptez davantage de services cloud lorsque vous êtes prêt.
- Grâce à la migration de SQL Server vers Azure Managed Instance ou des machines virtuelles, vous bénéficiez de trois ans de mises à jour critiques de sécurité de plus pour Windows Server 2008 R2 ou 2008, inclus sans frais supplémentaires. 
- Tirez parti des licences existantes SQL Server et Windows Server pour réaliser des économies de cloud exceptionnelles sur Azure.

[![Démarrer la migration vers Azure avec une image spécialisée](./media/WS08-image-banner-small.png)](uploading-specialized-WS08-image-to-azure.md)

Pour commencer la migration, consultez [Charger une image Windows Server 2008/2008 R2 spécialisée dans Azure](uploading-specialized-WS08-image-to-azure.md).

Pour vous aider à comprendre comment analyser les ressources informatiques existantes, évaluer ce que vous possédez, identifiez les avantages de la migration de services et d’applications spécifiques dans le cloud ou de la conservation de charges de travail en local et de la mise à niveau vers la dernière version de Windows Server, consultez [Guide de migration pour Windows Server](https://go.microsoft.com/fwlink/?linkid=872689).

## <a name="upgrade-sql-server-20082008-r2-in-parallel-with-your-windows-servers"></a>Mettre à niveau SQL Server 2008/2008 R2 en parallèle avec vos serveurs Windows

![Logo de SQL Server](media/sqlr2.jpg)

Si vous exécutez SQL Server 2008/2008 R2, vous pouvez effectuer une mise à niveau vers SQL Server [2016](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?view=sql-server-2016) ou [2017](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?view=sql-server-2017).


## <a name="additional-resources"></a>Ressources supplémentaires
[Microsoft Azure](https://docs.microsoft.com/azure/#pivot=products)