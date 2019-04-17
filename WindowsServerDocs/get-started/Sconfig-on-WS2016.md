---
title: Configurer une installation minimale de WindowsServer avec Sconfig.cmd
description: Décrit l’utilisation de Sconfig.cmd
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 10/17/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e6cac074-c6fc-46dd-9664-fa0342c0a5e8
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: high
ms.openlocfilehash: 2473b4ffae79c29ec7505616c139c03b21a4427b
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2018
ms.locfileid: "1284347"
---
# <a name="configure-a-server-core-installation-of-windows-server-2016-or-windows-server-version-1709-with-sconfigcmd"></a>Configurer une installation minimale de WindowsServer2016 ou WindowsServer, version1709, avec Sconfig.cmd
> S’applique à: WindowsServer (canal semi-annuel) et WindowsServer2016

Dans WindowsServer2016 et WindowsServer, version1709, vous pouvez utiliser l’outil de configuration de serveur (Sconfig.cmd) pour effectuer différentes tâches courantes de configuration et de gestion des serveurs en mode d’installation minimale. Pour utiliser cet outil, vous devez être membre du groupe Administrateurs.  
  
Vous pouvez utiliser Sconfig.cmd dans les installations minimales et les installations avec Expérience de bureau (WindowsServer2016 uniquement). 
  
## <a name="start-the-server-configuration-tool"></a>Démarrer l’outil de configuration de serveur  
  
1.  Basculez vers le lecteur système.  
  
2.  Tapez `Sconfig.cmd`, puis appuyez sur ENTRÉE. L’interface de l’outil de configuration de serveur s’affiche:  
  
 <img src="mainsconfigpage.png" style='float:left; padding:.5em;' alt="Screenshot of Sconfig.cmd user interface">  
Capture d’écran de l’interface utilisateur de Sconfig.cmd  
  
