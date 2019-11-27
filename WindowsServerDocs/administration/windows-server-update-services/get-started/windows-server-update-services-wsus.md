---
title: Prise en main de Windows Server Update Services (WSUS)
description: Rubrique Windows Server Update Services (WSUS) – Présentation du rôle serveur et de ses applications pratiques
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09ec5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 5/22/2017
ms.openlocfilehash: 89247f91f616233fc6e4967a0457ff34fac221da
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361648"
---
# <a name="windows-server-update-services-wsus"></a>Windows Server Update Services (WSUS)

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

WSUS (Windows Server Update Services) permet aux administrateurs informatiques de déployer les dernières mises à jour de produits Microsoft. Grâce à WSUS, vous pouvez gérer entièrement la distribution des mises à jour publiées par Microsoft Update sur les ordinateurs de votre réseau. Cette rubrique fournit une vue d’ensemble de ce rôle de serveur et des informations supplémentaires sur la façon de déployer et d’assurer la maintenance des services WSUS.

## <a name="wsus-server-role-description"></a>Description du rôle serveur WSUS
Un serveur WSUS vous fournit les fonctionnalités dont vous avez besoin pour gérer et distribuer des mises à jour par le biais d’une console de gestion. Un serveur WSUS peut également être la source de mises à jour d’autres serveurs WSUS de l’organisation. Le serveur WSUS qui agit en tant que source des mises à jour est appelé serveur en amont. Dans une implémentation WSUS, au moins un serveur WSUS de votre réseau doit se connecter à Microsoft Update pour obtenir les informations sur les mises à jour disponibles. En tant qu’administrateur, vous pouvez déterminer le nombre d’autres serveurs WSUS se connectant directement à Microsoft Update en fonction de la configuration et de la sécurité du réseau.

### <a name="practical-applications"></a>Cas pratiques
La gestion des mises à jour est le processus de contrôle du déploiement et de la gestion des versions de logiciels intermédiaires dans les environnements de production. Elle vous aide à assurer l’efficacité des opérations, à surmonter les failles de sécurité et à maintenir la stabilité de votre environnement de production. Si votre organisation ne peut pas déterminer et assurer un niveau connu de confiance dans ses systèmes d’exploitation et logiciels d’application, elle peut être confrontée à des failles de sécurité qui, si elles sont exploitées, peuvent aboutir à une perte de revenu et de propriété intellectuelle. Pour réduire au maximum cette menace, vous devez disposer de systèmes correctement configurés, utiliser les derniers logiciels et installer les mises à jour logicielles recommandées.

Les principaux scénarios dans lesquels WSUS ajoute une valeur commerciale sont les suivants :

-   gestion des mises à jour centralisée ;

-   automatisation de la gestion des mises à jour.

### <a name="new-and-changed-functionality"></a>Fonctionnalités nouvelles et modifiées

> [!NOTE]
> La mise à niveau à partir de n’importe quelle version de Windows Server qui prend en charge WSUS 3.2 vers Windows Server 2012 R2 nécessite tout d’abord la désinstallation de WSUS 3.2.
> 
> Dans Windows Server 2012, la mise à niveau à partir de n’importe quelle version de Windows Server avec WSUS 3.2 est bloquée pendant le processus d’installation si WSUS 3.2 est détecté. Dans ce cas, vous serez invité à désinstaller Windows Server Update Services avant de mettre à niveau le serveur.
> 
> Cependant, en raison des modifications apportées à la version de Windows Server et de Windows Server 2012 R2, l’installation n’est pas bloquée lors de la mise à niveau à partir d’une version de Windows Server avec WSUS 3.2. Si vous ne désinstallez pas WSUS 3.2 avant d’effectuer une mise à niveau de Windows Server 2012 R2 , les tâches post-installation pour WSUS dans Windows Server 2012 R2 échoueront. Dans ce cas, la seule solution consiste à formater le disque dur et à réinstaller Windows Server.

Windows Server Update Services est un rôle serveur intégré qui inclut les améliorations suivantes :

-   Il peut être ajouté et supprimé à l’aide du Gestionnaire de serveur.

-   Il comprend des applets de commande Windows PowerShell permettant de gérer les tâches administratives les plus importantes de WSUS.

-   Il ajoute la fonctionnalité de hachage SHA256 pour renforcer la sécurité.

-   Il assure la séparation du client et du serveur : les versions de l’agent de mise à jour automatique Windows Update (WUA) peuvent être livrées indépendamment de WSUS

### <a name="using-windows-powershell-to-manage-wsus"></a>Utilisation de Windows PowerShell pour gérer WSUS
Pour que les administrateurs système puissent automatiser leurs opérations, ils ont besoin de couverture par le biais d’une automatisation de ligne de commande. L’objectif principal est de faciliter l’administration WSUS en permettant aux administrateurs système d’automatiser leurs opérations quotidiennes.

**Quels avantages cette modification procure-t-elle ?**

En exposant des opérations WSUS principales via Windows PowerShell, les administrateurs système peuvent augmenter la productivité, réduire la courbe d’apprentissage pour les nouveaux outils et diminuer le nombre d’erreurs causées par des attentes déçues qui résultent d’un manque de cohérence entre des opérations similaires.

**En quoi le fonctionnement est-il différent ?**

Dans les versions antérieures du système d’exploitation Windows Server, il n’existait pas d’applet de commande Windows PowerShell et l’automatisation de la gestion des mises à jour s’avérait difficile. Les applets de commande Windows PowerShell pour les opérations WSUS ajoutent flexibilité et souplesse pour l’administrateur système.

## <a name="in-this-collection"></a>Contenu de cette collection
Les guides suivants portant sur la planification, le déploiement et la gestion de WSUS font partie de cette collection :

-   [Déployer Windows Server Update Services](../deploy/deploy-windows-server-update-services.md)

-   [Gérer les mises à jour à l’aide de Windows Server Update Services](../manage/update-management-with-windows-server-update-services.md)


