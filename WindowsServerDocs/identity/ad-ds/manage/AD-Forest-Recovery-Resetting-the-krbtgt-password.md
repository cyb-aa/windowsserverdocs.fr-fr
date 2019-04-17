---
title: "Récupération de la forêt ActiveDirectory - réinitialiser le mot de passe krbtgt"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.technology: identity-adfs
ms.openlocfilehash: 445fa18503e26d04e20a61cfe652424f78631abe
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>Récupération de la forêt ActiveDirectory - réinitialiser le mot de passe krbtgt 

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

 Utilisez la procédure suivante pour réinitialiser le mot de passe krbtgt pour le domaine. La procédure suivante s’applique à des contrôleurs de domaine accessible en écriture, mais les contrôleurs de domaine pas en lecture seule (RODC).  
  
> [!IMPORTANT]
>  Si vous envisagez de récupérer les RODC en ligne pendant la récupération de forêt, ne supprimez pas les comptes krbtgt pour le RODC. Le compte krbtgt pour un RODC est répertorié dans le format krbtgt_*nombre*.  
>   
>  Si vous utilisez un filtre de mot de passe personnalisé (par exemple, passfilt.dll) sur un contrôleur de domaine, vous pouvez recevoir une erreur lorsque vous essayez de réinitialiser le mot de passe krbtgt. Pour plus d’informations, y compris une solution de contournement, voir la Base de connaissances Microsoft [article 2549833](https://support.microsoft.com/kb/2549833) (https://support.microsoft.com/kb/2549833).  
  
## <a name="to-reset-the-krbtgt-password"></a>Pour réinitialiser le mot de passe krbtgt  
  
1.  Cliquez sur **Démarrer**, pointez sur **le panneau de configuration**, pointez sur **outils d’administration**, puis cliquez sur **ActiveDirectory Users and Computers**.  
2.  2.  Cliquez sur **affichage**, puis cliquez sur **fonctionnalités avancées**.  
3.  Dans l’arborescence de la console, double-cliquez sur le conteneur de domaine, puis cliquez sur **utilisateurs**.  
4.  Dans le volet d’informations, cliquez sur le **krbtgt** compte d’utilisateur, puis cliquez sur **réinitialiser le mot de passe**.  
![Réinitialiser le mot de passe](media/AD-Forest-Recovery-Resetting-the-krbtgt-password/resetpass1.png)
5.  Dans **nouveau mot de passe**, tapez un nouveau mot de passe, retapez le mot de passe dans **confirmer le mot de passe**, puis cliquez sur **OK**. Le mot de passe que vous spécifiez n’est pas important car le système génère un mot de passe fort automatiquement indépendant du mot de passe que vous spécifiez.  
  
    > [!NOTE]
    >  Vous devez effectuer cette opération deux fois. L’historique du mot de passe du compte krbtgt est de deux, ce qui signifie qu’il inclut les deux mots de passe plus récentes. Par la réinitialisation du mot de passe efficacement deux fois que vous désactivez les anciens mots de passe de l’historique, donc il n’existe aucun moyen d’un autre contrôleur de domaine est répliquées avec ce contrôleur de domaine à l’aide d’un ancien mot de passe.  
 
## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de forêt ActiveDirectory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md) 
  
