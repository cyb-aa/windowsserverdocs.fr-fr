---
title: Transition de Windows Server Essentials vers Windows Server 2012 Standard
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51bcf124-c215-4e9d-9fa8-a90fa2c2fa22
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 0d7ed80f61dcfa313f867afda5689b2c64b1406a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318706"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-standard"></a>Transition de Windows Server Essentials vers Windows Server 2012 Standard

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Windows Server® 2012 Essentials prend en charge jusqu’à 25 utilisateurs et 50 appareils. Lorsque les besoins de votre entreprise dépassent la limite, vous pouvez effectuer une transition de licence sur place de Windows Server Essentials vers Windows Server 2012 standard pour rester conforme à la licence.  
  
## <a name="how-the-transition-affects-user-and-device-limits"></a>Impact de la transition sur la limite du nombre d’utilisateurs et de périphériques  
 Une fois que vous avez effectué la transition vers Windows Server 2012 standard, les limites du compte d’utilisateur et des appareils sont supprimées, mais les fonctionnalités propres à Windows Server Essentials (telles que le tableau de bord, les Accès web à distance et la sauvegarde des ordinateurs clients) restent disponibles. Toutefois, les limitations techniques de ces fonctionnalités prennent en charge un nombre maximal de 75 comptes d’utilisateurs et de 75 périphériques. S’il devient nécessaire d’ajouter plus de 75 comptes d’utilisateur ou d’appareils, vous devez désactiver les fonctionnalités de Windows Server Essentials et utiliser les outils natifs Windows Server 2012 standard pour gérer les comptes d’utilisateur et les appareils.  
  
> [!IMPORTANT]
>   Windows Server 2012 standard nécessite une licence d’accès client (CAL) pour chaque utilisateur ou périphérique de votre environnement. Cela diffère de Windows Server Essentials, qui n’utilise pas le modèle de licence d’accès client et n’est fourni avec aucune licence d’accès client.  Lors de la transition de Windows Server Essentials vers Windows Server 2012 standard, vous devez acheter le nombre et le type appropriés de licences d’accès client pour votre environnement (la plupart des clients achètent des licences d’accès client utilisateur).  
  
## <a name="before-the-transition"></a>Avant la transition  
  
-   Avant de passer de Windows Server Essentials à Windows Server 2012 standard, vous devez sauvegarder entièrement les données du serveur.  
  
    > [!IMPORTANT]
    >  Sans une sauvegarde complète du serveur, vous ne pouvez pas rétablir l’état du serveur avant la transition.  
  
-   En outre, assurez-vous de lire et de comprendre le contrat de licence utilisateur final (CLUF) pour Windows Server 2012 standard. Pour afficher le CLUF :  
  
    1.  Ouvrez une fenêtre de commande comme administrateur.  
  
    2.  Exécutez la commande suivante :  
  
         **DISM/Online/Set-Edition : ServerStandard/geteula : chemin EULA**  
  
         Où **chemin cluf** représente l’emplacement auquel vous voulez enregistrer le fichier CLUF. Par exemple, C:\ws8std_cluf.rtf.  Utilisez bien .rtf comme extension de nom de fichier.  
  
    3.  Ouvrez l’emplacement où vous avez enregistré le fichier, puis double-cliquez sur le fichier pour l’ouvrir.  
  
## <a name="transition-to--windows-server-2012-standard"></a>Transition vers Windows Server 2012 standard  
 Une fois que vous avez décidé d’effectuer la transition de Windows Server Essentials vers Windows Server 2012 standard, effectuez les deux étapes suivantes :  
  
