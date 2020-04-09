---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: Délégation de l'administration des conteneurs et unités d'organisation par défaut
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a8523ee738b991714a9c8673b6faaff7d9003987
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822652"
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>Délégation de l'administration des conteneurs et unités d'organisation par défaut

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Chaque domaine de Active Directory contient un ensemble standard de conteneurs et d’unités d’organisation (UO) qui sont créés pendant l’installation de Active Directory Domain Services (AD DS). notamment :  
  
-   Conteneur de domaine, qui sert de conteneur racine à la hiérarchie  
  
-   Conteneur intégré, qui contient les comptes d’administrateur de services par défaut  
  
-   Conteneur Users, qui est l’emplacement par défaut pour les nouveaux groupes et comptes d’utilisateur créés dans le domaine  
  
-   Conteneur d’ordinateurs, qui est l’emplacement par défaut pour les nouveaux comptes d’ordinateur créés dans le domaine  
  
-   L’unité d’organisation contrôleurs de domaine, qui est l’emplacement par défaut pour les comptes d’ordinateur pour les comptes d’ordinateur des contrôleurs de domaine  
  
Le propriétaire de la forêt contrôle ces conteneurs et unités d’organisation par défaut.  
  
## <a name="domain-container"></a>Conteneur de domaine  
Le conteneur de domaine est le conteneur racine de la hiérarchie d’un domaine. Les modifications apportées aux stratégies ou à la liste de contrôle d’accès (ACL) sur ce conteneur peuvent avoir un impact sur l’ensemble du domaine. Ne déléguez pas le contrôle de ce conteneur. elle doit être contrôlée par les administrateurs de service.  
  
## <a name="users-and-computers-containers"></a>Conteneurs d’utilisateurs et d’ordinateurs  
Lorsque vous effectuez une mise à niveau de domaine sur place de Windows Server 2003 vers Windows Server 2008, les utilisateurs et les ordinateurs existants sont automatiquement placés dans les conteneurs utilisateurs et ordinateurs. Si vous créez un domaine de Active Directory, les conteneurs utilisateurs et ordinateurs sont les emplacements par défaut pour tous les nouveaux comptes d’utilisateur et les comptes d’ordinateur qui n’appartiennent pas à un contrôleur de domaine dans le domaine.  
  
> [!IMPORTANT]  
> Si vous avez besoin de déléguer le contrôle des utilisateurs ou des ordinateurs, ne modifiez pas les paramètres par défaut des conteneurs utilisateurs et ordinateurs. Au lieu de cela, créez de nouvelles unités d’organisation (si nécessaire) et déplacez les objets utilisateur et ordinateur à partir de leurs conteneurs par défaut et vers les nouvelles unités d’organisation. Déléguez le contrôle sur les nouvelles unités d’organisation, si nécessaire. Nous vous recommandons de ne pas modifier les personnes qui contrôlent les conteneurs par défaut.  
  
En outre, vous ne pouvez pas appliquer les paramètres de stratégie de groupe aux conteneurs utilisateurs et ordinateurs par défaut. Pour appliquer des stratégie de groupe aux utilisateurs et aux ordinateurs, créez de nouvelles unités d’organisation et déplacez les objets utilisateur et ordinateur dans ces unités d’organisation. Appliquez les paramètres de stratégie de groupe aux nouvelles unités d’organisation.  
  
Si vous le souhaitez, vous pouvez rediriger la création des objets placés dans les conteneurs par défaut pour les placer dans les conteneurs de votre choix.  
  
## <a name="well-known-users-and-groups-and-built-in-accounts"></a>Utilisateurs et groupes connus et comptes intégrés  
Par défaut, plusieurs utilisateurs et groupes connus et comptes intégrés sont créés dans un nouveau domaine. Nous recommandons que la gestion de ces comptes reste sous le contrôle des administrateurs de service. Ne déléguez pas la gestion de ces comptes à une personne qui n’est pas un administrateur de service. Le tableau suivant répertorie les utilisateurs et groupes connus et les comptes intégrés qui doivent rester sous le contrôle des administrateurs de service.  
  
|Utilisateurs et groupes connus|Comptes intégrés|  
|--------------------------------|----------------------|  
|Éditeurs de certificats<p>Contrôleurs de domaine<p>Propriétaires créateurs de la stratégie de groupe<p>KRBTGT<p>Invités du domaine<p>Administrateur<p>Admins du domaine<p>Administrateurs du schéma (domaine racine de la forêt uniquement)<p>Administrateurs de l’entreprise (domaine racine de la forêt uniquement)<p>Utilisateurs du domaine|Administrateur<p>Invité<p>Invités<p>Opérateurs de compte<p>Administrateurs<p>Opérateurs de sauvegarde<p>Générateurs d’approbations de forêt entrante<p>Opérateurs d'impression<p>Accès compatible avec les versions antérieures à Windows 2000<p>Opérateurs de serveur<p>Utilisateurs|  
  
## <a name="domain-controller-ou"></a>UO du contrôleur de domaine  
Lorsque des contrôleurs de domaine sont ajoutés au domaine, leurs objets ordinateur sont automatiquement ajoutés à l’unité d’organisation du contrôleur de domaine. Un ensemble de stratégies par défaut est appliqué à cette UO. Pour vous assurer que ces stratégies sont appliquées uniformément à tous les contrôleurs de domaine, nous vous recommandons de ne pas déplacer les objets ordinateur des contrôleurs de domaine en dehors de cette UO. Si vous n’appliquez pas les stratégies par défaut, un contrôleur de domaine peut ne pas fonctionner correctement.  
  
Par défaut, les administrateurs de service contrôlent cette UO. Ne déléguez pas le contrôle de cette UO à des personnes autres que les administrateurs de service.  
  


