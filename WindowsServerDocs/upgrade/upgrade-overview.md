---
title: Présentation des mises à niveau de Windows Server | Microsoft Docs
description: Découvrez des informations générales sur la mise à niveau de Windows Server, ainsi que les éléments à prendre en compte avant de procéder à la mise à niveau réelle.
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/10/2019
ms.openlocfilehash: 6f57e52657ca3c80c92d56c54ea87e43aabd1e99
ms.sourcegitcommit: 27f0caf74e88781054250455c3c1adf06deb6234
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71124789"
---
# <a name="overview-about-windows-server-upgrades"></a>Présentation des mises à niveau de Windows Server

Le processus de mise à niveau vers une version plus récente de Windows Server peut varier considérablement en fonction du système d’exploitation que vous utilisez, ainsi que de la voie que vous prenez. Nous utilisons les termes suivants pour distinguer les différentes actions qui peuvent être impliquées dans un nouveau déploiement de Windows Server.

- **Installation.** Également appelée « mise à niveau sur place ». Vous passez d’une version antérieure du système d’exploitation à une version plus récente, tout en restant sur le même matériel physique. **Il s’agit de la méthode que nous allons aborder dans cette section.**

    >[!Important]
    >Les mises à niveau sur place peuvent également être prises en charge par les sociétés de cloud public ou privé ; Toutefois, vous devez vérifier les détails auprès de votre fournisseur de Cloud. En outre, vous ne pourrez pas effectuer une mise à niveau sur place sur un serveur Windows Server configuré pour **Démarrer à partir du disque dur virtuel**.

- **Programme.** Également appelée « nouvelle installation ». Vous passez d’une version antérieure du système d’exploitation à une version plus récente, en supprimant l’ancien système d’exploitation.

- **Passage.** Vous passez d’une version antérieure du système d’exploitation à une version plus récente du système d’exploitation en effectuant un transfert vers un autre ensemble de matériels ou de machines virtuelles.

- **Mise à niveau propagée du système d’exploitation du cluster.** Vous mettez à niveau le système d’exploitation de vos nœuds de cluster sans arrêter les charges de travail Hyper-V ou Serveur de fichiers avec montée en puissance parallèle. Cette fonctionnalité élimine les temps d’arrêt susceptibles d’affecter les contrats de niveau de service. Pour plus d’informations, consultez [mise à niveau propagée de système d’exploitation de cluster](../failover-clustering/cluster-operating-system-rolling-upgrade.md)

- **Conversion de licence.** Convertissez une édition particulière de la version vers une autre édition de la même version en une seule étape à l’aide d’une simple commande et de la clé de licence appropriée. Nous appelons cette « conversion de licence ». Par exemple, si votre serveur exécute Windows Server 2016 Standard, vous pouvez effectuer une conversion vers Windows Server 2016 Datacenter.

## <a name="which-version-of-windows-server-should-i-upgrade-to"></a>À quelle version de Windows Server dois-je effectuer une mise à niveau ?

Nous vous recommandons d’effectuer la mise à niveau vers la dernière version de Windows Server : Windows Server 2019. L’exécution de la dernière version de Windows Server vous permet d’utiliser les fonctionnalités les plus récentes, notamment les dernières fonctionnalités de sécurité, et offre des performances optimales.

Toutefois, nous savons que cela n’est pas toujours possible. Vous pouvez utiliser le diagramme suivant pour déterminer la version de Windows Server vers laquelle vous pouvez effectuer la mise à niveau, en fonction de la version que vous utilisez actuellement :

![Chemins de mise à niveau sur place disponibles](media/upgrade-paths.png)

Windows Server peut généralement être mis à niveau par le biais d’au moins une version de et parfois même de deux versions. Par exemple, Windows Server 2012 R2 et Windows Server 2016 peuvent être mis à niveau sur place vers Windows Server 2019.

Vous pouvez également effectuer la mise à niveau d’une version d’évaluation du système d’exploitation vers une version commercialisée, d’une version commercialisée plus ancienne vers une version plus récente, ou, dans certains cas, d’une édition avec licence en volume du système d’exploitation vers une édition commerciale ordinaire. Pour plus d’informations sur les options de mise à niveau autres que la mise à niveau sur place, consultez [options de mise à niveau et de conversion pour Windows Server](../get-started/supported-upgrade-paths.md).
