---
title: Migrer vers MultiPoint services dans Windows Server 2016
description: Découvrez comment migrer à partir d’une version précédente de MultiPoint services
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16c217ad-700a-48a3-8398-4a7f7e9edb52
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 297de9ee2450856e24b9196a8bfb312991657e6d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389041"
---
# <a name="multipoint-services-migration-in-windows-server-2016"></a>Migration de MultiPoint services dans Windows Server 2016
>S'applique à : Windows Server 2016

Vous pouvez migrer à partir d’une version précédente de Windows Server 2016 MultiPoint services vers la version RTM de MultiPoint services. Les informations suivantes fournissent des informations sur la préparation et des étapes de migration et de vérification.

La documentation et les outils de migration facilitent la migration des paramètres et des données des rôles serveur d’un serveur existant vers un serveur de destination qui exécute Windows Server 2016. En utilisant les procédures décrites dans ce guide, vous pouvez simplifier le processus de migration, réduire la durée de la migration, accroître la précision du processus de migration et favoriser l’élimination des conflits qui, sans le recours à ces outils, pourraient se produire pendant le processus de migration. 

## <a name="what-to-know-before-you-begin"></a>Ce que vous devez savoir avant de commencer
Avant de commencer le processus de migration, veuillez noter les points suivants :

- Le processus de migration ne collecte ni n’enregistre automatiquement les paramètres des applications sur le rôle MultiPoint services. Vous devez créer un plan de migration personnalisé pour toutes les applications que vous souhaitez migrer. Cela est également vrai lorsque vous utilisez la fonctionnalité de bureaux virtuels dans MultiPoint services.
- Ce guide ne fournit pas de conseils pour déplacer des données enregistrées dans des dossiers utilisateur ou partagés sur le serveur MultiPoint. Cela s’applique aux stations standard et aux stations de bureaux virtuels.
- Ce guide ne contient pas d’instructions sur la migration lorsque le serveur source exécute plusieurs rôles. Si votre serveur exécute plusieurs rôles, vous devez concevoir une procédure de migration personnalisée spécifique à votre environnement de serveur, en fonction des informations fournies dans les guides de migration des rôles.
- Ce guide ne contient pas d’informations sur la migration des licences d’accès client Services Bureau à distance. Pour plus d’informations, consultez [migrer des licences d’accès Client Services Bureau à distance (CAL des services Bureau à distance)](https://technet.microsoft.com/library/dd851844.aspx).

## <a name="supported-migration-scenarios-for-multipoint-services-in-windows-server-2016"></a>Scénarios de migration pris en charge pour MultiPoint services dans Windows Server 2016
Les services de rôle de service MultiPoint sont disponibles dans Windows Server 2016 standard et Datacenter. Ce guide de migration décrit comment migrer les services de rôle multipoint services à partir d’un serveur source exécutant Windows Server 2016 vers un serveur de destination exécutant la même version.

## <a name="scenarios-that-are-not-supported"></a>Scénarios non pris en charge

Les scénarios de migration suivants ne sont pas pris en charge :

- Migration ou mise à niveau à partir de Windows MultiPoint Server 2012 et 2011.
- Migration d’un serveur source vers un serveur de destination qui exécute sur un système d’exploitation avec une autre langue d’interface utilisateur système installée.
- Migration du service de rôle MultiPoint services à partir des serveurs physiques vers des machines virtuelles.
- Migration des applications ou des paramètres d’application à partir du serveur MultiPoint.

## <a name="the-impact-of-migration-on-multipoint-services"></a>Impact de la migration sur MultiPoint services
N’oubliez pas que le rôle MultiPoint services n’est pas disponible pendant la migration. Planifiez la migration des données de manière à ce qu’elle se produise pendant les heures creuses afin de réduire au minimum le temps d’indisponibilité et de réduire l’impact sur les utilisateurs. Informez les utilisateurs que les ressources ne seront pas disponibles pendant cette période.

## <a name="migration-information-and-steps"></a>Informations et étapes relatives à la migration
Utilisez les informations suivantes pour planifier et effectuer la migration de MultiPoint services :

- [Rassemblez les informations dont vous avez besoin pour la migration.](multipoint-services-migration-preparation.md)
- [Migrez le service de rôle MultiPoint services.](multipoint-services-migration-steps.md)
- [Valider la migration et effectuer les tâches de nettoyage après la migration](multipoint-services-post-migration-steps.md)