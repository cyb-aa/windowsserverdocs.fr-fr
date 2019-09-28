---
title: Configurer un serveur de fichiers existant comme serveur de contenu
description: Cette rubrique fait partie du Guide de déploiement BranchCache pour Windows Server 2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les filiales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: bdac7d2a-25b4-4f61-bed1-b290700c18f3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f675322c32db0816d5afb155d53fad9f096ad650
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356693"
---
# <a name="configure-an-existing-file-server-as-a-content-server"></a>Configurer un serveur de fichiers existant comme serveur de contenu

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour installer le service de rôle **BranchCache pour fichiers réseau** du rôle serveur services de fichiers sur un ordinateur exécutant Windows Server 2016.  
  
> [!IMPORTANT]  
> Si le rôle serveur services de fichiers n’est pas déjà installé, ne suivez pas cette procédure. Au lieu de cela, consultez [installer un nouveau serveur de fichiers en tant que serveur de contenu](../../branchcache/deploy/Install-a-New-File-Server-as-a-Content-Server.md).  
  
Pour exécuter cette procédure, il est nécessaire d'appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
> [!NOTE]  
> Pour effectuer cette procédure à l’aide de Windows PowerShell, exécutez Windows PowerShell en tant qu’administrateur, tapez les commandes suivantes à l’invite Windows PowerShell, puis appuyez sur entrée.  
>   
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`  
>   
> Pour installer le service de rôle déduplication des données, tapez la commande suivante, puis appuyez sur entrée.  
>   
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`  
  
### <a name="to-install-the-branchcache-for-network-files-role-service"></a>Pour installer le service de rôle BranchCache pour fichiers réseau  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités s’ouvre. Cliquez sur **Suivant**.  
  
2.  Dans **Sélectionner le type d’installation**, vérifiez que installation basée sur **un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **suivant**.  
  
3.  Dans **Sélectionner le serveur de destination**, vérifiez que le serveur correct est sélectionné, puis cliquez sur **suivant**.  
  
4.  Dans **Sélectionner des rôles de serveurs**, dans **rôles**, Notez que le rôle **services de fichiers et de stockage** est déjà installé. Cliquez sur la flèche à gauche du nom de rôle pour développer la sélection des services de rôle, puis cliquez sur la flèche à gauche de **fichiers et de services iSCSI**.  
  
5.  Activez la case à cocher pour **BranchCache pour fichiers réseau**.  
  
    > [!TIP]  
    > Si vous ne l’avez pas encore fait, il est recommandé d’activer également la case à cocher de la **déduplication des données**.  
  
    Cliquez sur **Suivant**.  
  
6.  Dans **Sélectionner des fonctionnalités**, cliquez sur **suivant**.  
  
7.  Dans **confirmer les sélections pour l’installation**, passez en revue vos sélections, puis cliquez sur **installer**. Le volet progression de l' **installation** s’affiche pendant l’installation. Une fois l’installation terminée, cliquez sur **Fermer**.  
  


