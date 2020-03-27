---
title: Créer l’unité d’organisation Serveurs de fichiers BranchCache
description: Cette rubrique fait partie du Guide de déploiement BranchCache pour Windows Server 2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les filiales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 40e871c25e31bbb15964d856da0f83cdaf50c48b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319318"
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>Créer l’unité d’organisation Serveurs de fichiers BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour créer une unité d’organisation (UO) dans Active Directory Domain Services (AD DS) pour les serveurs de fichiers BranchCache.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.  
  
### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>Pour créer l’unité d’organisation serveurs de fichiers BranchCache  
  
1.  Sur un ordinateur sur lequel AD DS est installé, dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Active Directory utilisateurs et ordinateurs**. La console Utilisateurs et ordinateurs Active Directory s'ouvre.  
  
2.  Dans la console utilisateurs et ordinateurs Active Directory, cliquez avec le bouton droit sur le domaine auquel vous souhaitez ajouter une unité d’organisation. Par exemple, si votre domaine s'appelle example.com, cliquez avec le bouton droit sur **example.com**. Pointez sur **Nouveau**, puis cliquez sur **Unité d’organisation**. La boîte **de dialogue nouvel objet-unité d’organisation** s’ouvre.  
  
3.  Dans la boîte de dialogue **nouvel objet-unité d’organisation** , dans **nom**, tapez un nom pour la nouvelle unité d’organisation. Par exemple, si vous souhaitez nommer l'unité d'organisation Serveurs de fichiers BranchCache, tapez **Serveurs de fichiers BranchCache**, puis cliquez sur **OK**.  
  


