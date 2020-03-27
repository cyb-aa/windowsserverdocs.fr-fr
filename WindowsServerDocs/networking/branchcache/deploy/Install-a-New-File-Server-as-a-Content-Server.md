---
title: Installer un nouveau serveur de fichiers comme serveur de contenu
description: Cette rubrique fait partie du Guide de déploiement BranchCache pour Windows Server 2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les filiales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1f49fc3c-28a6-4d3d-b787-1be9e61e792f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5abe1520de24d366df43210219119ae9f806adc4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319234"
---
# <a name="install-a-new-file-server-as-a-content-server"></a>Installer un nouveau serveur de fichiers comme serveur de contenu

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour installer le rôle serveur services de fichiers et le service **de rôle BranchCache pour fichiers réseau** sur un ordinateur exécutant Windows Server 2016.  
  
Pour exécuter cette procédure, il est nécessaire d'appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
> [!NOTE]  
> Pour effectuer cette procédure à l’aide de Windows PowerShell, exécutez Windows PowerShell en tant qu’administrateur, tapez les commandes suivantes à l’invite Windows PowerShell, puis appuyez sur entrée.  
>   
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`  
>   
> `Restart-Computer`  
>   
> Pour installer le service de rôle déduplication des données, tapez la commande suivante, puis appuyez sur entrée.  
>   
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`  
  
### <a name="to-install-file-services-and-the-branchcache-for-network-files-role-service"></a>Pour installer les services de fichiers et le service de rôle BranchCache pour fichiers réseau  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités s’ouvre. Dans **Avant de commencer**, cliquez sur **Suivant**.  
  
2.  Dans **Sélectionner le type d’installation**, vérifiez que installation basée sur **un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **suivant**.  
  
3.  Dans **Sélectionner le serveur de destination**, vérifiez que le serveur correct est sélectionné, puis cliquez sur **suivant**.  
  
4.  Dans **Sélectionner des rôles de serveurs**, dans **rôles**, Notez que le rôle **services de fichiers et de stockage** est déjà installé. Cliquez sur la flèche à gauche du nom de rôle pour développer la sélection des services de rôle, puis cliquez sur la flèche à gauche de **fichiers et de services iSCSI**.  
  
5.  Activez les cases à cocher pour **serveur de fichiers** et **BranchCache pour fichiers réseau**.  
  
    > [!TIP]  
    > Il est recommandé d’activer également la case à cocher de la **déduplication des données**.
  
    Cliquez sur **Suivant**.  
  
6.  Dans **Sélectionner des fonctionnalités**, cliquez sur **suivant**.  
  
7.  Dans **confirmer les sélections pour l’installation**, passez en revue vos sélections, puis cliquez sur **installer**. Le volet progression de l' **installation** s’affiche pendant l’installation. Une fois l’installation terminée, cliquez sur **Fermer**.
