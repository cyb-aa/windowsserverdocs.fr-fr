---
title: Transition de WindowsServerEssentials vers Windows Server2012 Standard
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51bcf124-c215-4e9d-9fa8-a90fa2c2fa22
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d2005b72adede72b718fa5b49b93435f5fbac1bd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-standard"></a>Transition de WindowsServerEssentials vers Windows Server2012 Standard

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

 Windows Server® 2012Essentials prend en charge jusqu'à 25utilisateurs et 50appareils. Lorsque les besoins de votre entreprise dépassent la limite, vous pouvez effectuer une transition de licences sur place à partir de WindowsServerEssentials vers Windows Server2012 Standard de rester la conformité des licences.  
  
## <a name="how-the-transition-affects-user-and-device-limits"></a>Impact de la transition utilisateur et périphérique limite  
 Une fois la transition vers Windows Server2012 Standard, les limites de compte et les périphériques utilisateur sont supprimées, mais les fonctionnalités qui sont propres à WindowsServerEssentials (par exemple, le tableau de bord, accès Web à distance et sauvegarde de l’ordinateur client), sont toujours disponibles. Toutefois, les limitations techniques de ces fonctionnalités prennent en charge un maximum de 75comptes d’utilisateurs et 75périphériques. S’il devient nécessaire d’ajouter plus de 75comptes d’utilisateurs ou de périphériques, vous devez désactiver les fonctionnalités de WindowsServerEssentials et utiliser les outils natifs de Windows Server2012 Standard pour gérer les comptes d’utilisateur et les appareils.  
  
> [!IMPORTANT]
>   Windows Server2012 Standard nécessite une licence d’accès Client (CAL) pour chaque utilisateur ou appareil dans votre environnement. Cela est différent de WindowsServerEssentials, qui n’utilise pas le modèle de licence d’accès client et n’est pas fourni avec les licences d’accès client.  Lors de la transition de WindowsServerEssentials vers Windows Server2012 Standard, vous devez acheter le nombre approprié et le type de licences d’accès client pour votre environnement (la plupart des clients achètent des licences d’accès client utilisateur).  
  
## <a name="before-the-transition"></a>Avant la transition  
  
-   Avant la transition de WindowsServerEssentials vers Windows Server2012 Standard, vous devez sauvegarde complète des données du serveur.  
  
    > [!IMPORTANT]
    >  Sans une sauvegarde complète du serveur, vous ne pouvez pas restaurer le serveur à l’état où il se trouvait avant la transition.  
  
-   En outre, assurez-vous que vous lire et comprenez le contrat de licence utilisateur final (CLUF) pour Windows Server2012 Standard. Pour afficher le CLUF:  
  
    1.  Ouvrez une fenêtre de commande en tant qu’administrateur.  
  
    2.  Exécutez la commande suivante:  
  
         **DISM /online /set-edition: ServerStandard /geteula: chemin CLUF**  
  
         Où **chemin CLUF** représente l’emplacement auquel vous souhaitez enregistrer le fichier CLUF. Par exemple: C:\ws8std_eula.RTF.  Veillez à utiliser .rtf comme extension de nom de fichier.  
  
    3.  Ouvrez l’emplacement où vous avez enregistré le fichier, puis double-cliquez sur le fichier pour l’ouvrir.  
  
## <a name="transition-to--windows-server-2012-standard"></a>Transition vers Windows Server2012 Standard  
 Une fois que vous avez décidé de transition de WindowsServerEssentials vers Windows Server2012 Standard, complète ces deux étapes:  
  
