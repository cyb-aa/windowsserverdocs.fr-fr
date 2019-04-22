---
ms.assetid: 83f746e5-81db-4610-9977-1d5c57699f50
title: Création d’une conception de site
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f6d896213708f8c3ec5de44a1f85fb4ebd86b8c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819070"
---
# <a name="creating-a-site-design"></a>Création d’une conception de site

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Création d’un site implique le choix futurs emplacements des sites, création d’objets de site, création d’objets de sous-réseaux et associer les sous-réseaux de sites.  
  
## <a name="deciding-which-locations-will-become-sites"></a>Choix des futurs emplacements des sites

Décidez quels emplacements pour créer des sites pour comme suit :  
  
- Créez des sites pour tous les emplacements dans lesquels vous prévoyez de placer les contrôleurs de domaine. Consultez les informations documentées dans la feuille de calcul « Placement de contrôleur de domaine » (DSSTOPO_4.doc) pour identifier les emplacements qui incluent des contrôleurs de domaine.  
- Créer des sites à ces emplacements qui incluent des serveurs qui exécutent des applications qui nécessitent un site doit être créé. Certaines applications, tels que Distributed fichier système espaces de noms (DFSN), utilisent des objets de site pour localiser les serveurs le plus proche pour les clients.  

   > [!NOTE]  
   > Si votre organisation a plusieurs réseaux à proximité avec des connexions rapides et fiables, vous pouvez inclure tous les sous-réseaux de ces réseaux virtuels dans un seul site Active Directory. Par exemple, si la valeur de retour aller-retour réseau latence entre deux serveurs dans différents sous-réseaux est de 10 ms ou moins, vous pouvez inclure les deux sous-réseaux dans le même site Active Directory. Si la latence du réseau entre les deux emplacements est supérieure à 10 ms, vous ne devez pas inclure les sous-réseaux dans un seul site Active Directory. Même lorsque la latence est de 10 ms ou moins, vous pouvez choisir de déployer des sites distincts, si vous souhaitez segmenter le trafic entre les sites pour les applications basées sur Active Directory.  

- Si un site n’est pas requis pour un emplacement, ajouter le sous-réseau de l’emplacement à un site pour lequel l’emplacement est la vitesse du réseau (étendu WAN) étendu maximale et la bande passante disponible.  
  
Emplacements des documents qui deviendront des sites et les adresses de réseau et les masques de sous-réseau au sein de chaque emplacement. Pour une feuille de calcul pour vous aider à documenter les sites, consultez [travail aides pour Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558)Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip et télécharger ouvrir « sous-réseaux associer avec Sites » (DSSTOPO_6.doc).  
  
## <a name="creating-a-site-object-design"></a>Création d’un objet site

Pour chaque emplacement où vous avez décidé de créer des sites, envisagez de créer des objets de site dans les Services de domaine Active Directory (AD DS). Emplacements des documents qui deviendront des sites dans la feuille de calcul « Association de sous-réseaux avec les Sites ».  
  
Pour plus d’informations sur la façon de créer des objets de site, consultez l’article [créer un Site](https://go.microsoft.com/fwlink/?LinkId=107067).  
  
## <a name="creating-a-subnet-object-design"></a>Création d’un objet sous-réseau

Pour chaque sous-réseau IP et le masque de sous-réseau associé à chaque emplacement, envisagez de créer des objets de sous-réseaux dans AD DS qui représente toutes les adresses IP au sein du site.  
  
Lorsque vous créez un objet de sous-réseau Active Directory, les informations de sous-réseau IP de réseau et le masque de sous-réseau sont automatiquement converties en le format de notation de longueur de préfixe réseau <IP address> / <prefix length>. Par exemple, réseau IP version 4 (IPv4) 172.16.4.0 avec un masque de sous-réseau 255.255.252.0 s’affiche sous forme 172.16.4.0/22. Outre les adresses IPv4, Windows Server 2008 prend également en charge IP version 6 (IPv6) préfixes de sous-réseau, par exemple, 3FFE:FFFF:0:C000 :: / 64. Pour plus d’informations sur les sous-réseaux IP dans chaque emplacement, consultez la feuille de calcul « Emplacements et sous-réseaux » (DSSTOPO_2.doc) [collecte des informations réseau](../../ad-ds/plan/Collecting-Network-Information.md) et [annexe a : Emplacements et préfixes de sous-réseau](Appendix-A--Locations-and-Subnet-Prefixes.md).  
  
Associer chaque objet de sous-réseau avec un objet de site en faisant référence à la feuille de calcul « Association de sous-réseaux avec les Sites » (DSSTOPO_6.doc) dans « décider qui emplacements seront deviennent « section Sites pour déterminer quel sous-réseau doit être associée avec quel site. Document de l’objet de sous-réseau Active Directory associé à chaque emplacement dans la feuille de calcul « Association de sous-réseaux avec les Sites » (DSSTOPO_6.doc).  
  
Pour plus d’informations sur la création d’objets de sous-réseaux, consultez l’article [créer un sous-réseau](https://go.microsoft.com/fwlink/?LinkId=107068).
