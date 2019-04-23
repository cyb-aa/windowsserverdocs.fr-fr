---
ms.assetid: eb600904-24b8-4488-a278-c1c971dc2f2d
title: Planification du Placement des contrôleurs de domaine régional
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: bec8595ab6eae8eb6cedaf9307ab97ac9c8316b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880440"
---
# <a name="planning-regional-domain-controller-placement"></a>Planification du Placement des contrôleurs de domaine régional

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pour garantir la rentabilité, envisagez de placer les contrôleurs de domaine régional aussi peu que possible. Tout d’abord, passez en revue la feuille de calcul « Géographique emplacements et liaisons de Communication » (DSSTOPO_1.doc) utilisé dans [collecte des informations réseau](../../ad-ds/plan/Collecting-Network-Information.md) pour déterminer si un emplacement est un concentrateur.  
  
Envisagez de placer des contrôleurs de domaine régional pour chaque domaine qui est représenté dans chaque emplacement de concentrateur. Une fois que vous placez des contrôleurs de domaine régional dans tous les emplacements de concentrateur, évaluer la nécessité de placement des contrôleurs de domaine régional à des emplacements satellites. Élimination des contrôleurs de domaine régional inutiles à partir d’emplacements de satellites réduit les coûts de prise en charge nécessaires pour maintenir une infrastructure de serveur distant.  
  
En outre, vérifiez la sécurité physique des contrôleurs de domaine dans les emplacements de hub et satellites qui personnel non autorisé ne peut pas y accéder. Ne placez pas les contrôleurs de domaine accessible en écriture dans les emplacements de hub et satellites dans lequel vous ne pouvez pas garantir la sécurité physique du contrôleur de domaine. Une personne qui dispose d’un accès physique à un contrôleur de domaine accessible en écriture peut attaquer le système :  
  
- L’accès aux disques physiques en démarrant un autre système d’exploitation sur un contrôleur de domaine.  
- Suppression (et éventuellement remplacer) les disques physiques sur un contrôleur de domaine.  
- Obtention et la manipulation d’une copie de sauvegarde d’un état de système de contrôleur domaine.  
  
Ajouter des contrôleurs de domaine régional accessible en écriture uniquement aux emplacements dans lesquels vous pouvez garantir leur sécurité physique.  
  
Dans les emplacements avec une sécurité physique inadéquate, déploiement d’un contrôleur de domaine en lecture seule (RODC) est la solution recommandée. À l’exception des mots de passe de compte, un RODC contient tous les objets Active Directory et les attributs qui contient un contrôleur de domaine accessible en écriture. Toutefois, les modifications ne peuvent pas être apportées à la base de données qui est stocké sur le RODC. Modification doit être effectuée sur un contrôleur de domaine accessible en écriture et puis répliquées vers le RODC.  
  
Pour authentifier les ouvertures de session client et l’accès aux serveurs de fichiers local, la plupart des organisations placer les contrôleurs de domaine régional pour tous les domaines régionaux qui sont représentés dans un emplacement donné. Toutefois, vous devez prendre en compte plusieurs variables lors de l’évaluation si un emplacement de l’entreprise nécessite de ses clients pour que l’authentification locale ou les clients peuvent reposer sur l’authentification et de la requête sur une liaison réseau (étendu WAN). L’illustration suivante montre comment déterminer si vous devez placer les contrôleurs de domaine à des emplacements satellites.  
  
![placement de contrôleur de domaine régional de plan](media/Planning-Regional-Domain-Controller-Placement/49892c8c-2c99-4aab-92ba-808dbc8048e2.gif)  
  
## <a name="onsite-technical-expertise-availability"></a>Disponibilité de leur expertise technique sur site

Contrôleurs de domaine doivent être gérés en continu pour diverses raisons. Placer un contrôleur de domaine régional uniquement dans les emplacements qui incluent des membres du personnel qui permettre administrer le contrôleur de domaine, ou assurez-vous que le contrôleur de domaine peut être géré à distance.  
  
Dans les environnements de succursale avec généralement une mauvaise sécurité physique et le personnel avec très peu de connaissances technologie d’informations, déploiement de ce contrôleur est souvent la solution recommandée. Autorisations d’administrateur local pour un RODC peuvent être déléguées à n’importe quel utilisateur de domaine sans se voir accorder des droits d’utilisateur pour le domaine ou d’autres contrôleurs de domaine. Cela permet à l’utilisateur branche locale pour vous connecter à un RODC et effectuer un travail de maintenance sur le serveur, telles que la mise à niveau d’un pilote. Toutefois, l’utilisateur branche ne peut pas ouvrir une session sur un autre contrôleur de domaine ou effectuer toute autre tâche d’administration dans le domaine. De cette façon, l’utilisateur branche peut être délégué la possibilité de gérer efficacement le RODC dans la filiale sans compromettre la sécurité du reste du domaine ou de la forêt.  
  
## <a name="wan-link-availability"></a>Disponibilité de la liaison réseau étendu

Les liaisons WAN que rencontrer des pannes fréquentes peuvent entraîner une perte de productivité significative aux utilisateurs si l’emplacement n’inclut pas un contrôleur de domaine qui peut authentifier les utilisateurs. Si la disponibilité de votre liaison réseau étendu n’est pas 100 pour cent et vos sites distants ne peuvent pas tolérer une panne de service, placez un contrôleur de domaine régional dans les emplacements où les utilisateurs doivent pouvoir ouvrir une session ou l’accès à exchange server lors de la liaison WAN est hors service.  
  
