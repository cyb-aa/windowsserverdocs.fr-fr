---
title: Configurer un serveur de fichiers existant comme serveur de contenu
description: Cette rubrique fait partie de la BranchCache déploiement Guide pour Windows Server2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé d’optimiser l’utilisation de la bande passante réseau étendu dans les filiales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: bdac7d2a-25b4-4f61-bed1-b290700c18f3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: da4c38b6209dc10704aee8c79344ee2da98da272
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-an-existing-file-server-as-a-content-server"></a>Configurer un serveur de fichiers existant comme serveur de contenu

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour installer le **BranchCache pour fichiers réseau** service de rôle du rôle de serveur Services de fichiers sur un ordinateur exécutant Windows Server 2016.  
  
> [!IMPORTANT]  
> Si le rôle de serveur Services de fichiers n’est pas déjà installé, ne suivez pas cette procédure. Au lieu de cela, consultez [installer un nouveau serveur de fichiers comme serveur de contenu](../../branchcache/deploy/Install-a-New-File-Server-as-a-Content-Server.md).  
  
L’appartenance au groupe **administrateurs**, ou équivalent est le minimum requis pour effectuer cette procédure.  
  
> [!NOTE]  
> Pour effectuer cette procédure à l’aide de Windows PowerShell, exécutez Windows PowerShell en tant qu’administrateur, tapez les commandes suivantes à l’invite Windows PowerShell et appuyez sur ENTRÉE.  
>   
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`  
>   
> Pour installer le service de rôle déduplication des données, tapez la commande suivante et appuyez sur ENTRÉE.  
>   
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`  
  
### <a name="to-install-the-branchcache-for-network-files-role-service"></a>Pour installer le service BranchCache pour fichiers réseau rôle  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités**. L’Assistant Ajout de rôles et fonctionnalités s’ouvre. Cliquez sur **suivant**.  
  
2.  Dans **sélectionner le type d’installation**, assurez-vous que **installation basée sur un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **suivant**.  
  
3.  Dans **serveur de destination sélectionnez**, assurez-vous que le serveur correct est sélectionné, puis cliquez sur **suivant**.  
  
4.  Dans **sélectionner des rôles de serveur**, dans **rôles**, notez que le **Services de fichiers et de stockage** rôle est déjà installé; Cliquez sur la flèche vers la gauche du nom du rôle d’étendre la sélection de services de rôle, puis cliquez sur la flèche située à gauche du **de fichiers et iSCSI Services**.  
  
5.  Sélectionnez la case à cocher **BranchCache pour fichiers réseau**.  
  
    > [!TIP]  
    > Si vous n’avez pas déjà fait, il est recommandé de sélectionner également la case à cocher **la déduplication des données**.  
  
    Cliquez sur **suivant**.  
  
6.  Dans **sélectionner des fonctionnalités**, cliquez sur **suivant**.  
  
7.  Dans **confirmer les sélections d’installation**, passez en revue vos sélections, puis cliquez sur **installer**. Le **progression de l’Installation** volet s’affiche lors de l’installation. Lorsque l’installation est terminée, cliquez sur **fermer**.  
  


