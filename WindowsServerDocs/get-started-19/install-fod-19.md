---
title: Fonctionnalité à la demande (FOD) de compatibilité des applications ServerCore
description: Procédure pour installer les fonctionnalités de Windows Server à la demande
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: b8211ace56aa6565295a15adce26a8dfbc98e1e9
ms.sourcegitcommit: dbb4738fdac3b7911952ff11f1eaed9649d6567a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/07/2019
ms.locfileid: "9149899"
---
# Fonctionnalité à la demande (FOD) de compatibilité des applications ServerCore

> S’applique à Windows Server 2019 et Windows Server, version 1809

La **Fonctionnalité de compatibilité Server Core App à la demande** est un package de fonctionnalité facultative qui peut être ajouté aux installations de Windows Server 2019 Server Core, ou Windows Server, version 1809, à tout moment.

Pour plus d’informations sur les fonctionnalités à la demande (DOM), voir les [Fonctionnalités à la demande](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).


## Pourquoi installer la demande de compatibilité d’application? 

Compatibilité des applications, une fonctionnalité à la demande à Server Core, améliore considérablement la compatibilité des applications de l’option d’installation de Windows Server Core en incluant un sous-ensemble de fichiers binaires et des packages à partir de Windows Server avec expérience utilisateur, sans ajouter le Environnement graphique expérience de bureau de Windows Server. Ce package facultatif est disponible sur un fichier ISO distinct, ou à partir de Windows Update, mais ne peut être ajouté à des images et les installations de Windows Server Core.

Les deux valeurs principales que fournit la demande de compatibilité d’application sont:

1.  Augmente la compatibilité de Server Core pour les applications de serveur qui sont déjà dans le marché ou qui ont déjà été développées par les organisations et déployé.

2.  Aide à fournir des composants du système d’exploitation et compatibilité des applications accrue d’outils logiciels utilisés dans la résolution des problèmes et scénarios de débogage.

Composants de système d’exploitation qui sont disponibles comme partie de la demande de compatibilité Server Core application incluent:

-   Microsoft Management Console (mmc.exe)

-   L’Observateur d’événements (Eventvwr.msc)

-   Analyseur de performances (PerfMon.exe)

-   Moniteur de ressources (Resmon.exe)

-   Gestionnaire de périphériques (Devmgmt.msc)

