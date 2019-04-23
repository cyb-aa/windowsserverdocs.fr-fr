---
title: Installer | Mettre à niveau | Migrer vers Windows Server 2019
description: Guide pratique pour la nouvelle installation, mise à niveau sur place ou migrer vers Windows Server 2019.
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868710"
---
# <a name="install--upgrade--migrate-to-windows-server-2019"></a>Installer | Mettre à niveau | Migrer vers Windows Server 2019

>S'applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

> [!IMPORTANT]
> Le support étendu de Windows Server 2008 R2 et de Windows Server 2008 prend fin en janvier 2020. [En savoir plus sur vos options de mise à niveau](http://aka.ms/upgradecenter).

Il est peut-être temps de passer à une version plus récente de Windows Server. Selon la version que exécutez actuellement, vous disposez de nombreuses options de mise à niveau.

## <a name="clean-install"></a>Nettoyer l’installation
Si vous souhaitez déplacer à partir d’une version antérieure de Windows Server vers Windows Server 2019 sur le même matériel, vous devez faire un **nouvelle installation**, où vous installez simplement le système d’exploitation plus récent directement sur l’ancienne sur le même matériel, Par conséquent, la suppression du système d’exploitation précédent. Cette procédure est la plus simple, mais vous devez commencer par sauvegarder vos données et il vous faudra ensuite réinstaller vos applications. Il existe quelques éléments à connaître, tels que de la configuration système requise, par conséquent, veillez à consulter les détails de [Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124), [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) , et [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

## <a name="in-place-upgrade"></a>Mise à niveau sur place
Si vous souhaitez conserver le même matériel et tous les rôles de serveur que vous avez configuré sans le serveur de mise à plat, vous souhaiterez faire un **mise à niveau In situ**, par laquelle vous accédez à partir d’un ancien système d’exploitation de vers une version plus récente, en conservant vos paramètres de serveur les rôles et les données intactes. Par exemple, si votre serveur exécute Windows Server 2012 R2, vous pouvez mettre à niveau vers Windows Server 2016 ou Windows Server 2019. Toutefois, il n’existe pas de chemin de mise à niveau vers toutes les versions plus récentes pour certains systèmes d’exploitation plus anciens. Consultez le diagramme suivant pour les chemins de mise à niveau disponibles :

![Diagramme de mise à niveau sur place de Windows Server](media/upgrade-paths.png)

Pour obtenir des instructions sur la mise à niveau, visitez le [centre de mise à niveau de Windows Server](http://aka.ms/upgradecenter):

<a href="http://aka.ms/upgradecenter"><img src="media/upgrade-center.png" alt="Screenshot of Windows Upgrade Center" title="Centre de mise à niveau de Windows Server"></a>

## <a name="cluster-os-rolling-upgrade"></a>Mise à niveau propagée du système d’exploitation de cluster
Niveau propagée de cluster du système d’exploitation permet à un administrateur mettre à niveau le système d’exploitation des nœuds de cluster Windows Server 2012 R2 et Windows Server 2016 sans arrêter les charges de travail du serveur de fichiers avec montée en puissance parallèle ni Hyper-V. Cette fonctionnalité élimine les temps d’arrêt susceptibles d’affecter les contrats de niveau de service. Cette nouvelle fonctionnalité est décrite plus en détail dans l’article [Mise à niveau propagée de système d’exploitation de cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## <a name="migration"></a>Migration

Migration de Windows Server est lorsque vous déplacez un rôle ou une fonctionnalité à la fois à partir d’un ordinateur source qui exécute Windows Server vers un autre ordinateur de destination qui exécute Windows Server, le même réseau ou une version plus récente. À ces fins, la migration est définie comme étant le déplacement d’un rôle ou d’une fonctionnalité et des données associées vers un autre ordinateur, et non pas la mise à niveau de la fonctionnalité sur le même ordinateur. 

## <a name="license-conversion"></a>Conversion de licence
Dans certaines versions de systèmes d’exploitation, vous pouvez convertir une édition particulière de la version vers une autre édition de la même version en une seule étape à l’aide d’une simple commande et de la clé de licence appropriée. Cette opération est appelée **conversion de licence**. Par exemple, si votre serveur exécute Windows Server 2016 Standard, vous pouvez effectuer une conversion vers Windows Server 2016 Datacenter. Pour certaines versions de Windows Server, vous pouvez également effectuer librement des conversions entre les versions OEM, de licence en volume et commerciales avec la même commande et la clé appropriée.


 
 
