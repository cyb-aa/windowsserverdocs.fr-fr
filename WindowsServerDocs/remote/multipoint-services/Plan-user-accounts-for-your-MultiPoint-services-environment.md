---
title: Planifier des comptes d’utilisateur pour votre environnement MultiPoint Services
description: Informations de planification pour les comptes d’utilisateur dans MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d47be540-e891-47bd-85da-6df4bbf93b2f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 02862c1a317dfe5deff75be4a80595c8dc8bc3f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864170"
---
# <a name="plan-user-accounts-for-your-multipoint-services-environment"></a>Planifier des comptes d’utilisateur pour votre environnement MultiPoint Services
La meilleure façon d’implémenter des comptes d’utilisateur dans MultiPoint Services dépend de la taille et la complexité de votre déploiement :  
  
-   **Comptes d’utilisateurs locaux** - pour un déploiement de petite taille avec peu d’ordinateurs des Services en cours d’exécution MultiPoind et peu d’utilisateurs, il peut s’avérer plus pratique d’utiliser *comptes d’utilisateurs locaux* qui sont créés sur MultiPoint Services. Vous pouvez créer un compte individuel pour chaque personne qui utilisent le système, ou créez un compte générique pour chaque station, tout le monde peut utiliser pour se connecter. Les administrateurs de multiPoint Services, créer et gérer des comptes d’utilisateurs locaux à l’aide du gestionnaire MultiPoint. Les comptes locaux peuvent être administrateurs, ont des droits d’administration limités ou les utilisateurs standard sans accès au bureau de Services MultiPoint ou au gestionnaire MultiPoint.  
  
-   **Comptes de domaine** -si votre environnement comprend de nombreux ordinateurs exécutant MultiPoint Services et nombreux utilisateurs, vous probablement il sera utile plus configurer un Active Directory Domain Services \(AD DS\) domaine et l’utilisation *comptes d’utilisateur de domaine*, qui permettent à un utilisateur d’accéder à son propre profil utilisateur et paramètres à partir de n’importe quelle station dans le domaine. Comptes d’utilisateur de domaine doivent être créés sur le contrôleur de domaine par un administrateur de domaine.  
  
> [!NOTE]  
> Les sections suivantes décrivent des scénarios que vous pouvez implémenter pour les comptes d’utilisateurs locaux dans MultiPoint Services. Si vous utilisez des comptes d’utilisateur de domaine, consultez le scénario « un ou plusieurs MultiPoint serveurs dans un environnement de réseau de domaine » dans [exemples de scénarios : Comptes d’utilisateur multiPoint Services](Example-scenarios--MultiPoint-Services-user-accounts.md).  
  
## <a name="planning-local-user-accounts"></a>Planification des comptes d’utilisateurs locaux  
Les sections suivantes prendre en compte les avantages, les inconvénients et les exigences de plusieurs façons d’implémenter des comptes d’utilisateurs locaux individuelle ou partagée dans votre environnement Windows MultiPoint Services.  
  
### <a name="use-individual-local-user-accounts"></a>Utiliser des comptes d’utilisateur local individuels  
Lorsque vous créez des comptes d’utilisateurs locaux, vous avez les deux approches d’option.  Attribuer à chaque utilisateur à un serveur particulier qui exécute MultiPoint Services et la création d’un compte unique pour chaque utilisateur. Ou créez des comptes d’utilisateur local pour tous les utilisateurs sur chaque ordinateur qui exécute Multipoint services. Des principaux avantages de l’implémentation de comptes d’utilisateur individuels sont que chaque utilisateur a sa propre expérience de bureau Windows qui inclut des dossiers privés pour stocker les données. 
  
Système en matière de gestion, l’affectation d’utilisateurs à un ordinateur MultiPoint Services spécifique peut être plus facile. Par exemple, si vous avez deux serveurs MultiPoint avec cinq stations, vous pouvez créer des comptes d’utilisateurs locaux comme illustré dans le tableau suivant.  
  
**Tableau 1 : Affectation de comptes d’utilisateurs locaux à des ordinateurs spécifiques en cours d’exécution MultiPoint Services**  
  
|Ordinateur A|Ordinateur B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_06|  
|UserAccount_02|UserAccount_07|  
|UserAccount_03|UserAccount_08|  
|UserAccount_04|UserAccount_09|  
|UserAccount_05|UserAccount_10|  
  
Dans ce scénario, chaque utilisateur dispose d’un compte unique sur un ordinateur particulier. Par conséquent, tous les utilisateurs disposant d’un compte local sur l’ordinateur A peuvent se connecter à ses compte à partir de n’importe quelle station associée à l’ordinateur A. Toutefois, ces utilisateurs ne peuvent pas accéder à leurs comptes s’ils utilisent une station associée avec l’ordinateur B et vice versa. L’avantage de cette approche est que, en connectant toujours sur le même ordinateur, les utilisateurs peuvent toujours rechercher et accéder à leurs fichiers.  
  
