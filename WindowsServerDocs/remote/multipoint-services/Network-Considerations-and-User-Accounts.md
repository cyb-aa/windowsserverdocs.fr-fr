---
title: Considérations relatives au réseau et comptes d’utilisateur
description: Fournit des informations de planification pour différents scénarios de réseau et utilisateur avec MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef4859fc-b7ae-4827-ab9c-b1dc07ab6c16
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 9133f28d2c3b36b18a2b6bc81d238835156bf447
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880800"
---
# <a name="network-considerations-and-user-accounts"></a>Considérations relatives au réseau et comptes d’utilisateur
MultiPoint Services peuvent être déployées dans une variété d’environnements réseau, et il peut prendre en charge les comptes d’utilisateurs locaux et les comptes d’utilisateur de domaine. En règle générale, les comptes d’utilisateur MultiPoint Services seront gérés dans un des environnements de réseau suivants :  
  
-   Un seul ordinateur exécutant MultiPoint Services avec les comptes d’utilisateurs locaux  
  
-   Plusieurs ordinateurs exécutant MultiPoint Services, chacun avec un compte d’utilisateur local  
  
-   Plusieurs ordinateurs exécutant MultiPoint Services et qui sont à l’aide de comptes d’utilisateur de domaine

Par définition, *comptes d’utilisateurs locaux* est uniquement accessible à partir de l’ordinateur sur lequel ils ont été créés. Comptes d’utilisateurs locaux sont des comptes d’utilisateur qui sont créés sur un ordinateur spécifique qui exécute MultiPoint Services. En revanche, *comptes d’utilisateur de domaine* sont des comptes d’utilisateur qui résident sur un contrôleur de domaine, et ils sont accessibles à partir de n’importe quel ordinateur connecté au domaine. Lorsque vous choisissez le type d’environnement réseau à utiliser, considérez les points suivants :  
  
-   Ressources seront partagés parmi les serveurs ?  
  
-   Les utilisateurs basculez entre les serveurs ?  
  
-   Les utilisateurs accéderont-ils aux serveurs de base de données qui requièrent une authentification ?  
  
-   Les utilisateurs accéderont-ils aux serveurs web internes qui requièrent une authentification ?  
  
-   Y a-t-il une infrastructure de domaine Active Directory existante en place ?  
  
-   Qui utilisent le gestionnaire MultiPoint de la console pour gérer les bureaux des utilisateurs, afficher les miniatures, ajouter des utilisateurs, limiter les sites Web et ainsi de suite ? Cette personne géreront plusieurs serveurs ? Cette personne doit avoir des privilèges d’administrateur sur les serveurs.  
  
Les sections suivantes traitent de gestion des comptes utilisateur dans ces environnements de mise en réseau.  
  
## <a name="single-multipoint-server-with-local-user-accounts"></a>Serveur MultiPoint unique avec des comptes d’utilisateurs locaux  
Dans les environnements avec un seul ordinateur qui exécute MultiPoint Services, il n’est pas nécessaire de disposer d’un réseau. Toutefois, pour tirer parti des ressources Internet, la configuration réseau requise peut être aussi simple que d’un routeur et une connexion à un fournisseur de services Internet (ISP). Les connexions réseau qui sont associées à une carte réseau sur MultiPoint Services sont configurées par défaut, pour obtenir une adresse IP et l’adresse du serveur DNS automatiquement par le biais de DHCP. Les routeurs Internet sont généralement configurés comme serveurs DHCP, et fournissent des adresses IP privées aux ordinateurs qui s’y connecter sur le réseau interne. Par conséquent, un seul ordinateur exécutant MultiPoint Services peut être en mesure de se connecter à l’interface interne du routeur, obtenir des informations IP automatique et vous connecter à Internet sans effort important ou de la configuration par un administrateur.  
  
Une méthode courante pour gérer les utilisateurs dans ce type d’environnement consiste à créer un compte d’utilisateur local pour chaque personne qui auront accès au système. Toute personne disposant d’un compte d’utilisateur local sur cet ordinateur peut ouvrir une session à MultiPoint Services à partir de n’importe quelle station qui est associée au système. Comptes d’utilisateurs locaux peuvent être créées et gérées à partir du gestionnaire MultiPoint.  
  
## <a name="multiple-multipoint-server-systems-with-local-user-accounts"></a>Plusieurs systèmes MultiPoint Server avec les comptes d’utilisateurs locaux  
Étant donné que les comptes d’utilisateurs locaux sont uniquement accessibles depuis l’ordinateur sur lequel elles ont été créées, lorsque vous déployez plusieurs systèmes MultiPoint Services dans un environnement, vous pouvez gérer les comptes d’utilisateurs locaux de deux manières :  
  
