---
title: 'Étape 1 : installer le rôle serveur WSUS'
description: Rubrique de Windows Server Update Service (WSUS) - explique comment installer le rôle de serveur à l’aide du Gestionnaire de serveur
ms.prod: windows-server-threshold
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: fabc8619-350e-403b-96f8-116424931300
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4027f7656c17b2ba46fdf44e3af4de42bb056a6f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814000"
---
# <a name="step-1-install-the-wsus-server-role"></a>Étape 1 : Installer le rôle serveur WSUS

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

L’étape suivante du déploiement de votre serveur WSUS consiste à installer le rôle serveur WSUS. La procédure suivante explique comment installer le rôle serveur WSUS à l’aide du Gestionnaire de serveur.

> [!IMPORTANT]
> Cette procédure d’installation couvre uniquement l’installation de WSUS à l’aide de la base de données interne Windows. Les procédures permettant d’installer WSUS à l’aide de Microsoft SQL Server sont documentées dans [cet article](https://social.technet.microsoft.com/wiki/contents/articles/10020.installing-wsus-server-role-on-windows-server-2012-with-microsoft-sql-database.aspx).

### <a name="to-install-the-wsus-server-role"></a>Pour installer le rôle serveur WSUS

1.  Ouvrez une session sur le serveur sur lequel vous envisagez d’installer le rôle serveur WSUS en utilisant un compte qui est membre du groupe Administrateurs local.

2.  Dans **le Gestionnaire de serveur**, cliquez sur **gérer**, puis cliquez sur **ajouter des rôles et fonctionnalités**.

3.  Dans la page **Avant de commencer** , cliquez sur **Suivant**.

4.  Dans le **sélectionner type d’installation** page, vérifiez que **installation en fonction du rôle ou une fonctionnalité** option est sélectionnée et cliquez sur **suivant**.

5.  Sur le **server de sélectionner la destination** page, choisissez l’emplacement du serveur (à partir d’un pool de serveurs ou à partir d’un disque dur virtuel). Après avoir sélectionné l’emplacement, choisissez le serveur sur lequel vous voulez installer le rôle de serveur WSUS, puis cliquez sur **Suivant**.

6.  Sur le **sélectionner des rôles de serveur** page, sélectionnez **Windows Server Update Services**.  **Ajouter les fonctionnalités requises pour Windows Server Update Services** s’ouvre. Cliquez sur **Ajouter les fonctionnalités**, puis sur **Suivant**.

7.  Sur le **sélectionner des fonctionnalités**page. Conservez les sélections par défaut, puis cliquez sur **suivant**.

    > [!IMPORTANT]
    > WSUS requiert uniquement la configuration du rôle Serveur Web par défaut. Si vous êtes invité à indiquer une autre configuration du rôle Serveur Web lors de la configuration de WSUS, vous pouvez accepter en toute sécurité les valeurs par défaut et continuer la configuration de WSUS.

8.  Dans la page **Services WSUS (Windows Server Update Services)**, cliquez sur **Suivant**.

9. Dans la page **Sélectionner des services de rôle** , laissez les sélections par défaut et cliquez sur **Suivant**.

    > [!TIP]
    > Vous devez sélectionner un type de base de données. Si les options de base de données sont toutes décochées (non sélectionnées), les tâches postérieures à l’installation échouent.

10. Dans la page **Sélection de l’emplacement du contenu** , tapez un emplacement valide pour stocker les mises à jour. Par exemple, vous pouvez créer un dossier nommé WSUS_database à la racine du lecteur K spécifiquement à cette fin, et tapez **k:\WSUS_database** comme emplacement valide.

11. Cliquez sur **Suivant**. La page **Rôle de serveur web (IIS)** s’affiche. Vérifiez les informations, puis cliquez sur **Suivant**. Dans **sélectionner les services de rôle à installer pour le serveur Web (IIS)**, conserver les valeurs par défaut, puis cliquez sur **suivant**.

12. Dans la page **Confirmer les sélections d’installation**, passez en revue les options sélectionnées, puis cliquez sur **Installer**. L’Assistant d’installation de WSUS s’exécute. Le processus peut durer plusieurs minutes.

13. Une fois l’installation de WSUS terminée, dans la fenêtre de résumé de la page **Progression de l’installation** , cliquez sur **Lancer les tâches de post-installation**. Le message suivant s’affiche : **Veuillez patienter pendant la configuration de votre serveur**. Une fois la tâche terminée, le message suivant s’affiche : **Configuration terminée**. Cliquez sur **Fermer**.

14. Dans **Gestionnaire de serveur**, vérifiez si une notification s’affiche pour vous informer qu’un redémarrage est requis. Cela peut varier en fonction du rôle serveur installé. Si un redémarrage est requis, veillez à redémarrer le serveur pour terminer l’installation.

> [!IMPORTANT]
> À ce stade du processus d’installation est terminé, toutefois pour WSUS puisse fonctionner vous devez passer à [étape 2 : Configurer WSUS](2-configure-wsus.md).

