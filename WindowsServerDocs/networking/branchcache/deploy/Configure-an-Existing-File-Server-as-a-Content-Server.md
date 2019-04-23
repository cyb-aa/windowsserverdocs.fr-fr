---
title: Configurer un serveur de fichiers existant comme serveur de contenu
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server 2016, qui montre comment déployer BranchCache en mode cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les succursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: bdac7d2a-25b4-4f61-bed1-b290700c18f3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d031e8aa853849c322692552ca9107838cebb5e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866910"
---
# <a name="configure-an-existing-file-server-as-a-content-server"></a>Configurer un serveur de fichiers existant comme serveur de contenu

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour installer le **BranchCache pour fichiers réseau** service de rôle du rôle serveur Services de fichiers sur un ordinateur exécutant Windows Server 2016.  
  
> [!IMPORTANT]  
> Si le rôle de serveur Services de fichiers n’est pas déjà installé, ne suivez pas cette procédure. Au lieu de cela, consultez [installer un nouveau serveur de fichiers comme serveur de contenu](../../branchcache/deploy/Install-a-New-File-Server-as-a-Content-Server.md).  
  
Pour exécuter cette procédure, il est nécessaire d'appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
> [!NOTE]  
> Pour effectuer cette procédure à l’aide de Windows PowerShell, exécutez Windows PowerShell en tant qu’administrateur, tapez les commandes suivantes à l’invite Windows PowerShell, puis appuyez sur ENTRÉE.  
>   
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`  
>   
> Pour installer le service de rôle de la déduplication des données, tapez la commande suivante, puis appuyez sur ENTRÉE.  
>   
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`  
  
### <a name="to-install-the-branchcache-for-network-files-role-service"></a>Pour installer le BranchCache pour fichiers réseau  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et fonctionnalités s’ouvre. Cliquez sur **Suivant**.  
  
2.  Dans **sélectionner type d’installation**, vérifiez que **installation en fonction du rôle ou une fonctionnalité** est sélectionnée, puis cliquez sur **suivant**.  
  
3.  Dans **server de sélectionner la destination**, assurez-vous que le serveur correct est sélectionné, puis cliquez sur **suivant**.  
  
4.  Dans **sélectionner des rôles de serveur**, dans **rôles**, notez que le **Services de fichiers et de stockage** rôle est déjà installé ; cliquez sur la flèche à gauche du nom du rôle pour développer le sélection des services de rôle, puis cliquez sur la flèche à gauche de **fichiers et iSCSI Services**.  
  
5.  Sélectionnez la case à cocher **BranchCache pour fichiers réseau**.  
  
    > [!TIP]  
    > Si vous ne le n'avez pas déjà fait, il est recommandé que vous sélectionnez également la case à cocher **la déduplication des données**.  
  
    Cliquez sur **Suivant**.  
  
6.  Dans **sélectionner des fonctionnalités**, cliquez sur **suivant**.  
  
7.  Dans **confirmer les sélections d’installation**, passez en revue vos sélections, puis cliquez sur **installer**. Le **progression de l’Installation** volet s’affiche pendant l’installation. Lors de l’installation est terminée, cliquez sur **fermer**.  
  


