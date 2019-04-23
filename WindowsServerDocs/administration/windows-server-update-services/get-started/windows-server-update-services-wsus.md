---
title: Bien démarrer avec Windows Server Update Services (WSUS)
description: Rubrique de Windows Server Update Service (WSUS) - une vue d’ensemble du rôle serveur et ses applications pratiques
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7a6c64e0a4321553162b426e3d6857ff6ac3581c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830260"
---
# <a name="windows-server-update-services-wsus"></a>Windows Server Update Services (WSUS)

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

WSUS (Windows Server Update Services) permet aux administrateurs informatiques de déployer les dernières mises à jour de produits Microsoft. Vous pouvez utiliser WSUS pour gérer entièrement la distribution des mises à jour publiées via Microsoft Update sur les ordinateurs sur votre réseau. Cette rubrique fournit une vue d’ensemble de ce rôle de serveur et des informations supplémentaires sur la façon de déployer et d’assurer la maintenance des services WSUS.

## <a name="wsus-server-role-description"></a>Description du rôle serveur WSUS
Un serveur WSUS fournit des fonctionnalités que vous pouvez utiliser pour gérer et distribuer des mises à jour via une console de gestion. Un serveur WSUS peut également être la source de mise à jour pour d’autres serveurs WSUS au sein de l’organisation. Le serveur WSUS qui agit en tant que source des mises à jour est appelé serveur en amont. Dans une implémentation WSUS, au moins un serveur WSUS sur votre réseau doit être en mesure de se connecter à Microsoft Update pour obtenir des informations de mise à jour disponible. En tant qu’administrateur, vous pouvez déterminer - en fonction de sécurité réseau et de configuration - nombre d’autres serveurs WSUS se connecter directement à Microsoft Update.

### <a name="practical-applications"></a>Cas pratiques
La gestion des mises à jour est le processus de contrôle du déploiement et de la gestion des versions de logiciels intermédiaires dans les environnements de production. Elle vous aide à assurer l’efficacité des opérations, à surmonter les failles de sécurité et à maintenir la stabilité de votre environnement de production. Si votre organisation ne peut pas déterminer et assurer un niveau connu de confiance dans ses systèmes d’exploitation et logiciels d’application, elle peut être confrontée à des failles de sécurité qui, si elles sont exploitées, peuvent aboutir à une perte de revenu et de propriété intellectuelle. Pour réduire au maximum cette menace, vous devez disposer de systèmes correctement configurés, utiliser les derniers logiciels et installer les mises à jour logicielles recommandées.

Les principaux scénarios dans lesquels WSUS ajoute une valeur commerciale sont les suivants :

-   gestion des mises à jour centralisée ;

-   automatisation de la gestion des mises à jour.

### <a name="new-and-changed-functionality"></a>Fonctionnalités nouvelles et modifiées

> [!NOTE]
> Mise à niveau à partir de n’importe quelle version de Windows Server qui prend en charge WSUS 3.2 vers Windows Server 2012 R2 nécessite tout d’abord désinstaller WSUS 3.2.
> 
> Dans Windows Server 2012, la mise à niveau à partir de n’importe quelle version de Windows Server avec WSUS 3.2 est bloquée pendant le processus d’installation si WSUS 3.2 est détecté. Dans ce cas, vous devez d’abord désinstaller Windows Server Update Services avant la mise à niveau votre serveur.
> 
> Toutefois, en raison de modifications dans cette version de Windows Server et Windows Server 2012 R2, lors de la mise à niveau à partir de n’importe quelle version de Windows Server et WSUS 3.2, l’installation n’est pas bloquée. Désinstaller WSUS 3.2 avant d’effectuer une mise à niveau de Windows Server 2012 R2 provoquera la publication des tâches d’installation de WSUS dans Windows Server 2012 R2 à échouer. Dans ce cas, la seule mesure corrective connue consiste à formater le disque dur et la réinstallation de Windows Server.

Windows Server Update Services est un rôle serveur intégré qui inclut les améliorations suivantes :

-   Il peut être ajouté et supprimé à l’aide du Gestionnaire de serveur.

-   Inclut des applets de commande Windows PowerShell pour gérer les tâches administratives plus importantes de WSUS

-   Il ajoute la fonctionnalité de hachage SHA256 pour renforcer la sécurité.

-   Fournit la séparation du client et serveur : versions de Windows Update Agent (WUA) peuvent être livrées indépendamment de WSUS

### <a name="using-windows-powershell-to-manage-wsus"></a>Utilisation de Windows PowerShell pour gérer WSUS
Pour que les administrateurs système puissent automatiser leurs opérations, ils ont besoin de couverture par le biais d’une automatisation de ligne de commande. L’objectif principal est de faciliter l’administration WSUS en permettant aux administrateurs système d’automatiser leurs opérations quotidiennes.

**Quels avantages cette modification procure-t-elle ?**

En exposant des opérations WSUS principales via Windows PowerShell, les administrateurs système peuvent augmenter la productivité, réduire la courbe d’apprentissage pour les nouveaux outils et diminuer le nombre d’erreurs causées par des attentes déçues qui résultent d’un manque de cohérence entre des opérations similaires.

**En quoi le fonctionnement est-il différent ?**

Dans les versions antérieures du système d’exploitation Windows Server, il n’existait pas d’applet de commande Windows PowerShell et l’automatisation de la gestion des mises à jour s’avérait difficile. Les applets de commande Windows PowerShell pour les opérations WSUS ajoutent flexibilité et souplesse pour l’administrateur système.

## <a name="in-this-collection"></a>Dans cette collection
Les guides suivants pour la planification, déploiement et gestion de WSUS sont dans cette collection :

-   [Déployer Windows Server Update Services](../deploy/deploy-windows-server-update-services.md)

-   [Gérer les mises à jour à l’aide de Windows Server Update Services](../manage/update-management-with-windows-server-update-services.md)


