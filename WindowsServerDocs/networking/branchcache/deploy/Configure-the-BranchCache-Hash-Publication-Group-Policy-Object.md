---
title: Configurer l’objet de stratégie de groupe de la publication de hachages BranchCache
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server 2016, qui montre comment déployer BranchCache en mode cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les succursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da74fea7-52b2-4d6d-9d21-19184eedbe3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6e9a338dfebfb1dfadb258bcbdfcc8d75bd3ea86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862960"
---
# <a name="configure-the-branchcache-hash-publication-group-policy-object"></a>Configurer l’objet de stratégie de groupe de la publication de hachages BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour configurer la publication de hachages BranchCache objet de stratégie de groupe (GPO) afin que tous les serveurs de fichiers que vous avez ajouté à votre unité d’organisation ont le même paramètre de stratégie de publication hachage appliqué.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.  
  
> [!NOTE]  
> Avant d’effectuer cette procédure, vous devez créer l’unité de d’organisation serveurs de fichiers BranchCache, déplacez les serveurs de fichiers dans l’unité d’organisation et créer la GPO de la publication de hachages BranchCache. Pour plus d’informations, consultez [activer la Publication de hachages pour les serveurs de fichiers de membre de domaine](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-configure-the-branchcache-hash-publication-group-policy-object"></a>Pour configurer l’objet stratégie de groupe de la publication de hachages BranchCache  
  
1.  Exécuter Windows PowerShell en tant qu’administrateur, type **mmc**, puis appuyez sur ENTRÉE. La console MMC s'affiche.  
  
2.  Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**. La boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables** s'ouvre.  
  
3.  Dans **Ajouter ou supprimer des composants logiciels enfichables**, dans **Composants logiciels enfichables disponibles**, double-cliquez sur **Gestion des stratégies de groupe**, puis cliquez sur **OK**.  
  
4.  Dans la console MMC de gestion de la stratégie groupe, développez le chemin d'accès à l'objet de stratégie de groupe Publication de hachages BranchCache que vous avez créé précédemment. Par exemple, si votre forêt s'appelle example.com, votre domaine s'appelle example1.com, et votre objet de stratégie de groupe s'appelle **Publication de hachages BranchCache**, développez le chemin suivant : **Gestion de stratégie de groupe**, **forêt : example.com**, **domaines**, **exemple1.com**, **les objets de stratégie de groupe**,  **Publication de hachages de BranchCache**.  
  
5.  Cliquez avec le bouton droit sur l'objet de stratégie de groupe **Publication de hachages BranchCache**, puis cliquez sur **Modifier**. La console de l’éditeur de gestion de stratégie de groupe s’ouvre.  
  
6.  Dans la console Éditeur de gestion de stratégie de groupe, développez le chemin suivant : **Configuration ordinateur**, **Stratégies**, **Modèles d'administration**, **Réseau**, **Serveur Lanman**.  
  
7.  Dans la console de l'Éditeur de gestion des stratégies de groupe, cliquez sur **Serveur Lanman**. Dans le volet d'informations, double-cliquez sur **Publication de hachages pour BranchCache**. La boîte de dialogue **Publication de hachages pour BranchCache** s'ouvre.  
  
8.  Dans la boîte de dialogue **Publication de hachages pour BranchCache**, cliquez sur **Activé**.  
  
9. Dans **Options**, cliquez sur **autoriser la publication de hachages pour tous les dossiers partagés**, puis cliquez sur une des opérations suivantes :  
  
    1.  Pour activer la publication de hachages pour tous les dossiers partagés pour tous les fichiers des serveurs que vous avez ajouté à l’unité d’organisation, cliquez sur **autoriser la publication de hachages pour tous les dossiers partagés**.  
  
    2.  Pour activer la publication de hachages uniquement pour les dossiers partagés pour lesquels BranchCache est activé, cliquez sur **Autoriser la publication de hachages uniquement pour les dossiers partagés dans lesquels BranchCache est activé**.  
  
    3.  Pour désactiver la publication de hachages pour tous les dossiers partagés sur l'ordinateur même si BranchCache est activé sur les partages de fichiers, cliquez sur **N'autoriser la publication de hachages pour aucun dossier partagé**.  
  
10. Cliquez sur **OK**.  
  
> [!NOTE]  
> Dans la plupart des cas, vous devez enregistrer la console MMC et actualiser l'affichage afin de visualiser les modifications de configuration apportées.  
  


