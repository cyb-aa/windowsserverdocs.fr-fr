---
title: Vue d'ensemble du tableau de bord dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f70a79de-9c56-4496-89b5-20a1bff2293e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c2cc603f5e0303ada245956a524151393c538b27
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881570"
---
# <a name="overview-of-the-dashboard-in-windows-server-essentials"></a>Vue d'ensemble du tableau de bord dans Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Windows Server Essentials et Windows Server 2012 R2 Standard avec le rôle Expérience Windows Server Essentials activé incluent un tableau de bord d'administration, lequel simplifie les tâches de gestion de vos serveur et réseau Windows Server Essentials. À l'aide du tableau de bord Windows Server Essentials, vous pouvez :  
  
-   terminer la configuration de votre serveur ;  
  
-   accéder et effectuer des tâches d'administration courantes ;  
  
-   afficher les alertes du serveur et prendre des mesures en conséquence ;  
  
-   configurer et modifier les paramètres du serveur ;  
  
-   rechercher des rubriques d'aide sur le web et y accéder ;  
  
-   accéder aux ressources de la communauté sur le web ;  
  
-   gérer les comptes d'utilisateur ;  
  
-   gérer les périphériques et les sauvegardes ;  
  
-   gérer l'accès et les paramètres pour les disques durs et les dossiers de serveur ;  
  
-   afficher et gérer les applications complémentaires ;  
  
-   intégrer Microsoft Online Services.  
  
 Cette rubrique inclut :  
  
