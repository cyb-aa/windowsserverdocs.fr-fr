---
title: Vue d'ensemble du Launchpad dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 198d16cb-3d07-4706-be89-ad14a5f7dc47
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 63a161057f7068dcb9e02faa353270f0150200b4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310663"
---
# <a name="overview-of-the-launchpad-in-windows-server-essentials"></a>Vue d'ensemble du Launchpad dans Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Launchpad de Windows Server Essentials est une petite application qui est installée sur un ordinateur la première fois que celui-ci se connecte au serveur. Launchpad permet aux utilisateurs authentifiés d'accéder aux fonctionnalités clés de Windows Server Essentials, notamment les sauvegardes d'ordinateur, le partage de fichiers et de médias, ainsi que le site d'accès web à distance. Les utilisateurs peuvent accéder à ces fonctionnalités à partir d'ordinateurs joints ou non à un domaine. Launchpad fournit également des informations en temps réel et des notifications concernant l'état de l'ordinateur. Les administrateurs peuvent utiliser Launchpad pour accéder au tableau de bord du serveur, même si l'ordinateur n'est pas connecté au réseau.  
  
 Les OEM et les éditeurs de logiciels indépendants qui développent des compléments pour Windows Server Essentials peuvent utiliser Launchpad pour étendre les fonctionnalités du complément aux ordinateurs du réseau.  
  
 Les systèmes d'exploitation Windows suivants prennent en charge l'utilisation de Launchpad via Windows Server Essentials :  
  
- **Windows 8**: Toutes les éditions.  
  
- **Windows 7**: Toutes les éditions.  
- **Windows 10**: toutes les éditions. 
  
  Les systèmes d'exploitation suivants ne prennent pas en charge l'utilisation de Launchpad via Windows Server Essentials :  
  
- **Serveurs supplémentaires**: Vous ne pouvez pas exécuter Launchpad de Windows Server Essentials sur des ordinateurs supplémentaires dotés d'un système d'exploitation Windows Server.  
  
  Dans cette rubrique :  
  
- [Utiliser Launchpad](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Launchpad)  
  
- [Utiliser Launchpad avec un ordinateur Mac](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Mac)  
  
##  <a name="use-the-launchpad"></a><a name="BKMK_Launchpad"></a>Utiliser Launchpad  
 Les liens et informations ci-après sont disponibles sur Launchpad de Windows Server Essentials.  
  
### <a name="backup"></a>Secours  
 Cliquez sur **Sauvegarder** pour ouvrir les **Propriétés de la sauvegarde** de l'ordinateur. Dans la page **Propriétés de la sauvegarde** , vous pouvez :  
  
- démarrer ou arrêter une sauvegarde ;  
  
- afficher l'état et les détails de la dernière sauvegarde ;  
  
- spécifier le mode de gestion de l'alimentation de l'ordinateur durant l'exécution de la sauvegarde.  
  
  Pour plus d’informations sur l’utilisation de Launchpad pour sauvegarder votre ordinateur, consultez [gérer la sauvegarde des clients](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md).  
  
### <a name="remote-web-access"></a>accès Web distant  
 Cliquez sur **Accès web à distance** pour ouvrir le navigateur web sur le site d'accès web à distance. Le site d'accès web à distance vous permet de vous connecter à d'autres ordinateurs et d'accéder à certaines ressources réseau à partir de votre bureau ou depuis n'importe quel emplacement distant, à l'aide d'un ordinateur disposant d'une connexion Internet. Pour plus d’informations sur les Accès web distantes, consultez [gérer les accès Web distantes](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md).  
  
### <a name="shared-folders"></a>Dossiers partagés  
 Cliquez sur **Dossiers partagés** pour ouvrir l'Explorateur Windows à l'emplacement des dossiers partagés sur le serveur. Pour plus d’informations sur le partage de fichiers et de dossiers, consultez la rubrique [gérer les dossiers du serveur](Manage-Server-Folders-in-Windows-Server-Essentials.md).  
  
### <a name="dashboard"></a>Tableau de bord  
 Cliquez sur  **Tableau de bord** pour ouvrir la page **Connexion** qui permet d’accéder au tableau de bord Windows Server Essentials. Une fois que vous vous êtes connecté, une connexion Bureau à distance vers le tableau de bord du serveur s'ouvre. Pour plus d’informations sur le tableau de bord, consultez [vue d’ensemble du tableau de bord](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md).  
  
