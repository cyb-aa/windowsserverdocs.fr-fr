---
title: Fonctionnalité à la demande (FOD) de compatibilité des applications Server Core
description: Comment installer des fonctionnalités de Windows Server à la demande
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866110"
---
# <a name="server-core-app-compatibility-feature-on-demand-fod"></a>Fonctionnalité à la demande (FOD) de compatibilité des applications Server Core

> S’applique à Windows Server 2019 et Windows Server, version 1809

Le **fonctionnalité de compatibilité de Server Core application à la demande** est un fonctionnalité facultative du package qui peut être ajouté à des installations de Windows Server 2019 Server Core, ou Windows Server, version 1809, à tout moment.

Pour plus d’informations sur les fonctionnalités à la demande (DOM), consultez [fonctionnalités On Demand](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).


## <a name="why-install-the-app-compatibility-fod"></a>Pourquoi installer DOM de compatibilité d’application ? 

Compatibilité des applications, une fonctionnalité à la demande pour Server Core, améliore considérablement la compatibilité d’application de l’option d’installation de Windows Server Core en incluant un sous-ensemble des fichiers binaires et des packages à partir de Windows Server avec expérience utilisateur, sans ajouter le Environnement graphique d’expérience de bureau de Windows Server. Ce package facultatif est disponible sur un fichier ISO distinct, ou à partir de Windows Update, mais ne peut être ajouté aux images et les installations de Windows Server Core.

Les deux valeurs de principales que fournit le DOM de compatibilité d’application sont :

1.  Augmente la compatibilité de Server Core pour les applications serveur qui sont déjà dans le marché ou qui ont déjà été développés par les organisations et déployés.

2.  Aide en fournissant des composants de système d’exploitation et compatibilité des applications accrue des outils logiciels utilisés dans la résolution des problèmes et scénarios de débogage.

Composants de système d’exploitation qui sont disponibles comme partie du DOM de compatibilité de Server Core application incluent :

-   Microsoft Management Console (mmc.exe)

-   Observateur d’événements (Eventvwr.msc)

-   Analyseur de performances (PerfMon.exe)

-   Moniteur de ressource (Resmon.exe)

-   Gestionnaire de périphériques (Devmgmt.msc)

