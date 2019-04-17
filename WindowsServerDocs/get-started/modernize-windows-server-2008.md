---
title: Mise à niveau de Windows Server2008 et de Windows Server2008 R2
description: Windows Server2008 et 2008R2 approchent de leur fin de service. Découvrez comment effectuer une mise à niveau locale ou un réhébergement sur Azure.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: mikeblodge
ms.author: mikeblodge
ms.date: 07/12/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.localizationpriority: high
ms.openlocfilehash: 4127eab613abb429a200f513a11b944e05da0f76
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339367"
---
# Mise à niveau de Windows Server2008 et de Windows Server2008 R2

Le support étendu de Windows Server2008 et de Windows Server2008 R2 prend fin le 14janvier2020. Il existe deux chemins de modernisation possibles: une mise à niveau locale ou une migration avec un réhébergement dans Azure. **Si vous réhébergez dans Azure, vous pouvez faire migrer vos images Server existantes gratuitement.**

![Organigramme décrivant les chemins de mise à niveau à partir de Windows Server2008](media/WS08_upgrade_paths.png)


## Mise à niveau locale
Si vous voulez conserver vos serveurs locaux et que vous exécutez Windows Server2008 ou Windows Server2008 R2, vous devez [effectuer une mise à niveau vers Windows Server2012/2012R2](installation-and-upgrade.md#upgrading-to-windows-server-2012-r2) avant de pouvoir [effectuer une mise à niveau vers Windows Server2016](installation-and-upgrade.md#upgrading-to-windows-server-2016). Lors de la mise à niveau, vous avez toujours la possibilité d'effectuer une migration vers Azure avec un réhébergement.

Voir [Mise à niveau de Windows Server2008 R2 ou de Windows Server2008](installation-and-upgrade.md#upgrading-from-windows-server-2008-r2-or-windows-server-2008), pour en savoir plus sur vos options de mise à niveau locale.

Si vous exécutez Windows Server2003, vous devrez [effectuer une mise à niveau vers Windows Server2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff972408(v%3dws.10)). Voir [Chemins de mise à niveau pour Windows Server2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd979563(v=ws.10)) pour en savoir plus sur vos options de mise à niveau locale.


## Migration vers Azure
Vous pouvez faire migrer vos serveurs locaux de Windows Server2008 et Windows Server2008 R2 vers Azure, dans lequel vous pouvez continuer à les exécuter sur des ordinateurs virtuels. Dans Azure, vous resterez en conformité, vous optimiserez la sécurité et apporterez l’innovation du Cloud à votre travail. Avantages de la migration vers Azure:

- Mises à jour de sécurité dans Azure.
- Bénéficiez de trois de mises à jour de sécurité critiques et importantes de plus pour Windows Server2008 R2 ou 2008, inclus sans frais supplémentaires. 
- Mises à niveau sans frais dans Azure.
- Adoptez davantage de services cloud lorsque vous êtes prêt.
- Grâce à la migration de SQL Server vers Azure Managed Instance ou des ordinateurs virtuels, vous bénéficiez de trois ans de mises à jour critiques de sécurité de plus pour Windows Server2008 R2 ou 2008, inclus sans frais supplémentaires. 
- Tirez parti des licences existantes SQL Server et Windows Server pour réaliser des économies de cloud exceptionnelles sur Azure.

<a href="uploading-specialized-WS08-image-to-azure.md"><img src="media/WS08-image-banner-small.png"></a>

Pour commencer la migration, voir [Charger une image Windows Server2008/2008 R2 spécialisée dans Azure](uploading-specialized-WS08-image-to-azure.md).

Pour vous aider à comprendre comment analyser les ressources informatiques existantes, évaluer ce que vous possédez, identifiez les avantages de la migration de services et d'applications spécifiques dans le cloud ou de la conservation de charges de travail en local et de la mise à niveau vers la dernière version de Windows Server, voir [Guide de migration pour Windows Server](https://go.microsoft.com/fwlink/?linkid=872689).


## Mettre à niveau SQL Server2008/2008 R2 en parallèle avec vos serveurs Windows

![Logo de SQL Server](media/sqlr2.jpg)

Si vous exécutez SQL Server2008/2008 R2, vous pouvez effectuer une mise à niveau vers SQL Server [2016](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?view=sql-server-2016) ou [2017](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?view=sql-server-2017).


## Ressources supplémentaires
[MicrosoftAzure](https://docs.microsoft.com/azure/#pivot=products)