## <a name="authentication-availability"></a>Disponibilité de l’authentification

Certaines organisations, telles que les banques, exigent que les utilisateurs soit authentifié à tout moment. Placer un contrôleur de domaine régional dans un emplacement où la disponibilité de la liaison réseau étendu n’est pas 100 pour cent, mais les utilisateurs ont besoin d’authentification à tout moment.  
  
## <a name="logon-performance-over-wan-links"></a>Performances d’ouverture de session sur les liaisons WAN

Si la disponibilité de votre liaison WAN est hautement fiable, placer un contrôleur de domaine à l’emplacement varie selon les exigences de performances d’ouverture de session sur la liaison WAN. Les facteurs qui influencent les performances de l’ouverture de session sur le réseau WAN incluent la vitesse de liaison et la bande passante disponible, nombre d’utilisateurs et les profils d’utilisation et la quantité de trafic de réseau d’ouverture de session et le trafic de réplication.  
  
### <a name="wan-link-speed-and-bandwidth-utilization"></a>Utilisation de vitesse et la bande passante de liaison réseau étendu

Les activités d’un utilisateur unique peuvent congest une liaison WAN lente. Placer un contrôleur de domaine à un emplacement si les performances d’ouverture de session sur la liaison WAN sont inacceptable.  
  
Le pourcentage moyen d’utilisation de la bande passante indique comment encombré est d’une liaison réseau. Si une liaison réseau a l’utilisation de bande passante moyenne est supérieure à une valeur acceptable, placez un contrôleur de domaine à cet emplacement.  
  
### <a name="number-of-users-and-usage-profiles"></a>Nombre d’utilisateurs et les profils d’utilisation

Le nombre d’utilisateurs et leurs profils d’utilisation à un emplacement donné peut aider à déterminer si vous devez placer les contrôleurs de domaine régional à cet emplacement. Pour éviter les pertes de productivité si un lien WAN échoue, placez un contrôleur de domaine régional à un emplacement qui a moins de 100 utilisateurs.  
  
Les profils d’utilisation indiquent la façon dont les utilisateurs utilisent les ressources réseau. Vous n’avez pas besoin de placer un contrôleur de domaine dans un emplacement qui contient uniquement quelques utilisateurs n’accèdent fréquemment aux ressources réseau.  
  
### <a name="logon-network-traffic-vs-replication-traffic"></a>Trafic de réseau d’ouverture de session et le trafic de réplication

Si un contrôleur de domaine n’est pas disponible dans le même emplacement que le client Active Directory, le client crée un trafic d’ouverture de session sur le réseau. La quantité de trafic d’ouverture de session qui est créé sur le réseau physique est influencée par plusieurs facteurs, notamment les appartenances aux groupes ; nombre et taille des objets de stratégie de groupe (GPO) ; scripts d’ouverture de session ; et les fonctionnalités telles que les dossiers hors connexion, redirection de dossiers et les profils itinérants.  
  
En revanche, un contrôleur de domaine qui est placé à un emplacement donné génère le trafic de réplication sur le réseau. La fréquence et la quantité de mises à jour effectuées sur les partitions hébergées sur les contrôleurs de domaine influencent la quantité de trafic de réplication qui est créé sur le réseau. Les différents types de mises à jour pouvant être effectuées sur les partitions hébergées sur les contrôleurs de domaine incluent l’ajout ou modification des utilisateurs et des attributs d’utilisateur, modification des mots de passe et en ajoutant ou modifiant des groupes globaux, des imprimantes ou des volumes.  
  
Pour déterminer si vous devez placer un contrôleur de domaine régional à un emplacement, comparez le coût du trafic d’ouverture de session créé par un emplacement sans un contrôleur de domaine et le coût du trafic de réplication créé en plaçant un contrôleur de domaine à l’emplacement.  
  
Par exemple, prenons un réseau qui a des filiales sont connectés via des liaisons lentes au siège social et dans les contrôleurs de domaine peuvent facilement être ajoutées. Si le trafic de recherche d’ouverture de session et directory quotidiens de quelques utilisateurs de site distant provoque plus de trafic réseau que la réplication de toutes les données d’entreprise vers la branche, envisagez d’ajouter un contrôleur de domaine à la branche.  
  
Si la réduction des coûts de gestion des contrôleurs de domaine est plus important que le trafic réseau, centraliser les contrôleurs de domaine pour ce domaine et ne pas placer des contrôleurs de domaine régional à l’emplacement ou envisagez de placer les RODC à l’emplacement.  
  
Pour une feuille de calcul pour vous aider à documenter l’emplacement des contrôleurs de domaine régional et le nombre d’utilisateurs pour chaque domaine qui est représenté dans chaque emplacement, consultez [travail aides pour Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), téléchargez Job_ Aids_Designing_and_Deploying_Directory_and_Security_Services.zip et ouvrez « Domain Controller Placement » (DSSTOPO_4.doc).  
  
Vous devez voir les informations sur les emplacements dans lesquels vous devez placer les contrôleurs de domaine régional lors du déploiement de domaines régionaux. Pour plus d’informations sur le déploiement de domaines régionaux, consultez [déploiement de Windows Server 2008 domaines régionaux](https://technet.microsoft.com/library/cc755118.aspx).  
