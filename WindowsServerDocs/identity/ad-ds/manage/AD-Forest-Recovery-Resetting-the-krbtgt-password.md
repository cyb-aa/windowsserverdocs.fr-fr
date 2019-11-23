---
title: Récupération de la forêt Active Directory-réinitialisation du mot de passe krbtgt
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.technology: identity-adds
ms.openlocfilehash: 14dd09c6177d473547a67e1d79e9714f0a7a29b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390304"
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>Récupération de la forêt Active Directory-réinitialisation du mot de passe krbtgt

>S’applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Utilisez la procédure suivante pour réinitialiser le mot de passe krbtgt pour le domaine. La procédure suivante applique les contrôleurs de domaine accessibles en écriture, mais pas les contrôleurs de domaine en lecture seule (RODC).
  
> [!IMPORTANT]
> Si vous envisagez de récupérer des RODC en ligne pendant la récupération de la forêt, ne supprimez pas les comptes krbtgt pour les contrôleurs RODC. Le compte krbtgt pour un contrôleur de domaine en lecture seule est indiqué au format krbtgt_*numéro*.
>
> Si vous utilisez un filtre de mot de passe personnalisé (tel que Passfilt. dll) sur un contrôleur de périphérique, vous risquez de recevoir une erreur lorsque vous essayez de réinitialiser le mot de passe krbtgt. Pour plus d’informations, y compris une solution de contournement, consultez [l’article 2549833](https://support.microsoft.com/kb/2549833) de la base de connaissances Microsoft (https://support.microsoft.com/kb/2549833).
  
## <a name="to-reset-the-krbtgt-password"></a>Pour réinitialiser le mot de passe krbtgt  
  
1. Cliquez sur **Démarrer**, pointez sur **panneau de configuration**, pointez sur **Outils d’administration**, puis cliquez sur **Active Directory utilisateurs et ordinateurs**.
2. Cliquez sur **Affichage**, puis sur **Fonctionnalités avancées**.
3. Dans l’arborescence de la console, double-cliquez sur le conteneur de domaine, puis cliquez sur **utilisateurs**.
4. Dans le volet d’informations, cliquez avec le bouton droit sur le compte d’utilisateur **krbtgt** , puis cliquez sur **Réinitialiser le mot de passe**.
   ![de réinitialiser le mot de passe](media/AD-Forest-Recovery-Resetting-the-krbtgt-password/resetpass1.png)
5. Dans **nouveau mot**de passe, tapez un nouveau mot de passe, retapez le mot de passe dans **confirmer le mot de passe**, puis cliquez sur **OK**. Le mot de passe que vous spécifiez n’est pas significatif car le système génère un mot de passe fort automatiquement indépendant du mot de passe que vous spécifiez.
  
> [!NOTE]
> Vous devez effectuer cette opération deux fois. L’historique du mot de passe du compte krbtgt est deux, ce qui signifie qu’il contient les deux mots de passe les plus récents. En réinitialisant le mot de passe deux fois, vous effacez les anciens mots de passe de l’historique. il n’existe donc aucun moyen de répliquer un autre contrôleur de périphérique avec ce contrôleur de périphérique à l’aide d’un ancien mot de passe.

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md) 
