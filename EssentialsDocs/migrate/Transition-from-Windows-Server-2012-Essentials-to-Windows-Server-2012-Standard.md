---
title: Transition de Windows Server Essentials vers Windows Server 2012 Standard
description: Décrit comment utiliser Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882500"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-standard"></a>Transition de Windows Server Essentials vers Windows Server 2012 Standard

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Windows Server® 2012 Essentials prend en charge jusqu'à 25 utilisateurs et 50 appareils. Lorsque les besoins de votre entreprise dépassent la limite, vous pouvez effectuer une transition de licence de place à partir de Windows Server Essentials pour Windows Server 2012 Standard restent la conformité des licences.  
  
## <a name="how-the-transition-affects-user-and-device-limits"></a>Impact de la transition sur la limite du nombre d’utilisateurs et de périphériques  
 Une fois la transition vers Windows Server 2012 Standard, les limites de compte et les appareils des utilisateurs sont supprimés, mais les fonctionnalités qui sont propres à Windows Server Essentials (par exemple, le tableau de bord, accès Web à distance et sauvegarde de l’ordinateur client), est toujours disponibles. Toutefois, les limitations techniques de ces fonctionnalités prennent en charge un nombre maximal de 75 comptes d’utilisateurs et de 75 périphériques. S’il devient nécessaire d’ajouter plus de 75 comptes d’utilisateur ou d’appareils, vous devez désactiver les fonctionnalités de Windows Server Essentials et utiliser les outils natifs de Windows Server 2012 Standard pour gérer les comptes d’utilisateur et les appareils.  
  
> [!IMPORTANT]
>   Windows Server 2012 Standard requiert une licence d’accès Client (CAL) pour chaque utilisateur ou appareil dans votre environnement. Cela est différent de Windows Server Essentials, qui n’utilise pas le modèle de licence d’accès client et ne sont pas fournies avec des licences d’accès client.  Lors de la transition à partir de Windows Server Essentials pour Windows Server 2012 Standard, vous devrez acheter le nombre approprié et le type de CAL pour votre environnement (la plupart des clients achètent des licences d’accès client utilisateur).  
  
## <a name="before-the-transition"></a>Avant la transition  
  
-   Avant la transition de Windows Server Essentials pour Windows Server 2012 Standard, vous devez effectuer une sauvegarde complète des données du serveur.  
  
    > [!IMPORTANT]
    >  Sans une sauvegarde complète du serveur, vous ne pouvez pas rétablir l’état du serveur avant la transition.  
  
-   En outre, assurez-vous que vous lirez et comprenez le contrat de licence utilisateur final (CLUF) pour Windows Server 2012 Standard. Pour afficher le CLUF :  
  
    1.  Ouvrez une fenêtre de commandes en tant qu’administrateur.  
  
    2.  Exécutez la commande suivante :  
  
         **dism /online /set-edition:ServerStandard /geteula: eula path**  
  
         Où **chemin cluf** représente l’emplacement auquel vous voulez enregistrer le fichier CLUF. Par exemple, C:\ws8std_cluf.rtf.  Utilisez bien .rtf comme extension de nom de fichier.  
  
    3.  Ouvrez l’emplacement où vous avez enregistré le fichier, puis double-cliquez sur le fichier pour l’ouvrir.  
  
## <a name="transition-to--windows-server-2012-standard"></a>Transition vers Windows Server 2012 Standard  
 Une fois que vous avez décidé d’effectuer une transition entre Windows Server Essentials pour Windows Server 2012 Standard, complète ces deux étapes :  
  