> [!NOTE]
>  Pour utiliser cette fonctionnalité, vous devez disposer de l'accès ou des autorisations nécessaires pour vous connecter au serveur.  
  
### <a name="microsoft-office-365"></a>Microsoft Office 365  
 Le lien **Microsoft Office 365** apparaît uniquement dans Launchpad si l'utilisateur dispose d'un compte Office 365. Cliquez sur  **Microsoft Office 365** pour accéder à des liens supplémentaires vers les ressources Office 365. Pour plus d’informations, consultez [Guide de démarrage rapide à l’utilisation de Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).  
  
### <a name="computer-health-alerts"></a>Alertes d'intégrité de l'ordinateur  
 Les alertes qui apparaissent dans Launchpad fournissent un état rapide de l'intégrité immédiate de l'ordinateur. Pour afficher des informations sur une alerte d'intégrité, cliquez sur un indicateur d'alerte pour ouvrir l'Afficheur des alertes. Les alertes d'intégrité s'affichent dans la visionneuse en fonction du niveau de gravité. Les alertes les plus graves s'affichent en premier dans la liste. Les alertes les moins graves s'affichent plus loin dans la liste. Pour plus d’informations sur les alertes d’intégrité de l’ordinateur, consultez [gérer l’intégrité du système](Manage-System-Health-in-Windows-Server-Essentials.md).  
  
##  <a name="use-the-launchpad-with-a-mac-computer"></a><a name="BKMK_Mac"></a>Utiliser Launchpad avec un ordinateur Mac  
 Vous pouvez connecter un ordinateur Mac® exécutant Mac OS X® 10,5 ou version ultérieure à Windows Server Essentials, Windows Server Essentials ou Windows Server 2012 R2 ou en téléchargeant et en installant le logiciel connecteur. Une fois que vous avez fini d'installer le logiciel Connecteur, vous pouvez choisir de lancer automatiquement Launchpad au démarrage.  
  
 Launchpad est une petite application qui permet aux utilisateurs authentifiés d'accéder aux fonctionnalités clés du serveur, notamment le partage de fichiers et de médias, les compléments et l'accès web à distance. Launchpad fournit également des informations en temps réel et des notifications concernant l'état de l'ordinateur.  
  
> [!NOTE]
>  Les administrateurs du serveur ne peuvent pas utiliser Launchpad ou l'accès web à distance sur un ordinateur Mac pour ouvrir le tableau de bord du serveur et gérer ce dernier.  
  
### <a name="backup"></a>Secours  
 Cliquez sur **Sauvegarder** pour configurer Time Machine et sauvegarder votre ordinateur, ainsi que pour changer les paramètres Time Machine. Pour plus d'informations sur Time Machine, voir la documentation du fabricant de votre ordinateur.  
  
### <a name="remote-web-access"></a>accès Web distant  
 Cliquez sur **accès Web à distance** pour ouvrir le navigateur Web sur le site accès Web distant. La Accès web distante vous permet d’accéder aux fichiers et dossiers partagés sur le serveur depuis n’importe quel emplacement distant avec un ordinateur Internet. Vous pouvez télécharger des fichiers, lire de la musique et des vidéos sur le lecteur multimédia web. En outre, vous pouvez afficher des images et lire des diaporamas. Pour plus d’informations, consultez [utiliser des accès Web distantes](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  
  
### <a name="shared-folders"></a>Dossiers partagés  
 Cliquez sur **Dossiers partagés** pour ouvrir le Finder à l'emplacement des dossiers partagés sur le serveur. Pour plus d’informations sur le partage de fichiers et de dossiers, consultez [utiliser des dossiers partagés](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md).  
  
### <a name="computer-health-alerts"></a>Alertes d'intégrité de l'ordinateur  
 Les alertes qui apparaissent dans Launchpad fournissent un état rapide de l'intégrité immédiate de l'ordinateur. Pour afficher des informations sur une alerte d'intégrité, cliquez sur un indicateur d'alerte pour ouvrir l'Afficheur des alertes. Les alertes d'intégrité s'affichent dans la visionneuse en fonction du niveau de gravité. Les alertes les plus graves s'affichent en premier dans la liste. Les alertes les moins graves s'affichent plus loin dans la liste.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Connectez-vous](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)
