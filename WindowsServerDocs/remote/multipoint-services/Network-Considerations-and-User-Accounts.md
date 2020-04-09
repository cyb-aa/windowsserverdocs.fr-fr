---
title: Considérations relatives au réseau et comptes d’utilisateur
description: Fournit des informations de planification pour différents scénarios réseau et utilisateur avec MultiPoint services
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: ef4859fc-b7ae-4827-ab9c-b1dc07ab6c16
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: ed9770ff6e91e548dfc38a1de927646590a25165
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853412"
---
# <a name="network-considerations-and-user-accounts"></a>Considérations relatives au réseau et comptes d’utilisateur
MultiPoint services peut être déployé dans divers environnements réseau et peut prendre en charge des comptes d’utilisateur locaux et des comptes d’utilisateur de domaine. En règle générale, les comptes d’utilisateurs MultiPoint services sont gérés dans l’un des environnements réseau suivants :  
  
-   Un ordinateur unique exécutant MultiPoint services avec des comptes d’utilisateurs locaux  
  
-   Plusieurs ordinateurs exécutant MultiPoint services, chacun avec un compte d’utilisateur local  
  
-   Plusieurs ordinateurs exécutant MultiPoint services et qui utilisent des comptes d’utilisateur de domaine

Par définition, les *comptes d’utilisateurs locaux* ne sont accessibles qu’à partir de l’ordinateur sur lequel ils ont été créés. Les comptes d’utilisateurs locaux sont des comptes d’utilisateur qui sont créés sur un ordinateur spécifique qui exécute MultiPoint services. En revanche, les *comptes d’utilisateur de domaine* sont des comptes d’utilisateur qui résident sur un contrôleur de domaine et sont accessibles à partir de n’importe quel ordinateur connecté au domaine. Lorsque vous choisissez le type d’environnement réseau à utiliser, tenez compte des éléments suivants :  
  
-   Les ressources seront-elles partagées entre les serveurs ?  
  
-   Les utilisateurs vont-ils passer d’un serveur à un autre ?  
  
-   Les utilisateurs auront-ils accès aux serveurs de base de données qui nécessitent une authentification ?  
  
-   Les utilisateurs auront-ils accès aux serveurs Web internes qui nécessitent une authentification ?  
  
-   Existe-t-il une infrastructure de domaine Active Directory existante ?  
  
-   Qui utilisera la console MultiPoint Manager pour gérer les postes de travail des utilisateurs, afficher les miniatures, ajouter des utilisateurs, limiter les sites Web, etc. ? Cette personne gère-t-elle plus d’un serveur ? Cette personne doit disposer de privilèges d’administrateur sur les serveurs.  
  
Les sections suivantes traitent de la gestion des comptes d’utilisateur dans ces environnements réseau.  
  
## <a name="single-multipoint-server-with-local-user-accounts"></a>Serveur MultiPoint unique avec comptes d’utilisateurs locaux  
Dans les environnements avec un seul ordinateur qui exécute MultiPoint services, il n’est pas nécessaire de disposer d’un réseau. Toutefois, pour tirer parti des ressources Internet, les exigences en matière de mise en réseau peuvent être aussi simples qu’un routeur et une connexion à un fournisseur de services Internet (ISP). Les connexions réseau associées à une carte réseau sur MultiPoint services sont configurées, par défaut, pour obtenir automatiquement une adresse IP et une adresse de serveur DNS par le biais de DHCP. Les routeurs Internet sont généralement configurés en tant que serveurs DHCP et fournissent des adresses IP privées aux ordinateurs qui se connectent à eux sur le réseau interne. Par conséquent, un seul ordinateur exécutant MultiPoint services peut être en mesure de se connecter à l’interface interne du routeur, d’obtenir des informations IP automatiques et de se connecter à Internet sans effort ou configuration par un administrateur.  
  
Une méthode courante pour gérer les utilisateurs dans ce type d’environnement consiste à créer un compte d’utilisateur local pour chaque personne qui aura accès au système. Toute personne disposant d’un compte d’utilisateur local sur cet ordinateur peut se connecter à MultiPoint services à partir de n’importe quelle station associée au système. Les comptes d’utilisateurs locaux peuvent être créés et gérés à partir du gestionnaire MultiPoint.  
  
## <a name="multiple-multipoint-server-systems-with-local-user-accounts"></a>Plusieurs systèmes MultiPoint Server avec des comptes d’utilisateur locaux  
Étant donné que les comptes d’utilisateurs locaux ne sont accessibles qu’à partir de l’ordinateur sur lequel ils ont été créés, lorsque vous déployez plusieurs systèmes MultiPoint services dans un environnement, vous pouvez gérer les comptes d’utilisateurs locaux de l’une des deux manières suivantes :  
  
