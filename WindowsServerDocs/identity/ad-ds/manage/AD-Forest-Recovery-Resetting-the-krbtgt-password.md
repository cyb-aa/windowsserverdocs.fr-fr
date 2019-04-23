---
title: Récupération de forêt AD - réinitialiser le mot de passe krbtgt
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.technology: identity-adds
ms.openlocfilehash: 1ac0dcb9da1d10a417c128cb8498a5d8362d9a9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883740"
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>Récupération de forêt AD - réinitialiser le mot de passe krbtgt

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Utilisez la procédure suivante pour réinitialiser le mot de passe krbtgt pour le domaine. La procédure suivante s’applique à des contrôleurs de domaine accessible en écriture, mais les contrôleurs de domaine pas en lecture seule (RODC).
  
> [!IMPORTANT]
> Si vous envisagez de récupérer les RODC en ligne lors de la récupération de forêt, ne supprimez pas les comptes krbtgt pour le RODC. Le compte krbtgt pour un RODC est répertorié dans le format krbtgt_*nombre*.
>
> Si vous utilisez un filtre de mot de passe personnalisé (par exemple, passfilt.dll) sur un contrôleur de domaine, vous pouvez recevoir une erreur lorsque vous essayez de réinitialiser le mot de passe krbtgt. Pour plus d’informations, y compris une solution de contournement, consultez Base de connaissances Microsoft [article 2549833](https://support.microsoft.com/kb/2549833) (https://support.microsoft.com/kb/2549833).
  
## <a name="to-reset-the-krbtgt-password"></a>Pour réinitialiser le mot de passe krbtgt  
  
1. Cliquez sur **Démarrer**, pointez sur **le panneau de configuration**, pointez sur **outils d’administration**, puis cliquez sur **Active Directory Users and Computers**.
2. Cliquez sur **Affichage**, puis sur **Fonctionnalités avancées**.
3. Dans l’arborescence de la console, double-cliquez sur le conteneur de domaine, puis cliquez sur **utilisateurs**.
4. Dans le volet détails, cliquez sur le **krbtgt** compte d’utilisateur, puis cliquez sur **réinitialiser le mot de passe**.
   ![Réinitialiser le mot de passe](media/AD-Forest-Recovery-Resetting-the-krbtgt-password/resetpass1.png)
5. Dans **nouveau mot de passe**, tapez un nouveau mot de passe, retapez le mot de passe **confirmer le mot de passe**, puis cliquez sur **OK**. Le mot de passe que vous spécifiez n’est pas significatif, car le système génère un mot de passe fort automatiquement indépendant du mot de passe que vous spécifiez.
  
> [!NOTE]
> Vous devez effectuer cette opération à deux reprises. L’historique de mot de passe du compte krbtgt est de deux, ce qui signifie qu’il inclut les deux mots de passe plus récente. En réinitialisant le mot de passe deux fois vous désactivez efficacement des anciens mots de passe de l’historique, il n’existe aucun moyen d’un autre contrôleur de domaine répliquera avec ce contrôleur de domaine à l’aide d’un ancien mot de passe.

## <a name="next-steps"></a>Étapes suivantes

- [Guide de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md) 
