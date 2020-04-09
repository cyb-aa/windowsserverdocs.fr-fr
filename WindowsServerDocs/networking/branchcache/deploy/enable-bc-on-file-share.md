---
title: Activer BranchCache sur un partage de fichiers (facultatif)
description: Cette rubrique fait partie du Guide de déploiement BranchCache pour Windows Server 2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les filiales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 9c465a9e-c504-44ec-9ebc-4e06ba54db30
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 501fd1b1ac3b0bc7a0767ec57502ba18a8ee76e4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861082"
---
# <a name="enable-branchcache-on-a-file-share-optional"></a>Activer BranchCache sur un partage de fichiers (facultatif)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour activer BranchCache sur un partage de fichiers.  
  
> [!IMPORTANT]  
> Vous n’avez pas besoin d’effectuer cette procédure si vous configurez le paramètre de publication de hachage avec la valeur **autoriser la publication de hachages pour tous les dossiers partagés**.  
  
Pour exécuter cette procédure, il est nécessaire d'appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-enable-branchcache-on-a-file-share"></a>Pour activer BranchCache sur un partage de fichiers  
  
1.  Ouvrez Windows PowerShell, tapez **mmc**, puis appuyez sur Entrée. La console MMC s'affiche.  
  
2.  Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**. La boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables** s'ouvre.  
  
3.  Dans **Ajouter ou supprimer des composants logiciels enfichables**, dans **composants logiciels enfichables disponibles**, double-cliquez sur **dossiers partagés**. L’Assistant dossiers partagés s’ouvre avec l’objet ordinateur local sélectionné. Configurez l’affichage de votre choix, cliquez sur **Terminer**, puis sur **OK**.  
  
4.  Double-cliquez sur **dossiers partagés (local)** , puis cliquez sur **partages**.  
  
5.  Dans le volet d’informations, cliquez avec le bouton droit sur un partage, puis cliquez sur **Propriétés**. La boîte de dialogue **Propriétés** du partage s’ouvre.  
  
6.  Dans la boîte de dialogue **Propriétés** , sous l’onglet **général** , cliquez sur **paramètres hors connexion**. La boîte de dialogue **paramètres hors connexion** s’ouvre.  
  
7.  Assurez-vous que **seuls les fichiers et les programmes spécifiés par les utilisateurs sont disponibles hors connexion** est sélectionné, puis cliquez sur **activer BranchCache**.  
  
8.  Cliquez deux fois sur **OK**.  
  