-   Vous pouvez créer des comptes d’utilisateur pour des personnes spécifiques sur des ordinateurs spécifiques exécutant MultiPoint services.  
  
-   Vous pouvez utiliser le gestionnaire MultiPoint pour créer des comptes pour chaque utilisateur sur chaque ordinateur qui exécute MultiPoint services.  
  
Par exemple, si vous envisagez d’affecter des utilisateurs à un ordinateur spécifique exécutant MultiPoint services, vous pouvez créer quatre comptes d’utilisateur locaux sur l’ordinateur A (User01, User02, user03 et user04) et quatre comptes d’utilisateur locaux sur l’ordinateur B (user05, user06, user07 et user08). Dans ce scénario, les utilisateurs 01\-04 peuvent se connecter à l’ordinateur A à partir de n’importe quelle station qui y est connectée. Toutefois, ils ne peuvent pas se connecter à l’ordinateur B. Il en va de même pour les utilisateurs 05\-08, qui peuvent se connecter uniquement à l’ordinateur B, mais pas à l’ordinateur A. en fonction de l’environnement de déploiement spécifique, cela peut être acceptable ou même souhaitable.  
  
Toutefois, si chaque utilisateur doit être en mesure de se connecter à l’un des ordinateurs exécutant MultiPoint services, un compte d’utilisateur local doit être créé pour chaque utilisateur sur chaque ordinateur qui exécute MultiPoint services. Le choix de gérer les utilisateurs de cette manière introduit certaines complexités. Par exemple, si User01 ouvre une session sur l’ordinateur A le lundi et enregistre un fichier dans le dossier documents, puis que l’utilisateur ouvre une session sur l’ordinateur B le mardi, le fichier qui a été enregistré dans le dossier documents sur l’ordinateur A n’est pas accessible sur l’ordinateur B.  
  
En outre, si un utilisateur dispose de comptes sur l’ordinateur A et sur l’ordinateur B, il n’existe aucun moyen de synchroniser automatiquement les mots de passe des comptes. Cela peut entraîner des difficultés à se connecter au mot de passe du compte sur un ordinateur, mais pas sur l’autre. Vous pouvez simplifier la gestion des comptes d’utilisateur dans ce type d’environnement réseau en affectant chaque utilisateur à un ordinateur unique qui exécute MultiPoint services. De cette façon, l’utilisateur peut se connecter à n’importe quelle station associée à cet ordinateur et accéder aux fichiers appropriés.  
  
## <a name="multiple-multipoint-services-systems-with-domain-accounts"></a>Plusieurs systèmes MultiPoint services avec des comptes de domaine  
Les environnements de domaine sont courants dans les environnements réseau de grande taille qui incluent plusieurs serveurs. Par exemple, vous pouvez joindre un ou plusieurs ordinateurs exécutant le rôle MultiPoint services à un domaine, puis utiliser Microsoft Active Directory pour gérer les comptes d’utilisateurs accessibles depuis n’importe quel ordinateur du domaine. Cela permet de créer et d’accéder à des comptes d’utilisateur de domaine à partir de n’importe quelle station d’un système MultiPoint services joint au domaine.  
 
Lorsque vous déployez MultiPoint services dans un environnement de domaine, plusieurs facteurs sont à prendre en compte :  
  
-   Si les comptes de domaine sont utilisés, ils ne peuvent pas être gérés à partir du gestionnaire MultiPoint.  
  
-   Par défaut, MultiPoint services est configuré pour accorder à chaque utilisateur l’autorisation de se connecter à une seule station à la fois. Si vous décidez d’autoriser les utilisateurs à se connecter à plusieurs stations en même temps à l’aide d’un compte unique, vous pouvez utiliser l’option **modifier les paramètres du serveur** dans le gestionnaire multipoint.  
  
-   L’emplacement des contrôleurs de domaine peut affecter la vitesse et la fiabilité avec lesquels les utilisateurs pourront s’authentifier auprès du domaine et localiser des ressources.  
  
## <a name="single-user-account-for-multiple-stations"></a>Compte d’utilisateur unique pour plusieurs stations  
MultiPoint services a la possibilité de se connecter simultanément à plusieurs stations sur le même ordinateur à l’aide d’un seul compte d’utilisateur. Cette fonctionnalité est utile dans les environnements où les utilisateurs ne reçoivent pas de noms d’utilisateur uniques, et où l’utilisation d’un compte d’utilisateur unique peut simplifier la gestion du système MultiPoint services.  
  
