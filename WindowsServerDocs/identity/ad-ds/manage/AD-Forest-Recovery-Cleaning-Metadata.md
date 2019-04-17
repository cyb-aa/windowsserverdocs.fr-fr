---
title: "Récupération de la forêt ActiveDirectory - nettoyage des métadonnées des contrôleurs de domaine supprimé"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.technology: identity-adfs
ms.openlocfilehash: 3027c59b58801b44d20127e6bcf62dd7319708bd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>Récupération de la forêt ActiveDirectory - nettoyage des métadonnées des contrôleurs de domaine accessible en écriture supprimés 

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2
 
 Nettoyage des métadonnées supprime les données ActiveDirectory qui identifie un contrôleur de domaine pour le système de réplication.  
  
 Utilisez la procédure suivante pour supprimer les objets de contrôleur de domaine pour les contrôleurs de domaine que vous souhaitez ajouter au réseau à réinstaller ADDS.  
  
 Si vous utilisez la version des utilisateurs ActiveDirectory et des ordinateurs ou les Sites ActiveDirectory et Services qui est inclus les outils d’Administration de serveur distant (RSAT), nettoyage des métadonnées est effectué automatiquement lorsque vous supprimez un objet contrôleur de domaine.  
  

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>La suppression d’un contrôleur de domaine à l’aide d’ActiveDirectory Users and Computers  
 Lorsque vous utilisez la version d’utilisateurs ActiveDirectory, les ordinateurs ou centre d’administration ActiveDirectory dans Server Administration outils distant (RSAT), nettoyage des métadonnées est effectué automatiquement lorsque vous supprimez l’objet contrôleur de domaine. L’objet serveur et l’objet ordinateur sont également automatiquement supprimés.  
  
 Comme alternative, vous pouvez également utiliser les Sites ActiveDirectory et les Services de serveur distant pour supprimer un objet contrôleur de domaine. Si vous utilisez des Services et Sites ActiveDirectory, vous devez supprimer l’objet serveur associé et un objet Paramètres NTDS avant de supprimer l’objet contrôleur de domaine.  
  
 Pour télécharger le serveur distant:  

-   [Outils d’Administration de serveur distant pour Windows10](https://www.microsoft.com/download/details.aspx?id=45520)
  
-   [Outils d’Administration de serveur distant pour Windows8](https://www.microsoft.com/download/details.aspx?id=28972)  

-   [Outils d’Administration de serveur distant pour Windows7 avec Service Pack1 (SP1)](https://www.microsoft.com/download/details.aspx?id=7887)  
  
-   [Outils d’Administration de serveur distant Microsoft pour WindowsVista](https://www.microsoft.com/download/details.aspx?id=21090)  
  
 La procédure suivante est le même pour les contrôleurs de domaine qui exécutent soit Windows Server2016, 2008, 2008R2 ou 2012. La contrôleur de domaine cible de l’opération de nettoyage des métadonnées peut exécuter n’importe quelle version de Windows Server.  
  
### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>Pour supprimer un objet contrôleur de domaine à l’aide d’ActiveDirectory Users and Computers dans RSAT  
  
1.  Cliquez sur **Démarrer**, cliquez sur **outils d’administration**, puis cliquez sur **ActiveDirectory Users and Computers**.  
2.  Dans l’arborescence de la console, double-cliquez sur le conteneur de domaine, puis double-cliquez sur le **contrôleurs de domaine** unité d’organisation (UO).  
3.  Dans le volet d’informations, cliquez sur le contrôleur de domaine que vous souhaitez supprimer, puis cliquez sur **supprimer**. 
![Supprimer](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png) 
4.  Cliquez sur **Oui** pour confirmer la suppression. Sélectionnez le **ce contrôleur de domaine est définitivement hors connexion et ne peut plus être rétrogradé à l’aide de l’Assistant Installation Services Directory domaine (DCPROMO) Active** case à cocher, puis cliquez sur **supprimer**.  
5.  Si le contrôleur de domaine a un serveur de catalogue global, cliquez sur **Oui** confirmer que la suppression.  
  
## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de forêt ActiveDirectory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)
  
