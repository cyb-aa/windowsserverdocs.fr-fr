---
ms.assetid: 83f746e5-81db-4610-9977-1d5c57699f50
title: Création d’une conception de site
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 0464510b498296570bdef5edb122c6c26e1fe18f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822772"
---
# <a name="creating-a-site-design"></a>Création d’une conception de site

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La création d’une conception de site implique de déterminer les emplacements qui deviendront des sites, de créer des objets de site, de créer des objets de sous-réseau et d’associer les sous-réseaux avec les sites.  
  
## <a name="deciding-which-locations-will-become-sites"></a>Choix des emplacements qui deviendront des sites

Décidez à quels emplacements créer des sites comme suit :  
  
- Créez des sites pour tous les emplacements dans lesquels vous envisagez de placer des contrôleurs de domaine. Reportez-vous aux informations documentées dans la feuille de calcul « placement du contrôleur de domaine » (DSSTOPO_4. doc) pour identifier les emplacements incluant des contrôleurs de domaine.  
- Créez des sites pour les emplacements qui incluent des serveurs exécutant des applications qui nécessitent la création d’un site. Certaines applications, telles que les espaces de noms système de fichiers DFS, utilisent des objets de site pour localiser les serveurs les plus proches des clients.  

   > [!NOTE]  
   > Si votre organisation dispose de plusieurs réseaux à proximité avec des connexions rapides et fiables, vous pouvez inclure tous les sous-réseaux de ces réseaux dans un seul site de Active Directory. Par exemple, si la latence réseau aller-retour entre deux serveurs de différents sous-réseaux est de 10 ms ou moins, vous pouvez inclure les deux sous-réseaux dans le même site de Active Directory. Si la latence du réseau entre les deux emplacements est supérieure à 10 ms, vous ne devez pas inclure les sous-réseaux dans un seul site de Active Directory. Même lorsque la latence est de 10 ms ou moins, vous pouvez choisir de déployer des sites distincts si vous souhaitez segmenter le trafic entre les sites pour les applications basées sur Active Directory.  

- Si un site n’est pas requis pour un emplacement, ajoutez le sous-réseau de l’emplacement à un site pour lequel l’emplacement dispose de la vitesse maximale du réseau étendu (WAN) et de la bande passante disponible.  
  
Emplacements des documents qui deviendront des sites et les adresses réseau et les masques de sous-réseau dans chaque emplacement. Pour obtenir une feuille de calcul pour vous aider dans la documentation des sites, consultez [les outils d’aide pour le kit de déploiement Windows Server 2003](https://go.microsoft.com/fwlink/?LinkID=102558), téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip et ouvrez « Association de sous-réseaux avec des sites » (DSSTOPO_6. doc).  
  
## <a name="creating-a-site-object-design"></a>Création d’une conception d’objet de site

Pour chaque emplacement où vous avez décidé de créer des sites, envisagez de créer des objets de site dans Active Directory Domain Services (AD DS). Emplacements des documents qui deviendront des sites dans la feuille de calcul « Association de sous-réseaux avec des sites ».  
  
Pour plus d’informations sur la création d’objets de site, consultez l’article [créer un site](https://go.microsoft.com/fwlink/?LinkId=107067).  
  
## <a name="creating-a-subnet-object-design"></a>Création d’une conception d’objet sous-réseau

Pour chaque sous-réseau IP et masque de sous-réseau associé à chaque emplacement, envisagez de créer des objets de sous-réseau dans AD DS représentant toutes les adresses IP au sein du site.  
  
Lorsque vous créez un objet de sous-réseau Active Directory, les informations relatives au sous-réseau IP et au masque de sous-réseau sont automatiquement converties dans le format de notation de la longueur du préfixe réseau <IP address>/<prefix length>. Par exemple, l’adresse IP de la version 4 (IPv4) du réseau 172.16.4.0 avec un masque de sous-réseau 255.255.252.0 apparaît comme 172.16.4.0/22. Outre les adresses IPv4, Windows Server 2008 prend également en charge les préfixes de sous-réseau IP version 6 (IPv6), par exemple 3FFE : FFFF : 0 : C000 ::/64. Pour plus d’informations sur les sous-réseaux IP dans chaque emplacement, reportez-vous à la feuille de calcul « emplacements et sous-réseaux » (DSSTOPO_2. doc) dans [collecte des informations réseau](../../ad-ds/plan/Collecting-Network-Information.md) et [annexe A : emplacements et préfixes de sous-réseau](Appendix-A--Locations-and-Subnet-Prefixes.md).  
  
Associez chaque objet de sous-réseau à un objet de site en faisant référence à la feuille de calcul « Association de sous-réseaux avec des sites » (DSSTOPO_6. doc) dans la section « choix des emplacements qui deviendront des sites » pour déterminer le sous-réseau à associer au site. Documentez l’objet sous-réseau Active Directory associé à chaque emplacement dans la feuille de calcul « Association de sous-réseaux avec des sites » (DSSTOPO_6. doc).  
  
Pour plus d’informations sur la création d’objets de sous-réseau, consultez l’article [créer un sous-réseau](https://go.microsoft.com/fwlink/?LinkId=107068).
