---
ms.assetid: eb600904-24b8-4488-a278-c1c971dc2f2d
title: Planification de l’emplacement des contrôleurs de domaine régionaux
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cb1d83d5afca92de85c4de8b3e9125e119250f66
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822112"
---
# <a name="planning-regional-domain-controller-placement"></a>Planification de l’emplacement des contrôleurs de domaine régionaux

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pour garantir une rentabilité, prévoyez de placer le moins possible de contrôleurs de domaine régionaux. Tout d’abord, examinez la feuille de calcul « emplacements géographiques et liaisons de communication » (DSSTOPO_1. doc) utilisée pour la [collecte d’informations réseau](../../ad-ds/plan/Collecting-Network-Information.md) afin de déterminer si un emplacement est un Hub.  
  
Prévoyez de placer les contrôleurs de domaine régionaux pour chaque domaine représenté dans chaque emplacement de concentrateur. Après avoir placé les contrôleurs de domaine régionaux dans tous les emplacements de concentrateur, évaluez la nécessité de placer les contrôleurs de domaine régionaux aux emplacements satellites. L’élimination des contrôleurs de domaine régionaux superflus des emplacements satellites réduit les coûts de support nécessaires à la maintenance d’une infrastructure de serveur distant.  
  
En outre, assurez la sécurité physique des contrôleurs de domaine dans les emplacements Hub et satellites afin que le personnel non autorisé puisse y accéder. Ne placez pas de contrôleurs de domaine accessibles en écriture dans des emplacements Hub et satellites dans lesquels vous ne pouvez pas garantir la sécurité physique du contrôleur de domaine. Une personne disposant d’un accès physique à un contrôleur de domaine accessible en écriture peut attaquer le système en :  
  
- Accès aux disques physiques en démarrant un autre système d’exploitation sur un contrôleur de domaine.  
- Suppression (et éventuellement remplacement) de disques physiques sur un contrôleur de domaine.  
- Obtention et manipulation d’une copie de sauvegarde de l’état du système d’un contrôleur de domaine.  
  
Ajoutez des contrôleurs de domaine régionaux accessibles en écriture uniquement aux emplacements dans lesquels vous pouvez garantir leur sécurité physique.  
  
Dans les emplacements avec une sécurité physique inadéquate, le déploiement d’un contrôleur de domaine en lecture seule (RODC) est la solution recommandée. À l’exception des mots de passe de compte, un RODC contient tous les objets et attributs Active Directory qu’un contrôleur de domaine accessible en écriture détient. Toutefois, les modifications ne peuvent pas être apportées à la base de données stockée sur le contrôleur de domaine en lecture seule. Les modifications doivent être apportées sur un contrôleur de domaine accessible en écriture, puis répliquées sur le RODC.  
  
Pour authentifier les connexions client et l’accès aux serveurs de fichiers locaux, la plupart des organisations placent les contrôleurs de domaine régionaux pour tous les domaines régionaux représentés dans un emplacement donné. Toutefois, vous devez prendre en compte de nombreuses variables lorsque vous évaluez si un emplacement d’entreprise requiert que ses clients aient une authentification locale ou que les clients puissent s’appuyer sur l’authentification et la requête sur une liaison de réseau étendu (WAN). L’illustration suivante montre comment déterminer s’il faut placer les contrôleurs de domaine aux emplacements satellites.  
  
![planifier le placement régional du DC](media/Planning-Regional-Domain-Controller-Placement/49892c8c-2c99-4aab-92ba-808dbc8048e2.gif)  
  
## <a name="onsite-technical-expertise-availability"></a>Disponibilité de l’expertise technique sur site

Les contrôleurs de domaine doivent être gérés en continu pour diverses raisons. Placez un contrôleur de domaine régional uniquement dans des emplacements qui incluent le personnel qui peut administrer le contrôleur de domaine ou assurez-vous que le contrôleur de domaine peut être géré à distance.  
  
Dans les environnements de succursales avec une sécurité physique médiocre et un personnel avec peu de connaissances en matière de technologies de l’information, le déploiement d’un RODC est souvent la solution recommandée. Les autorisations d’administrateur local pour un RODC peuvent être déléguées à n’importe quel utilisateur de domaine sans accorder à cet utilisateur des droits d’utilisateur pour le domaine ou d’autres contrôleurs de domaine. Cela permet à un utilisateur de la filiale locale de se connecter à un RODC et d’effectuer des tâches de maintenance sur le serveur, telles que la mise à niveau d’un pilote. Toutefois, l’utilisateur de la succursale ne peut pas se connecter à un autre contrôleur de domaine ou effectuer d’autres tâches d’administration dans le domaine. De cette façon, l’utilisateur de la succursale peut être déléguer la capacité de gérer efficacement le contrôleur de domaine en lecture seule dans la filiale sans compromettre la sécurité du reste du domaine ou de la forêt.  
  
## <a name="wan-link-availability"></a>Disponibilité des liaisons WAN

Les liaisons de réseau étendu qui rencontrent des pannes fréquentes peuvent entraîner une perte de productivité significative pour les utilisateurs si l’emplacement ne contient pas de contrôleur de domaine pouvant authentifier les utilisateurs. Si la disponibilité de votre liaison de réseau étendu n’est pas de 100% et que vos sites distants ne peuvent pas tolérer une panne de service, placez un contrôleur de domaine régional à des emplacements où les utilisateurs ont besoin d’ouvrir une session ou d’accéder à Exchange Server lorsque la liaison de réseau étendu est arrêtée.  
  
