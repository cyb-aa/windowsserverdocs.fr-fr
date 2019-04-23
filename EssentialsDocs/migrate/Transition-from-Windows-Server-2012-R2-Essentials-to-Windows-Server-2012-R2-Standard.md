---
title: Transition de Windows Server Essentials vers Windows Server 2012 R2 Standard
description: Décrit comment utiliser Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840080"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-r2-standard"></a>Transition de Windows Server Essentials vers Windows Server 2012 R2 Standard

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Windows Server 2016 est le système d’exploitation prêt pour le cloud qui prend en charge de vos charges de travail actuelles tout en introduisant de nouvelles technologies qui facilitent la transition vers le cloud computing. Contenu de Windows Server 2016 vous aide à vous préparer.

 Windows Server Essentials prend en charge jusqu'à 25 utilisateurs et 50 appareils. Lorsque les besoins de votre entreprise dépassent la limite, vous pouvez effectuer une transition de licence de place de Windows Server Essentials vers Windows Server 2012 R2 Standard restent la conformité des licences.  
  
 Une fois la transition vers Windows Server 2012 R2 Standard, les limites de compte et les appareils des utilisateurs sont supprimés, mais les fonctionnalités qui sont propres à Windows Server Essentials (tels que le tableau de bord, accès Web à distance et sauvegarde de l’ordinateur client) est toujours disponibles. Toutefois, les limitations techniques de ces fonctionnalités prennent en charge un nombre maximal de 100 comptes d’utilisateurs et de 200 appareils. La fonctionnalité de sauvegarde de l'ordinateur client prendra en charge la sauvegarde de 75 appareils au plus.  
  
> [!IMPORTANT]
>   Windows Server 2012 R2 Standard requiert une licence d’accès client (CAL) pour chaque utilisateur ou appareil dans votre environnement. Cela est différent de Windows Server Essentials, qui n’utilise pas le modèle de licence d’accès client et ne sont pas fournies avec des licences d’accès client. Lors de la transition à partir de Windows Server Essentials vers Windows Server 2012 R2 Standard, vous devrez acheter le nombre approprié et le type de CAL pour votre environnement (la plupart des clients achètent des licences d’accès client utilisateur).  
  
## <a name="before-the-transition"></a>Avant la transition  
  
-   Avant la transition de Windows Server Essentials vers Windows Server 2012 R2 Standard, vous devez effectuer une sauvegarde complète des données du serveur.  
  
    > [!IMPORTANT]
    >  Sans une sauvegarde complète du serveur, vous ne pouvez pas rétablir l’état du serveur avant la transition.  
  
-   En outre, assurez-vous que vous lirez et comprenez le contrat de licence utilisateur final (CLUF) pour Windows Server 2012 R2 Standard. Pour afficher le CLUF :  
  
    1.  Ouvrez une fenêtre de commandes en tant qu’administrateur.  
  
    2.  Exécutez la commande suivante :  
  
         **DISM / en ligne/set-edition : ServerStandard /geteula :** *chemin CLUF* (où *chemin CLUF* représente l’emplacement auquel vous souhaitez enregistrer le fichier CLUF, par exemple : C:\ws8std_eula.rtf). Utilisez bien .rtf comme extension de nom de fichier.  
  
    3.  Ouvrez l’emplacement où vous avez enregistré le fichier, puis double-cliquez sur le fichier pour l’ouvrir.  
  
## <a name="transition-to--windows-server-2012-r2-standard"></a>Transition vers Windows Server 2012 R2 Standard  
 Une fois que vous avez décidé d’effectuer une transition entre Windows Server Essentials vers Windows Server 2012 R2 Standard, complète ces deux étapes :  
  
1.  Achetez une licence Windows Server 2012 R2 Standard et le nombre approprié de l’utilisateur et/ou les licences d’accès client périphérique pour votre environnement.  
  
     Vous pouvez acheter une licence pour Windows Server 2012 R2 Standard à partir d’un détaillant, un serveur de distribution, ou à l’aide d’un [Microsoft Partner](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
    > [!NOTE]
    >  Si vous avez initialement acheté Windows Server 2012 R2 Standard et exercé vos droits à une version antérieure pour installer l’une de vos deux instances virtuelles en tant que Windows Server Essentials, il est inutile d’acheter d’autres.  
    >   
    >  Si vous achetez Windows Server 2012 R2 Standard via le canal de licence en Volume, vous pouvez télécharger une image ISO et une clé de produit pour Windows Server 2012 R2 Standard à partir de Volume Licensing Service Center (VLSC).  
    >   
    >  Si vous achetez Windows Server 2012 R2 Standard à partir de n’importe quel autre canal, vous pouvez télécharger une image ISO et une clé de produit d’évaluation pour Windows Server Essentials à partir de la [centre d’évaluation TechNet](https://technet.microsoft.com/evalcenter/jj659306.aspx). La réalisation de la transition comme décrit à l’étape suivante convertit le produit d’évaluation en un produit entièrement sous licence et pris en charge.  
  
2.  Ouvrez Windows PowerShell en tant qu'administrateur, puis exécutez la commande suivante :  
  
     **dism /online /set-edition:ServerStandard /accepteula /productkey:** *Clé de produit* (où *clé de produit* est la clé de produit pour votre copie de Windows Server 2012 R2 Standard).  
  
     Le serveur redémarre pour terminer le processus de transition.  
  
 Après la transition, les fonctionnalités de Windows Server Essentials restent sur le serveur et sont prises en charge jusqu'à 100 utilisateurs et 200 appareils.  
  
## <a name="see-also"></a>Voir aussi  
  

-   [Migrer des données de serveur vers Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrer des données de serveur vers Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

