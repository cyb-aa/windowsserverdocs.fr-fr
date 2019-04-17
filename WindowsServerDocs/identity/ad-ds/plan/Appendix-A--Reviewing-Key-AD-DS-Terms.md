---
ms.assetid: 87196b65-a356-409f-9af0-b5950797d668
title: "Annexe A - consulter les termes du contrat de clé AD DS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ce7c0b639ded498b15200dfd594b3f34d6492dce
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-a-reviewing-key-ad-ds-terms"></a>Annexe a: examen des termes du contrat de clé ADDS

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Les termes suivants sont pertinents pour le processus de déploiement pour Windows Server 2008 Active Directory Services de domaine (AD DS).  
  
## <a name="active-directory-domain"></a>Domaine Active Directory  
Une unité d’administration dans un réseau d’ordinateurs qui, pour faciliter l’administration, regroupe plusieurs fonctionnalités, notamment les éléments suivants:  
  
-   **Identité de l’utilisateur au niveau du réseau**. Dans les domaines, les identités des utilisateurs peuvent être créées une seule fois et puis référencées sur n’importe quel ordinateur qui est joint à la forêt dans laquelle se trouve le domaine. Les contrôleurs de domaine qui constituent un domaine stockent les comptes d’utilisateur et les informations d’identification utilisateur, telles que des mots de passe ou des certificats, en toute sécurité.  
  
-   **Authentification**. Contrôleurs de domaine fournissent des services d’authentification pour les utilisateurs. Ils fournissent également des données d’autorisation supplémentaires, tels que les appartenances au groupe utilisateur. Les administrateurs peuvent utiliser ces services pour contrôler l’accès aux ressources sur le réseau.  
  
-   **Relations d’approbation**. Domaines étendent les services d’authentification aux utilisateurs dans d’autres domaines dans leur propre forêt au moyen des approbations bidirectionnelles automatique. Domaines étendent également les services d’authentification aux utilisateurs dans les domaines dans d’autres forêts au moyen d’approbations de forêt ou des approbations externes créées manuellement.  
  
-   **Administration de la stratégie**. Un domaine est une étendue de stratégies administratives, telles que les règles de réutilisation des mots de passe et la complexité de mot de passe.  
  
-   **Réplication**. Un domaine définit une partition de l’arborescence de répertoires qui fournit les données qui est suffisant pour fournir des services requis et qui sont répliquées entre les contrôleurs de domaine. De cette façon, tous les contrôleurs de domaine sont des homologues dans un domaine, et ils sont gérés en tant qu’unité.  
  
## <a name="active-directory-forest"></a>Forêt Active Directory  
Une collection d’un ou plusieurs domaines Active Directory qui partagent une structure logique commune, schéma d’annuaire et configuration du réseau, ainsi que les relations d’approbation transitive bidirectionnelle automatique. Chaque forêt est une instance unique de l’annuaire, et il définit une limite de sécurité.  
  
## <a name="active-directory-functional-level"></a>Niveau fonctionnel de Active Directory  
Un domaine Active Directory qui active les fonctionnalités avancées d’AD DS de domaine ou forêt.  
  
## <a name="migration"></a>Migration  
Le processus de déplacement d’un objet d’un domaine source vers un domaine cible, tout en conservant ou modification des caractéristiques de l’objet pour le rendre accessible dans le nouveau domaine.  
  
## <a name="domain-restructure"></a>Restructuration des domaines  
Un processus de migration implique la modification de la structure de domaine d’une forêt. Une restructuration des domaines peut impliquer la consolidation ou l’ajout de domaines, et il peut avoir lieu entre des forêts ou au sein d’une forêt.  
  
## <a name="domain-consolidation"></a>Consolidation des domaines  
Processus de restructuration qui implique l’élimination des domaines AD DS en fusionnant leur contenu avec le contenu d’autres domaines.  
  
## <a name="domain-upgrade"></a>Mise à niveau du domaine  
Le processus de mise à niveau le service d’annuaire d’un domaine vers une version ultérieure du service d’annuaire. Cela inclut la mise à niveau le système d’exploitation sur tous les contrôleurs de domaine et d’augmenter le niveau fonctionnel de domaine Active Directory, le cas échéant.  
  
## <a name="in-place-domain-upgrade"></a>Mise à niveau sur place  
Le processus de mise à niveau les systèmes d’exploitation de tous les contrôleurs de domaine dans un domaine donné, par exemple, la mise à niveau de Windows Server 2003 vers Windows Server 2008 et augmenter le niveau fonctionnel du domaine, le cas échéant, en laissant les objets de domaine, tels que les utilisateurs et groupes, en place.  
  
## <a name="forest-root-domain"></a>Domaine racine de forêt  
Le premier domaine créé dans la forêt Active Directory. Ce domaine est automatiquement désigné comme le domaine racine de forêt. Il fournit la base de l’infrastructure de la forêt Active Directory.  
  
## <a name="regional-domain"></a>Domaine régional  
Un domaine enfant est créé dans une région géographique pour optimiser le trafic de réplication.  
  