En revanche, il est également possible de répliquer des comptes d’utilisateur individuels sur tous les ordinateurs qui exécutent MultiPoint Services, comme illustré dans le tableau suivant.  
  
**Tableau 2 : Réplication des comptes d’utilisateur sur tous les ordinateurs exécutant MultiPoint Services**  
  
|Ordinateur A|Ordinateur B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_01|  
|UserAccount_02|UserAccount_02|  
|UserAccount_03|UserAccount_03|  
|UserAccount_04|UserAccount_04|  
|UserAccount_05|UserAccount_05|  
  
L’avantage de cette approche est que les utilisateurs ont un compte d’utilisateur local sur tous les Services MultiPoint disponibles. Toutefois, les inconvénients peuvent compenser cet avantage. Par exemple, même si le nom d’utilisateur et le mot de passe pour une personne donnée sont les mêmes sur les deux ordinateurs, les comptes ne sont pas liés entre eux. Par conséquent, si un utilisateur ouvre une session sur son ou son compte sur l’ordinateur A le lundi, enregistre un fichier et ouvre une session son compte sur l’ordinateur B le mardi, il est impossible d’accéder au fichier précédemment enregistré sur l’ordinateur A. en outre , réplication des comptes d’utilisateur sur plusieurs ordinateurs augmente les exigences de stockage et de la surcharge d’administration.  
  
### <a name="use-generic-local-user-accounts"></a>Utiliser des comptes d’utilisateurs locaux générique  
Si votre système MultiPoint Services n’est pas connecté à un domaine, et que vous ne souhaitez pas créer un compte individuel pour chaque utilisateur, vous pouvez créer des comptes génériques pour chaque station. Par exemple, si vous avez deux ordinateurs qui exécutent MultiPoint Services, et cinq stations sont associées à chaque ordinateur, vous pouvez décider de créer des comptes d’utilisateur semblables à celles présentées dans le tableau suivant.  
  
**Tableau 3 : Création de comptes d’utilisateur générique, un seul compte par station**  
  
|Ordinateur A|Ordinateur B|  
|--------------|--------------|  
|Computer_A-Station_01|Computer_B-Station_01|  
|Computer_A-Station_02|Computer_B-Station_02|  
|Computer_A-Station_03|Computer_B-Station_03|  
|Computer_A-Station_04|Computer_B-Station_04|  
|Computer_A-Station_05|Computer_B-Station_05|  
  
Dans ce scénario, chaque compte de la console a le même mot de passe, et les mots de passe et les noms de compte d’utilisateur générique sont disponibles à tous les utilisateurs. L’avantage de cette approche est que la surcharge de gestion des comptes d’utilisateur est susceptible d’être moins si vous utilisez des comptes individuels, car il existe généralement moins stations que les utilisateurs. En outre, la surcharge engendrée par la réplication des comptes d’utilisateur sur chaque serveur est éliminée.  
  
Une autre option consiste à créer des comptes génériques sur chaque serveur. Chaque utilisateur se connecte à un serveur sous le même compte. Pour cela, vous devez activer plusieurs sessions par compte. Vous pouvez simplifier davantage en utilisant le même nom de compte et le mot de passe sur tous les serveurs. Cela simplifie l’ouverture de session pour les utilisateurs, qui doivent uniquement connaître un nom de compte et mot de passe à utiliser n’importe quelle station sur n’importe quel serveur. Il convient de noter que, dans ce scénario tous les utilisateurs peuvent voir toute modification qui permet de n’importe quel utilisateur. Par exemple, si un fichier est enregistré sur le bureau, tous les utilisateurs peuvent voir le fichier.  
  
> [!IMPORTANT]  
> Il est important de comprendre que lorsque des utilisateurs partagent un compte d’utilisateur, une par serveur ou un par station, les fichiers enregistrés sur le serveur, voire des fichiers enregistrés dans Mes Documents - ne sont pas privés. Tout utilisateur qui ouvre une session avec le compte a accès à ces fichiers. Lorsque vous utilisez un seul compte par station, si un utilisateur enregistre des fichiers dans Mes Documents sur un poste, l’utilisateur n’a pas accès à ces fichiers sur une autre station. Le même se produit lors de la connexion à différents ordinateurs de MultiPoint Services.  
  
Pour permettre aux utilisateurs pour accéder à leurs fichiers à partir de n’importe quelle station, vous pouvez utiliser un serveur de fichiers, créez un partage de fichiers pour chaque compte d’utilisateur ou permettre aux utilisateurs de stocker leurs documents personnels sur un lecteur flash USB ou tout autre périphérique de stockage privé. Des disques mémoire flash USB autoriser des utilisateurs individuels stocker des documents privés même s’ils partagent un compte d’utilisateur sur un MultiPoint Services.