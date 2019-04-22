---
title: Récupération de forêt AD - supprimer le catalogue global
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 60087a62-11e6-4750-a70e-510f35315688
ms.technology: identity-adds
ms.openlocfilehash: d730ce65fc179aee6a98f7cfc1a5b693bfcd6c93
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817070"
---
# <a name="ad-forest-recovery---removing-the-global-catalog"></a>Récupération de forêt AD - suppression du catalogue global  

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

 Utilisez la procédure suivante pour supprimer le catalogue global à partir d’un contrôleur de domaine. 
  
 Restauration d’un serveur de catalogue global à partir de la sauvegarde peut entraîner le catalogue global contenant des données plus récentes pour l’une de ses réplicas partielles que le domaine correspondant qui fait autorité pour ce réplica partiel. Dans ce cas, les données plus récentes ne seront pas supprimées à partir du catalogue global et peuvent même être répliqués vers d’autres serveurs de catalogue global. Par conséquent, même si vous n’avez restauré un contrôleur de domaine qui a été un serveur de catalogue global, soit par inadvertance ou car il s’agit de la sauvegarde Solitaire que vous avez confiance, vous devez supprimer le catalogue global peu après que l’opération de restauration est terminée. Lorsque le catalogue global est supprimé, l’ordinateur supprime tous ses réplicas partielles. 
  
## <a name="to-remove-the-global-catalog-using-active-directory-sites-and-services"></a>Pour supprimer le catalogue global à l’aide des Services et Sites Active Directory  
 
1. Ouvrez le Gestionnaire de serveur, cliquez sur **outils** et cliquez sur **Sites Active Directory et Services**. 
2. Dans l’arborescence de la console, développez le **Sites** conteneur, puis sélectionnez le site approprié qui contient le serveur cible. 
3. Développez le **serveurs** conteneur, puis développez le *server* objet pour le contrôleur de domaine à partir duquel vous souhaitez supprimer le catalogue global. 
4. Avec le bouton droit **Paramètres NTDS**, puis cliquez sur **propriétés**. 
5. Effacer la **catalogue Global** case à cocher. 
   ![Supprimer le catalogue global](media/AD-Forest-Recovery-Remove-GC/removegc1.png)
6. Cliquez sur **Appliquer**.
  
## <a name="to-remove-the-global-catalog-using-repadmin"></a>Pour supprimer le catalogue global à l’aide de Repadmin  
  
Ouvrez une invite de commandes avec élévation de privilèges, tapez la commande suivante et appuyez sur ENTRÉE :  

   ```
   repadmin.exe /options DC_NAME –IS_GC  
   ```  

## <a name="next-steps"></a>Étapes suivantes

- [Guide de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)