-   Vous pouvez créer des comptes d’utilisateur pour des personnes spécifiques sur des ordinateurs spécifiques en cours d’exécution MultiPoint Services.  
  
-   Vous pouvez utiliser le gestionnaire MultiPoint pour créer des comptes pour chaque utilisateur sur chaque ordinateur qui exécute MultiPoint Services.  
  
Par exemple, si vous envisagez d’affecter des utilisateurs à un ordinateur spécifique exécutant MultiPoint Services, vous pouvez créer quatre comptes d’utilisateurs locaux sur l’ordinateur A (user01, user02, user03 et user04) et quatre comptes d’utilisateurs locaux sur l’ordinateur B (user05 user06, user07 et user08). Dans ce scénario, les utilisateurs 01\-04 peuvent se connecter à l’ordinateur A à partir de n’importe quelle station qui y est connectée ; Toutefois, ils ne peuvent pas ouvrir une session sur l’ordinateur B. Vaut également pour les utilisateurs 05\-08, ce qui serait en mesure de se connecter uniquement à l’ordinateur B, mais pas à l’ordinateur A. en fonction de l’environnement de déploiement spécifique, cela peut être acceptable, voire même souhaitables.  
  
Toutefois, si chaque utilisateur doit être en mesure de se connecter à un ordinateur qui exécute MultiPoint Services, un compte d’utilisateur local doit être créé pour chaque utilisateur sur chaque ordinateur qui exécute MultiPoint Services. Choisir de gérer les utilisateurs de cette manière introduit certaines difficultés. Par exemple, si user01 ouvre une session sur l’ordinateur A le lundi et enregistre un fichier dans le dossier Documents, puis l’utilisateur se connecte à l’ordinateur B le mardi, le fichier a été enregistré dans le dossier Documents sur l’ordinateur A n’est ne pas accessible sur l’ordinateur B.  
  
En outre, si un utilisateur a des comptes sur l’ordinateur A et B, il n’existe aucun moyen de synchroniser automatiquement les mots de passe pour les comptes. Cela peut entraîner des difficultés à une session si le mot de passe du compte doit être modifiée sur un seul ordinateur, mais pas l’autre des utilisateurs. Vous pouvez simplifier la gestion des comptes utilisateur dans ce type d’environnement réseau en assignant à chaque utilisateur à un seul ordinateur qui exécute MultiPoint Services. De cette façon, l’utilisateur peut se connecter des stations qui sont associés à cet ordinateur et accéder aux fichiers appropriés.  
  
## <a name="multiple-multipoint-services-systems-with-domain-accounts"></a>Plusieurs systèmes MultiPoint Services avec les comptes de domaine  
Les environnements de domaine sont courantes dans les grands environnements de réseau qui comprennent plusieurs serveurs. Par exemple, votre peut joindre un ou plusieurs ordinateurs exécutant le rôle de MultiPoint Services à un domaine, puis utiliser Microsoft Active Directory pour gérer les comptes d’utilisateur qui est accessible à partir de n’importe quel ordinateur dans le domaine. Ainsi, pour les comptes d’utilisateur de domaine individuel vers sont créés et accessibles à partir de n’importe quelle station dans n’importe quel système MultiPoint Services est joint au domaine.  
 
Lorsque vous déployez MultiPoint Services dans un environnement de domaine, il existe plusieurs facteurs à prendre en compte :  
  
-   Si les comptes de domaine sont utilisés, ils ne peuvent pas être gérés à partir du gestionnaire MultiPoint.  
  
-   Par défaut, MultiPoint Services est configuré pour autoriser chaque utilisateur pour vous connecter à la station qu’une seule à la fois. Si vous décidez d’autoriser les utilisateurs à se connecter à plusieurs stations en même temps à l’aide d’un seul compte, vous pouvez utiliser la **modifier les paramètres de serveur** option dans le gestionnaire MultiPoint.  
  
-   L’emplacement des contrôleurs de domaine peut affecter la vitesse et la fiabilité avec laquelle les utilisateurs pourront s’authentifier auprès du domaine et de localiser les ressources.  
  
## <a name="single-user-account-for-multiple-stations"></a>Compte d’utilisateur unique pour plusieurs stations  
MultiPoint Services a la possibilité de se connecter à plusieurs stations sur le même ordinateur simultanément à l’aide d’un seul compte d’utilisateur. Cette fonctionnalité est utile dans les environnements où les utilisateurs ne reçoivent pas les noms d’utilisateur unique et à l’aide d’un compte d’utilisateur unique peut simplifier la gestion du système MultiPoint Services.  
  
