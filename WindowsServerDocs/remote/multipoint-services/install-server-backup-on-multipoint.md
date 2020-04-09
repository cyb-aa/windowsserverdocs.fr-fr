---
title: Installer la sauvegarde du serveur sur votre serveur MultiPoint
description: Vous guide tout au long des étapes d’installation des outils de sauvegarde et de récupération
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: e4331370-ba07-4529-92ab-db14a41bfc3b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: f8fe5ac8b57105d421af431b12c8dc17250b622d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820332"
---
# <a name="install-server-backup-on-your-multipoint-server"></a>Installer la sauvegarde du serveur sur votre serveur MultiPoint
Il est recommandé de prendre en compte un plan de sauvegarde et de récupération pour vos serveurs MultiPoint.
  
Un bon plan de sauvegarde et de récupération est important pour tous les environnements de taille. Sauvegarde Windows Server est une fonctionnalité de Windows Server 2016 qui fournit un ensemble d’assistants et d’autres outils qui vous permettent d’effectuer des tâches de sauvegarde et de récupération de base pour le serveur sur lequel il est installé. Vous pouvez utiliser Sauvegarde Windows Server pour sauvegarder un serveur complet (tous les volumes), des volumes sélectionnés, l’état du système ou des fichiers ou dossiers spécifiques, et pour créer une sauvegarde que vous pouvez utiliser pour reconstruire votre système.  
  
Vous pouvez récupérer des volumes, des dossiers, des fichiers, certaines applications, ainsi que l’état du système. Et, pour les incidents comme les défaillances de disque dur, vous pouvez régénérer un système à partir de zéro ou en utilisant un autre matériel. Pour ce faire, vous devez disposer d’une sauvegarde du serveur complet ou uniquement des volumes qui contiennent des fichiers de système d’exploitation et de l’environnement de récupération Windows. Cela restaure votre système complet sur votre ancien système ou sur un nouveau disque dur.  
  
Une fonctionnalité clé de Sauvegarde Windows Server est la possibilité de planifier l’exécution automatique des sauvegardes.  
  
Utilisez les procédures suivantes pour configurer le type de sauvegarde dont vous avez besoin.  
  
## <a name="install-backup-and-recovery-tools"></a>Installer les outils de sauvegarde et de récupération  
  
1.  À partir de l’écran d' **Accueil** , ouvrez **Gestionnaire de serveur**.  
  
2.  Cliquez sur **Ajouter des rôles et des fonctionnalités** pour démarrer l’Assistant Ajout de rôles. Ensuite, cliquez sur **suivant** après avoir consulté les notes **avant de commencer** .  
  
3.  Sélectionnez l’option **installation basée sur un rôle ou une fonctionnalité** , puis cliquez sur **suivant**.  
  
4.  Sélectionnez l’ordinateur local que vous gérez, puis cliquez sur **suivant**.  
  
    L'Assistant Ajout de fonctionnalités s'ouvre.  
  
5.  Dans la page **Sélectionner des fonctionnalités** , développez fonctionnalités de sauvegarde Windows Server, activez les cases à cocher **sauvegarde Windows Server** et **outils en ligne de commande**, puis cliquez sur **suivant**.  
  
    > [!NOTE]  
    > Ou, si vous souhaitez simplement installer le composant logiciel enfichable et l’outil en ligne de commande Wbadmin, développez **fonctionnalités sauvegarde Windows Server**, puis activez la case à cocher **sauvegarde Windows Server** uniquement — Assurez-vous que la case à cocher **outils en ligne de commande** est désactivée.  
  
6.  Sur la page **confirmer les sélections d’installation** , passez en revue vos choix, puis cliquez sur **installer**.  
  
    Si des erreurs se produisent pendant l’installation, la page résultats de l' **installation** indiquera les erreurs.  
  
7.  Une fois l’installation terminée, vous devez être en mesure d’accéder à ces outils de sauvegarde et de récupération :  
  
    -   Pour ouvrir le composant logiciel enfichable Sauvegarde Windows Server, dans l’écran d' **Accueil** , tapez **sauvegarde**, puis cliquez sur **sauvegarde Windows Server** dans les résultats.  
  
    -   Pour démarrer l’outil Wbadmin et afficher la syntaxe pour ses commandes : dans l’écran d' **Accueil** , tapez **Command**. Dans les résultats, cliquez avec le bouton droit sur **invite de commandes**, cliquez sur **exécuter en tant qu’administrateur** au bas de la page, puis cliquez sur **Oui** à l’invite de confirmation. À l’invite de commandes, tapez **Wbadmin/ ?** et appuyez sur Entrée. Vous devez voir la syntaxe de commande et les descriptions de l’outil.  
  
## <a name="configure-backups-using-windows-server-backup"></a>Configurer des sauvegardes à l’aide de Sauvegarde Windows Server  
  
-   Suivez les instructions de [sauvegarde de votre serveur](https://technet.microsoft.com/library/cc753528.aspx). 