-   [Fonctionnalités de base du tableau de bord](#BKMK_Design)  
  
-   [Fonctionnalités de la page d’accueil du tableau de bord](#BKMK_Home)  
  
-   [Sections d’administration du tableau de bord](#BKMK_Features)  
  
-   [Accès du tableau de bord Windows Server Essentials](#BKMK_AccessDb)  
  
-   [Utiliser le mode sans échec](#BKMK_UseSafeMode)  
  
##  <a name="BKMK_Design"></a> Fonctionnalités de base du tableau de bord  
 Le tableau de bord Windows Server Essentials vous permet d'accéder rapidement aux informations clés et aux fonctionnalités de gestion de votre serveur. Le tableau de bord inclut plusieurs sections. Le tableau suivant décrit les sections.  
  
 
  
|Élément|Fonctionnalité du tableau de bord|Description|  
|----------|-----------------------|-----------------|  
|1|Barre de navigation|Cliquez sur une section dans la barre de navigation pour accéder aux informations et aux tâches associées à cette section. Chaque fois que vous ouvrez le tableau de bord, la **page d'accueil** apparaît par défaut.|  
|2|Onglets de sous-section|Les onglets de sous-section permettent d'accéder à un second niveau de tâches d'administration Windows Server Essentials.|  
|3|Volet Liste|La liste affiche les objets que vous pouvez gérer et inclut des informations de base sur chaque objet.|  
|4|Volet d'informations|Le volet d'informations affiche des informations supplémentaires sur un objet que vous sélectionnez dans la liste.|  
|5|Volet Tâches|Le volet Tâches contient des liens vers des outils et des informations qui vous aident à gérer les propriétés d'un objet spécifique (par exemple, un compte d'utilisateur ou un ordinateur) ou des paramètres globaux de la catégorie d'objet. Le volet Tâches est divisé en deux sections :<br /><br /> **Tâches de l’objet** section contient des liens vers des outils et des informations qui vous aident à gérer les propriétés d’un objet que vous sélectionnez dans la vue liste (par exemple, un compte d’utilisateur ou un ordinateur).<br /><br /> **Tâches globales** section contient des liens vers des outils et des informations qui vous aident à gérer les tâches globales pour des fonctionnalités. Les tâches globales incluent des tâches pour ajouter de nouveaux objets, définir une stratégie et ainsi de suite.|  
|6|Informations et paramètres|Cette section permet l'accès direct aux paramètres du serveur et fournit un lien vers des informations concernant la page du tableau de bord que vous affichez.|  
|7|État des alertes|L'état des alertes fournit une indication visuelle rapide sur l'intégrité du serveur. Cliquez sur l'image d'alerte pour afficher les alertes critiques et importantes.<br /><br /> **Remarque :** Dans Windows Server Essentials et Windows Server 2012 R2 Standard avec le rôle expérience Windows Server Essentials installé, l’état des alertes est disponible sur le **informations et paramètres** onglet.|  
|8|Barre d'état|La barre d'état affiche le nombre d'objets affichés dans la liste. Les applications complémentaires peuvent également afficher un autre état.|  
  
##  <a name="BKMK_Home"></a> Fonctionnalités de la page d’accueil du tableau de bord  
 Lorsque vous ouvrez le tableau de bord, la **page d'accueil** s'ouvre par défaut avec la catégorie **CONFIGURATION** affichée. La **page d'accueil** du tableau de bord Windows Server Essentials fournit un accès rapide aux tâches et aux informations qui vous permettent de personnaliser votre serveur et de configurer les fonctionnalités clés. La page d'accueil se compose de quatre zones fonctionnelles, qui présentent les informations et les tâches de configuration relatives aux options que vous sélectionnez. Le tableau suivant décrit les fonctionnalités.  
  
|Élément|Fonctionnalité|Description|  
|----------|-------------|-----------------|  
|1|Barre de navigation|Cliquez sur une section dans la barre de navigation pour accéder aux informations et aux tâches associées à cette section. Chaque fois que vous ouvrez le tableau de bord, la **page d’accueil** apparaît par défaut.|  
|2|Volet de catégorie|Ce volet affiche des fonctionnalités qui vous permettent d'accéder rapidement à des informations et à des outils de configuration qui vous aideront à configurer et personnaliser le serveur. Cliquez sur une catégorie pour afficher les tâches et les ressources associées à cette catégorie.|  
|3|Volet Tâches|Ce volet affiche des liens vers des tâches et des informations qui s'appliquent à la catégorie sélectionnée. Cliquez sur une tâche ou une ressource pour afficher des informations supplémentaires.|  
|4|Volet Actions|Ce volet fournit une brève description d'une fonctionnalité ou d'une tâche, et il fournit des liens vers des Assistants de configuration et des pages d'informations. Cliquez sur un lien pour prendre des mesures complémentaires.|  
  
##  <a name="BKMK_Features"></a> Sections d’administration du tableau de bord  
 Le tableau de bord Windows Server Essentials organise les informations réseau et les tâches d'administration en zones fonctionnelles. La page de gestion de chaque zone fonctionnelle fournit des informations sur les objets associés à cette zone, par exemple **Utilisateurs**ou **Périphériques**. La page de gestion comprend également des tâches permettant d'afficher ou de modifier des paramètres, ou encore d'exécuter des programmes qui automatisent des tâches nécessitant plusieurs étapes.  
  
 Le tableau ci-dessous décrit les sections d'administration du tableau de bord qui sont disponibles par défaut après l'installation. Ce tableau répertorie également les tâches disponibles dans chaque section.  
  
|Section|Description|  
|-------------|-----------------|  
|Dossier de base|La **page d'accueil** apparaît par défaut chaque fois que vous ouvrez le tableau de bord. Elle inclut des tâches et des informations dans les catégories suivantes :<br /><br /> **Le programme d’installation** section effectuer les tâches de cette catégorie pour configurer votre serveur pour la première fois. Pour plus d’informations sur ces tâches, consultez [installer et configurer Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials.md).<br /><br /> **E-mail** section Choisissez une option dans cette catégorie pour intégrer un service de messagerie avec le serveur.<br /><br /> **Remarque :** Cette catégorie est uniquement disponible dans Windows Server Essentials.<br /><br /> **SERVICES** section Choisissez une tâche de cette catégorie pour intégrer Microsoft online services avec le serveur.<br /><br /> **Remarque :** Cette catégorie est uniquement disponible dans Windows Server Essentials et Windows Server 2012 R2 Standard avec le rôle expérience Windows Server Essentials activé.<br /><br /> **ADD-INS** section cliquez sur cette catégorie pour installer des compléments précieux pour votre entreprise.<br /><br /> **ÉTAT rapide** section affiche l’état du serveur de haut niveau. Cliquez sur un état pour afficher des informations et les options de configuration de cette fonctionnalité. Si vous effectuez toutes les tâches de la catégorie CONFIGURATION, cette catégorie apparaît en haut du volet de catégorie.<br /><br /> **AIDE** section utiliser la zone de recherche pour rechercher de l’aide sur le Web. Cliquez sur un lien pour visiter le site web relatif à l'option sélectionnée.|  
|Utilisateurs|Pour que les utilisateurs puissent accéder aux ressources fournies par Windows Server Essentials, vous devez créer des comptes d'utilisateur à l'aide du tableau de bord Windows Server Essentials. Après avoir créé des comptes d'utilisateur, vous pouvez gérer ces comptes à l'aide des tâches disponibles dans la page **Utilisateurs** du tableau de bord. Dans cette page, vous pouvez effectuer les tâches suivantes :<br /><br /> -Afficher la liste des comptes d’utilisateur.<br /><br /> -Permet d’afficher et gérer les propriétés du compte utilisateur.<br /><br /> -Activer ou désactiver des comptes d’utilisateur.<br /><br /> -Ajouter ou supprimer des comptes d’utilisateur.<br /><br /> -Affecter des comptes de réseau local pour les comptes Microsoft online services si votre serveur est intégré à Office 365.<br /><br /> -Modifier les mots de passe de compte utilisateur et de gérer la stratégie de mot de passe.<br /><br /> Pour plus d’informations sur la gestion des comptes d’utilisateur, consultez [Manage User Accounts](Manage-User-Accounts-in-Windows-Server-Essentials.md).|  
|Groupes d'utilisateurs|**Remarque :** Cette fonctionnalité est disponible uniquement dans Windows Server Essentials et Windows Server 2012 R2 Standard avec le rôle expérience Windows Server Essentials activé.<br /><br /> Dans cette page, vous pouvez effectuer les tâches suivantes :<br /><br /> -Afficher la liste des groupes d’utilisateurs.<br /><br /> -Permet d’afficher et gérer des groupes d’utilisateurs.<br /><br /> -Ajouter ou supprimer des groupes d’utilisateurs.|  
|Groupes de distribution|**Remarque :** Cette fonctionnalité est disponible uniquement dans Windows Server Essentials et Windows Server 2012 R2 Standard avec le rôle expérience Windows Server Essentials activé. Cet onglet s'affiche uniquement quand Windows Server Essentials est intégré avec Office 365.<br /><br /> Dans cette page, vous pouvez effectuer les tâches suivantes :<br /><br /> -Afficher la liste des groupes de distribution.<br /><br /> -Ajouter ou supprimer des groupes de distribution.|  
|Appareils|Une fois que vous avez connecté des ordinateurs au réseau Windows Server Essentials, vous pouvez gérer les ordinateurs à partir de la page **Périphériques** du tableau de bord. Dans cette page, vous pouvez effectuer les tâches suivantes :<br /><br /> -Afficher la liste des ordinateurs qui sont joints à votre réseau.<br /><br /> -Gérer les appareils mobiles en tirant parti de la fonctionnalité de gestion des appareils mobiles Office 365.<br /><br /> **Remarque :** Cette fonctionnalité est uniquement disponible dans Windows Server Essentials et Windows Server 2012 R2 Standard avec le rôle expérience Windows Server Essentials activé.<br /><br /> -Permet d’afficher les propriétés de l’ordinateur et les alertes d’intégrité pour chaque ordinateur.<br /><br /> -Configurer et gérer des sauvegardes de l’ordinateur.<br /><br /> -Permet de restaurer des fichiers et dossiers sur des ordinateurs.<br /><br /> -Établir une connexion Bureau à distance à un ordinateur<br /><br /> -Personnaliser les paramètres de sauvegarde de l’ordinateur et l’historique des fichiers<br /><br /> Pour plus d’informations sur la gestion des ordinateurs et des sauvegardes, consultez [gérer les appareils](Manage-Devices-in-Windows-Server-Essentials.md).|  
|Stockage|Selon la version de Windows Server Essentials que vous exécutez, la section **Stockage** du tableau de bord contient les sections suivantes par défaut.<br /><br /> -Le **dossiers du serveur** sous-section comprend des tâches qui vous aident à afficher et gérer les propriétés des dossiers du serveur. Cette page inclut également des tâches permettant d'ouvrir et d'ajouter des dossiers du serveur.<br /><br /> -Le **disques durs** page comprend des tâches qui vous aident à afficher et vérifier l’intégrité des disques qui sont attachés au serveur.<br /><br /> -Dans Windows Server Essentials et Windows Server 2012 R2 Standard avec le rôle expérience Windows Server Essentials activé, le **bibliothèques SharePoint** page comprend des tâches qui vous aident à gérer les bibliothèques SharePoint dans Office 365 service.<br /><br /> Pour plus d’informations sur la gestion des dossiers du serveur, consultez [Manage Server Folders](Manage-Server-Folders-in-Windows-Server-Essentials.md).<br /><br /> Pour plus d’informations sur la gestion des disques durs, consultez [Manage Server Storage](Manage-Server-Storage-in-Windows-Server-Essentials.md).|  
|Applications|-Le **Applications** section du tableau de bord Windows Server Essentials contient deux sous-sections par défaut.<br /><br /> Pour plus d’informations sur la gestion des applications complémentaires, consultez [gérer les Applications](Manage-Applications-in-Windows-Server-Essentials.md).<br /><br /> -Le **Add-ins** sous-section affiche une liste des compléments installés et fournit des tâches qui vous permettent de supprimer un complément et pour accéder aux informations supplémentaires sur un complément sélectionné.<br /><br /> -Le **Microsoft Pinpoint** sous-section affiche une liste des applications qui sont disponibles à partir de Microsoft Pinpoint.|  
|Office 365|L'onglet **Office 365** s'affiche uniquement quand Windows Server Essentials est intégré à Microsoft Office 365. Cette section contient des informations sur les comptes d'administrateur et d'abonnement Office 365.|  
  
> [!NOTE]
>  Si vous installez un complément pour le tableau de bord Windows Server Essentials, ce complément peut créer des sections d'administration supplémentaires. Ces sections peuvent apparaître dans la barre de navigation principale ou sous un onglet de sous-section.  
  
##  <a name="BKMK_AccessDb"></a> Accès du tableau de bord Windows Server Essentials  
 Vous pouvez accéder au tableau de bord à l'aide des différentes méthodes répertoriées ci-dessous. La méthode que vous choisissez varie selon que vous accédez au tableau de bord à partir du serveur, à partir d'un ordinateur connecté au réseau Windows Server Essentials ou à partir d'un ordinateur distant.  
  
 Pour aider à tenir à jour un réseau sécurisé, seuls les utilisateurs disposant d'autorisations d'administration peuvent accéder au tableau de bord Windows Server Essentials. En outre, vous ne pouvez pas utiliser le compte Administrateur intégré pour vous connecter au tableau de bord Windows Server Essentials à partir de Launchpad.  
  
###  <a name="BKMK_Server"></a> Le tableau de bord à partir du serveur d’accès  
 Quand vous installez Windows Server Essentials, le processus d'installation crée un raccourci vers le tableau de bord sur l'**écran d'accueil**, ainsi que sur le Bureau. Si aucun raccourci ne figure dans ces emplacements, vous pouvez utiliser le volet **Rechercher** pour rechercher et exécuter le programme de tableau de bord.  
  
##### <a name="to-access-the-dashboard-from-the-server"></a>Pour accéder au tableau de bord à partir du serveur  
  
-   Connectez-vous au serveur en tant qu'administrateur, puis effectuez l'une des opérations suivantes :  
  
    -   Sur l' **écran d'accueil** , cliquez sur **Tableau de bord**.  
  
    -   Sur le Bureau, double-cliquez sur **Tableau de bord**.  
  
    -   Dans le volet **Rechercher** , tapez **dashboard**, puis cliquez sur **Tableau de bord** dans les résultats de recherche.  
  
###  <a name="BKMK_Network"></a> Le tableau de bord à partir d’un ordinateur est connecté au réseau d’accès  
 Dans Windows Server Essentials, une fois que vous avez joint un ordinateur au réseau, les administrateurs peuvent utiliser Launchpad pour accéder au tableau de bord du serveur à partir de l'ordinateur.  
  
> [!WARNING]
>  Dans Windows Server Essentials, pour vous connecter au tableau de bord à partir de l’ordinateur client, cliquez sur l’icône de barre d’état, puis ouvrez le tableau de bord dans le menu contextuel.  
  
##### <a name="to-access-the-dashboard-by-using-the-launchpad"></a>Pour accéder au tableau de bord à l'aide de Launchpad  
  
1.  À partir d'un ordinateur joint au réseau, ouvrez **Launchpad**.  
  
2.  Dans le menu de Launchpad, cliquez sur **Tableau de bord**.  
  
3.  Dans la page **Connexion** du tableau de bord, tapez vos informations d'identification d'administrateur réseau et appuyez sur Entrée.  
  
     Une connexion à distance au tableau de bord Windows Server Essentials s'ouvre.  
  
###  <a name="BKMK_Remote"></a> Le tableau de bord d’accès à partir d’un ordinateur distant  
 Lorsque vous travaillez à partir d’un ordinateur distant, vous pouvez accéder au tableau de bord Windows Server Essentials en utilisant le site accès Web à distance.  
  
##### <a name="to-access-the-dashboard-by-using-remote-web-access"></a>Pour accéder au tableau de bord à l'aide de l'accès web à distance  
  
1.  À partir de l’ordinateur distant, ouvrez un navigateur Internet, tel qu’Internet Explorer.  
  
2.  Dans la barre Adresse du navigateur Internet, tapez la commande suivante :**https://<DomainName\>/distant**, où *DomainName* est le nom de domaine externe de votre organisation (par exemple, contoso.com).  
  
3.  Tapez votre nom d’utilisateur et le mot de passe pour vous connecter au site accès Web à distance.  
  
4.  Dans le **ordinateurs** section de l’accès Web à distance**accueil** page, localisez votre serveur et cliquez sur **Connect**.  
  
     Une connexion à distance au tableau de bord s'ouvre.  
  
    > [!NOTE]
    >  Si votre serveur n'apparaît pas dans la section **Ordinateurs** de la page d'accueil, cliquez sur **Se connecter à d'autres ordinateurs**, recherchez votre serveur dans la liste, puis cliquez sur le serveur pour vous y connecter.  
    >   
    >  Pour autoriser un utilisateur à accéder à du tableau de bord à partir de l’accès Web à distance, ouvrez la page de propriétés pour le compte d’utilisateur, puis sélectionnez le **tableau de bord Server** option sur le **accès en tout lieu** onglet.  
  
##  <a name="BKMK_UseSafeMode"></a> Utiliser le mode sans échec  
 La fonctionnalité de mode sans échec de Windows Server Essentials surveille les compléments installés sur le serveur. Si le tableau de bord devient instable ou ne répond pas, le mode sans échec détecte et isole les compléments susceptibles d'être à l'origine du problème et les affiche dans la page **Paramètres du mode sans échec** la prochaine fois que vous ouvrez le tableau de bord. Dans la page **Paramètres du mode sans échec**, vous pouvez désactiver et activer des compléments un par un pour déterminer le complément qui provoque le problème. Vous pouvez ensuite choisir de laisser le complément désactivé pour des démarrages futurs ou vous pouvez désinstaller le complément.  
  
 Vous pouvez afficher la liste de tous les compléments installés sur le serveur à un moment quelconque. Vous pouvez également ouvrir le tableau de bord en mode sans échec, ce qui désactive automatiquement tous les compléments non-Microsoft. Windows Server Essentials vous permet également d'accéder au tableau de bord en mode sans échec à partir d'un autre ordinateur sur le réseau.  
  
#### <a name="to-view-a-list-of-installed-add-ins"></a>Pour afficher la liste des compléments installés  
  
-   Dans le tableau de bord, cliquez sur **Aide**, puis sur **Paramètres du mode sans échec**.  
  
#### <a name="to-open-the-dashboard-in-safe-mode"></a>Pour ouvrir le tableau de bord en mode sans échec  
  
-   Dans le volet **Rechercher** , tapez **safe**, puis cliquez sur **Tableau de bord (mode sans échec)** dans les résultats de recherche.  
  
#### <a name="to-open-the-dashboard-in-safe-mode-from-another-computer-on-the-network"></a>Pour ouvrir le tableau de bord en mode sans échec à partir d'un autre ordinateur sur le réseau  
  
1.  À partir d'un ordinateur connecté au réseau, ouvrez Launchpad de Windows Server Essentials, puis cliquez sur **Tableau de bord**.  
  
2.  Dans la page de connexion du tableau de bord, tapez le nom d'utilisateur et le mot de passe d'un compte qui possède l'autorisation de se connecter au serveur. Cochez la case **M'autoriser à sélectionner les compléments à charger**, puis cliquez sur la flèche pour vous connecter.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer des Applications](Manage-Applications-in-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)
