---
title: Récupération de la forêt Active Directory-supprimer le catalogue global
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 60087a62-11e6-4750-a70e-510f35315688
ms.technology: identity-adds
ms.openlocfilehash: 3ba1336828ad6031ce7fb47a659d084494466e4a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409099"
---
# <a name="ad-forest-recovery---removing-the-global-catalog"></a>Récupération de la forêt Active Directory-suppression du catalogue global  

>S’applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

 Utilisez la procédure suivante pour supprimer le catalogue global d’un contrôleur de périphérique. 
  
 La restauration d’un serveur de catalogue global à partir d’une sauvegarde peut entraîner la conservation par le catalogue global de données plus récentes pour l’un de ses réplicas partiels que le domaine correspondant faisant autorité pour ce réplica partiel. Dans ce cas, les données les plus récentes ne sont pas supprimées du catalogue global et peuvent même être répliquées sur d’autres serveurs de catalogue global. Par conséquent, même si vous avez restauré un contrôleur de service qui était un serveur de catalogue global, soit par inadvertance, soit parce que la sauvegarde solitaires que vous avez approuvée, vous devez supprimer le catalogue global peu de temps après l’opération de restauration. Lorsque le catalogue global est supprimé, l’ordinateur supprime tous ses réplicas partiels. 
  
## <a name="to-remove-the-global-catalog-using-active-directory-sites-and-services"></a>Pour supprimer le catalogue global à l’aide de sites et services Active Directory  
 
1. Ouvrez Gestionnaire de serveur, cliquez sur **Outils** , puis sur **Active Directory des sites et des services**. 
2. Dans l’arborescence de la console, développez le conteneur **sites** , puis sélectionnez le site approprié qui contient le serveur cible. 
3. Développez le conteneur **serveurs** , puis développez l’objet *serveur* pour le contrôleur de périphérique dont vous souhaitez supprimer le catalogue global. 
4. Cliquez avec le bouton droit sur **Paramètres NTDS**, puis cliquez sur **Propriétés**. 
5. Désactivez la case à cocher **catalogue global** . 
   ![supprimer le](media/AD-Forest-Recovery-Remove-GC/removegc1.png) GC
6. Cliquez sur **Appliquer**.
  
## <a name="to-remove-the-global-catalog-using-repadmin"></a>Pour supprimer le catalogue global à l’aide de repadmin  
  
Ouvrez une invite de commandes avec élévation de privilèges, tapez la commande suivante et appuyez sur entrée :  

   ```
   repadmin.exe /options DC_NAME –IS_GC  
   ```  

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)
