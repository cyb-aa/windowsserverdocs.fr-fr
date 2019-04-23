---
title: Stratégies de restriction logicielle
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c0befad-07c3-4262-b418-372d01850305
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: ab44013b947d33adc12c54b527415bf16c46a4c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875820"
---
# <a name="software-restriction-policies"></a>Stratégies de restriction logicielle

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique destinée aux professionnels de l’informatique décrit les stratégies de Restriction logicielle (SRP) dans Windows Server 2012 et Windows 8 et fournit des liens vers des informations techniques sur Windows Server 2003 à compter de stratégies de restriction logicielle.

Pour des conseils de dépannage et des procédures, consultez [administrer les stratégies de Restriction logicielle](administer-software-restriction-policies.md) et [résoudre les problèmes des stratégies de Restriction logicielle](troubleshoot-software-restriction-policies.md).

## <a name="BKMK_OVER"></a>Description des stratégies de Restriction logicielle
La fonctionnalité Stratégies de restriction logicielle est une fonctionnalité fondée sur les stratégies de groupe qui identifie les programmes logiciels s’exécutant sur les ordinateurs d’un domaine et qui contrôle la capacité de ces programmes à s’exécuter. Les stratégies de restriction logicielle s’inscrivent dans la stratégie de gestion et de sécurité Microsoft conçue pour aider les entreprises à accroître la fiabilité, l’intégrité et la facilité de gestion de leurs ordinateurs.

Les stratégies de restriction logicielle peuvent aussi contribuer à créer une configuration fortement restreinte pour vos ordinateurs, dans laquelle seule l’exécution d’applications clairement identifiées est autorisée. Les stratégies de restriction logicielle sont intégrées à Microsoft Active Directory et à la stratégie de groupe. Vous avez également la possibilité de créer des stratégies de restriction logicielle sur des ordinateurs autonomes. Les stratégies de restriction logicielle sont des stratégies d’approbation, c’est-à-dire des règles définies par un administrateur pour limiter des scripts et d’autres formes de code qui n’apparaissent pas entièrement fiables depuis l’exécution.

Vous pouvez définir ces stratégies au moyen de l’extension des stratégies de restriction logicielle disponible dans l’Éditeur d’objets de stratégie de groupe ou par le biais du composant logiciel enfichable Stratégie de sécurité locale de la console MMC (Microsoft Management Console).

Pour obtenir des informations détaillées sur les stratégies de restriction logicielle, voir la [Vue d’ensemble technique des stratégies de restriction logicielle](software-restriction-policies-technical-overview.md).

## <a name="BKMK_APP"></a>Applications pratiques
Les administrateurs peuvent faire appel aux stratégies de restriction logicielle pour la réalisation des tâches suivantes :

-   Définir ce qu’est un code de confiance

-   Concevoir une stratégie de groupe souple réglementant les scripts, les fichiers exécutables et les contrôles ActiveX

Les stratégies de restriction logicielle sont mises en place par le système d’exploitation et par des applications (notamment les applications d’exécution de scripts) conformes aux stratégies de restriction logicielle.

Les administrateurs peuvent en particulier faire appel aux stratégies de restriction logicielle pour la réalisation des tâches suivantes :

-   Spécifier quels logiciels (fichiers exécutables) peuvent être exécutés sur les clients

-   Empêcher les utilisateurs d’exécuter des programmes spécifiques sur des ordinateurs partagés

-   Préciser les personnes autorisées à ajouter des éditeurs approuvés sur les clients

-   Définir la portée des stratégies de restriction logicielle (préciser si elles affectent tous les utilisateurs ou un sous-ensemble d’utilisateurs sur les clients)

-   Empêcher l’exécution des fichiers exécutables sur l’ordinateur local, l’unité d’organisation, le site ou le domaine. Ceci peut s’avérer judicieux dans des cas où vous n’avez pas recours aux stratégies de restriction logicielle pour résoudre d’éventuels problèmes liés à des utilisateurs malveillants.

## <a name="BKMK_NEW"></a>Fonctionnalités nouvelles et modifiées
Aucune modification n’a été apportée aux fonctionnalités des stratégies de restriction logicielle.

## <a name="BKMK_DEP"></a>Fonctionnalité supprimée ou déconseillée
Il n’existe aucune fonctionnalité supprimée ou déconseillée dans le cadre des stratégies de restriction logicielle.

## <a name="BKMK_SOFT"></a>Configuration logicielle requise
Vous pouvez accéder à l’extension des stratégies de restriction logicielle disponible dans l’Éditeur d’objets de stratégie de groupe via la console MMC.

Pour créer et maintenir des stratégies de restriction logicielle sur l’ordinateur local, vous devez disposer des fonctionnalités suivantes :

-   Éditeur d’objets de stratégie de groupe

-   Windows Installer

-   Authenticode et WinVerifyTrust

Si la structure que vous concevez nécessite un déploiement de ces stratégies dans le domaine, vous aurez besoin des fonctionnalités suivantes en plus des éléments de la liste ci-dessus :

-   Services de domaine Active Directory

-   Stratégie de groupe

## <a name="BKMK_INSTALL"></a>Informations de gestionnaire de serveur
Les stratégies de restriction logicielle constituent une extension de l’Éditeur d’objets de stratégie de groupe et ne sont pas installées par le biais de la fonctionnalité d’ajout de rôles et de fonctionnalités du Gestionnaire de serveur.

## <a name="BKMK_LINKS"></a>Voir aussi
Le tableau qui suit propose des liens menant à des ressources appropriées permettant de comprendre et d’exploiter les stratégies de restriction logicielle.

|Type de contenu|Références|
|--------|-------|
|**Évaluation du produit**|[Verrouillage d’applications avec les stratégies de Restriction logicielle](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|
|**Planification**|[Software Restriction Policies Technical Overview](software-restriction-policies-technical-overview.md) (Windows Server 2012)<br /><br />[Informations techniques de référence sur les stratégies de restriction logicielle](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx) (Windows Server 2003)|
|**Déploiement**|Aucune ressource disponible.|
|**Opérations**|[Administrer les stratégies de Restriction logicielle](administer-software-restriction-policies.md) (Windows Server 2012)<br /><br />[Aide produit pour les stratégies de restriction logicielle](https://technet.microsoft.com/library/cc779607(v=WS.10).aspx) (Windows Server 2003)|
|**Résolution des problèmes**|[Résoudre les problèmes de stratégies de Restriction logicielle](troubleshoot-software-restriction-policies.md) (Windows Server 2012)<br /><br />[Résolution des problèmes liés aux stratégies de restriction logicielle](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx) (Windows Server 2003)|
|**Sécurité**|[Menaces et contre-mesures pour les stratégies de restriction logicielle](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx) (Windows Server 2008)<br /><br />[Menaces et contre-mesures pour les stratégies de restriction logicielle](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx) (Windows Server 2008 R2)|
|**Outils et paramètres**|[Outils et paramètres des stratégies de restriction logicielle](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx) (Windows Server 2003)|
|**Ressources de la communauté**|[Verrouillage d’applications avec les stratégies de Restriction logicielle](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|



