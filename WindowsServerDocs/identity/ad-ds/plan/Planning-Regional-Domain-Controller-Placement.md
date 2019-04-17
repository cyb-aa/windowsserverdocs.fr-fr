---
ms.assetid: eb600904-24b8-4488-a278-c1c971dc2f2d
title: "Planification du Placement des contrôleurs de domaine régional"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c5254560e9b1a10030fa37ec0966868986212a4a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="planning-regional-domain-controller-placement"></a>Planification du Placement des contrôleurs de domaine régional

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Pour garantir rentabilité, envisagez de placer les contrôleurs de domaine régional aussi peu que possible. Tout d’abord, passez en revue la feuille de calcul (DSSTOPO_1.doc) «Géographique emplacements et des liaisons de Communication» utilisé dans [collecte des informations réseau](../../ad-ds/plan/Collecting-Network-Information.md) pour déterminer si un emplacement est un concentrateur.  
  
Envisagez de placer les contrôleurs de domaine régional pour chaque domaine qui est représenté dans chaque emplacement hub. Une fois que vous placez des contrôleurs de domaine régional dans tous les emplacements de hub, évaluez la nécessité de placement des contrôleurs de domaine régional aux emplacements satellites. Éliminer les contrôleurs de domaine régional inutiles à partir des emplacements satellites réduit les coûts de support technique nécessaires pour conserver une infrastructure de serveur distant.  
  
Assurez-vous en outre, la sécurité physique des contrôleurs de domaine dans des emplacements hub et satellite afin que les personnes non autorisées ne peuvent pas y accéder. Ne placez pas les contrôleurs de domaine accessible en écriture dans le hub et satellite emplacements dans lesquels vous ne peut pas garantir la sécurité physique du contrôleur de domaine. Une personne qui a un accès physique à un contrôleur de domaine accessible en écriture permettre attaquer le système:  
  
-   L’accès aux disques physiques en démarrant un autre système d’exploitation sur un contrôleur de domaine.  
  
-   Suppression (et éventuellement le remplacement) les disques physiques sur un contrôleur de domaine.  
  
-   Obtention et manipuler une copie d’une sauvegarde l’état système contrôleur domaine.  
  
Ajouter des contrôleurs de domaine régional accessible en écriture uniquement aux emplacements dans laquelle vous pouvez garantir leur sécurité physique.  
  
Dans les emplacements avec une sécurité physique inadéquate, déploiement d’un contrôleur de domaine en lecture seule (RODC) est la solution recommandée. À l’exception des mots de passe de compte, un RODC contient tous les objets ActiveDirectory et les attributs qui contient un contrôleur de domaine accessible en écriture. Toutefois, ne peuvent pas être modifiés à la base de données qui est stocké sur le RODC. Modifications doivent être effectuées sur un contrôleur de domaine accessible en écriture et puis répliquées sur le RODC.  
  
Pour authentifier les ouvertures de session client et l’accès aux serveurs de fichiers local, la plupart des organisations placez des contrôleurs de domaine régional pour tous les domaines régionaux qui sont représentés dans un emplacement donné. Toutefois, vous devez envisager de variables lors de l’évaluation d’un emplacement de l’entreprise requiert que l’authentification locale ses clients ou les clients peuvent s’appuient sur l’authentification et de la requête via une liaison réseau (étendu WAN). L’illustration suivante montre comment déterminer si vous devez placer les contrôleurs de domaine à des emplacements satellites.  
  
![Sélection élective de plan du contrôleur de domaine régional](media/Planning-Regional-Domain-Controller-Placement/49892c8c-2c99-4aab-92ba-808dbc8048e2.gif)  
  
## <a name="onsite-technical-expertise-availability"></a>Disponibilité des compétences techniques sur site  
Contrôleurs de domaine doivent être gérés en permanence pour diverses raisons. Placez un contrôleur de domaine régional uniquement dans les emplacements qui incluent le personnel qui permettre administrer le contrôleur de domaine, ou vérifiez que le contrôleur de domaine peut être géré à distance.  
  
Dans les environnements de succursales avec généralement une sécurité physique et le personnel avec très peu de connaissances technologie informations déploiement d’un RODC est souvent la solution recommandée. Autorisations d’administrateur local pour un RODC peuvent être déléguées à n’importe quel utilisateur de domaine sans se voir accorder les droits d’utilisateur pour le domaine ou d’autres contrôleurs de domaine. Cela permet à l’utilisateur branche local pour ouvrir une session sur un RODC et effectuer des opérations de maintenance sur le serveur, telles que la mise à niveau d’un pilote. Toutefois, l’utilisateur branche ne peut pas ouvrir une session sur tout autre contrôleur de domaine ou effectuer toutes les tâches d’administration dans le domaine. De cette façon, l’utilisateur branche peut être déléguée la possibilité de gérer efficacement le RODC dans la filiale sans compromettre la sécurité du reste du domaine ou de la forêt.  
  
## <a name="wan-link-availability"></a>Disponibilité de la liaison réseau étendu  
Les liaisons WAN qui rencontrent des pannes fréquentes peuvent entraîner une perte de productivité importantes pour les utilisateurs si l’emplacement n’inclut pas un contrôleur de domaine peut authentifier les utilisateurs. Si votre disponibilité de la liaison réseau étendu n’est pas 100 pour cent et vos sites distants ne peuvent pas tolérer une interruption de service, placez un contrôleur de domaine régional dans les emplacements où les utilisateurs ont besoin d’ouvrir une session ou d’accès à exchange server lorsque la liaison WAN est hors service.  
  