##  <a name="BKMK_Domainworkgroup"></a> Paramètres de domaine ou groupe de travail  
 Les paramètres de domaine ou groupe de travail actuels sont affichés dans l’écran de l’outil de configuration de serveur par défaut. Pour joindre un domaine ou un groupe de travail, accédez à la page de paramètres **Domaine/Groupe de travail** à partir du menu principal, puis spécifiez les informations requises selon les instructions indiquées dans les pages suivantes.  
  
 Lorsqu’un utilisateur de domaine n’est pas membre du groupe Administrateurs local, vous ne pouvez pas utiliser son compte pour modifier les paramètres système, tels que le nom du système. Pour ajouter un utilisateur de domaine au groupe Administrateurs local, autorisez l’ordinateur à redémarrer. Ensuite, ouvrez une session sur l’ordinateur en tant qu’administrateur local et effectuez les étapes décrites dans la section [Paramètres de l’administrateur local](assetId:///3c2f8ca4-6adc-4ebd-8daf-eb0de16c2c7d#BKMK_Localadministratorsettings), plus loin dans ce document.  
  
> [!NOTE]
>  Vous devez redémarrer le serveur pour que les modifications apportées aux appartenances de domaine ou de groupe de travail soient appliquées. Toutefois, vous pouvez attendre d’avoir apporté toutes les modifications souhaitées avant de redémarrer le serveur une seule fois à la fin. Par défaut, les ordinateurs virtuels en cours d’exécution sont automatiquement enregistrés avant le redémarrage du serveur Hyper-V.  
  
## <a name="computer-name-settings"></a>Paramètres du nom de l’ordinateur  
 Le nom de l’ordinateur actuel est affiché dans l’écran de l’outil de configuration de serveur par défaut. Pour modifier le nom de l’ordinateur, accédez à la page de paramètres Nom de l’ordinateur à partir du menu principal, puis suivez les instructions.  
  
> [!NOTE]
>  Vous devez redémarrer le serveur pour que les modifications apportées aux appartenances de domaine ou de groupe de travail soient appliquées. Toutefois, vous pouvez attendre d’avoir apporté toutes les modifications souhaitées avant de redémarrer le serveur une seule fois à la fin. Par défaut, les ordinateurs virtuels en cours d’exécution sont automatiquement enregistrés avant le redémarrage du serveur Hyper-V.  
  
##  <a name="BKMK_Localadministratorsettings"></a> Paramètres de l’administrateur local  
 Pour ajouter d’autres utilisateurs au groupe Administrateurs local, utilisez l’option **Ajouter l’administrateur local** du menu principal. Sur un ordinateur membre d’un domaine, entrez le nom de l’utilisateur en respectant le format suivant: domaine\nom_utilisateur. Sur un ordinateur qui n’est pas membre d’un domaine (ordinateur de groupe de travail), entrez uniquement le nom de l’utilisateur. Les modifications sont immédiatement appliquées.  
  
## <a name="network-settings"></a>Paramètres du réseau  
 Vous pouvez choisir de configurer l’adresse IP pour qu’elle soit affectée automatiquement par un serveur DHCP, ou d’affecter manuellement une adresse IP statique. Cette option vous permet également de configurer les paramètres DNS du serveur.  
  
> [!NOTE]
>  Ces options font partie des nombreuses nouvelles options disponibles avec les applets de commande Windows PowerShell pour la mise en réseau. Pour plus d’informations, voir les [applets de commande de la carte réseau](https://technet.microsoft.com/library/jj134956.aspx) dans la bibliothèque technique de WindowsServer.  
  
## <a name="windows-update-settings"></a>Paramètres de WindowsUpdate  
 Les paramètres Windows Update actuels sont affichés dans l’écran de l’outil de configuration de serveur par défaut. Vous pouvez configurer le serveur pour les mises à jour automatiques ou manuelles à l’aide de l’option de configuration **Paramètres de WindowsUpdate** du menu principal.  
  
 Lorsque l’option **Mises à jour automatiques** est sélectionnée, le système vérifie si de nouvelles mises à jour sont disponibles, et les installe le cas échéant. Ce processus a lieu tous les jours à3h00. Les paramètres prennent effet immédiatement. Si l’option **Manuel** est sélectionnée, le système ne vérifie pas automatiquement si des mises à jour sont disponibles.  
  
 Vous pouvez à tout moment télécharger et installer les mises à jour applicables à l’aide de l’option **Télécharger et installer les mises à jour** du menu principal.

 L’option **Télécharger uniquement** recherche les mises à jour, les télécharge le cas échéant, puis vous informe dans le centre de notifications qu’elles sont prêtes à être installées. Il s’agit de l’option par défaut.  
  
## <a name="remote-desktop-settings"></a>Paramètres du Bureau à distance  
 Les paramètres actuels du Bureau à distance sont affichés dans l’écran de l’outil de configuration de serveur par défaut. Vous pouvez configurer les paramètres du Bureau à distance ci-dessous en utilisant l’option **Bureau à distance** du menu principal et en suivant les instructions à l’écran.  
  
-   Activer le Bureau à distance pour les ordinateurs clients exécutant le Bureau à distance avec une authentification au niveau du réseau  
  
-   Activer le Bureau à distance pour les ordinateurs clients exécutant l’une des versions du Bureau à distance  
  
-   Désactiver le Bureau à distance  
  
## <a name="date-and-time-settings"></a>Paramètres de date et d’heure  
 Vous pouvez afficher et modifier les paramètres de date et d’heure en utilisant l’option **Date et heure** du menu principal. 

## <a name="telemetry-settings"></a>Paramètres de télémétrie
Cette option vous permet de configurer les données envoyées à Microsoft.

## <a name="windows-activation-settings"></a>Paramètres d’activation deWindows
Cette option vous permet de configurer Activation de Windows.
  
## <a name="to-enable-remote-management"></a>Pour activer la gestion à distance  
Vous pouvez activer la gestion à distance dans de nombreux cas en utilisant l’option **Configurer l’administration à distance** du menu principal:  
  
-   Gestion à distance de MMC (Microsoft Management Console)  
-   Windows PowerShell  
-   Gestionnaire de serveur  
  
## <a name="to-log-off-restart-or-shut-down-the-server"></a>Pour fermer une session, ou redémarrer ou arrêter le serveur  
 Pour fermer une session, ou redémarrer ou arrêter le serveur, utilisez l’option de menu correspondante dans le menu principal. Ces options sont également disponibles dans le menu Sécurité de Windows, auquel vous pouvez accéder à tout moment à partir de n’importe quelle application en appuyant sur Ctrl+Alt+Suppr.  
  
## <a name="to-exit-to-the-command-line"></a>Pour quitter pour revenir à la ligne de commande  
 Sélectionnez l’option **Quitter pour revenir à la ligne de commande** et appuyez sur Entrée pour quitter et revenir à la ligne de commande. Pour revenir à l’outil de configuration de serveur, tapez **Sconfig.cmd**, puis appuyez sur Entrée.