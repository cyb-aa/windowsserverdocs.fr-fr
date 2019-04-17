---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: "Déléguer l’Administration des unités d’organisation et des conteneurs par défaut"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2504208210c03193451d19478f3bc8c98ec98f23
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>Déléguer l’Administration des unités d’organisation et des conteneurs par défaut

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Chaque domaine Active Directory contient un ensemble standard de conteneurs et unités d’organisation (UO) qui sont créées pendant l’installation des Services de domaine Active Directory (AD DS). Ce sont les suivants:  
  
-   Conteneur de domaine, qui fait Office de conteneur racine pour la hiérarchie  
  
-   Conteneur intégré, qui contient la valeur par défaut les comptes administrateur de service  
  
-   Conteneur d’utilisateurs, qui est l’emplacement par défaut pour les nouveaux comptes d’utilisateur et les groupes créés dans le domaine  
  
-   Conteneur des ordinateurs, ce qui est l’emplacement par défaut pour les nouveaux comptes d’ordinateur créé dans le domaine  
  
-   Unité d’organisation des contrôleurs de domaine qui est l’emplacement par défaut pour les comptes d’ordinateur pour les comptes d’ordinateur de contrôleurs de domaine  
  
Le propriétaire de la forêt contrôle ces conteneurs par défaut et les unités d’organisation.  
  
## <a name="domain-container"></a>Conteneur de domaine  
Le conteneur de domaine est le conteneur de la racine de la hiérarchie d’un domaine. Modifications apportées aux stratégies ou la liste de contrôle d’accès (ACL) sur ce conteneur peuvent avoir potentiellement l’impact de l’échelle du domaine. Ne déléguez pas de contrôle de ce conteneur; Il doit être contrôlé par les administrateurs de service.  
  
## <a name="users-and-computers-containers"></a>Utilisateurs et ordinateurs des conteneurs  
Lorsque vous effectuez une mise à niveau sur place de Windows Server 2003 vers Windows Server 2008, les utilisateurs et ordinateurs existants sont automatiquement placés dans les conteneurs utilisateurs et les ordinateurs. Si vous créez des conteneurs domaine, utilisateurs et ordinateurs Active Directory sont les emplacements par défaut pour tous les nouveaux comptes d’utilisateur et les comptes d’ordinateur de contrôleur de domaine dans le domaine.  
  
> [!IMPORTANT]  
> Si vous avez besoin déléguer le contrôle sur les utilisateurs ou ordinateurs, ne modifiez pas les paramètres par défaut sur les utilisateurs et les ordinateurs des conteneurs. Au lieu de cela, créez des nouvelles unités d’organisation (le cas échéant) et déplacer les objets utilisateur et ordinateur à partir de leurs conteneurs par défaut et dans les nouvelles unités d’organisation. Déléguer le contrôle sur les nouvelles unités d’organisation, en fonction des besoins. Nous vous recommandons pas modifier qui contrôle les conteneurs par défaut.  
  
En outre, vous ne pouvez pas appliquer les paramètres de stratégie de groupe pour les ordinateurs et utilisateurs par défaut des conteneurs. Pour appliquer la stratégie de groupe aux utilisateurs et ordinateurs, créez de nouvelles unités d’organisation et déplacez les objets utilisateur et ordinateur dans les unités d’organisation. Appliquer les paramètres de stratégie de groupe pour les nouvelles unités d’organisation.  
  
Si vous le souhaitez, vous pouvez rediriger la création d’objets qui sont placés dans les conteneurs par défaut doivent être placés dans des conteneurs de votre choix.  
  
## <a name="well-known-users-and-groups-and-built-in-accounts"></a>Utilisateurs connus et des groupes et comptes intégrés  
Par défaut, plusieurs utilisateurs connus et les groupes et comptes intégrés sont créés dans un nouveau domaine. Nous recommandons que le gestion de ces comptes reste sous le contrôle des administrateurs de service. Pas déléguer la gestion de ces comptes à une personne qui n’est pas un administrateur de service. Le tableau suivant répertorie les utilisateurs connus et les groupes et les comptes intégrés qui doivent rester sous le contrôle des administrateurs de service.  
  
|Groupes et utilisateurs connus|Comptes intégrés|  
|--------------------------------|----------------------|  
|Éditeurs de certificats<br /><br />Contrôleurs de domaine<br /><br />Groupe Propriétaires créateurs de la stratégie<br /><br />KRBTGT<br /><br />Invités du domaine<br /><br />Administrateur<br /><br />Admins du domaine<br /><br />Administrateurs du schéma (domaine racine forêt uniquement)<br /><br />Administrateurs de l’entreprise (domaine racine forêt uniquement)<br /><br />Utilisateurs du domaine|Administrateur<br /><br />Invité<br /><br />Invités<br /><br />Opérateurs de compte<br /><br />Administrateurs<br /><br />Opérateurs de sauvegarde<br /><br />Générateurs d’approbations de forêt entrante<br /><br />Opérateurs d’impression<br /><br />Accès Compatible pré-Windows 2000<br /><br />Opérateurs de serveur<br /><br />Utilisateurs|  
  
## <a name="domain-controller-ou"></a>Contrôleur de domaine unité d’organisation  
Lorsque des contrôleurs de domaine sont ajoutés au domaine, leurs objets ordinateur sont automatiquement ajoutés à l’unité d’organisation du contrôleur de domaine. Cette unité d’organisation dispose d’un ensemble de stratégies appliquées par défaut. Pour vous assurer que ces stratégies sont appliquées uniformément à tous les contrôleurs de domaine, nous recommandons que vous ne déplacez pas les objets ordinateur des contrôleurs de domaine hors cette unité d’organisation. Pour appliquer les stratégies par défaut peut provoquer un contrôleur de domaine pour ne pas fonctionner correctement.  
  
Par défaut, les administrateurs de service contrôlent cette unité d’organisation. Pas déléguer le contrôle de cette unité d’organisation à des personnes autres que les administrateurs de service.  
  


