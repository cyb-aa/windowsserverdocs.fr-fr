---
title: Installer | Mettre à niveau | Migrer vers Windows Server 2019
description: Procédure de nouvelle installation, de mise à niveau sur place ou de migrer vers Windows Server 2019.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4e99cca754
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 58c363fc0a1e336519bc6ec4276651345cc2b5eb
ms.sourcegitcommit: 07ac08dea2b8f2763c2614a999dc7967018aa0b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/07/2018
ms.locfileid: "6121388"
---
# Installer | Mettre à niveau | Migrer vers Windows Server 2019

>S’applique à: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

> [!IMPORTANT]
> Le support étendu de Windows Server2008R2 et de Windows Server2008 prend fin en janvier2020. [En savoir plus sur les options de mise à niveau](http://aka.ms/upgradecenter).

Il est peut-être temps de passer à une version plus récente de Windows Server. Selon la version que exécutez actuellement, vous disposez de nombreuses options de mise à niveau.

## Nouvelle installation
Si vous voulez déplacer à partir d’une version antérieure de Windows Server pour Windows Server 2019 sur le même matériel, vous devez procéder à une **nouvelle installation**, où vous installez simplement au système d’exploitation plus récent directement sur l’ancienne version sur le même matériel, en supprimant ainsi le système d’exploitation précédent. Cette procédure est la plus simple, mais vous devez commencer par sauvegarder vos données et il vous faudra ensuite réinstaller vos applications. Il existe quelques éléments à prendre en compte, notamment la configuration requise du système, par conséquent, veillez à consulter les informations de [Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124), [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418)et [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

## Mise à niveau sur place
Si vous souhaitez conserver le même matériel et tous les rôles de serveur que vous avez configuré sans aplatir le serveur, vous devrez effectuer une **mise à niveau sur place**par lequel vous évoluez d’un ancien système d’exploitation vers une version plus récente, en conservant vos paramètres, les rôles de serveur et données inchangées. Par exemple, si votre serveur exécute Windows Server 2012 R2, vous pouvez le mettre à niveau vers Windows Server 2016 ou Windows Server 2019. Toutefois, il n’existe pas de chemin de mise à niveau vers toutes les versions plus récentes pour certains systèmes d’exploitation plus anciens. Consultez le diagramme suivant pour les chemins de mise à niveau disponibles:

![Diagramme de mise à niveau sur place de Windows Server](media/upgrade-paths.png)

Pour obtenir des instructions pas à pas sur la mise à niveau, visitez le [Centre de mise à niveau de Windows Server](http://aka.ms/upgradecenter):

<a href="http://aka.ms/upgradecenter"><img src="media/upgrade-center.png" alt="Screenshot of Windows Upgrade Center" title="Centre de mise à niveau de Windows Server"></a>

## Mise à niveau propagée du système d’exploitation de cluster
Niveau propagée de cluster du système d’exploitation permet à un administrateur mettre à niveau le système d’exploitation des nœuds de cluster à partir de Windows Server 2012 R2 et Windows Server 2016 sans arrêter le Hyper-V ou les charges de travail du serveur de fichiers avec montée en. Cette fonctionnalité élimine les temps d’arrêt susceptibles d’affecter les contrats de niveau de service. Cette nouvelle fonctionnalité est décrite plus en détail dans l’article [Mise à niveau propagée de système d’exploitation de cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## Migration

Migration de Windows Server est lorsque vous déplacez un rôle ou une fonctionnalité à la fois à partir d’un ordinateur source qui exécute Windows Server vers un autre ordinateur de destination qui exécute Windows Server, la même version ou une version plus récente. À ces fins, la migration est définie comme étant le déplacement d’un rôle ou d’une fonctionnalité et des données associées vers un autre ordinateur, et non pas la mise à niveau de la fonctionnalité sur le même ordinateur. 

## Conversion de licence
Dans certaines versions de systèmes d’exploitation, vous pouvez convertir une édition particulière de la version vers une autre édition de la même version en une seule étape à l’aide d’une simple commande et de la clé de licence appropriée. Cette opération est appelée **conversion de licence**. Par exemple, si votre serveur exécute WindowsServer2016Standard, vous pouvez effectuer une conversion vers WindowsServer2016Datacenter. Pour certaines versions de WindowsServer, vous pouvez également effectuer librement des conversions entre les versions OEM, de licence en volume et commerciales avec la même commande et la clé appropriée.


 
 
