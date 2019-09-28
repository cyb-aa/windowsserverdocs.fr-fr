---
ms.assetid: 87196b65-a356-409f-9af0-b5950797d668
title: Annexe A-examen des conditions de AD DS clés
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 81beba874440f7a75c2d7932357fae70f046d996
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409019"
---
# <a name="appendix-a-reviewing-key-ad-ds-terms"></a>Annexe A : Examen des conditions d’AD DS clés

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les termes suivants s’appliquent au processus de déploiement de Windows Server 2008 Active Directory Domain Services (AD DS).  
  
## <a name="active-directory-domain"></a>domaine Active Directory,  
Unité administrative d’un réseau informatique qui, à des fins de gestion, regroupe plusieurs fonctionnalités, notamment les suivantes :  
  
-   **Identité de l’utilisateur au niveau du réseau**. Dans les domaines, les identités des utilisateurs peuvent être créées une seule fois, puis référencées sur n’importe quel ordinateur joint à la forêt dans laquelle se trouve le domaine. Les contrôleurs de domaine qui composent un domaine stockent les comptes d’utilisateur et les informations d’identification de l’utilisateur, tels que les mots de passe ou les certificats, en toute sécurité.  
  
-   **Authentification**. Les contrôleurs de domaine fournissent des services d’authentification pour les utilisateurs. Ils fournissent également des données d’autorisation supplémentaires, telles que des appartenances aux groupes d’utilisateurs. Les administrateurs peuvent utiliser ces services pour contrôler l’accès aux ressources sur le réseau.  
  
-   **Relations d’approbation**. Les domaines étendent les services d’authentification aux utilisateurs situés dans d’autres domaines de leur propre forêt par le biais de l’approbation bidirectionnelle automatique. Les domaines étendent également les services d’authentification aux utilisateurs dans les domaines d’autres forêts au moyen d’approbations de forêt ou de l’approbation externe créée manuellement.  
  
-   **Administration**de la stratégie. Un domaine est une étendue de stratégies administratives, telles que la complexité du mot de passe et les règles de réutilisation des mots de passe.  
  
-   **Réplication**. Un domaine définit une partition de l’arborescence de répertoires qui fournit des données suffisantes pour fournir les services requis et qui est répliquée entre les contrôleurs de domaine. De cette façon, tous les contrôleurs de domaine sont des homologues d’un domaine et sont gérés en tant qu’unité.  
  
## <a name="active-directory-forest"></a>Forêt Active Directory  
Collection d’un ou de plusieurs domaines Active Directory qui partagent une structure logique commune, un schéma d’annuaire et une configuration réseau, ainsi que des relations d’approbation transitives automatiques et bidirectionnelles. Chaque forêt est une instance unique de l’annuaire et définit une limite de sécurité.  
  
## <a name="active-directory-functional-level"></a>Niveau fonctionnel Active Directory  
Paramètre de AD DS qui permet des fonctionnalités avancées AD DS à l’ensemble du domaine ou de la forêt.  
  
## <a name="migration"></a>Migration  
Processus de déplacement d’un objet d’un domaine source vers un domaine cible, tout en préservant ou en modifiant les caractéristiques de l’objet pour le rendre accessible dans le nouveau domaine.  
  
## <a name="domain-restructure"></a>Restructuration de domaine  
Processus de migration qui implique la modification de la structure de domaine d’une forêt. Une restructuration de domaine peut impliquer la consolidation ou l’ajout de domaines, et elle peut avoir lieu entre des forêts ou au sein d’une forêt.  
  
## <a name="domain-consolidation"></a>Consolidation de domaines  
Processus de restructuration qui implique l’élimination des domaines AD DS en fusionnant leur contenu avec le contenu d’autres domaines.  
  
## <a name="domain-upgrade"></a>Mise à niveau de domaine  
Processus de mise à niveau du service d’annuaire d’un domaine vers une version ultérieure du service d’annuaire. Cela comprend la mise à niveau du système d’exploitation sur tous les contrôleurs de domaine et l’élévation du niveau fonctionnel d’AD DS, le cas échéant.  
  
## <a name="in-place-domain-upgrade"></a>Mise à niveau de domaine sur place  
Processus de mise à niveau des systèmes d’exploitation de tous les contrôleurs de domaine dans un domaine donné, par exemple la mise à niveau de Windows Server 2003 vers Windows Server 2008 et l’augmentation du niveau fonctionnel du domaine, le cas échéant, tout en laissant les objets de domaine, tels que les utilisateurs et les groupes, sur place.  
  
## <a name="forest-root-domain"></a>Domaine racine de la forêt  
Premier domaine créé dans la forêt Active Directory. Ce domaine est automatiquement désigné comme domaine racine de la forêt. Il fournit la base de l’infrastructure de forêt Active Directory.  
  
## <a name="regional-domain"></a>Domaine régional  
Domaine enfant créé dans une région géographique pour optimiser le trafic de réplication.  
  


