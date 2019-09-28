---
title: Prise en main de Windows Server Update Services (WSUS)
description: Rubrique Windows Server Update Service (WSUS)-vue d’ensemble du rôle de serveur et de ses applications pratiques
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
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361648"
---
# <a name="windows-server-update-services-wsus"></a>Windows Server Update Services (WSUS)

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

WSUS (Windows Server Update Services) permet aux administrateurs informatiques de déployer les dernières mises à jour de produits Microsoft. Vous pouvez utiliser WSUS pour gérer entièrement la distribution des mises à jour publiées par le biais de Microsoft Update vers les ordinateurs de votre réseau. Cette rubrique fournit une vue d’ensemble de ce rôle de serveur et des informations supplémentaires sur la façon de déployer et d’assurer la maintenance des services WSUS.

## <a name="wsus-server-role-description"></a>Description du rôle serveur WSUS
Un serveur WSUS fournit des fonctionnalités que vous pouvez utiliser pour gérer et distribuer des mises à jour par le biais d’une console de gestion. Un serveur WSUS peut également être la source de mise à jour d’autres serveurs WSUS au sein de l’organisation. Le serveur WSUS qui agit en tant que source des mises à jour est appelé serveur en amont. Dans une implémentation WSUS, au moins un serveur WSUS sur votre réseau doit être en mesure de se connecter à Microsoft Update pour obtenir les informations de mise à jour disponibles. En tant qu’administrateur, vous pouvez déterminer, en fonction de la sécurité et de la configuration du réseau, le nombre d’autres serveurs WSUS qui se connectent directement à Microsoft Update.

### <a name="practical-applications"></a>Cas pratiques
La gestion des mises à jour est le processus de contrôle du déploiement et de la gestion des versions de logiciels intermédiaires dans les environnements de production. Elle vous aide à assurer l’efficacité des opérations, à surmonter les failles de sécurité et à maintenir la stabilité de votre environnement de production. Si votre organisation ne peut pas déterminer et assurer un niveau connu de confiance dans ses systèmes d’exploitation et logiciels d’application, elle peut être confrontée à des failles de sécurité qui, si elles sont exploitées, peuvent aboutir à une perte de revenu et de propriété intellectuelle. Pour réduire au maximum cette menace, vous devez disposer de systèmes correctement configurés, utiliser les derniers logiciels et installer les mises à jour logicielles recommandées.

Les principaux scénarios dans lesquels WSUS ajoute une valeur commerciale sont les suivants :

-   gestion des mises à jour centralisée ;

-   automatisation de la gestion des mises à jour.

### <a name="new-and-changed-functionality"></a>Fonctionnalités nouvelles et modifiées

> [!NOTE]
> Pour effectuer une mise à niveau à partir de n’importe quelle version de Windows Server qui prend en charge WSUS 3,2 vers Windows Server 2012 R2, vous devez d’abord désinstaller WSUS 3,2.
> 
> Dans Windows Server 2012, la mise à niveau à partir de n’importe quelle version de Windows Server avec WSUS 3,2 est bloquée pendant le processus d’installation si WSUS 3,2 est détecté. Dans ce cas, vous êtes invité à désinstaller Windows Server Update Services avant de mettre à niveau votre serveur.
> 
> Toutefois, en raison des modifications apportées à cette version de Windows Server et de Windows Server 2012 R2, lors de la mise à niveau à partir de n’importe quelle version de Windows Server et de WSUS 3,2, l’installation n’est pas bloquée. Si vous ne désinstallez pas WSUS 3,2 avant d’effectuer une mise à niveau vers Windows Server 2012 R2, les tâches de publication de serveur WSUS dans Windows Server 2012 R2 échoueront. Dans ce cas, la seule mesure corrective connue consiste à formater le disque dur et à réinstaller Windows Server.

Windows Server Update Services est un rôle serveur intégré qui inclut les améliorations suivantes :

-   Il peut être ajouté et supprimé à l’aide du Gestionnaire de serveur.

-   Comprend des applets de commande Windows PowerShell pour gérer les tâches administratives les plus importantes dans WSUS

-   Il ajoute la fonctionnalité de hachage SHA256 pour renforcer la sécurité.

-   Fournit la séparation du client et du serveur : les versions de l’agent de Windows Update (WUA) peuvent être livrées indépendamment de WSUS

### <a name="using-windows-powershell-to-manage-wsus"></a>Utilisation de Windows PowerShell pour gérer WSUS
Pour que les administrateurs système puissent automatiser leurs opérations, ils ont besoin de couverture par le biais d’une automatisation de ligne de commande. L’objectif principal est de faciliter l’administration WSUS en permettant aux administrateurs système d’automatiser leurs opérations quotidiennes.

**Quels avantages cette modification procure-t-elle ?**

En exposant des opérations WSUS principales via Windows PowerShell, les administrateurs système peuvent augmenter la productivité, réduire la courbe d’apprentissage pour les nouveaux outils et diminuer le nombre d’erreurs causées par des attentes déçues qui résultent d’un manque de cohérence entre des opérations similaires.

**En quoi le fonctionnement est-il différent ?**

Dans les versions antérieures du système d’exploitation Windows Server, il n’existait pas d’applet de commande Windows PowerShell et l’automatisation de la gestion des mises à jour s’avérait difficile. Les applets de commande Windows PowerShell pour les opérations WSUS ajoutent flexibilité et souplesse pour l’administrateur système.

## <a name="in-this-collection"></a>Dans cette collection
Les guides suivants pour la planification, le déploiement et la gestion de WSUS se trouvent dans ce regroupement :

-   [Déployer Windows Server Update Services](../deploy/deploy-windows-server-update-services.md)

-   [Gérer les mises à jour à l’aide de Windows Server Update Services](../manage/update-management-with-windows-server-update-services.md)


