---
title: Migrer vers les Services MultiPoint dans Windows Server 2016
description: Apprenez à migrer à partir d’une version précédente de MultiPoint Services
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16c217ad-700a-48a3-8398-4a7f7e9edb52
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 24c35c31bf920c41bafa16901ee30a023565dad8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861200"
---
# <a name="multipoint-services-migration-in-windows-server-2016"></a>Migration de multiPoint Services dans Windows Server 2016
>S'applique à : Windows Server 2016

Vous pouvez migrer à partir d’une version précédente de Windows Server 2016 MultiPoint Services vers la version RTM de MultiPoint Services. Les informations suivantes fournissent des informations et de migration et de vérification des étapes préparation.

Outils et documentation sur la migration facilitent la migration des paramètres de rôle de serveur et les données d’un serveur existant vers un serveur de destination qui exécute Windows Server 2016. En utilisant les procédures décrites dans ce guide, vous pouvez simplifier le processus de migration, réduire la durée de la migration, accroître la précision du processus de migration et favoriser l’élimination des conflits qui, sans le recours à ces outils, pourraient se produire pendant le processus de migration. 

## <a name="what-to-know-before-you-begin"></a>Éléments à connaître avant de commencer
Avant de commencer le processus de migration, notez les points suivants :

- Le processus de migration ne pas automatiquement rassembler et enregistrer les paramètres pour les applications sur le rôle de MultiPoint Services. Vous devez créer un plan de migration personnalisé pour toutes les applications que vous souhaitez migrer. Cela est également vrai lorsque vous utilisez la fonctionnalité de bureaux virtuels dans MultiPoint Services.
- Ce guide ne fournit pas de conseils pour déplacer des données enregistrement dans utilisateur ou dossiers partagés sur le serveur MultiPoint. Cela s’applique aux stations régulières et de bureau virtuel stations.
- Ce guide ne contient pas d’instructions sur la migration lorsque le serveur source exécute plusieurs rôles. Si votre serveur exécute plusieurs rôles, vous devez concevoir une procédure de migration personnalisée spécifique à votre environnement de serveur, en fonction des informations fournies dans les guides de migration de rôles.
- Ce guide ne contient pas d’informations pour la migration des Services Bureau à distance licences d’accès client. Pour obtenir ces informations, consultez [migrer distant Desktop Services licences d’accès Client (CAL RDS)](https://technet.microsoft.com/library/dd851844.aspx).

## <a name="supported-migration-scenarios-for-multipoint-services-in-windows-server-2016"></a>Scénarios de migration pris en charge pour les Services MultiPoint dans Windows Server 2016
Les services de rôle Service de MultiPoint est disponible dans Windows Server 2016 Standard et centre de données. Ce guide de migration décrit comment migrer les services de rôle de Multipoint Services à partir d’un serveur source exécutant Windows Server 2016 vers un serveur de destination exécutant la même version.

## <a name="scenarios-that-are-not-supported"></a>Scénarios qui ne sont pas pris en charge

Les scénarios de migration suivants ne sont pas pris en charge :

- Migration ou la mise à niveau à partir de Windows MultiPoint Server 2012 et 2011.
- Migration à partir d’un serveur source vers un serveur de destination qui s’exécute sur le système d’exploitation avec un autre système de langue d’interface utilisateur.
- Migration du service de rôle de MultiPoint Services à partir de serveurs physiques à des machines virtuelles.
- Migration des applications ou des paramètres d’application à partir du serveur MultiPoint.

## <a name="the-impact-of-migration-on-multipoint-services"></a>L’impact de la migration sur MultiPoint Services
N’oubliez pas que le rôle de MultiPoint Services ne sera pas disponible pendant la migration. Planifiez la migration des données de manière à ce qu’elle se produise pendant les heures creuses afin de réduire au minimum le temps d’indisponibilité et de réduire l’impact sur les utilisateurs. Informez les utilisateurs que les ressources ne seront pas disponibles pendant cette période.

## <a name="migration-information-and-steps"></a>Étapes et des informations sur la migration
Utilisez les informations suivantes pour planifier et effectuer la migration de MultiPoint Services :

- [Collectez les informations que vous avez besoin pour la migration.](multipoint-services-migration-preparation.md)
- [Migrer le service de rôle de MultiPoint Services.](multipoint-services-migration-steps.md)
- [Valider la migration et effectuer les tâches de nettoyage après la migration](multipoint-services-post-migration-steps.md)