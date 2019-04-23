---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: Délégation de l'administration des conteneurs et unités d'organisation par défaut
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d482854fd82b4bf0d0e61315d36e6222470ca55
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830890"
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>Délégation de l'administration des conteneurs et unités d'organisation par défaut

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Chaque domaine Active Directory contient un ensemble standard de conteneurs et unités d’organisation (UO) qui sont créées pendant l’installation des Services de domaine Active Directory (AD DS). notamment :  
  
-   Conteneur de domaine, qui sert de conteneur racine pour la hiérarchie  
  
-   Conteneur intégré, qui contient la valeur par défaut de comptes d’administrateur de service  
  
-   Conteneur d’utilisateurs, qui est l’emplacement par défaut pour les nouveaux comptes d’utilisateur et les groupes créés dans le domaine  
  
-   Conteneur des ordinateurs, qui est l’emplacement par défaut pour les nouveaux comptes d’ordinateur créé dans le domaine  
  
-   Unité d’organisation des contrôleurs de domaine qui est l’emplacement par défaut pour les comptes d’ordinateur pour les comptes d’ordinateurs de contrôleurs de domaine  
  
Le propriétaire de la forêt contrôle ces unités d’organisation et les conteneurs par défaut.  
  
## <a name="domain-container"></a>Conteneur de domaine  
Le conteneur de domaine est le conteneur racine de la hiérarchie d’un domaine. Modifications apportées aux stratégies ou de la liste de contrôle d’accès (ACL) sur ce conteneur peuvent potentiellement affecter l’échelle du domaine. Ne pas déléguer le contrôle de ce conteneur ; Il doit être contrôlé par les administrateurs de service.  
  
## <a name="users-and-computers-containers"></a>Utilisateurs et ordinateurs conteneurs  
Lorsque vous effectuez une mise à niveau sur place domaine à partir de Windows Server 2003 vers Windows Server 2008, les utilisateurs et les ordinateurs existants sont automatiquement placés dans les conteneurs utilisateurs et les ordinateurs. Si vous créez un nouveaux conteneurs domaine, utilisateurs et ordinateurs Active Directory sont les emplacements par défaut pour tous les nouveaux comptes d’utilisateur et les comptes d’ordinateur de contrôleur de domaine dans le domaine.  
  
> [!IMPORTANT]  
> Si vous avez besoin déléguer le contrôle sur les utilisateurs ou ordinateurs, ne modifiez pas les paramètres par défaut sur les ordinateurs et utilisateurs conteneurs. Au lieu de cela, créez les nouvelles unités d’organisation (si nécessaire) et déplacer les objets utilisateur et ordinateur à partir de leurs conteneurs par défaut et dans les nouvelles unités d’organisation. Déléguer le contrôle sur les nouvelles unités d’organisation, en fonction des besoins. Nous vous recommandons de pas modifier qui contrôle les conteneurs par défaut.  
  
En outre, vous ne pouvez pas appliquer les paramètres de stratégie de groupe pour les ordinateurs et utilisateurs par défaut conteneurs. Pour appliquer la stratégie de groupe aux utilisateurs et ordinateurs, créez les nouvelles unités d’organisation et déplacer les objets utilisateur et ordinateur dans ces unités d’organisation. Appliquez les paramètres de stratégie de groupe pour les nouvelles unités d’organisation.  
  
Si vous le souhaitez, vous pouvez rediriger la création d’objets qui sont placés dans les conteneurs par défaut pour être placés dans des conteneurs de votre choix.  
  
## <a name="well-known-users-and-groups-and-built-in-accounts"></a>Utilisateurs connus et les groupes et les comptes intégrés  
Par défaut, plusieurs utilisateurs bien connus et les groupes et les comptes intégrés sont créés dans un nouveau domaine. Nous recommandons que la gestion de ces comptes reste sous le contrôle des administrateurs de service. Ne pas déléguer la gestion de ces comptes à une personne qui n’est pas un administrateur de service. Le tableau suivant répertorie les utilisateurs connus et les groupes et les comptes intégrés qui doivent rester sous le contrôle des administrateurs de service.  
  
|Groupes et utilisateurs connus|Comptes intégrés|  
|--------------------------------|----------------------|  
|Éditeurs de certificats<br /><br />Contrôleurs de domaine<br /><br />Propriétaires créateurs de la stratégie de groupe<br /><br />KRBTGT<br /><br />Invités du domaine<br /><br />Administrateur<br /><br />Administrateurs du domaine<br /><br />Administrateurs de schéma (domaine racine forêt uniquement)<br /><br />Administrateurs de l’entreprise (domaine racine forêt uniquement)<br /><br />Utilisateurs du domaine|Administrateur<br /><br />Invité<br /><br />Invités<br /><br />Opérateurs de compte<br /><br />Administrateurs<br /><br />Opérateurs de sauvegarde<br /><br />Générateurs d’approbation de forêt entrante<br /><br />Opérateurs d'impression<br /><br />Accès Compatible pré-Windows 2000<br /><br />Opérateurs de serveur<br /><br />Utilisateurs|  
  
## <a name="domain-controller-ou"></a>Contrôleur de domaine unité d’organisation  
Lorsque les contrôleurs de domaine sont ajoutés au domaine, leurs objets d’ordinateur sont automatiquement ajoutés à l’unité d’organisation du contrôleur de domaine. Cette unité d’organisation a un ensemble de stratégies appliquées à ce dernier par défaut. Pour vous assurer que ces stratégies sont appliquées uniformément à tous les contrôleurs de domaine, nous vous recommandons de pas déplacer les objets d’ordinateur des contrôleurs de domaine en dehors de cette unité d’organisation. Appliquer les stratégies par défaut peut provoquer un contrôleur de domaine ne parviennent pas à fonctionner correctement.  
  
Par défaut, les administrateurs de service contrôlent cette unité d’organisation. Pas déléguer le contrôle de cette unité d’organisation à des personnes autres que les administrateurs de service.  
  