1. Achetez une licence pour Windows Server 2012 standard et le nombre approprié de licences d’accès client d’utilisateur et/ou de périphérique pour votre environnement.  
  
    Vous pouvez acheter une licence pour Windows Server 2012 standard auprès d’un détaillant ou d’un distributeur, ou à l’aide d’un [partenaire Microsoft](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
   > [!NOTE]
   >  Si vous avez acheté Windows Server 2012 standard initialement et que vous avez fait appel à vos droits de mise à niveau vers une version antérieure pour installer l’une de vos deux instances virtuelles sous Windows Server Essentials, il n’est pas nécessaire d’acheter d’autres éléments.  
   >   
   >  Si vous achetez Windows Server 2012 standard par le biais du canal de licence en volume, vous pouvez télécharger une image ISO et une clé de produit pour Windows Server 2012 standard à partir du centre de gestion des licences en volume (VLSC).  
   >   
   >  Si vous achetez Windows Server 2012 standard à partir de tous les autres canaux, vous pouvez télécharger une image ISO et une clé de produit d’évaluation pour Windows Server Essentials à partir du [Centre d’évaluation TechNet](https://technet.microsoft.com/evalcenter/jj659306.aspx). La réalisation de la transition comme décrit à l’étape suivante convertit le produit d’évaluation en un produit entièrement sous licence et pris en charge.  
  
2. Ouvrez Windows PowerShell en tant qu’administrateur, puis exécutez la commande suivante.  
  
    **DISM/Online/Set-Edition : ServerStandard/AcceptEULA/ProductKey :** *clé de produit*  
  
    Où *clé de produit* est la clé de produit de votre copie de Windows Server 2012 standard.  
  
    Le serveur redémarre pour terminer le processus de transition.  
  
   Après la transition, les fonctionnalités de Windows Server Essentials restent sur le serveur et sont prises en charge pour jusqu’à 75 utilisateurs et 75 appareils. Si vous dépassez l’une de ces limites, vous devez utiliser les outils natifs de Windows Server 2012 standard pour gérer les comptes d’utilisateur et les appareils.  
  
   En outre, après la transition vers Windows Server 2012 standard, les fonctionnalités multimédias de Windows Server Essentials ne sont plus disponibles. Cela comprend les fonctionnalités multimédias de l’accès web à distance et les paramètres multimédias sur le tableau de bord.  
  
## <a name="turn-off--windows-server-essentials-features"></a>Désactiver les fonctionnalités de Windows Server Essentials  
 Si vous n’avez plus besoin du tableau de bord Windows Server Essentials ou d’autres fonctionnalités à valeur ajoutée pour gérer le serveur, vous pouvez désactiver les fonctionnalités et les supprimer de votre serveur.  
  
 L' **Assistant désactiver les fonctionnalités de Windows Server Essentials** vous aide à désinstaller les fonctionnalités. Il nettoie également le serveur des fichiers qui ont été créés par le logiciel serveur Windows Server Essentials.  Certaines opérations de nettoyage sont effectuées immédiatement alors que d’autres sont lancées après le redémarrage du serveur.  
  
 L' **Assistant désactiver les fonctionnalités de Windows Server Essentials** exige que vous désinstalliez manuellement tous les compléments avant de pouvoir terminer l’Assistant. Pour afficher une liste des compléments installés, ouvrez la page Application du tableau de bord. L’Assistant vous avertit s’il détecte des compléments installés et vous invite à les désinstaller.  
  
 L' **Assistant désactiver les fonctionnalités de Windows Server Essentials** vous permet de choisir de conserver ou non les fichiers de sauvegarde pour les ordinateurs clients après la désactivation des fonctionnalités de Windows Server Essentials.  
  
 Il existe deux façons d’exécuter l' **Assistant désactiver les fonctionnalités de Windows Server Essentials** à partir du tableau de bord :  
  
#### <a name="from-the-alert"></a>À partir de l’alerte  
  
1.  À partir du Tableau de bord, ouvrez l'Afficheur des alertes.  
  
2.  Dans organiser la liste, sélectionnez l’alerte qui fournit des informations sur la désactivation des fonctionnalités Windows Server Essentials après la transition.  
  
3.  Dans l’alerte, cliquez sur **Désactiver les fonctionnalités de Windows Server Essentials**.  
  
#### <a name="from-the-get-help-and-support-pane"></a>À partir du volet d’aide et de support  
  
1. Dans la page d’accueil, cliquez sur Obtenir de l’aide et du support.  
  
2. Cliquez sur l' **Assistant désactiver les fonctionnalités de Windows Server Essentials**.  
  
   Il est possible que certaines tâches effectuées par l' **Assistant désactiver les fonctionnalités de Windows Server Essentials** ne se terminent pas correctement. Dans certains cas, cela peut empêcher le tableau de bord de fonctionner. Si cette situation se produit, vous pouvez démarrer l’Assistant manuellement en exécutant le fichier :  
  
   **%systemdrive%\Program Files\Windows Server\Bin\TurnOffFeaturesWizard.exe**  
  
## <a name="see-also"></a>Voir aussi  
  

-   [Passage à Windows Server 2012 R2 Standard](Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Migrer les données du serveur vers Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Passage à Windows Server 2012 R2 Standard](../migrate/Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Migrer les données du serveur vers Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