1.  Achetez une licence pour Windows Server2012 Standard et le nombre approprié de licences d’accès Client utilisateur et/ou par appareil pour votre environnement.  
  
     Vous pouvez acheter une licence pour Windows Server2012 Standard à partir d’un détaillant, un distributeur, ou à l’aide d’un [MicrosoftPartner](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
    > [!NOTE]
    >  Si vous avez acheté Windows Server2012 Standard initialement et exercé vos droits antérieure pour installer l’une de vos deux instances virtuelles en tant que WindowsServerEssentials, il est inutile d’acheter d’autres éléments.  
    >   
    >  Si vous achetez Windows Server2012 Standard via le canal de licence en Volume, vous pouvez télécharger une image ISO et une clé de produit pour Windows Server2012 Standard à partir de Volume Licensing Service Center (VLSC).  
    >   
    >  Si vous achetez Windows Server2012 Standard à partir de tous les autres canaux peut télécharger une image ISO et une clé de produit d’évaluation pour WindowsServerEssentials à partir de la [centre d’évaluation TechNet](https://technet.microsoft.com/evalcenter/jj659306.aspx). Effectuer la transition comme décrit dans l’étape suivante convertit le produit d’évaluation pour un produit entièrement sous licence et pris en charge.  
  
2.  Ouvrez Windows PowerShell en tant qu’administrateur, puis exécutez la commande suivante.  
  
     **DISM /online /set-edition: ServerStandard /accepteula /productkey:***clé de produit*  
  
     Où *clé de produit* est la clé de produit pour votre copie de Windows Server2012 Standard.  
  
     Le serveur redémarre pour terminer le processus de transition.  
  
 Après la transition, les fonctionnalités de WindowsServerEssentials restent sur le serveur et sont prises en charge jusqu'à 75utilisateurs et 75périphériques. Si vous dépassez une de ces limites, vous devez utiliser les outils natifs de Windows Server2012 Standard pour gérer les comptes d’utilisateur et les appareils.  
  
 En outre, une fois la transition vers Windows Server2012 Standard, les fonctionnalités multimédias de WindowsServerEssentials ne sont plus disponibles. Cela inclut les fonctionnalités multimédias de l’accès Web à distance et les paramètres multimédias sur le tableau de bord.  
  
## <a name="turn-off--windows-server-essentials-features"></a>Désactiver les fonctionnalités de WindowsServerEssentials  
 Si vous n’avez plus besoin du tableau de bord WindowsServerEssentials ou d’autres fonctionnalités à valeur ajoutée pour gérer le serveur, vous pouvez désactiver les fonctionnalités et les supprimer de votre serveur.  
  
 Le **désactiver la fonctionnalité Assistant de fonctionnalités de WindowsServerEssentials** vous aide à désinstaller les fonctionnalités. Il supprime également le serveur de fichiers qui ont été créés par le logiciel de serveur WindowsServerEssentials.  Certaines opérations de nettoyage sont effectuées immédiatement, tandis que d’autres sont lancées après le redémarrage du serveur.  
  
 Le **désactiver la fonctionnalité Assistant de fonctionnalités de WindowsServerEssentials** nécessite que vous désinstalliez manuellement tous les compléments avant de pouvoir terminer l’Assistant. Pour afficher la liste des compléments installés, ouvrez la page d’Application dans le tableau de bord. L’Assistant vous avertit s’il détecte des compléments installés et vous invite à les désinstaller.  
  
 Le **désactiver la fonctionnalité Assistant de fonctionnalités de WindowsServerEssentials** vous permet de choisir si vous souhaitez conserver les fichiers de sauvegarde pour les clients ordinateurs après avoir désactivé les fonctionnalités de WindowsServerEssentials.  
  
 Il existe deux façons d’exécuter le **désactiver la fonctionnalité Assistant de fonctionnalités de WindowsServerEssentials** à partir du tableau de bord:  
  
#### <a name="from-the-alert"></a>À partir de l’alerte  
  
1.  Dans le tableau de bord, ouvrez l’Afficheur des alertes.  
  
2.  Dans la liste organiser, sélectionnez l’alerte qui indique des informations sur la désactivation des fonctionnalités de WindowsServerEssentials après la transition.  
  
3.  Dans l’alerte, cliquez sur **désactiver les fonctionnalités de WindowsServerEssentials**.  
  
#### <a name="from-the-get-help-and-support-pane"></a>À partir du volet d’aide et Support  
  
1.  Sur la page d’accueil, cliquez sur Aide et Support.  
  
2.  Cliquez sur **désactiver la fonctionnalité Assistant de fonctionnalités de WindowsServerEssentials**.  
  
 Il est possible que certaines tâches effectuées par le **désactiver la fonctionnalité Assistant de fonctionnalités de WindowsServerEssentials** se termine pas correctement. Dans certains cas, cela peut empêcher le tableau de bord à partir d’en cours d’exécution. Si cela se produit, vous pouvez démarrer l’Assistant manuellement en exécutant le fichier:  
  
 **%SystemDrive%\Program Files\Windows Server\Bin\TurnOffFeaturesWizard.exe**  
  
## <a name="see-also"></a>Voir aussi  
  

-   [Transition vers Windows Server 2012 R2 Standard](Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Migrer les données de serveur vers Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Transition vers Windows Server 2012 R2 Standard](../migrate/Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Migrer les données de serveur vers Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

