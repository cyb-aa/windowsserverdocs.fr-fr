---
title: "Vue d’ensemble du tableau de bord dans WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="overview-of-the-dashboard-in-windows-server-essentials"></a>Vue d’ensemble du tableau de bord dans WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials
 
 WindowsServerEssentials et Windows Server2012R2 Standard avec le rôle expérience WindowsServerEssentials activé incluent un tableau de bord d’administration, ce qui simplifie les tâches que vous exécutez pour gérer votre réseau WindowsServerEssentials et le serveur. À l’aide du tableau de bord WindowsServerEssentials, vous pouvez:  
  
-   Terminer la configuration de votre serveur  
  
-   Accéder et effectuer des tâches d’administration courantes  
  
-   Afficher les alertes du serveur et d’agir sur ces derniers  
  
-   Définir et modifier les paramètres du serveur  
  
-   Accès ou rechercher des rubriques d’aide sur le web  
  
-   Accéder aux ressources de la Communauté sur le web  
  
-   Gérer les comptes d’utilisateur  
  
-   Gérer les appareils et des sauvegardes  
  
-   Gérer l’accès et les paramètres pour les disques durs et les dossiers du serveur  
  
-   Afficher et gérer les applications complémentaires  
  
-   Intégrer à Microsoft online services  
  
 Cette rubrique inclut:  
  
