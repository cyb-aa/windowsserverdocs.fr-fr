---
title: Stratégies de restriction logicielle
description: Sécurité de Windows Server
ms.prod: windows-server
ms.technology: security-software-restriction-policies
ms.topic: article
ms.assetid: 5c0befad-07c3-4262-b418-372d01850305
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 905af608bcf5f43ea8883bc62d0344887c89baa7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855742"
---
# <a name="software-restriction-policies"></a>Stratégies de restriction logicielle

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique destinée aux professionnels de l’informatique décrit les stratégies de restriction logicielle (SRP) dans Windows Server 2012 et Windows 8, et fournit des liens vers des informations techniques sur les SRP à partir de Windows Server 2003.

Pour obtenir des procédures et des conseils de dépannage, consultez [administrer les stratégies de restriction logicielle](administer-software-restriction-policies.md) et [résoudre les problèmes de stratégies de restriction logicielle](troubleshoot-software-restriction-policies.md).

## <a name="software-restriction-policies-description"></a><a name="BKMK_OVER"></a>Description des stratégies de restriction logicielle
La fonctionnalité Stratégies de restriction logicielle est une fonctionnalité fondée sur les stratégies de groupe qui identifie les programmes logiciels s’exécutant sur les ordinateurs d’un domaine et qui contrôle la capacité de ces programmes à s’exécuter. Les stratégies de restriction logicielle s’inscrivent dans la stratégie de gestion et de sécurité Microsoft conçue pour aider les entreprises à accroître la fiabilité, l’intégrité et la facilité de gestion de leurs ordinateurs.

Les stratégies de restriction logicielle peuvent aussi contribuer à créer une configuration fortement restreinte pour vos ordinateurs, dans laquelle seule l’exécution d’applications clairement identifiées est autorisée. Les stratégies de restriction logicielle sont intégrées à Microsoft Active Directory et à la stratégie de groupe. Vous avez également la possibilité de créer des stratégies de restriction logicielle sur des ordinateurs autonomes. Les stratégies de restriction logicielle sont des stratégies d’approbation, c’est-à-dire des règles définies par un administrateur pour limiter des scripts et d’autres formes de code qui n’apparaissent pas entièrement fiables depuis l’exécution.

Vous pouvez définir ces stratégies au moyen de l’extension des stratégies de restriction logicielle disponible dans l’Éditeur d’objets de stratégie de groupe ou par le biais du composant logiciel enfichable Stratégie de sécurité locale de la console MMC (Microsoft Management Console).

Pour obtenir des informations détaillées sur les stratégies de restriction logicielle, voir la [Vue d’ensemble technique des stratégies de restriction logicielle](software-restriction-policies-technical-overview.md).

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Applications pratiques
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

## <a name="new-and-changed-functionality"></a><a name="BKMK_NEW"></a>Fonctionnalités nouvelles et modifiées
Aucune modification n’a été apportée aux fonctionnalités des stratégies de restriction logicielle.

## <a name="removed-or-deprecated-functionality"></a><a name="BKMK_DEP"></a>Fonctionnalités supprimées ou déconseillées
Il n’existe aucune fonctionnalité supprimée ou déconseillée dans le cadre des stratégies de restriction logicielle.

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Configuration logicielle requise
Vous pouvez accéder à l’extension des stratégies de restriction logicielle disponible dans l’Éditeur d’objets de stratégie de groupe via la console MMC.

Pour créer et maintenir des stratégies de restriction logicielle sur l’ordinateur local, vous devez disposer des fonctionnalités suivantes :

-   Éditeur d’objets de stratégie de groupe

-   Windows Installer

-   Authenticode et WinVerifyTrust

Si la structure que vous concevez nécessite un déploiement de ces stratégies dans le domaine, vous aurez besoin des fonctionnalités suivantes en plus des éléments de la liste ci-dessus :

-   Services de domaine Active Directory

-   Stratégie de groupe

## <a name="server-manager-information"></a><a name="BKMK_INSTALL"></a>Informations Gestionnaire de serveur
Les stratégies de restriction logicielle constituent une extension de l’Éditeur d’objets de stratégie de groupe et ne sont pas installées par le biais de la fonctionnalité d’ajout de rôles et de fonctionnalités du Gestionnaire de serveur.

## <a name="see-also"></a><a name="BKMK_LINKS"></a>Voir aussi
Le tableau qui suit propose des liens menant à des ressources appropriées permettant de comprendre et d’exploiter les stratégies de restriction logicielle.

|Type de contenu|Références|
|--------|-------|
|**Évaluation du produit**|[Verrouillage d’applications avec des stratégies de restriction logicielle](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|
|**Planification**|[Vue d’ensemble technique des stratégies de restriction logicielle](software-restriction-policies-technical-overview.md) (Windows Server 2012)<p>[Informations techniques de référence sur les stratégies de restriction logicielle](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx) (Windows Server 2003)|
|**Déploiement**|Aucune ressource disponible.|
|**Opérations**|[Administrer les stratégies de restriction logicielle](administer-software-restriction-policies.md) (Windows Server 2012)<p>[Aide produit pour les stratégies de restriction logicielle](https://technet.microsoft.com/library/cc779607(v=WS.10).aspx) (Windows Server 2003)|
|**Résolution des problèmes**|[Résoudre les problèmes liés aux stratégies de restriction logicielle](troubleshoot-software-restriction-policies.md) (Windows Server 2012)<p>[Résolution des problèmes liés aux stratégies de restriction logicielle](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx) (Windows Server 2003)|
|**Sécurité**|[Menaces et contre-mesures pour les stratégies de restriction logicielle](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx) (Windows Server 2008)<p>[Menaces et contre-mesures pour les stratégies de restriction logicielle](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx) (Windows Server 2008 R2)|
|**Outils et paramètres**|[Outils et paramètres des stratégies de restriction logicielle](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx) (Windows Server 2003)|
|**Ressources de la communauté**|[Verrouillage d’applications avec des stratégies de restriction logicielle](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|



