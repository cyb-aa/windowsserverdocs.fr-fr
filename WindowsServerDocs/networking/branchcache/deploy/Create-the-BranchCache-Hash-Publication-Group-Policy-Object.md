---
title: Créer l’objet de stratégie de groupe de la publication de hachages BranchCache
description: Cette rubrique fait partie du Guide de déploiement BranchCache pour Windows Server 2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les filiales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: c3d33bed-83ef-4eb8-acf9-0719ecb4a931
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e546705d022bbac2588ace5b3e2c6c807c96da63
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319307"
---
# <a name="create-the-branchcache-hash-publication-group-policy-object"></a>Créer l’objet de stratégie de groupe de la publication de hachages BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour créer la publication de hachage BranchCache stratégie de groupe objet (GPO).  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.  
  
> [!NOTE]  
> Avant d'exécuter cette procédure, vous devez créer l'unité d'organisation Serveurs de fichiers BranchCache et déplacer les serveurs de fichiers dans l'unité d'organisation. Pour plus d’informations, consultez [activer la publication de hachages pour les serveurs de fichiers membres du domaine](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-create-the-branchcache-hash-publication-group-policy-object"></a>Pour créer l’objet de stratégie de groupe publication de hachages BranchCache  
  
1.  Ouvrez Windows PowerShell, tapez **mmc**, puis appuyez sur Entrée. La console MMC s'affiche.  
  
2.  Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**. La boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables** s'ouvre.  
  
3.  Dans **Ajouter ou supprimer des composants logiciels enfichables**, dans **Composants logiciels enfichables disponibles**, double-cliquez sur **Gestion des stratégies de groupe**, puis cliquez sur **OK**.  
  
4.  Dans la console MMC de gestion de la stratégie groupe, développez le chemin d'accès à l'unité d'organisation Serveurs de fichiers BranchCache que vous avez créée précédemment. Par exemple, si votre forêt est nommée example.com, votre domaine est nommé example1.com, et votre unité d’organisation est nommée serveurs de fichiers BranchCache, développez le chemin suivant : **stratégie de groupe Management**, **Forest : example.com**, **Domains**, **example1.com**, **serveurs de fichiers BranchCache**.  
  
5.  Cliquez avec le bouton droit sur **serveurs de fichiers BranchCache**, puis cliquez sur **créer un objet de stratégie de groupe dans ce domaine, et le lier ici**. La boîte de dialogue **Nouvel objet GPO** s'ouvre. Dans **nom**, tapez un nom pour le nouvel objet de stratégie de groupe. Par exemple, si vous souhaitez nommer l'objet Publication de hachages BranchCache, tapez **Publication de hachages BranchCache**. Cliquez sur **OK**.  
  