-   [Fonctionnalités de base du tableau de bord](#BKMK_Design)  
  
-   [Fonctionnalités de la page d’accueil du tableau de bord](#BKMK_Home)  
  
-   [Sections d’administration du tableau de bord](#BKMK_Features)  
  
-   [Accès du tableau de bord WindowsServerEssentials](#BKMK_AccessDb)  
  
-   [Utiliser le mode sans échec](#BKMK_UseSafeMode)  
  
##  <a name="BKMK_Design"></a>Fonctionnalités de base du tableau de bord  
 Le tableau de bord WindowsServerEssentials vous permet d’accéder rapidement aux fonctionnalités clés de gestion des informations et de votre serveur. Le tableau de bord inclut plusieurs sections. Le tableau qui suit décrit les sections.  
  
 
  
|Élément|Fonctionnalité du tableau de bord|Description|  
|----------|-----------------------|-----------------|  
|1|Barre de navigation|Cliquez sur une section sur la barre de Navigation pour accéder aux informations et aux tâches associées à cette section. Chaque fois que vous ouvrez le tableau de bord, les **accueil** page s’affiche par défaut.|  
|2|Onglets de sous-section|Les onglets de sous-section donnent accès à une seconde couche de tâches d’administration WindowsServerEssentials.|  
|3|Volet Liste|La liste affiche les objets que vous pouvez gérer et inclut des informations de base sur chaque objet.|  
|4|Volet d’informations|Le volet d’informations affiche des informations supplémentaires sur un objet que vous sélectionnez dans la liste.|  
|5|Volet des tâches|Le volet Tâches contient des liens vers des outils et informations qui vous aident à gérer les propriétés pour un objet spécifique (par exemple, un compte d’utilisateur ou un ordinateur) ou des paramètres globaux de la catégorie d’objet. Le volet de tâches est divisé en deux sections:<br /><br /> **Tâches associées aux objets** section contient des liens vers des outils et informations qui vous aident à gérer les propriétés d’un objet que vous sélectionnez dans la liste (par exemple, un compte d’utilisateur ou un ordinateur).<br /><br /> **Tâches globales** section contient des liens vers des outils et informations qui vous aident à gérer les tâches globales pour des fonctionnalités. Tâches globales incluent des tâches pour ajouter de nouveaux objets, définir la stratégie et ainsi de suite.|  
|6|Informations et paramètres|Cette section fournit un accès direct pour les paramètres du serveur et un lien vers des informations sur la page du tableau de bord que vous visualisez.|  
|7|État des alertes|L’état des alertes fournit une indication visuelle rapide sur l’intégrité du serveur. Cliquez sur l’image d’alerte pour afficher les alertes critiques et importantes.<br /><br /> **Remarque:** dans WindowsServerEssentials et Windows Server2012R2 Standard avec le rôle expérience WindowsServerEssentials installé, l’état des alertes est disponible sur le **informations et des paramètres** onglet.|  
|8|Barre d’état|La barre d’état affiche le nombre d’objets qui apparaissent dans la liste. Applications Add-in peuvent également afficher un autre état.|  
  
##  <a name="BKMK_Home"></a>Fonctionnalités de la page d’accueil du tableau de bord  
 Lorsque vous ouvrez le tableau de bord, les **accueil** page s’affiche par défaut avec la **le programme d’installation** catégorie affichée. Le **accueil** page du tableau de bord WindowsServerEssentials fournit un accès rapide aux tâches et les informations que vous aident à Personnalisez votre serveur et configurez les fonctionnalités clés. La page d’accueil se compose de quatre zones fonctionnelles, qui exposent les tâches de configuration et des informations pour les options que vous sélectionnez. Le tableau qui suit décrit les fonctionnalités.  
  
|Élément|Fonctionnalité|Description|  
|----------|-------------|-----------------|  
|1|Barre de navigation|Cliquez sur une section sur la barre de Navigation pour accéder aux informations et aux tâches associées à cette section. Chaque fois que vous ouvrez le tableau de bord, les **accueil** page affiche par défaut.|  
|2|Volet de catégorie|Ce volet affiche des fonctionnalités qui permettent d’accéder rapidement aux informations et outils de configuration qui vous aident à configurer et personnaliser le serveur. Cliquez sur une catégorie pour afficher les tâches et les ressources qui sont associés à cette catégorie.|  
|3|Volet des tâches|Ce volet affiche des liens vers les tâches et les informations qui s’appliquent à la catégorie sélectionnée. Cliquez sur une tâche ou une ressource pour afficher des informations supplémentaires.|  
|4|Volet Actions|Ce volet fournit une brève description d’une fonctionnalité ou d’une tâche et fournit des liens vers des Assistants de configuration et les pages d’informations. Cliquez sur un lien permettant de prendre des mesures complémentaires.|  
  
##  <a name="BKMK_Features"></a>Sections d’administration du tableau de bord  
 Le tableau de bord WindowsServerEssentials organise les informations de réseau et les tâches d’administration en zones fonctionnelles. La page de gestion pour chaque zone fonctionnelle fournit des informations sur les objets associés à cette zone, par exemple **utilisateurs**, ou **périphériques**. La page de gestion comprend également les tâches que vous pouvez utiliser pour afficher ou modifier les paramètres, ou pour exécuter des programmes qui automatisent les tâches nécessitant plusieurs étapes.  
  
 Le tableau suivant décrit les sections d’administration du tableau de bord qui sont disponibles par défaut après l’installation. Le tableau répertorie également les tâches qui sont disponibles dans chaque section.  
  
|Section|Description|  
|-------------|-----------------|  
|Accueil|Le **accueil** page s’affiche par défaut chaque fois que vous ouvrez le tableau de bord. Elle inclut les tâches et les informations dans les catégories suivantes:<br /><br /> **Le programme d’installation** section effectuer les tâches de cette catégorie pour configurer votre serveur pour la première fois. Pour plus d’informations sur ces tâches, consultez [installer et configurer WindowsServerEssentials](../install/Install-and-Configure-Windows-Server-Essentials.md).<br /><br /> **MESSAGERIE** section Choisir une option dans cette catégorie pour intégrer un service de messagerie avec le serveur.<br /><br /> **Remarque:** cette catégorie est disponible uniquement dans WindowsServerEssentials.<br /><br /> **SERVICES** section Choisissez une tâche de cette catégorie pour intégrer Microsoft online services avec le serveur.<br /><br /> **Remarque:** cette catégorie est uniquement disponible dans WindowsServerEssentials et Windows Server2012R2 Standard avec le rôle expérience WindowsServerEssentials activé.<br /><br /> **Compléments** section cliquez sur cette catégorie pour installer des compléments précieux pour votre entreprise.<br /><br /> **ÉTAT rapide** section affiche l’état de haut niveau de serveur. Cliquez sur un état pour afficher les options de configuration et les informations de cette fonctionnalité. Si vous effectuez toutes les tâches dans la catégorie de paramétrage, cette catégorie apparaît en haut du volet de catégorie.<br /><br /> **AIDE** section utiliser la zone de recherche pour rechercher de l’aide sur le Web. Cliquez sur un lien pour visiter le site Web de l’option sélectionnée.|  
|Utilisateurs|Pour les utilisateurs à accéder aux ressources de WindowsServerEssentials fournit, vous devez créer des comptes d’utilisateur à l’aide du tableau de bord WindowsServerEssentials. Après avoir créé les comptes d’utilisateur, vous pouvez gérer les comptes à l’aide des tâches qui sont disponibles sur le **utilisateurs** page du tableau de bord. Les tâches que vous pouvez effectuer sur cette page incluent:<br /><br /> -Afficher la liste des comptes d’utilisateur.<br /><br /> -Permet d’afficher et gérer les propriétés du compte d’utilisateur.<br /><br /> -Permet d’activer ou désactiver des comptes d’utilisateur.<br /><br /> -Ajouter ou supprimer des comptes d’utilisateur.<br /><br /> -Affecter des comptes de réseau local pour les comptes Microsoft online services si votre serveur est intégré à Office 365.<br /><br /> -Modifier les mots de passe de compte utilisateur et de gérer la stratégie de mot de passe.<br /><br /> Pour plus d’informations sur la gestion des comptes d’utilisateur, voir [gérer les comptes d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md).|  
|Groupes d’utilisateurs|**Remarque:** cette fonctionnalité est disponible uniquement dans WindowsServerEssentials et Windows Server2012R2 Standard avec le rôle expérience WindowsServerEssentials activé.<br /><br /> Les tâches que vous pouvez effectuer sur cette page incluent:<br /><br /> -Afficher la liste des groupes d’utilisateurs.<br /><br /> -Permet d’afficher et gérer des groupes d’utilisateurs.<br /><br /> -Ajouter ou supprimer des groupes d’utilisateurs.|  
|Groupes de distribution|**Remarque:** cette fonctionnalité est disponible uniquement dans WindowsServerEssentials et Windows Server2012R2 Standard avec le rôle expérience WindowsServerEssentials activé. Cet onglet s’affiche uniquement quand WindowsServerEssentials est intégré à Office 365.<br /><br /> Les tâches que vous pouvez effectuer sur cette page incluent:<br /><br /> -Afficher la liste des groupes de distribution.<br /><br /> -Ajouter ou supprimer des groupes de distribution.|  
|Périphériques|Une fois que vous vous connectez des ordinateurs au réseau WindowsServerEssentials, vous pouvez gérer les ordinateurs à partir de la **périphériques** page du tableau de bord. Les tâches que vous pouvez effectuer sur cette page incluent:<br /><br /> -Afficher la liste des ordinateurs qui sont joints à votre réseau.<br /><br /> -Gérer les périphériques mobiles en tirant parti de la fonctionnalité de gestion des périphériques mobiles Office 365.<br /><br /> **Remarque:** cette fonctionnalité est uniquement disponible dans WindowsServerEssentials et Windows Server2012R2 Standard avec le rôle expérience WindowsServerEssentials activé.<br /><br /> -Permet d’afficher les propriétés de l’ordinateur et les alertes d’intégrité pour chaque ordinateur.<br /><br /> -Configurer et gérer les sauvegardes de l’ordinateur.<br /><br /> -Permet de restaurer des fichiers et dossiers sur les ordinateurs.<br /><br /> -Établir une connexion Bureau à distance à un ordinateur<br /><br /> -Personnaliser les paramètres de sauvegarde de l’ordinateur et de l’historique des fichiers<br /><br /> Pour plus d’informations sur la gestion des ordinateurs et des sauvegardes, consultez [gérer les appareils](Manage-Devices-in-Windows-Server-Essentials.md).|  
|Stockage|Selon la version de WindowsServerEssentials vous êtes en cours d’exécution, le **stockage** section du tableau de bord contient les sections suivantes par défaut.<br /><br /> -La **dossiers du serveur** sous-section comprend des tâches qui vous aident à afficher et gérer les propriétés des dossiers du serveur. La page inclut également des tâches pour ouvrir et ajouter des dossiers du serveur.<br /><br /> -La **disques durs** page comprend des tâches qui vous aident à afficher et vérifier l’intégrité des lecteurs qui sont connectés au serveur.<br /><br /> -Dans WindowsServerEssentials et Windows Server2012R2 Standard avec le rôle expérience WindowsServerEssentials activé, le **bibliothèques SharePoint** page comprend des tâches qui vous aident à gérer les bibliothèques SharePoint dans le service Office 365.<br /><br /> Pour plus d’informations sur la gestion des dossiers du serveur, voir [Manage Server Folders](Manage-Server-Folders-in-Windows-Server-Essentials.md).<br /><br /> Pour plus d’informations sur la gestion des disques durs, consultez [Manage Server Storage](Manage-Server-Storage-in-Windows-Server-Essentials.md).|  
|Applications|-La **Applications** section du tableau de bord WindowsServerEssentials contient deux sous-sections par défaut.<br /><br /> Pour plus d’informations sur la gestion des applications complémentaires, voir [gérer les Applications](Manage-Applications-in-Windows-Server-Essentials.md).<br /><br /> -La **compléments** sous-section affiche une liste des compléments installés et décrit les tâches qui vous permettent de supprimer un complément et pour accéder à des informations supplémentaires sur un complément sélectionné.<br /><br /> -La **MicrosoftPinpoint** sous-section affiche une liste des applications qui sont disponibles à partir de MicrosoftPinpoint.|  
|Office 365|Le **Office 365** onglet affiche uniquement quand WindowsServerEssentials est intégré à MicrosoftOffice 365. Cette section contient des informations de compte administrateur et d’abonnement Office 365.|  
  
> [!NOTE]
>  Si vous installez un complément pour le tableau de bord WindowsServerEssentials, le complément peut créer des sections d’administration supplémentaires. Ces sections peuvent apparaître sur la barre de navigation, ou sur un onglet de sous-section.  
  
##  <a name="BKMK_AccessDb"></a>Accès du tableau de bord WindowsServerEssentials  
 Vous pouvez accéder le tableau de bord à l’aide de l’une des méthodes suivantes. La méthode que vous choisissez varie selon que vous accédiez du tableau de bord à partir du serveur, à partir d’un ordinateur qui est connecté au réseau WindowsServerEssentials ou à partir d’un ordinateur distant.  
  
 Pour aider à gérer un réseau sécurisé, seuls les utilisateurs disposant des autorisations d’administration peuvent accéder le tableau de bord WindowsServerEssentials. En outre, vous ne pouvez pas utiliser le compte administrateur intégré pour vous connecter au tableau de bord de WindowsServerEssentials à partir du Launchpad.  
  
###  <a name="BKMK_Server"></a>Le tableau de bord à partir du serveur d’accès  
 Lorsque vous installez WindowsServerEssentials, le processus d’installation crée un raccourci vers le tableau de bord sur le **Démarrer** écran et également sur le bureau. Si le raccourci est manquant dans ces emplacements, vous pouvez utiliser le **recherche** volet pour rechercher et exécuter le programme de tableau de bord.  
  
##### <a name="to-access-the-dashboard-from-the-server"></a>Pour accéder à du tableau de bord à partir du serveur  
  
-   Connectez-vous au serveur en tant qu’administrateur, puis effectuez l’une des opérations suivantes:  
  
    -   Sur le **Démarrer**, cliquez sur **tableau de bord**.  
  
    -   Sur le bureau, double-cliquez sur **tableau de bord**.  
  
    -   Dans le **recherche** volet, tapez **tableau de bord**, puis cliquez sur **tableau de bord** dans les résultats de recherche.  
  
###  <a name="BKMK_Network"></a>Le tableau de bord à partir d’un ordinateur qui est connecté au réseau d’accès  
 Dans WindowsServerEssentials, une fois que vous joignez un ordinateur au réseau, les administrateurs peuvent utiliser Launchpad pour accéder au serveur du tableau de bord à partir de l’ordinateur.  
  
> [!WARNING]
>  Dans WindowsServerEssentials, pour vous connecter au tableau de bord à partir de l’ordinateur client, avec le bouton droit de l’icône, puis ouvrez le tableau de bord dans le menu contextuel.  
  
##### <a name="to-access-the-dashboard-by-using-the-launchpad"></a>Accéder à du tableau de bord à l’aide du Launchpad  
  
1.  À partir d’un ordinateur qui est joint au réseau, ouvrez le **Launchpad**.  
  
2.  Dans le menu de Launchpad, cliquez sur **tableau de bord**.  
  
3.  Sur le tableau de bord **connectez-vous** page, tapez vos informations d’identification d’administrateur réseau, puis appuyez sur ENTRÉE.  
  
     Une connexion à distance au tableau de bord de WindowsServerEssentials s’ouvre.  
  
###  <a name="BKMK_Remote"></a>Accès du tableau de bord à partir d’un ordinateur distant  
 Lorsque vous travaillez à partir d’un ordinateur distant, vous pouvez accéder le tableau de bord WindowsServerEssentials à l’aide du site accès Web à distance.  
  
##### <a name="to-access-the-dashboard-by-using-remote-web-access"></a>Pour accéder à du tableau de bord à l’aide de l’accès Web à distance  
  
1.  À partir de l’ordinateur distant, ouvrez un navigateur Internet, telles qu’Internet Explorer.  
  
2.  Dans la barre d’adresses navigateur Internet, tapez la commande suivante:**https://<DomainName\>/remote**, où *DomainName* est le nom de domaine externe de votre organisation (par exemple, contoso.com).  
  
3.  Tapez votre nom d’utilisateur et un mot de passe pour se connecter au site accès Web à distance.  
  
4.  Dans le **ordinateurs** section de l’accès Web à distance**accueil** page; localisez votre serveur et cliquez sur **Connect**.  
  
     Une connexion à distance au tableau de bord s’ouvre.  
  
    > [!NOTE]
    >  Si votre serveur n’apparaît pas dans le **ordinateurs** section de la page d’accueil, cliquez sur **se connecter à d’autres ordinateurs**, recherchez votre serveur dans la liste, puis cliquez sur le serveur pour s’y connecter.  
    >   
    >  Pour autoriser un utilisateur à accéder à du tableau de bord à partir de l’accès Web à distance, ouvrez la page de propriétés pour le compte d’utilisateur, puis sélectionnez le **tableau de bord Server** option sur le **accès en tout lieu** onglet.  
  
##  <a name="BKMK_UseSafeMode"></a>Utiliser le mode sans échec  
 La fonctionnalité de mode sans échec de WindowsServerEssentials surveille les compléments installés sur le serveur. Si le tableau de bord devient instable ou ne répond pas, le mode sans échec détecte et isole les compléments susceptibles d’entraîner le problème et les affiche dans le **paramètres du Mode sans échec** page la prochaine fois que vous ouvrez le tableau de bord. À partir de la **paramètres du Mode sans échec** page, vous pouvez désactiver et activer des compléments un par un pour déterminer le complément qui provoque le problème. Vous pouvez ensuite choisir de laisser le complément désactivé pour des démarrages futurs ou vous pouvez désinstaller le complément.  
  
 Vous pouvez afficher une liste de tous les modules complémentaires qui sont installés sur le serveur à tout moment. Vous pouvez également ouvrir le tableau de bord en mode sans échec, ce qui désactive automatiquement tous les compléments non-Microsoft. WindowsServerEssentials vous permet également d’accéder à du tableau de bord en mode sans échec depuis un autre ordinateur sur le réseau.  
  
#### <a name="to-view-a-list-of-installed-add-ins"></a>Pour afficher la liste des compléments installés  
  
-   Dans le tableau de bord, cliquez sur **aide**, puis cliquez sur **les paramètres de mode sans échec**.  
  
#### <a name="to-open-the-dashboard-in-safe-mode"></a>Pour ouvrir le tableau de bord en mode sans échec  
  
-   Dans le **recherche** volet, tapez **sécurisée**, puis cliquez sur **tableau de bord (mode sans échec)** dans les résultats de recherche.  
  
#### <a name="to-open-the-dashboard-in-safe-mode-from-another-computer-on-the-network"></a>Pour ouvrir le tableau de bord en mode sans échec à partir d’un autre ordinateur sur le réseau  
  
1.  À partir d’un ordinateur qui est connecté au réseau, ouvrez le Launchpad de WindowsServerEssentials, puis cliquez sur **tableau de bord**.  
  
2.  Sur la page de connexion du tableau de bord, tapez le nom d’utilisateur et mot de passe d’un compte qui a l’autorisation de se connecter au serveur, sélectionnez le **M’autoriser à sélectionner les compléments à charger** case à cocher, puis cliquez sur la flèche pour vous connecter.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer des Applications](Manage-Applications-in-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)
