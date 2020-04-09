---
title: Planifier des comptes d’utilisateur pour votre environnement MultiPoint Services
description: Informations de planification pour les comptes d’utilisateur dans MultiPoint services
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: d47be540-e891-47bd-85da-6df4bbf93b2f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 28ee7a1475ec55352fe344842b8df7633abb9137
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853392"
---
# <a name="plan-user-accounts-for-your-multipoint-services-environment"></a>Planifier des comptes d’utilisateur pour votre environnement MultiPoint Services
La meilleure façon d’implémenter des comptes d’utilisateur dans MultiPoint services dépend de la taille et de la complexité de votre déploiement :  
  
-   **Comptes d’utilisateurs locaux** : pour un petit déploiement avec seulement quelques ordinateurs exécutant les services MultiPoind et quelques utilisateurs, il peut s’avérer plus pratique d’utiliser des *comptes d’utilisateur locaux* créés sur multipoint services. Vous pouvez créer un compte individuel pour chaque personne qui utilisera le système ou créer un compte générique pour chaque station, que tout le monde peut utiliser pour se connecter. Les administrateurs MultiPoint services créent et gèrent les comptes d’utilisateurs locaux à l’aide du gestionnaire MultiPoint. Les comptes locaux peuvent être des administrateurs, disposer de droits d’administration limités ou être des utilisateurs standard sans accès au bureau MultiPoint services ou au gestionnaire MultiPoint.  
  
-   **Comptes de domaine** : Si votre environnement comporte de nombreux ordinateurs exécutant multipoint services et de nombreux utilisateurs, il peut s’avérer plus utile de configurer un Active Directory Domain Services \(AD DS\) domaine et d’utiliser des *comptes d’utilisateur de domaine*, qui permettent à un utilisateur d’accéder à son propre profil utilisateur et à ses paramètres à partir de n’importe quelle station du domaine. Les comptes d’utilisateur de domaine doivent être créés sur le contrôleur de domaine par un administrateur de domaine.  
  
> [!NOTE]  
> Les sections suivantes abordent les scénarios que vous pouvez implémenter pour les comptes d’utilisateurs locaux dans MultiPoint services. Si vous utilisez des comptes d’utilisateur de domaine, consultez le scénario « un ou plusieurs serveurs MultiPoint dans un environnement réseau de domaine » dans [exemples de scénarios : comptes d’utilisateur multipoint services](Example-scenarios--MultiPoint-Services-user-accounts.md).  
  
## <a name="planning-local-user-accounts"></a>Planification de comptes d’utilisateurs locaux  
Les sections suivantes étudient les avantages, les inconvénients et les exigences de plusieurs façons d’implémenter des comptes d’utilisateur locaux ou partagés dans votre environnement Windows MultiPoint services.  
  
### <a name="use-individual-local-user-accounts"></a>Utiliser des comptes d’utilisateur locaux individuels  
Lors de la création de comptes d’utilisateurs locaux, vous avez le choix entre deux approches.  Affectez chaque utilisateur à un serveur particulier exécutant MultiPoint services et en créant un compte unique pour chaque utilisateur. Ou créez des comptes d’utilisateurs locaux pour tous les utilisateurs sur chaque ordinateur exécutant multipoint services. L’un des principaux avantages de l’implémentation de comptes d’utilisateur individuels est que chaque utilisateur dispose de sa propre expérience de bureau Windows qui inclut des dossiers privés pour le stockage des données. 
  
Du point de vue de la gestion du système, l’attribution d’utilisateurs à un ordinateur MultiPoint services spécifique peut être plus pratique. Par exemple, si vous avez deux serveurs MultiPoint avec cinq stations chacune, vous pouvez créer des comptes d’utilisateur locaux, comme illustré dans le tableau suivant.  
  
**Tableau 1 : attribution de comptes d’utilisateurs locaux à des ordinateurs spécifiques exécutant MultiPoint services**  
  
|Ordinateur A|Ordinateur B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_06|  
|UserAccount_02|UserAccount_07|  
|UserAccount_03|UserAccount_08|  
|UserAccount_04|UserAccount_09|  
|UserAccount_05|UserAccount_10|  
  
Dans ce scénario, chaque utilisateur a un compte unique sur un ordinateur particulier. Par conséquent, toute personne disposant d’un compte local sur l’ordinateur A peut se connecter à son compte ou à son compte à partir de n’importe quelle station associée à l’ordinateur A. Toutefois, ces utilisateurs ne peuvent pas accéder à leurs comptes s’ils utilisent une station associée à l’ordinateur B, et vice versa. L’avantage de cette approche est que, en se connectant toujours au même ordinateur, les utilisateurs peuvent toujours trouver et accéder à leurs fichiers.  
  