## <a name="authentication-availability"></a>Disponibilité de l’authentification  
Certaines organisations, telles que des banques, exigent que les utilisateurs soit authentifié à tout moment. Placer un contrôleur de domaine régional dans un emplacement où la disponibilité de la liaison réseau étendu n’est pas 100 pour cent, mais les utilisateurs requièrent une authentification à tout moment.  
  
## <a name="logon-performance-over-wan-links"></a>Performances d’ouverture de session sur les liaisons WAN  
Si votre disponibilité de la liaison WAN est très fiable, en plaçant un contrôleur de domaine à l’emplacement dépend des exigences de performances d’ouverture de session sur la liaison WAN. Facteurs qui influencent les performances d’ouverture de session sur le réseau WAN incluent la vitesse de liaison et la bande passante disponible, nombre d’utilisateurs et les profils d’utilisation et la quantité de trafic d’ouverture de session et le trafic de réplication.  
  
### <a name="wan-link-speed-and-bandwidth-utilization"></a>Utilisation de la vitesse et la bande passante de liaison WAN  
Les activités d’un utilisateur peuvent congest une liaison réseau étendue lente. Placer un contrôleur de domaine à un emplacement si les performances d’ouverture de session sur la liaison WAN sont inacceptable.  
  
Le pourcentage moyen de l’utilisation de la bande passante indique comment encombrés une liaison réseau est. Si une liaison réseau dispose d’utilisation de la bande passante moyenne est supérieure à une valeur acceptable, placez un contrôleur de domaine à cet emplacement.  
  
### <a name="number-of-users-and-usage-profiles"></a>Nombre d’utilisateurs et les profils d’utilisation  
Le nombre d’utilisateurs et leurs profils d’utilisation à un emplacement donné peut aider à déterminer si vous devez placer les contrôleurs de domaine régional à cet emplacement. Pour éviter les pertes de productivité en cas d’échec d’une liaison WAN, placez un contrôleur de domaine régional à un emplacement disposant d’au moins 100utilisateurs.  
  
Les profils d’utilisation indiquent comment les utilisateurs utilisent les ressources réseau. Vous n’avez pas besoin de placer un contrôleur de domaine dans un emplacement qui contient seulement quelques utilisateurs fréquemment d’accéder aux ressources réseau.  
  
### <a name="logon-network-traffic-vs-replication-traffic"></a>Trafic de réseau d’ouverture de session et le trafic de réplication  
Si un contrôleur de domaine n’est pas disponible dans le même emplacement que le client ActiveDirectory, le client crée le trafic d’ouverture de session sur le réseau. La quantité de trafic réseau d’ouverture de session qui est créé sur le réseau physique est influencée par plusieurs facteurs, notamment les appartenances au groupe; nombre et taille des objets de stratégie de groupe (GPO); scripts d’ouverture de session; et des fonctionnalités telles que les dossiers hors connexion, redirection de dossiers et les profils itinérants.  
  
En revanche, un contrôleur de domaine qui est placé à un emplacement donné génère le trafic de réplication sur le réseau. La fréquence et la quantité de mises à jour effectuées sur les partitions hébergées sur les contrôleurs de domaine influencent la quantité de trafic de réplication qui est créé sur le réseau. Les différents types de mises à jour qui peuvent être effectuées sur les partitions hébergées sur les contrôleurs de domaine incluent l’ajout ou modification des utilisateurs et des attributs utilisateur, modification des mots de passe et ajout ou modification des groupes globaux, les imprimantes ou les volumes.  
  
Pour déterminer si vous devez placer un contrôleur de domaine régional à un emplacement, comparer le coût de trafic d’ouverture de session créé par un emplacement sans un contrôleur de domaine et le coût du trafic de réplication créé en plaçant un contrôleur de domaine à l’emplacement.  
  
Par exemple, imaginons un réseau disposant de filiales sont connectés via des liaisons lentes au siège social et dans les contrôleurs de domaine peuvent aisément être ajoutés. Si le trafic de recherche d’ouverture de session et Active tous les jours de quelques utilisateurs du site distant provoque plus le trafic réseau à répliquer toutes les données d’entreprise et la branche, envisagez d’ajouter un contrôleur de domaine à la branche.  
  
Si ce qui réduit le coût de gestion des contrôleurs de domaine n’est plus important que le trafic réseau, centraliser les contrôleurs de domaine pour ce domaine et ne pas placer tous les contrôleurs de domaine régional à l’emplacement ou envisagez de placer un RODC à l’emplacement.  
  
Pour une feuille de calcul pour vous aider à la documentation de l’emplacement des contrôleurs de domaine régional et le nombre d’utilisateurs pour chaque domaine qui est représenté dans chaque emplacement, voir le travail d’identificateurs d’applet pour Windows Server2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), téléchargez < DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip</DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>, and Ouvrez (DSSTOPO_4.doc) "Placement du contrôleur de domaine".  
  
Vous devez consulter les informations sur les emplacements dans lesquels vous devez placer les contrôleurs de domaine régional lors du déploiement de domaines régionaux. Pour plus d’informations sur le déploiement de domaines régionaux, voir [déploiement de Windows Server2008domaines régionaux](https://technet.microsoft.com/library/cc755118.aspx).  
  


