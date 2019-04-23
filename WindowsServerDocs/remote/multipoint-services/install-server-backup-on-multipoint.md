---
title: Installer le serveur de sauvegarde sur votre serveur MultiPoint
description: Vous guide à travers les étapes pour installer les outils de sauvegarde et de récupération
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4331370-ba07-4529-92ab-db14a41bfc3b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 51932c5f0796cfd757d3322e10c17de2a3081f4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832490"
---
# <a name="install-server-backup-on-your-multipoint-server"></a>Installer le serveur de sauvegarde sur votre serveur MultiPoint
Il est recommandé de prendre en compte un plan de sauvegarde et récupération pour vos serveurs MultiPoint.
  
Un bon plan de sauvegarde et de récupération est important pour n’importe quel environnement de taille. Sauvegarde Windows Server est une fonctionnalité de Windows Server 2016 qui fournit un ensemble d’Assistants et autres outils qui vous permettent d’effectuer des tâches de sauvegarde et de récupération de base pour le serveur sur lequel il est installé. Vous pouvez utiliser sauvegarde Windows Server pour sauvegarder un serveur complet (tous les volumes), volumes sélectionnés, l’état du système, ou de fichiers ou dossiers spécifiques et pour créer une sauvegarde que vous pouvez utiliser pour reconstruire votre système.  
  
Vous pouvez récupérer des volumes, des dossiers, des fichiers, certaines applications, ainsi que l’état du système. Et, pour les incidents tels que les défaillances de disque dur, vous pouvez reconstruire un système à partir de zéro ou à l’aide de matériel de remplacement. Pour ce faire, vous devez disposer d’une sauvegarde de serveur entier ou simplement des volumes qui contiennent les fichiers de système d’exploitation et de l’environnement de récupération Windows. Cette opération restaure votre système complète sur votre ancien système ou sur un nouveau disque dur.  
  
Une fonctionnalité clé de sauvegarde de Windows Server est la possibilité de planifier des sauvegardes pour s’exécuter automatiquement.  
  
Utilisez les procédures suivantes pour configurer le type de sauvegarde que vous avez besoin.  
  
## <a name="install-backup-and-recovery-tools"></a>Installer les outils de sauvegarde et récupération  
  
1.  À partir de la **Démarrer** écran, ouvrez **le Gestionnaire de serveur**.  
  
2.  Cliquez sur **Ajout de rôles et fonctionnalités** pour démarrer l’Assistant Ajout de rôles. Puis cliquez sur **suivant** après avoir examiné les **avant de commencer** notes.  
  
3.  Sélectionnez le **en fonction du rôle ou fonctionnalité une installation basée sur** option, puis cliquez sur **suivant**.  
  
4.  Sélectionnez l’ordinateur local que vous gérez, puis cliquez sur **suivant**.  
  
    L'Assistant Ajout de fonctionnalités s'ouvre.  
  
5.  Sur le **sélectionner des fonctionnalités** , développez les fonctionnalités de la sauvegarde Windows Server, sélectionnez les cases à cocher **sauvegarde Windows Server** et **les outils de ligne de commande**, puis cliquez sur  **Suivant**.  
  
    > [!NOTE]  
    > Ou, si vous souhaitez simplement installer le composant logiciel enfichable et l’outil de ligne de commande Wbadmin, développez **fonctionnalités de la sauvegarde Windows Server**, puis sélectionnez le **sauvegarde Windows Server** case à cocher uniquement, assurez-vous que le **Les outils de ligne de commande** à cocher est désactivée.  
  
6.  Sur le **confirmer les sélections d’Installation** page, passez en revue vos choix, puis cliquez sur **installer**.  
  
    Si des erreurs se produisent pendant l’installation, le **résultats de l’Installation** page Notez les erreurs.  
  
7.  Après que l’installation se termine correctement, il se peut que vous devez être en mesure d’accéder à ces outils de sauvegarde et de récupération :  
  
    -   Pour ouvrir la sauvegarde Windows Server-composant logiciel enfichable, sur le **Démarrer** , tapez **sauvegarde**, puis cliquez sur **sauvegarde Windows Server** dans les résultats.  
  
    -   Pour démarrer l’outil Wbadmin et afficher la syntaxe pour ses commandes : Sur le **Démarrer** , tapez **commande**. Dans les résultats, cliquez sur **invite de commandes**, cliquez sur **exécuter en tant qu’administrateur** en bas de la page, puis cliquez sur **Oui** à l’invite de confirmation. À l’invite de commandes, tapez **wbadmin / ?** et appuyez sur ENTRÉE. Vous devez voir la syntaxe de commande et les descriptions de l’outil.  
  
## <a name="configure-backups-using-windows-server-backup"></a>Configurer des sauvegardes à l’aide de la sauvegarde de Windows Server  
  
-   Suivez les instructions de [sauvegarde de votre serveur](https://technet.microsoft.com/library/cc753528.aspx). 