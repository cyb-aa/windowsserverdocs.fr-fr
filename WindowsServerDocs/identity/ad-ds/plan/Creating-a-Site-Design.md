---
ms.assetid: 83f746e5-81db-4610-9977-1d5c57699f50
title: "Création d’une conception de Site"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2bcbf30159721e1fc2e12af103ca6f3e79f3ec97
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-design"></a>Création d’une conception de Site

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Création d’une conception de site implique le choix des futurs emplacements des sites, création d’objets de site, création d’objets de sous-réseaux et associer les sous-réseaux de sites.  
  
## <a name="deciding-which-locations-will-become-sites"></a>Choix des futurs emplacements des sites  
Décidez quels emplacements pour créer des sites pour comme suit:  
  
-   Créer des sites pour tous les emplacements dans lesquels vous prévoyez de placer les contrôleurs de domaine. Consultez les informations décrites dans la feuille de calcul (DSSTOPO_4.doc) «Placement du contrôleur de domaine» pour identifier les emplacements qui incluent des contrôleurs de domaine.  
  
-   Créer des sites à ces emplacements qui incluent les serveurs qui exécutent des applications qui nécessitent un site à créer. Certaines applications, telles que distribuées fichier système espaces de noms (DFSN), utilisent des objets de site pour localiser les serveurs le plus proche pour les clients.  
  
    > [!NOTE]  
    > Si votre organisation possède plusieurs réseaux à proximité des connexions rapides et fiables, vous pouvez inclure tous les sous-réseaux pour ces réseaux dans un seul site ActiveDirectory. Par exemple, si le retour en réseau à latence entre deux serveurs dans différents sous-réseaux est de 10ms ou moins, vous pouvez inclure les deux sous-réseaux dans le même site ActiveDirectory. Si la latence du réseau entre les deux emplacements est supérieure à 10ms, vous ne devez pas inclure les sous-réseaux dans un seul site ActiveDirectory. Même lorsque la latence est de 10ms ou moins, vous pouvez choisir de déployer des sites distincts pour segmenter le trafic entre les sites pour les applications basée sur ActiveDirectory.  
  
-   Si un site n’est pas requis pour un emplacement, ajoutez le sous-réseau de l’emplacement à un site pour lequel l’emplacement a la vitesse du réseau (étendu WAN) étendu maximale et bande passante disponible.  
  
Emplacements de document qui vont devenir des sites et les adresses réseau et les masques de sous-réseau au sein de chaque emplacement. Pour une feuille de calcul pour vous aider à la documentation de sites, voir tâche identificateurs d’applet pour Windows Server2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip et (DSSTOPO_6.doc) "Association de sous-réseaux avec Sites ouverts".  
  
## <a name="creating-a-site-object-design"></a>Création d’un modèle d’objet de site  
Pour chaque emplacement où vous avez décidé de créer des sites, envisagez de créer des objets de site dans les Services de domaine ActiveDirectory (ADDS). Emplacements de document qui vont devenir des sites dans la feuille de calcul «Associer des sous-réseaux avec des Sites».  
  
Pour plus d’informations sur la façon de créer des objets de site, voir créer un Site ([https://go.microsoft.com/fwlink/?LinkId=107067](https://go.microsoft.com/fwlink/?LinkId=107067)).  
  
## <a name="creating-a-subnet-object-design"></a>Création d’un modèle d’objet de sous-réseau  
Pour chaque sous-réseau IP et le masque de sous-réseau associé à chaque emplacement, envisagez de créer des objets de sous-réseaux dans ADDS représentant toutes les adresses IP au sein du site.  
  
Lorsque vous créez un objet de sous-réseau ActiveDirectory, les informations de sous-réseau IP et le masque de sous-réseau sont automatiquement traduites dans le format de notation de longueur de préfixe réseau <IP address>/<prefix length>. Par exemple, réseau IP version4 (IPv4) adresse 172.16.4.0 avec un masque de sous-réseau 255.255.252.0 s’affiche en tant que 172.16.4.0/22. Outre les adresses IPv4, Windows Server2008 prend également en charge IP version6 (IPv6) préfixes de sous-réseau, par exemple, 3FFE:FFFF:0:C000:: / 64. Pour plus d’informations sur les sous-réseaux IP dans chaque emplacement, consultez la feuille de calcul (DSSTOPO_2.doc) «Emplacements et sous-réseaux» dans [collecte des informations réseau](../../ad-ds/plan/Collecting-Network-Information.md) et [annexe a: emplacements et préfixes de sous-réseau ](Appendix-A--Locations-and-Subnet-Prefixes.md).  
  
Associer chaque objet de sous-réseau avec un objet de site en faisant référence à la feuille de calcul (DSSTOPO_6.doc) «Associer des sous-réseaux avec des Sites» dans «décider qui emplacements seront devient «section Sites pour déterminer sur quel sous-réseau doit être associé avec le site. L’objet de sous-réseau ActiveDirectory associé à chaque emplacement dans la feuille de calcul (DSSTOPO_6.doc) «Association de sous-réseaux avec Sites» du document.  
  
Pour plus d’informations sur la création d’objets de sous-réseaux, voir créer un sous-réseau ([https://go.microsoft.com/fwlink/?LinkId=107068](https://go.microsoft.com/fwlink/?LinkId=107068)).  
  


