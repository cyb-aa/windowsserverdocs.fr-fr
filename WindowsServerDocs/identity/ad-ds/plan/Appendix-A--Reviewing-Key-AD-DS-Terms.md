---
ms.assetid: 87196b65-a356-409f-9af0-b5950797d668
title: Annexe A - examiner les termes du contrat de clé AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5434db4124c471c613f159dec28e27dee70e7086
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852020"
---
# <a name="appendix-a-reviewing-key-ad-ds-terms"></a>Annexe A : Examen des termes du contrat de clé AD DS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les termes suivants sont pertinents pour le processus de déploiement pour Windows Server 2008 Active Directory Domain Services (AD DS).  
  
## <a name="active-directory-domain"></a>domaine Active Directory,  
Une unité administrative dans un réseau d’ordinateurs qui, pour simplifier la gestion, regroupe plusieurs fonctionnalités, notamment les suivantes :  
  
-   **Identité de l’utilisateur de l’échelle du réseau**. Dans les domaines, les identités des utilisateurs peuvent être créées qu’une seule fois et ensuite référencées sur n’importe quel ordinateur est joint à la forêt dans laquelle se trouve le domaine. Les contrôleurs de domaine qui composent un domaine stockent les comptes d’utilisateur et les informations d’identification utilisateur, telles que les mots de passe ou des certificats, en toute sécurité.  
  
-   **Authentification**. Contrôleurs de domaine fournissent des services d’authentification pour les utilisateurs. Qu’il fournit également des données d’autorisation supplémentaires, tels que les appartenances de groupe utilisateur. Les administrateurs peuvent utiliser ces services pour contrôler l’accès aux ressources sur le réseau.  
  
-   **Relations d’approbation**. Domaines étendent les services d’authentification aux utilisateurs dans d’autres domaines dans leur propre forêt au moyen des approbations bidirectionnelles automatique. Domaines également étendent les services d’authentification pour les utilisateurs de domaines dans d’autres forêts au moyen d’approbations de forêt ou des approbations externes créées manuellement.  
  
-   **Administration de la stratégie**. Un domaine est une étendue de stratégies administratives, telles que les règles de réutilisation de mot de passe et la complexité de mot de passe.  
  
-   **Réplication**. Un domaine définit une partition de l’arborescence de répertoires qui fournit des données qui est suffisante pour offrir des services requis et qui sont répliquées entre les contrôleurs de domaine. De cette façon, tous les contrôleurs de domaine sont des homologues dans un domaine, et ils sont gérés en tant qu’unité.  
  
## <a name="active-directory-forest"></a>Forêt Active Directory  
Collection d’un ou plusieurs domaines Active Directory qui partagent une structure logique commune, de schéma d’annuaire et de configuration du réseau, ainsi que les relations d’approbation transitive bidirectionnelle automatique. Chaque forêt est une instance unique de l’annuaire, et il définit une limite de sécurité.  
  
## <a name="active-directory-functional-level"></a>Niveau fonctionnel Active Directory  
Un AD DS qui active les fonctionnalités avancées d’AD DS de l’échelle du domaine ou forêt.  
  
## <a name="migration"></a>Migration  
Le processus de déplacement d’un objet à partir d’un domaine source à un domaine cible, tout en préservant ou en modifier les caractéristiques de l’objet pour le rendre accessible dans le nouveau domaine.  
  
## <a name="domain-restructure"></a>Restructuration des domaines  
Un processus de migration implique la modification de la structure de domaine d’une forêt. Une restructuration des domaines peut impliquer la consolidation ou l’ajout de domaines, et il peut avoir lieu entre des forêts ou au sein d’une forêt.  
  
## <a name="domain-consolidation"></a>Consolidation de domaine  
Processus de restructuration qui implique l’élimination des domaines AD DS en fusionnant leur contenu avec le contenu d’autres domaines.  
  
## <a name="domain-upgrade"></a>Mise à niveau du domaine  
Le processus de mise à niveau le service d’annuaire d’un domaine vers une version ultérieure du service d’annuaire. Cela inclut la mise à niveau le système d’exploitation sur tous les contrôleurs de domaine et de déclencher le niveau fonctionnel du domaine Active Directory, le cas échéant.  
  
## <a name="in-place-domain-upgrade"></a>Mise à niveau sur place  
Le processus de mise à niveau les systèmes d’exploitation de tous les contrôleurs de domaine dans un domaine donné, par exemple, la mise à niveau de Windows Server 2003 vers Windows Server 2008 et augmenter le niveau fonctionnel du domaine, le cas échéant, tout en laissant les objets de domaine, tels que des utilisateurs et les groupes, en place.  
  
## <a name="forest-root-domain"></a>Domaine racine de forêt  
Le premier domaine qui est créé dans la forêt Active Directory. Ce domaine est automatiquement désigné comme le domaine racine de forêt. Il fournit la base de l’infrastructure de la forêt Active Directory.  
  
## <a name="regional-domain"></a>Domaine régional  
Un domaine enfant qui est créé dans une région géographique pour optimiser le trafic de réplication.  
  


