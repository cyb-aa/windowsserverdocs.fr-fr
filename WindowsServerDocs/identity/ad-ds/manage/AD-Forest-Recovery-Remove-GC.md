---
title: "Récupération de la forêt ActiveDirectory - supprimer le catalogue global"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 60087a62-11e6-4750-a70e-510f35315688
ms.technology: identity-adfs
ms.openlocfilehash: b7da7c03cbae4691e8f902f1be0cb33c0c912248
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---removing-the-global-catalog"></a>Récupération de la forêt ActiveDirectory - supprimer le catalogue global  

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

 Utilisez la procédure suivante pour supprimer le catalogue global à partir d’un contrôleur de domaine.  
  
 Restauration d’un serveur de catalogue global à partir de la sauvegarde peut entraîner le catalogue global contenant des données plus récentes pour un de ses réplicas partielles du domaine correspondant qui fait autorité pour ce réplica partiel. Dans ce cas, les données les plus récentes ne seront pas supprimées du catalogue global et peuvent même répliquer vers d’autres serveurs de catalogue global. Par conséquent, même si vous n’avez restauré un contrôleur de domaine qui a été un serveur de catalogue global, soit par inadvertance ou car il s’agissait de la sauvegarde Solitaire vous approuvé, vous devez supprimer le catalogue global immédiatement après que l’opération de restauration est terminée. Lorsque le catalogue global est supprimé, l’ordinateur supprime tous ses réplicas partielles.  
  
## <a name="to-remove-the-global-catalog-using-active-directory-sites-and-services"></a>Pour supprimer le catalogue global à l’aide des Services et Sites ActiveDirectory  
 
1.  Ouvrez le Gestionnaire de serveur, cliquez sur **outils** et cliquez sur **Services et Sites ActiveDirectory**.  
2.  Dans l’arborescence de la console, développez le **Sites** conteneur, puis sélectionnez le site approprié qui contient le serveur cible.  
3.  Développez le **serveurs** conteneur, puis développez le *server* objet pour le contrôleur de domaine à partir duquel vous souhaitez supprimer le catalogue global.  
4.  Avec le bouton droit **Paramètres NTDS**, puis cliquez sur **propriétés**.  
5.  Désactivez le **catalogue Global** case à cocher.  
![Supprimer le catalogue global](media/AD-Forest-Recovery-Remove-GC/removegc1.png)
6.  Cliquez sur **appliquer**.
  
## <a name="to-remove-the-global-catalog-using-repadmin"></a>Pour supprimer le catalogue global à l’aide de Repadmin  
  
1.  Ouvrez une invite de commandes avec élévation de privilèges, tapez la commande suivante et appuyez sur ENTRÉE:  
  
    ```  
    repadmin.exe /options DC_NAME –IS_GC  
    ```  
  
 ## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de forêt ActiveDirectory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)
