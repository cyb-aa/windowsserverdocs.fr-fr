---
title: "Stratégies de Restriction logicielle"
description: "Sécurité de Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="software-restriction-policies"></a>Stratégies de Restriction logicielle

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique destinée aux professionnels de l’informatique décrit les stratégies de Restriction logicielle (SRP) dans Windows Server2012 et Windows8 et fournit des liens vers des informations techniques sur Windows Server2003 à compter de stratégies de restriction logicielle.

Pour des procédures et des conseils de dépannage, consultez [administrer les stratégies de Restriction logicielle](administer-software-restriction-policies.md) et [résoudre les problèmes de stratégies de Restriction logicielle](troubleshoot-software-restriction-policies.md).

## <a name="BKMK_OVER"></a>Description des stratégies de Restriction logicielle
Stratégies de Restriction logicielle (SRP) est la fonctionnalité de stratégie de groupe qui identifie les programmes logiciels s’exécutant sur des ordinateurs dans un domaine et contrôle la capacité de ces programmes à exécuter. Stratégies de restriction logicielle font partie de la sécurité de Microsoft et de la stratégie de gestion pour aider les entreprises à accroître la fiabilité, d’intégrité et la facilité de gestion de leurs ordinateurs.

Vous pouvez également utiliser des stratégies de restriction logicielle pour créer une configuration fortement restreinte pour vos ordinateurs, dans laquelle vous uniquement spécifiquement les applications identifiées est autorisée à s’exécuter. Stratégies de restriction logicielle sont intégrées avec MicrosoftActiveDirectory et la stratégie de groupe. Vous pouvez également créer des stratégies de restriction logicielle sur des ordinateurs autonomes. Stratégies de restriction logicielle sont des stratégies d’approbation, qui sont définies par un administrateur pour limiter des scripts et tout autre code qui n’est pas entièrement fiable depuis l’exécution des réglementations.

Vous pouvez définir ces stratégies par le biais de l’extension stratégies de Restriction logicielle de l’éditeur de stratégie de groupe ou le composant logiciel enfichable Stratégie de sécurité locale pour la Console MMC (MicrosoftManagement Console).

Pour plus d’informations détaillées sur les stratégies de restriction logicielle, consultez le [Software Restriction Policies Technical Overview](software-restriction-policies-technical-overview.md).

## <a name="BKMK_APP"></a>Applications pratiques
Les administrateurs peuvent utiliser des stratégies de restriction logicielle pour les tâches suivantes:

-   Définir ce qu’est un code de confiance

-   Conception une stratégie de groupe souple réglementant les scripts, les fichiers exécutables et les contrôles ActiveX

Stratégies de restriction logicielle sont appliquées par le système d’exploitation et par les applications (par exemple, les applications de l’écriture de scripts) conformes aux stratégies de restriction logicielle.

Plus précisément, les administrateurs peuvent utiliser des stratégies de restriction logicielle pour les raisons suivantes:

-   Spécifier quels logiciels (fichiers exécutables) peuvent s’exécuter sur les clients

-   Empêcher les utilisateurs d’exécuter des programmes spécifiques sur les ordinateurs partagés

-   Spécifier qui peut ajouter des éditeurs approuvés pour les clients

-   Définir la portée des stratégies de restriction logicielle (spécifier si elles affectent tous les utilisateurs ou un sous-ensemble d’utilisateurs sur les clients)

-   Empêcher les fichiers exécutables d’en cours d’exécution sur l’ordinateur local, une unité d’organisation (UO), un site ou un domaine. Cela est approprié dans le cas lorsque vous n’utilisez pas de stratégies de restriction logicielle à résoudre les problèmes potentiels avec les utilisateurs malveillants.

## <a name="BKMK_NEW"></a>Fonctionnalités nouvelles et modifiées
Il n’existe aucune modification dans les fonctionnalités des stratégies de Restriction logicielle.

## <a name="BKMK_DEP"></a>Fonctionnalité supprimée ou déconseillée
Il n’existe aucune fonctionnalité supprimée ou déconseillée pour les stratégies de Restriction logicielle.

## <a name="BKMK_SOFT"></a>Configuration logicielle requise
L’extension stratégies de Restriction logicielle pour l’éditeur de stratégie de groupe locale est accessible par le biais de la console MMC.

Les fonctionnalités suivantes sont requises pour créer et gérer des stratégies de restriction logicielle sur l’ordinateur local:

-   Éditeur de stratégie de groupe

-   Windows Installer

-   Authenticode et WinVerifyTrust

Si votre conception appelle pour le déploiement du domaine de ces stratégies, en plus de la liste ci-dessus, les fonctionnalités suivantes sont requises:

-   Services de domaine ActiveDirectory

-   Stratégie de groupe

## <a name="BKMK_INSTALL"></a>Informations du Gestionnaire de serveur
Stratégies de Restriction logicielle est une extension de l’éditeur de stratégie de groupe Local et n’est pas installé par le biais du Gestionnaire de serveur, ajouter des rôles et fonctionnalités.

## <a name="BKMK_LINKS"></a>Voir aussi
Le tableau suivant fournit des liens vers des ressources pertinentes pour comprendre et utiliser des stratégies de restriction logicielle.

|Type de contenu|Références|
|--------|-------|
|**Évaluation du produit**|[Verrouillage d’applications avec les stratégies de Restriction logicielle](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|
|**Planification**|[Présentation technique de stratégies de Restriction logicielle](software-restriction-policies-technical-overview.md) (Windows Server2012)<br /><br />[Référence technique de stratégies de Restriction logicielle](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx) (Windows Server2003)|
|**Déploiement**|Aucune ressource disponible.|
|**Opérations**|[Administrer les stratégies de Restriction logicielle](administer-software-restriction-policies.md) (Windows Server2012)<br /><br />[Aide produit de stratégies de Restriction logicielle](https://technet.microsoft.com/library/cc779607(v=WS.10).aspx) (Windows Server2003)|
|**Résolution des problèmes**|[Résoudre les problèmes de stratégies de Restriction logicielle](troubleshoot-software-restriction-policies.md) (Windows Server2012)<br /><br />[Résolution des problèmes de stratégies de Restriction logicielle](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx) (Windows Server2003)|
|**Sécurité**|[Menaces et contre-mesures de Restriction logicielle stratégies](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx) (Windows Server2008)<br /><br />[Menaces et contre-mesures de Restriction logicielle stratégies](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx) (Windows Server2008R2)|
|**Outils et paramètres**|[Stratégies de Restriction logicielle outils et paramètres](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx) (Windows Server2003)|
|**Ressources de la Communauté**|[Verrouillage d’applications avec les stratégies de Restriction logicielle](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|



