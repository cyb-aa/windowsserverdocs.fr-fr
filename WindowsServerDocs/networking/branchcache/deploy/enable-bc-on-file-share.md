---
title: Activer BranchCache sur un partage de fichiers (facultatif)
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server 2016, qui montre comment déployer BranchCache en mode cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les succursales
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
ms.openlocfilehash: 36d8379378529a94874c82e0aa90a6440f0281b2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822230"
---
# <a name="enable-branchcache-on-a-file-share-optional"></a>Activer BranchCache sur un partage de fichiers (facultatif)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour activer BranchCache sur un partage de fichiers.  
  
> [!IMPORTANT]  
> Vous n’avez pas besoin effectuer cette procédure si vous configurez le paramètre de publication de hachage avec la valeur **autoriser la publication de hachages pour tous les dossiers partagés**.  
  
Pour exécuter cette procédure, il est nécessaire d'appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-enable-branchcache-on-a-file-share"></a>Pour activer BranchCache sur un partage de fichiers  
  
1.  Ouvrez Windows PowerShell, saisissez **mmc**, puis appuyez sur ENTRÉE. La console MMC s'affiche.  
  
2.  Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**. La boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables** s'ouvre.  
  
3.  Dans **ajouter ou supprimer des composants logiciel enfichables**, dans **des composants logiciels enfichables disponibles**, double-cliquez sur **dossiers partagés**. L’Assistant de dossiers partagés s’ouvre avec l’objet de l’ordinateur Local sélectionné. Configurer l’affichage de votre choix, cliquez sur **Terminer**, puis cliquez sur **OK**.  
  
4.  Double-cliquez sur **dossiers partagés (Local)**, puis cliquez sur **partages**.  
  
5.  Dans le volet de détails, cliquez sur un partage, puis cliquez sur **propriétés**. Le partage **propriétés** boîte de dialogue s’ouvre.  
  
6.  Dans le **propriétés** boîte de dialogue le **général** , cliquez sur **paramètres hors connexion**. Le **paramètres hors connexion** boîte de dialogue s’ouvre.  
  
7.  Vérifiez que **uniquement les fichiers et les programmes spécifiés par les utilisateurs sont disponibles hors connexion** est sélectionnée, puis cliquez sur **BranchCache activer**.  
  
8.  Cliquez deux fois sur **OK**.  
  

