---
title: Transition de WindowsServerEssentials vers Windows Server2012R2 Standard
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a14689e3-2310-4229-bd3e-dafc0e739e02
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d371e24b17310c0687666185f56fe07a135ff91f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-r2-standard"></a>Transition de WindowsServerEssentials vers Windows Server2012R2 Standard

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Windows Server2016 est le système d’exploitation prête pour le cloud qui prend en charge de vos charges de travail actuels lors de l’introduction de nouvelles technologies qui facilitent la transition vers le cloud computing. Contenu Windows Server2016 permet de vous préparer.

 WindowsServerEssentials prend en charge jusqu'à 25utilisateurs et 50appareils. Lorsque les besoins de votre entreprise dépassent la limite, vous pouvez effectuer une transition de licences sur place à partir de WindowsServerEssentials vers Windows Server2012R2 Standard de rester la conformité des licences.  
  
 Une fois la transition vers Windows Server2012R2 Standard, les limites de compte et les périphériques utilisateur sont supprimées, mais les fonctionnalités qui sont propres à WindowsServerEssentials (par exemple, le tableau de bord, accès Web à distance et sauvegarde des ordinateurs clients) sont toujours disponibles. Toutefois, les limitations techniques de ces fonctionnalités prennent en charge un maximum de 100comptes d’utilisateurs et 200appareils. La fonctionnalité de sauvegarde d’ordinateur client prendra en charge la sauvegarde de jusqu'à 75périphériques.  
  
> [!IMPORTANT]
>   Windows Server2012R2 Standard nécessite une licence d’accès client (CAL) pour chaque utilisateur ou appareil dans votre environnement. Cela est différent de WindowsServerEssentials, qui n’utilise pas le modèle de licence d’accès client et n’est pas fourni avec les licences d’accès client. Lors de la transition de WindowsServerEssentials vers Windows Server2012R2 Standard, vous devez acheter le nombre approprié et le type de licences d’accès client pour votre environnement (la plupart des clients achètent des licences d’accès client utilisateur).  
  
## <a name="before-the-transition"></a>Avant la transition  
  
-   Avant la transition de WindowsServerEssentials vers Windows Server2012R2 Standard, vous devez sauvegarde complète des données du serveur.  
  
    > [!IMPORTANT]
    >  Sans une sauvegarde complète du serveur, vous ne pouvez pas restaurer le serveur à l’état où il se trouvait avant la transition.  
  
-   En outre, assurez-vous que vous lire et comprenez le contrat de licence utilisateur final (CLUF) pour Windows Server2012R2 Standard. Pour afficher le CLUF:  
  
    1.  Ouvrez une fenêtre de commande en tant qu’administrateur.  
  
    2.  Exécutez la commande suivante:  
  
         **DISM /online /set-edition: ServerStandard /geteula:***chemin CLUF* (où *chemin CLUF* représente l’emplacement auquel vous souhaitez enregistrer le fichier CLUF, par exemple: C:\ws8std_eula.rtf). Veillez à utiliser .rtf comme extension de nom de fichier.  
  
    3.  Ouvrez l’emplacement où vous avez enregistré le fichier, puis double-cliquez sur le fichier pour l’ouvrir.  
  
## <a name="transition-to--windows-server-2012-r2-standard"></a>Transition vers Windows Server2012R2 Standard  
 Une fois que vous avez décidé de transition de WindowsServerEssentials vers Windows Server2012R2 Standard, complète ces deux étapes:  
  
1.  Achetez une licence pour Windows Server2012R2 Standard et le nombre approprié de l’utilisateur et/ou les licences d’accès client périphérique pour votre environnement.  
  
     Vous pouvez acheter une licence pour Windows Server2012R2 Standard à partir d’un détaillant, un distributeur, ou à l’aide d’un [MicrosoftPartner](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
    > [!NOTE]
    >  Si vous avez acheté Windows Server2012R2 Standard à initialement et exercé vos droits antérieure pour installer l’une de vos deux instances virtuelles en tant que WindowsServerEssentials, il est inutile d’acheter d’autres éléments.  
    >   
    >  Si vous achetez Windows Server2012R2 Standard via le canal de licence en Volume, vous pouvez télécharger une image ISO et une clé de produit pour Windows Server2012R2 Standard à partir de Volume Licensing Service Center (VLSC).  
    >   
    >  Si vous achetez Windows Server2012R2 Standard à partir d’un autre moyen, vous pouvez télécharger une image ISO et une clé de produit d’évaluation pour WindowsServerEssentials à partir de la [centre d’évaluation TechNet](https://technet.microsoft.com/evalcenter/jj659306.aspx). Effectuer la transition comme décrit dans l’étape suivante convertit le produit d’évaluation pour un produit entièrement sous licence et pris en charge.  
  
2.  Ouvrez Windows PowerShell en tant qu’administrateur, puis exécutez la commande suivante:  
  
     **DISM /online /set-edition: ServerStandard /accepteula /productkey:***clé de produit* (où *clé de produit* est la clé de produit pour votre copie de Windows Server2012R2 Standard).  
  
     Le serveur redémarre pour terminer le processus de transition.  
  
 Après la transition, les fonctionnalités de WindowsServerEssentials restent sur le serveur et sont prises en charge jusqu'à 100utilisateurs et 200appareils.  
  
## <a name="see-also"></a>Voir aussi  
  

-   [Migrer les données de serveur vers Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrer les données de serveur vers Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