## <a name="authentication-availability"></a>Disponibilité de l’authentification

Certaines organisations, telles que les banques, exigent que les utilisateurs soient authentifiés à tout moment. Placez un contrôleur de domaine régional dans un emplacement où la disponibilité de la liaison de réseau étendu n’est pas de 100%, mais les utilisateurs nécessitent une authentification à tout moment.  
  
## <a name="logon-performance-over-wan-links"></a>Performances d’ouverture de session sur les liaisons WAN

Si la disponibilité de la liaison WAN est très fiable, le fait de placer un contrôleur de domaine à l’emplacement dépend des exigences de performances de connexion sur la liaison WAN. Les facteurs qui influencent les performances de connexion sur le réseau étendu incluent la vitesse de liaison et la bande passante disponible, le nombre d’utilisateurs et de profils d’utilisation, ainsi que la quantité de trafic réseau de connexion par rapport au trafic de réplication.  
  
### <a name="wan-link-speed-and-bandwidth-utilization"></a>Vitesse des liaisons WAN et utilisation de la bande passante

Les activités d’un seul utilisateur peuvent encombrer une liaison WAN lente. Placez un contrôleur de domaine à un emplacement si les performances de connexion sur la liaison de réseau étendu ne sont pas acceptables.  
  
Le pourcentage moyen d’utilisation de la bande passante indique le niveau d’encombrement d’une liaison réseau. Si une liaison réseau a une utilisation moyenne de la bande passante supérieure à une valeur acceptable, placez un contrôleur de domaine à cet emplacement.  
  
### <a name="number-of-users-and-usage-profiles"></a>Nombre d’utilisateurs et de profils d’utilisation

Le nombre d’utilisateurs et leurs profils d’utilisation à un emplacement donné peuvent aider à déterminer si vous devez placer les contrôleurs de domaine régionaux à cet emplacement. Pour éviter la perte de productivité en cas de défaillance d’une liaison de réseau étendu, placez un contrôleur de domaine régional sur un emplacement disposant de 100 utilisateurs ou plus.  
  
Les profils d’utilisation indiquent la manière dont les utilisateurs utilisent les ressources réseau. Vous n’avez pas besoin de placer un contrôleur de domaine dans un emplacement qui ne contient que quelques utilisateurs qui n’accèdent pas fréquemment aux ressources du réseau.  
  
### <a name="logon-network-traffic-vs-replication-traffic"></a>Trafic réseau de connexion et trafic de réplication

Si un contrôleur de domaine n’est pas disponible dans le même emplacement que le client Active Directory, le client crée le trafic d’ouverture de session sur le réseau. La quantité de trafic réseau de connexion qui est créée sur le réseau physique est influencée par plusieurs facteurs, y compris les appartenances aux groupes ; nombre et taille des objets de stratégie de groupe (GPO); scripts d’ouverture de session ; et des fonctionnalités telles que les dossiers hors connexion, la redirection de dossiers et les profils itinérants.  
  
En revanche, un contrôleur de domaine placé à un emplacement donné génère le trafic de réplication sur le réseau. La fréquence et la quantité de mises à jour effectuées sur les partitions hébergées sur les contrôleurs de domaine influencent la quantité de trafic de réplication qui est créée sur le réseau. Les différents types de mises à jour que vous pouvez effectuer sur les partitions hébergées sur les contrôleurs de domaine incluent l’ajout ou la modification des utilisateurs et des attributs d’utilisateur, la modification des mots de passe et l’ajout ou la modification de groupes globaux, d’imprimantes ou de volumes.  
  
Pour déterminer si vous devez placer un contrôleur de domaine régional à un emplacement, comparez le coût du trafic d’ouverture de session créé par un emplacement sans contrôleur de domaine par rapport au coût du trafic de réplication créé par le placement d’un contrôleur de domaine à l’emplacement.  
  
Par exemple, imaginez un réseau dont les filiales sont connectées via des liaisons lentes vers le siège social et dans lesquelles les contrôleurs de domaine peuvent facilement être ajoutés. Si le trafic quotidien de recherche dans l’annuaire et d’ouverture de session d’un petit nombre d’utilisateurs de sites distants entraîne un trafic réseau supérieur à la réplication de toutes les données d’entreprise vers la succursale, envisagez d’ajouter un contrôleur de domaine à la branche.  
  
Si la réduction du coût de la maintenance des contrôleurs de domaine est plus importante que le trafic réseau, Centralisez les contrôleurs de domaine pour ce domaine et ne placez pas de contrôleurs de domaine régionaux à l’emplacement ou envisagez de placer les contrôleurs de domaine en lecture seule à l’emplacement.  
  
Pour obtenir une feuille de calcul qui vous aide à documenter le placement des contrôleurs de domaine régionaux et le nombre d’utilisateurs pour chaque domaine représenté dans chaque emplacement, consultez [Outils d’aide pour le kit de déploiement de Windows Server 2003](https://go.microsoft.com/fwlink/?LinkID=102558), Télécharger Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip et ouvrez « emplacement du contrôleur de domaine » (DSSTOPO_4. doc).  
  
Vous devrez vous référer aux informations sur les emplacements dans lesquels vous devez placer les contrôleurs de domaine régionaux lorsque vous déployez des domaines régionaux. Pour plus d’informations sur le déploiement de domaines régionaux, voir [déploiement de domaines régionaux Windows Server 2008](https://technet.microsoft.com/library/cc755118.aspx).  