-   Explorateur de fichiers (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   Gestion des disques (Diskmgmt.msc)

-   Gestionnaire du Cluster de basculement (CluAdmin.msc)

    -   Nécessite l’ajout de la fonctionnalité de serveur de Windows de Clustering de basculement tout d’abord.

        -   Utilisez la Powershell Cmdlet pour ajouter, `Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools`

        -   Pour exécuter le Gestionnaire du Cluster de basculement, entrez **cluadmin** à l’invite de commandes.

## <a name="installing-the-app-compatibility-fod"></a>L’installation de la compatibilité des applications DOM

 >[!IMPORTANT] 
   >Le DOM de compatibilité d’application peut uniquement être installé sur Server Core. N’essayez pas à ajouter le DOM de compatibilité de Server Core application vers une installation de Windows Server de Windows Server avec la fonctionnalité expérience utilisateur.

### <a name="to-add-the-server-core-app-compatibility-feature-on-demand-fod-to-a-running-instance-of-server-core"></a>Pour ajouter la fonctionnalité de compatibilité des applications serveur principales à la demande (DOM) à une instance en cours d’exécution de Server Core

 >[!NOTE] 
   > Cette procédure utilise Deployment Image Servicing and Management (DISM.exe), un outil de ligne de commande. Pour plus d’informations sur les commandes DISM, consultez [Options de ligne de commande de maintenance de packages de fonctionnalités DISM](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-capabilities-package-servicing-command-line-options).

>[!NOTE] 
   > Les packages facultatifs DOM mêmes ISO utilisable pour les installations Server Core de Windows Server 2019, ou Windows Server, version 1809, installations.

>[!NOTE] 
   > Si votre ordinateur ou une machine virtuelle qui exécute Server Core est en mesure de se connecter à Windows Update, les étapes 1 à 7 ci-dessous peut être ignorée. Mais veillez à laisser désactiver/source et /LimitAccess à partir de la commande DISM à l’étape 8.

1. Téléchargez les packages facultatifs du serveur DOM ISO et copier l’image ISO sur un dossier partagé sur votre réseau local :

 - Si vous avez une licence en volume, vous pouvez télécharger le fichier image ISO de DOM de serveur à partir du même portail où le fichier image ISO du système d’exploitation est obtenu : [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
 - Le fichier image ISO de DOM de serveur est également disponible sur le [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) ou sur le [portail Visual Studio](https://visualstudio.microsoft.com) pour les abonnés.


2. Connectez-vous en tant qu’administrateur sur l’ordinateur Server Core, qui est connecté à votre réseau local et que vous souhaitez ajouter le DOM à.

3. Utilisez **net utilisation**, ou toute autre méthode pour se connecter à l’emplacement de l’ISO FOD.

4. Copiez l’ISO FOD dans un dossier local de votre choix.

5. Démarrez PowerShell en entrant **powershell.exe** à une invite de commandes.

6. Montez l’ISO FoD à l’aide de la commande suivante :

        Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

7. Type **quitter** pour quitter PowerShell.

8.  Exécutez la commande suivante :

        DISM /Online /Add-Capability /CapabilityName:"ServerCore.AppCompatibility~~~~0.0.1.0" /Source:drive_letter_of_mounted_ISO: /LimitAccess

9.  Une fois la barre de progression terminée, redémarrez le système d’exploitation.

### <a name="to-optionally-add-internet-explorer-11-to-server-core-after-adding-the-server-core-app-compatibility-fod"></a>Pour ajouter éventuellement Internet Explorer 11 pour Server Core (après avoir ajouté le DOM de compatibilité Application Server Core)

 >[!NOTE]  
   > Server Core application compatibilité DOM est requis pour l’ajout d’Internet Explorer 11, mais Internet Explorer 11 n’est pas nécessaire d’ajouter le DOM de compatibilité Application Server Core.

1.  Connectez-vous en tant qu’administrateur sur l’ordinateur Server Core ayant le DOM de compatibilité d’application déjà ajouté et le package facultatif Server DOM QU'ISO copié localement.

2.  Démarrez PowerShell en entrant **powershell.exe** à une invite de commandes.

3.  Montez l’ISO FoD à l’aide de la commande suivante :

         Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

4.  Type **quitter** pour quitter PowerShell.


5.  Exécutez la commande suivante :

        Dism /online /add-package:drive_letter_of_mounted_iso:"Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

6.  Une fois la barre de progression terminée, redémarrez le système d’exploitation.

 
#### <a name="release-notes-and-suggestions-for-the-server-core-app-compatibility-fod-and-internet-explorer-11-optional-package"></a>Notes de publication et des suggestions pour le package facultatif de Server Core application compatibilité DOM et Internet Explorer 11

- **Important :** Veuillez lire les notes de publication de Windows Server 2019 pour tout problème, les considérations relatives à l’ou les instructions avant de procéder à l’installation et l’utilisation du package facultatif Server Core application compatibilité DOM et Internet Explorer 11.
 
 >[!NOTE] 
   > Il est possible de rencontrer lors de l’ajout du DOM de compatibilité d’application après l’utilisation de la mise à jour de Windows pour installer les mises à jour cumulatives le scintillement avec l’expérience de console Server Core.  Ce problème est résolu avec décembre 2018 met à jour.  Pour plus d’informations et la résolution, consultez [article de la Base de connaissances 4481610 : Écran clignote après l’installation de Server Core application compatibilité DOM dans Windows Server 2019 Server Core](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core).

- Après l’installation du DOM de compatibilité d’application et de redémarrage du serveur, la couleur de frame de fenêtre de console commande devient un dégradé Bleu différents.

- Si vous choisissez d’installer également le package facultatif Internet Explorer 11, notez que double-clic pour ouvrir les fichiers .htm enregistré localement n’est pas pris en charge. Toutefois, vous pouvez **avec le bouton droit** et choisissez **ouvrir avec IE**, ou vous pouvez l’ouvrir directement à partir d’Internet Explorer **fichier** -> **ouvrir**. 

- Pour améliorer la compatibilité des applications de Server Core avec le DOM de compatibilité d’application, la Console de gestion IIS a été ajoutée pour Server Core en tant que composant facultatif.  Toutefois, il est absolument nécessaire d’abord ajouter le DOM de compatibilité d’application pour utiliser la Console de gestion IIS. Console de gestion IIS s’appuie sur Microsoft Management Console (mmc.exe), qui est uniquement disponible sur Server Core avec l’ajout des DOM de compatibilité d’application.  Utiliser Powershell [ **Install-WindowsFeature** ](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps) pour ajouter la Console de gestion IIS.

- Comme un point général de conseils, lors de l’installation d’applications sur le serveur de base (avec ou sans ces packages facultatifs) il est parfois nécessaire d’utiliser des instructions et des options d’installation sans assistance. 
    
 - Par exemple, SQL Server Management Studio pour SQL Server 2016 et SQL Server 2017 peut être installé sur Server Core et est entièrement fonctionnelle lorsque le DOM de compatibilité d’application est présent.  Consultez, [installer SQL Server à partir de l’invite de commandes](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017).
 - Si SQL Server Management Studio n’est pas souhaité, il est inutile d’installer Server Core application compatibilité DOM.  Consultez, [installer SQL Server sur Server Core](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017).