1.  Achetez une licence Windows Server 2012 Standard et le nombre approprié de licences d’accès Client utilisateur et/ou par appareil pour votre environnement.  
  
     Vous pouvez acheter une licence pour Windows Server 2012 Standard à partir d’un détaillant, un serveur de distribution, ou à l’aide d’un [Microsoft Partner](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
    > [!NOTE]
    >  Si vous avez acheté Windows Server 2012 Standard à initialement et exercé vos droits à une version antérieure pour installer l’une de vos deux instances virtuelles en tant que Windows Server Essentials, il est inutile d’acheter d’autres.  
    >   
    >  Si vous achetez Windows Server 2012 Standard via le canal de licence en Volume, vous pouvez télécharger une image ISO et une clé de produit pour Windows Server 2012 Standard à partir de Volume Licensing Service Center (VLSC).  
    >   
    >  Si vous achetez Windows Server 2012 Standard à partir de tous les autres canaux peuvent télécharger une image ISO et une clé de produit d’évaluation pour Windows Server Essentials à partir de la [centre d’évaluation TechNet](https://technet.microsoft.com/evalcenter/jj659306.aspx). La réalisation de la transition comme décrit à l’étape suivante convertit le produit d’évaluation en un produit entièrement sous licence et pris en charge.  
  
2.  Ouvrez Windows PowerShell en tant qu’administrateur, puis exécutez la commande suivante.  
  
     **dism /online /set-edition:ServerStandard /accepteula /productkey:** *Clé de produit*  
  
     Où *clé de produit* est la clé de produit pour votre copie de Windows Server 2012 Standard.  
  
     Le serveur redémarre pour terminer le processus de transition.  
  
 Après la transition, les fonctionnalités de Windows Server Essentials restent sur le serveur et sont prises en charge jusqu'à 75 utilisateurs et 75 périphériques. Si vous dépassez une de ces limites, vous devez utiliser les outils natifs de Windows Server 2012 Standard pour gérer les comptes d’utilisateur et les appareils.  
  
 En outre, une fois la transition vers Windows Server 2012 Standard, les fonctionnalités multimédias de Windows Server Essentials ne sont plus disponibles. Cela comprend les fonctionnalités multimédias de l’accès web à distance et les paramètres multimédias sur le tableau de bord.  
  
## <a name="turn-off--windows-server-essentials-features"></a>Désactiver les fonctionnalités de Windows Server Essentials  
 Si vous n’avez plus besoin du tableau de bord Windows Server Essentials ou d’autres fonctionnalités à valeur ajoutée pour gérer le serveur, vous pouvez désactiver les fonctionnalités et les supprimer de votre serveur.  
  
 Le **mettre hors tension de l’Assistant de fonctionnalités de Windows Server Essentials** vous permet de désinstaller les fonctionnalités. Il supprime également le serveur de fichiers qui ont été créés par le logiciel de serveur Windows Server Essentials.  Certaines opérations de nettoyage sont effectuées immédiatement alors que d’autres sont lancées après le redémarrage du serveur.  
  
 Le **mettre hors tension de l’Assistant de fonctionnalités de Windows Server Essentials** exige que vous désinstalliez manuellement tous les compléments avant de pouvoir terminer l’Assistant. Pour afficher une liste des compléments installés, ouvrez la page Application du tableau de bord. L’Assistant vous avertit s’il détecte des compléments installés et vous invite à les désinstaller.  
  
 Le **mettre hors tension de l’Assistant de fonctionnalités de Windows Server Essentials** vous permet de choisir s’il faut garder les fichiers de sauvegarde pour le client ordinateurs après avoir désactivé les fonctionnalités de Windows Server Essentials.  
  
 Il existe deux façons d’exécuter le **mettre hors tension de l’Assistant de fonctionnalités de Windows Server Essentials** du tableau de bord :  
  
#### <a name="from-the-alert"></a>À partir de l’alerte  
  
1.  À partir du tableau de bord, ouvrez l’Afficheur des alertes.  
  
2.  Dans la liste organiser, sélectionnez l’alerte qui fournit des informations sur la désactivation des fonctionnalités de Windows Server Essentials après la transition.  
  
3.  Dans l’alerte, cliquez sur **désactiver des fonctionnalités Windows Server Essentials**.  
  
#### <a name="from-the-get-help-and-support-pane"></a>À partir du volet d’aide et de support  
  
1.  Dans la page d’accueil, cliquez sur Obtenir de l’aide et du support.  
  
2.  Cliquez sur **mettre hors tension de l’Assistant de fonctionnalités de Windows Server Essentials**.  
  
 Il est possible que certaines tâches effectuées par le **mettre hors tension de l’Assistant de fonctionnalités de Windows Server Essentials** se termine pas correctement. Dans certains cas, cela peut empêcher le tableau de bord de fonctionner. Si cette situation se produit, vous pouvez démarrer l’Assistant manuellement en exécutant le fichier :  
  
 **%SystemDrive%\Program Files\Windows Server\Bin\TurnOffFeaturesWizard.exe**  
  
## <a name="see-also"></a>Voir aussi  
  

-   [Transition vers Windows Server 2012 R2 Standard](Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Migrer des données de serveur vers Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Transition vers Windows Server 2012 R2 Standard](../migrate/Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Migrer des données de serveur vers Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

