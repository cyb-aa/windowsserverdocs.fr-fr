---
title: Activer la publication de hachages pour les serveurs de fichiers n’appartenant pas au domaine
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server 2016, qui montre comment déployer BranchCache en mode cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les succursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 11584b73-f9e2-4530-afa5-b8df970e6b24
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 00be97abbd583e4c5e762775ea563ba0720d5142
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814950"
---
# <a name="enable-hash-publication-for-non-domain-member-file-servers"></a>Activer la publication de hachages pour les serveurs de fichiers n’appartenant pas au domaine

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour configurer la publication de hachages pour BranchCache à l’aide de la stratégie de groupe ordinateur local sur un serveur de fichiers exécutant Windows Server 2016 avec le **BranchCache pour fichiers réseau** service de rôle des Services de fichiers rôle de serveur installé.  
  
Cette procédure est conçue pour une utilisation sur un serveur de fichiers membre n’appartenant pas au domaine. Si vous exécutez cette procédure sur un serveur de fichiers membre appartenant au domaine et que vous configurez également BranchCache à l'aide de la stratégie de groupe du domaine, les paramètres de stratégie de groupe du domaine remplacent les paramètres de la stratégie de groupe locale.  
  
Pour exécuter cette procédure, il est nécessaire d'appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
> [!NOTE]  
> Si vous avez un ou plusieurs serveurs de fichiers membres du domaine, vous pouvez les ajouter à une unité d'organisation dans les services ADDS (Active Directory Domain Services), puis utiliser la stratégie de groupe pour configurer la publication de hachages pour tous les serveurs de fichiers en même temps, plutôt que de configurer chaque serveur de fichiers individuellement. Pour plus d’informations, consultez [activer la Publication de hachages pour les serveurs de fichiers de membre de domaine](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-enable-hash-publication-for-one-file-server"></a>Pour activer la publication de hachages pour un serveur de fichiers  
  
1.  Ouvrez Windows PowerShell, saisissez **mmc**, puis appuyez sur ENTRÉE. La console MMC s'affiche.  
  
2.  Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**. La boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables** s'ouvre.  
  
3.  Dans **Ajouter ou supprimer des composants logiciels enfichables**, dans **Composants logiciels enfichables disponibles**, double-cliquez sur **Éditeur d'objets de stratégie de groupe**. L'Assistant Stratégie de groupe s'ouvre avec l'objet Ordinateur local sélectionné. Cliquez sur **Terminer**, puis sur **OK**.  
  
4.  Dans la console MMC de l'Éditeur d'objets de stratégie de groupe, développez le chemin suivant : **Stratégie de l’ordinateur local**, **Configuration ordinateur**, **modèles d’administration**, **réseau**, **serveur Lanman**. Cliquez sur **Serveur Lanman**.  
  
5.  Dans le volet d'informations, double-cliquez sur **Publication de hachages pour BranchCache**. La boîte de dialogue **Publication de hachages pour BranchCache** s'ouvre.  
  
6.  Dans la boîte de dialogue **Publication de hachages pour BranchCache**, cliquez sur **Activé**.  
  
7.  Dans **Options**, cliquez sur **autoriser la publication de hachages pour tous les dossiers partagés**, puis cliquez sur une des opérations suivantes :  
  
    1.  Pour activer la publication de hachages pour tous les dossiers partagés sur cet ordinateur, cliquez sur **autoriser la publication de hachages pour tous les dossiers partagés**.  
  
    2.  Pour activer la publication de hachages uniquement pour les dossiers partagés pour lesquels BranchCache est activé, cliquez sur **Autoriser la publication de hachages uniquement pour les dossiers partagés dans lesquels BranchCache est activé**.  
  
    3.  Pour désactiver la publication de hachages pour tous les dossiers partagés sur l'ordinateur même si BranchCache est activé sur les partages de fichiers, cliquez sur **N'autoriser la publication de hachages pour aucun dossier partagé**.  
  
8.  Cliquez sur **OK**.  
  


