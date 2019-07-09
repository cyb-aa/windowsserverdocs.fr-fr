---
title: Installer | Mettre à niveau | Migrer vers Windows Server 2019
description: Guide pratique pour l’installation, la mise à niveau ou la migration sur place vers Windows Server 2019.
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
ms.openlocfilehash: 1fd955a640832eb161666f74b93d91bb2c3eff11
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66810816"
---
# <a name="install--upgrade--migrate-to-windows-server-2019"></a>Installer | Mettre à niveau | Migrer vers Windows Server 2019

>S'applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

> [!IMPORTANT]
> Le support étendu de Windows Server 2008 R2 et de Windows Server 2008 prend fin en janvier 2020. [En savoir plus sur vos options de mise à niveau](http://aka.ms/upgradecenter).

Il est peut-être temps de passer à une version plus récente de Windows Server. Selon la version exécutée, vous disposez de nombreuses options de mise à niveau.

## <a name="clean-install"></a>Nouvelle installation
Si vous souhaitez effectuer une mise à niveau depuis une ancienne version de Windows Server vers Windows Server 2019 sur le même matériel, vous devriez effectuer une **nouvelle installation**, où vous installez simplement la dernière version du système d’exploitation directement sur l’ancienne version, sur le même matériel, en supprimant ainsi le système d’exploitation précédent. Cette procédure est la plus simple, mais vous devez commencer par sauvegarder vos données et il vous faudra ensuite réinstaller vos applications. Vous devez prendre en compte plusieurs éléments, notamment la configuration système requise. Par conséquent, veillez à consulter les informations relatives à [Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124), [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) et [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

## <a name="in-place-upgrade"></a>Mise à niveau sur place

Si vous souhaitez conserver le même matériel et tous les rôles serveur que vous avez configurés sans aplatir le serveur, vous voudrez faire une **mise à niveau sur place**, par laquelle vous passez d’un ancien système d’exploitation à une version plus récente, tout en conservant vos paramètres, vos rôles serveur et vos données. Par exemple, si votre serveur exécute Windows Server 2012 R2, vous pouvez le mettre à niveau vers Windows Server 2016 ou Windows Server 2019. Toutefois, il n’existe pas de chemin de mise à niveau vers toutes les versions plus récentes pour certains systèmes d’exploitation plus anciens. Consultez le diagramme suivant pour connaître les chemins de mise à niveau disponibles :

![Diagramme de mise à niveau sur place de Windows Server](media/upgrade-paths.png)

Pour obtenir des instructions détaillées sur la mise à niveau, consultez le [centre de mise à niveau Windows Server](http://aka.ms/upgradecenter) :

[![Capture d’écran du centre de mise à niveau Windows Server](media/upgrade-center.png)](http://aka.ms/upgradecenter)

## <a name="cluster-os-rolling-upgrade"></a>Mise à niveau propagée du système d’exploitation de cluster

La mise à niveau propagée de système d’exploitation de cluster permet à un administrateur de mettre à niveau le système d’exploitation des nœuds de cluster Windows Server 2012 R2 et Windows Server 2016, sans arrêter les charges de travail du serveur de fichiers avec montée en puissance parallèle ni Hyper-V. Cette fonctionnalité élimine les temps d’arrêt susceptibles d’affecter les contrats de niveau de service. Cette nouvelle fonctionnalité est décrite plus en détail dans l’article [Mise à niveau propagée de système d’exploitation de cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## <a name="migration"></a>Migration

La migration de Windows Server vous permet de migrer un rôle ou une fonctionnalité à la fois d’un ordinateur source qui exécute Windows Server vers un autre ordinateur cible qui exécute la même version ou une version plus récente de Windows Server. À ces fins, la migration est définie comme étant le déplacement d’un rôle ou d’une fonctionnalité et des données associées vers un autre ordinateur, et non pas la mise à niveau de la fonctionnalité sur le même ordinateur. 

## <a name="license-conversion"></a>Conversion de licence
Dans certaines versions de systèmes d’exploitation, vous pouvez convertir une édition particulière de la version vers une autre édition de la même version en une seule étape à l’aide d’une simple commande et de la clé de licence appropriée. Cette opération est appelée **conversion de licence**. Par exemple, si votre serveur exécute Windows Server 2016 Standard, vous pouvez effectuer une conversion vers Windows Server 2016 Datacenter. N’oubliez pas que, alors que vous pouvez passer d’une version Server 2016 Standard à une version Server 2016 Datacenter, vous ne pouvez pas inverser le processus et passer d’une version Datacenter à une version Standard. Pour certaines versions de Windows Server, vous pouvez également effectuer librement des conversions entre les versions OEM, de licence en volume et commerciales avec la même commande et la clé appropriée.


 
 
