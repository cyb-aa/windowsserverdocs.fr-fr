---
title: Transition de Windows Server Essentials vers Windows Server 2012 R2 Standard
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: a14689e3-2310-4229-bd3e-dafc0e739e02
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f7914ff205382ed2c74cb130061f850e2c0675f5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852292"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-r2-standard"></a>Transition de Windows Server Essentials vers Windows Server 2012 R2 Standard

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Windows Server 2016 est le système d’exploitation Cloud qui prend en charge vos charges de travail actuelles tout en introduisant de nouvelles technologies qui facilitent la transition vers cloud computing. Le contenu de Windows Server 2016 vous aide à vous préparer.

 Windows Server Essentials prend en charge jusqu’à 25 utilisateurs et 50 appareils. Lorsque les besoins de votre entreprise dépassent la limite, vous pouvez effectuer une transition de licence sur place de Windows Server Essentials vers Windows Server 2012 R2 Standard pour rester conforme à la licence.  
  
 Une fois que vous avez effectué la transition vers Windows Server 2012 R2 Standard, les limites du compte d’utilisateur et des appareils sont supprimées, mais les fonctionnalités propres à Windows Server Essentials (telles que le tableau de bord, les Accès web à distance et la sauvegarde des ordinateurs clients) restent disponibles. Toutefois, les limitations techniques de ces fonctionnalités prennent en charge un nombre maximal de 100 comptes d’utilisateurs et de 200 appareils. La fonctionnalité de sauvegarde de l'ordinateur client prendra en charge la sauvegarde de 75 appareils au plus.  
  
> [!IMPORTANT]
>   Windows Server 2012 R2 Standard nécessite une licence d’accès client (CAL) pour chaque utilisateur ou périphérique de votre environnement. Cela diffère de Windows Server Essentials, qui n’utilise pas le modèle de licence d’accès client et n’est fourni avec aucune licence d’accès client. Lors de la transition de Windows Server Essentials vers Windows Server 2012 R2 Standard, vous devez acheter le nombre et le type appropriés de licences d’accès client pour votre environnement (la plupart des clients achètent des CAL utilisateur).  
  
## <a name="before-the-transition"></a>Avant la transition  
  
-   Avant de passer de Windows Server Essentials à Windows Server 2012 R2 Standard, vous devez sauvegarder entièrement les données du serveur.  
  
    > [!IMPORTANT]
    >  Sans une sauvegarde complète du serveur, vous ne pouvez pas rétablir l’état du serveur avant la transition.  
  
-   En outre, assurez-vous de lire et de comprendre le contrat de licence utilisateur final (CLUF) pour Windows Server 2012 R2 Standard. Pour afficher le CLUF :  
  
    1.  Ouvrez une fenêtre de commande comme administrateur.  
  
    2.  Exécutez la commande suivante :  
  
         **DISM/Online/Set-Edition : ServerStandard/geteula :** *EULA Path* (où *EULA Path* représente l’emplacement où vous souhaitez enregistrer le fichier eula, par exemple : c:\ ws8std_eula. rtf). Utilisez bien .rtf comme extension de nom de fichier.  
  
    3.  Ouvrez l’emplacement où vous avez enregistré le fichier, puis double-cliquez sur le fichier pour l’ouvrir.  
  
## <a name="transition-to--windows-server-2012-r2-standard"></a>Passage à Windows Server 2012 R2 Standard  
 Une fois que vous avez décidé d’effectuer la transition de Windows Server Essentials vers Windows Server 2012 R2 Standard, effectuez les deux étapes suivantes :  
  
1. Achetez une licence pour Windows Server 2012 R2 Standard et le nombre approprié de licences d’accès client d’utilisateur et/ou de périphérique pour votre environnement.  
  
    Vous pouvez acheter une licence pour Windows Server 2012 R2 Standard auprès d’un détaillant ou d’un distributeur, ou à l’aide d’un [partenaire Microsoft](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
   > [!NOTE]
   >  Si vous avez déjà acheté Windows Server 2012 R2 Standard et que vous avez testé vos droits de rétrogradation pour installer l’une de vos deux instances virtuelles en tant que Windows Server Essentials, vous n’avez rien à acheter.  
   >   
   >  Si vous achetez Windows Server 2012 R2 standard par le biais du canal de licence en volume, vous pouvez télécharger une image ISO et une clé de produit pour Windows Server 2012 R2 standard à partir du centre de gestion des licences en volume (VLSC).  
   >   
   >  Si vous achetez Windows Server 2012 R2 standard à partir de n’importe quel autre canal, vous pouvez télécharger une image ISO et une clé de produit d’évaluation pour Windows Server Essentials à partir du [Centre d’évaluation TechNet](https://technet.microsoft.com/evalcenter/jj659306.aspx). La réalisation de la transition comme décrit à l’étape suivante convertit le produit d’évaluation en un produit entièrement sous licence et pris en charge.  
  
2. Ouvrez Windows PowerShell en tant qu'administrateur, puis exécutez la commande suivante :  
  
    **DISM/Online/Set-Edition : ServerStandard/AcceptEULA/ProductKey :** *clé* de produit (où *clé de produit* est la clé de produit de votre copie de Windows Server 2012 R2 Standard).  
  
    Le serveur redémarre pour terminer le processus de transition.  
  
   Après la transition, les fonctionnalités de Windows Server Essentials restent sur le serveur et sont prises en charge pour jusqu’à 100 utilisateurs et 200 appareils.  
  
## <a name="see-also"></a>Voir aussi  
  

-   [Migrer les données du serveur vers Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrer les données du serveur vers Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