En revanche, il est également possible de répliquer des comptes d’utilisateur individuels sur tous les ordinateurs exécutant MultiPoint services, comme illustré dans le tableau suivant.  
  
**Tableau 2 : réplication des comptes d’utilisateur sur tous les ordinateurs exécutant MultiPoint services**  
  
|Ordinateur A|Ordinateur B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_01|  
|UserAccount_02|UserAccount_02|  
|UserAccount_03|UserAccount_03|  
|UserAccount_04|UserAccount_04|  
|UserAccount_05|UserAccount_05|  
  
L’un des avantages de cette approche est que les utilisateurs disposent d’un compte d’utilisateur local sur chaque service MultiPoint disponible. Toutefois, les inconvénients peuvent l’emporter sur cet avantage. Par exemple, même si le nom d’utilisateur et le mot de passe d’une personne particulière sont identiques sur les deux ordinateurs, les comptes ne sont pas liés entre eux. Par conséquent, si un utilisateur se connecte à son compte sur l’ordinateur A le lundi, enregistre un fichier, puis ouvre une session sur son compte sur l’ordinateur B le mardi, il ne peut pas accéder au fichier enregistré précédemment sur l’ordinateur A. en outre, la réplication des comptes d’utilisateur sur plusieurs ordinateurs augmente la charge administrative et les besoins de stockage.  
  
### <a name="use-generic-local-user-accounts"></a>Utiliser des comptes d’utilisateurs locaux génériques  
Si votre système MultiPoint services n’est pas connecté à un domaine et que vous ne souhaitez pas créer de compte individuel pour chaque utilisateur, vous pouvez créer des comptes génériques pour chaque station. Par exemple, si vous avez deux ordinateurs exécutant MultiPoint services et que cinq stations sont associées à chaque ordinateur, vous pouvez décider de créer des comptes d’utilisateur similaires à ceux répertoriés dans le tableau suivant.  
  
**Tableau 3 : création de comptes d’utilisateurs génériques, un compte par station**  
  
|Ordinateur A|Ordinateur B|  
|--------------|--------------|  
|Computer_A-Station_01|Computer_B-Station_01|  
|Computer_A-Station_02|Computer_B-Station_02|  
|Computer_A-Station_03|Computer_B-Station_03|  
|Computer_A-Station_04|Computer_B-Station_04|  
|Computer_A-Station_05|Computer_B-Station_05|  
  
Dans ce scénario, chaque compte de station a le même mot de passe, et les mots de passe et les noms de compte d’utilisateur générique sont disponibles pour tous les utilisateurs. L’avantage de cette approche est que la surcharge liée à la gestion des comptes d’utilisateur est susceptible d’être inférieure à l’utilisation de comptes individuels, car il y a généralement moins de stations que d’utilisateurs. En outre, la surcharge causée par la réplication des comptes d’utilisateur sur chaque serveur est éliminée.  
  
Une autre option consiste à créer des comptes génériques sur chaque serveur. Chaque utilisateur se connecte à un serveur en tant que compte identique. Pour ce faire, vous devez activer plusieurs sessions par compte. Vous pouvez simplifier la simplification en utilisant le même nom de compte et le même mot de passe sur tous les serveurs. Cela simplifie l’ouverture de session pour les utilisateurs, qui n’ont besoin que de connaître un nom de compte et un mot de passe pour utiliser n’importe quelle station sur n’importe quel serveur. Il convient de noter que dans ce scénario, tous les utilisateurs peuvent voir toutes les modifications apportées par l’utilisateur. Par exemple, si un fichier est enregistré sur le bureau, tous les utilisateurs peuvent voir le fichier.  
  
> [!IMPORTANT]  
> Il est important de comprendre que lorsque les utilisateurs partagent un compte d’utilisateur, soit un par serveur, soit un par poste, les fichiers enregistrés sur le serveur (même les fichiers enregistrés dans mes documents) ne sont pas privés. Tout utilisateur qui ouvre une session avec le compte a accès à ces fichiers. Lorsque vous utilisez un compte par station, si un utilisateur enregistre des fichiers dans mes documents sur une station, l’utilisateur n’a pas accès à ces fichiers sur une autre station. Il en est de même lorsque vous vous connectez à différents ordinateurs MultiPoint services.  
  
Pour permettre aux utilisateurs d’accéder à leurs fichiers à partir de n’importe quelle station, vous pouvez utiliser un serveur de fichiers, créer un partage de fichiers pour chaque compte d’utilisateur ou laisser les utilisateurs stocker leurs documents personnels sur un lecteur flash USB ou un autre dispositif de stockage privé. Les lecteurs flash USB permettent à des utilisateurs individuels de stocker des documents privés, même s’ils partagent un compte d’utilisateur sur des services MultiPoint.