-   Explorateur de fichiers (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   Gestion des disques (Diskmgmt.msc)

-   Gestionnaire du Cluster de basculement (CluAdmin.msc)

    -   Nécessite l’ajout de la fonctionnalité de serveur Windows de Clustering de basculement tout d’abord.

        -   Permet d’ajouter, Powershell Cmdlet `Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools`

        -   Pour exécuter le Gestionnaire du Cluster de basculement, entrez **cluadmin** à l’invite de commandes.

## L’installation de la compatibilité des applications DOM

 >[!IMPORTANT] 
   >La demande de compatibilité d’application ne peut être installé sur Server Core. N’essayez pas d’ajouter la demande de compatibilité Application Server Core vers une installation de Windows Server de Windows Server avec expérience utilisateur.

### Pour ajouter la fonctionnalité de compatibilité des applications Server Core à la demande (DOM) à une instance en cours d’exécution de Server Core

 >[!NOTE] 
   > Cette procédure utilise Deployment Image Servicing and Management (DISM.exe), un outil de ligne de commande. Pour plus d’informations sur les commandes DISM, reportez-vous à la section [Options de ligne de commande de maintenance du Package de fonctionnalités DISM](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-capabilities-package-servicing-command-line-options).

>[!NOTE] 
   > Les packages facultatifs DOM mêmes ISO peuvent servir pour les installations Server Core de Windows Server 2019 ou Windows Server, version 1809, installations.

>[!NOTE] 
   > Si votre ordinateur ou la machine virtuelle qui est en cours d’exécution Server Core est en mesure de se connecter à Windows Update, les étapes 1 à 7 ci-dessous peut être ignorée. Toutefois, veillez à laisser /Source et /LimitAccess à partir de la commande DISM à l’étape 8.

1. Téléchargez les packages facultatifs DOM Server ISO et copiez le fichier ISO sur un dossier partagé sur votre réseau local:

 - Si vous disposez d’une licence en volume, vous pouvez télécharger le fichier image ISO de demande de serveur à partir du portail même où le fichier image ISO du système d’exploitation est obtenu: [Centre de gestion de licences en Volume](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
 - Le fichier image ISO de demande de serveur est également disponible dans le [Centre d’évaluation de Microsoft](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) ou sur le [portail de Visual Studio](https://visualstudio.microsoft.com) pour les abonnés.


2. Connectez-vous en tant qu’administrateur sur l’ordinateur de Server Core, qui est connecté à votre réseau local et que vous souhaitez ajouter la demande à.

3. Utilisez **net use**, ou une autre méthode pour se connecter à l’emplacement de l’ISO FOD.

4. Copiez l’ISO FOD dans un dossier local de votre choix.

5. Démarrez PowerShell en entrant **powershell.exe** à une invite de commandes.

6. Montez l’ISO FoD à l’aide de la commande suivante:

        Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

7. Tapez **Quitter** pour quitter PowerShell.

8.  Exécutez la commande suivante:

        DISM /Online /Add-Capability /CapabilityName:"ServerCore.AppCompatibility~~~~0.0.1.0" /Source:drive_letter_of_mounted_ISO: /LimitAccess

9.  Une fois la barre de progression est terminée, redémarrez le système d’exploitation.

### Si vous le souhaitez ajouter Internet Explorer 11 à Server Core (après l’ajout de la demande de compatibilité Application Server Core)

 >[!NOTE]  
   > La demande de compatibilité Application Server Core est requise pour l’ajout d’Internet Explorer 11, mais Internet Explorer 11 n’est pas nécessaire d’ajouter la demande de compatibilité Application Server Core.

1.  Connectez-vous en tant qu’administrateur sur l’ordinateur de Server Core qui possède la demande de compatibilité d’application déjà ajouté et le package facultatif Server DOM QU'ISO copié en local.

2.  Démarrez PowerShell en entrant **powershell.exe** à une invite de commandes.

3.  Montez l’ISO FoD à l’aide de la commande suivante:

         Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

4.  Tapez **Quitter** pour quitter PowerShell.


5.  Exécutez la commande suivante:

        Dism /online /add-package:drive_letter_of_mounted_iso:"Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

6.  Une fois la barre de progression est terminée, redémarrez le système d’exploitation.

 
#### Notes de publication et suggestions pour le package facultatif Server Core App compatibilité DOM et Internet Explorer 11

- **Important:** Veuillez lire les notes de publication de Windows Server 2019 problèmes, considérations ou conseils avant de poursuivre l’installation et utilisation du package facultatif Server Core App compatibilité DOM et Internet Explorer 11.
 
 >[!NOTE] 
   > Il est possible de rencontrer scintillement avec l’expérience de console Server Core lors de l’ajout de la demande de compatibilité d’application après l’utilisation de Windows Update pour installer les mises à jour cumulatives.  Ce problème est résolu avec décembre 2018 met à jour.  Pour plus d’informations et la résolution, consultez [l’article de la Base de connaissances 4481610: écran clignote après l’installation Server Core App compatibilité DOM dans Windows Server 2019 Server Core](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core).

- Après l’installation de la demande de compatibilité d’application et le redémarrage du serveur, la couleur de trame de fenêtre de console commande deviendra une autre nuance de bleu.

- Si vous choisissez d’installer également le package facultatif Internet Explorer 11, notez que double-clic pour ouvrir des fichiers enregistrés localement .htm n’est pas pris en charge. Toutefois, vous pouvez **avec le bouton droit** et choisissez **Ouvrir avec Internet Explorer**, ou vous pouvez l’ouvrir directement à partir d’Internet Explorer **fichier** -> **Ouvrir**. 

- Pour améliorer la compatibilité des applications de Server Core avec la demande de compatibilité d’application, la Console de gestion IIS a été ajoutée à Server Core en tant que composant facultatif.  Toutefois, il est absolument nécessaire pour tout d’abord ajouter la demande de compatibilité d’application pour utiliser la Console de gestion IIS. Console de gestion IIS s’appuie sur la Console de gestion Microsoft (mmc.exe), qui est uniquement disponible sur Server Core avec l’ajout de la demande de compatibilité d’application.  Utilisez Powershell [**Install-WindowsFeature**](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps) pour ajouter la Console de gestion IIS.

- Comme un point général des recommandations, lorsque l’installation d’applications sur Server Core (avec ou sans ces packages facultatifs) qu’il est parfois nécessaire d’utiliser des instructions et des options d’installation en mode silencieux. 
    
 - Par exemple, SQL Server Management Studio pour SQL Server 2016 et SQL Server 2017 peut être installé sur Server Core et est totalement fonctionnelle lors de la demande de compatibilité d’application est présent.  Consultez, [installer SQL Server à partir de l’invite de commandes](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017).
 - Si SQL Server Management Studio n’est pas souhaité, il est inutile d’installer la demande de compatibilité Application Server Core.  Consultez, [installer SQL Server sur Server Core](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017).

