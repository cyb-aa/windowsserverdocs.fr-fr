---
title: Activer BranchCache sur un partage de fichiers (facultatif)
description: Cette rubrique fait partie de la BranchCache déploiement Guide pour Windows Server2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé d’optimiser l’utilisation de la bande passante réseau étendu dans les filiales
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-bc
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9c465a9e-c504-44ec-9ebc-4e06ba54db30
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33ed40ef91d9389bb7940dcf928cba43f0c9dbd2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="enable-branchcache-on-a-file-share-optional"></a>Activer BranchCache sur un partage de fichiers (facultatif)

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour activer BranchCache sur un partage de fichiers.  
  
> [!IMPORTANT]  
> Vous n’avez pas besoin effectuer cette procédure si vous configurez le paramètre de publication de hachage avec la valeur **autoriser la publication de hachages pour tous les dossiers partagés**.  
  
L’appartenance au groupe **administrateurs**, ou équivalent est le minimum requis pour effectuer cette procédure.  
  
### <a name="to-enable-branchcache-on-a-file-share"></a>Pour activer BranchCache sur un partage de fichiers  
  
1.  Ouvrez Windows PowerShell, tapez **mmc**, puis appuyez sur ENTRÉE. La Console MMC (MicrosoftManagement Console) s’ouvre.  
  
2.  Dans la console MMC, sur le **fichier** menu, cliquez sur **ajouter/supprimer un composant logiciel enfichable**. Le **ajouter ou supprimer des composants logiciel enfichables** boîte de dialogue s’ouvre.  
  
3.  Dans **ajouter ou supprimer des composants logiciel enfichables**, dans **des composants logiciels enfichables disponibles**, double-cliquez sur **dossiers partagés**. L’Assistant de dossiers partagés s’ouvre avec l’objet ordinateur Local sélectionné. Configurer l’affichage que vous préférez, cliquez sur **Terminer**, puis cliquez sur **OK**.  
  
4.  Double-cliquez sur **dossiers partagés (Local)**, puis cliquez sur **partages**.  
  
5.  Dans le volet d’informations, cliquez sur un partage, puis cliquez sur **propriétés**. Du partage **propriétés** boîte de dialogue s’ouvre.  
  
6.  Dans le **propriétés** boîte de dialogue le **général**, cliquez sur **paramètres hors connexion**. Le **paramètres hors connexion** boîte de dialogue s’ouvre.  
  
7.  Vérifiez que **uniquement les fichiers et les programmes spécifiés par les utilisateurs sont disponibles hors connexion** est sélectionné, puis cliquez sur **activer BranchCache**.  
  
8.  Cliquez sur **OK** deux fois.  
  

