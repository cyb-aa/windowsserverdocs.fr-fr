---
title: Vue d’ensemble des mises à niveau de Windows Server | Microsoft Docs
description: Découvrez des informations générales sur la mise à niveau de Windows Server ainsi que les éléments à prendre en compte avant de procéder à la mise à niveau réelle.
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/10/2019
ms.openlocfilehash: 6f57e52657ca3c80c92d56c54ea87e43aabd1e99
ms.sourcegitcommit: 27f0caf74e88781054250455c3c1adf06deb6234
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71124789"
---
# <a name="overview-about-windows-server-upgrades"></a>Vue d’ensemble des mises à niveau de Windows Server

Le processus de mise à niveau de Windows Server peut varier grandement selon le système d’exploitation dont vous partez et de la méthode choisie. Nous utilisons les termes suivants pour distinguer entre les différentes actions, chacune pouvant être impliquées dans un nouveau déploiement de Windows Server.

- **Mise à niveau.** Également appelé « mise à niveau sur place ». Vous passez d’une version antérieure du système d’exploitation à une version plus récente, tout en restant sur le même matériel physique. **C’est cette méthode que nous allons aborder dans cette section.**

    >[!Important]
    >Les mises à niveau sur place peuvent également être prises en charge par des entreprises offrant un cloud public ou privé ; vous devez cependant vérifier les informations détaillées auprès de votre fournisseur cloud. En outre, vous ne pourrez pas effectuer une mise à niveau sur place sur un serveur Windows Server configuré pour le **Démarrage à partir d’un disque dur virtuel**.

- **Installation.** Également appelée « nouvelle installation ». Vous passez d’une version antérieure du système d’exploitation à une version plus récente, en supprimant l’ancien système d’exploitation.

- **Migration.** Vous passez d’une version plus ancienne du système d’exploitation à une version plus récente du système d’exploitation en effectuant un transfert vers un autre ensemble de matériels ou de machines virtuelles.

- **Mise à niveau propagée du système d’exploitation de cluster.** Vous mettez à niveau le système d’exploitation des nœuds de votre cluster sans arrêter les charges de travail Hyper-V ou celles du serveur de fichiers de scale-out. Cette fonctionnalité élimine les temps d’arrêt susceptibles d’affecter les contrats de niveau de service. Pour plus d’informations, consultez [Mise à niveau propagée du système d’exploitation de cluster](../failover-clustering/cluster-operating-system-rolling-upgrade.md).

- **Conversion de licence.** Convertissez une édition particulière de la version vers une autre édition de la même version en une seule étape à l’aide d’une simple commande et de la clé de licence appropriée. Cette opération est appelée « conversion de licence ». Par exemple, si votre serveur exécute Windows Server 2016 Standard, vous pouvez effectuer une conversion vers Windows Server 2016 Datacenter.

## <a name="which-version-of-windows-server-should-i-upgrade-to"></a>Vers quelle version de Windows Server dois-je effectuer une mise à niveau ?

Nous vous recommandons d’effectuer la mise à niveau vers la dernière version de Windows Server : Windows Server 2019. Exécuter la dernière version de Windows Server vous permet d’utiliser les fonctionnalités les plus récentes, notamment les dernières fonctionnalités de sécurité, et de bénéficier des meilleures performances.

Nous savons cependant que ce n’est pas toujours possible. Vous pouvez utiliser le diagramme suivant pour déterminer vers quelle version de Windows Server vous pouvez effectuer une mise à niveau, en fonction de la version que vous utilisez actuellement :

![Chemins de mise à niveau sur place disponibles](media/upgrade-paths.png)

Windows Server peut généralement être mis à niveau via au moins une version, et parfois même deux versions. Par exemple, Windows Server 2012 R2 et Windows Server 2016 peuvent être mis à niveau sur place vers Windows Server 2019.

Vous pouvez aussi effectuer une mise à niveau à partir d’une version d’évaluation du système d’exploitation vers une version commercialisée, d’une version commercialisée plus ancienne vers une version plus récente ou, dans certains cas, d’une édition de licence en volume du système d’exploitation vers une version commercialisée ordinaire. Pour plus d’informations sur les options de mise à niveau autres que la mise à niveau sur place, consultez [Options de mise à niveau et de conversion pour Windows Server](../get-started/supported-upgrade-paths.md).
