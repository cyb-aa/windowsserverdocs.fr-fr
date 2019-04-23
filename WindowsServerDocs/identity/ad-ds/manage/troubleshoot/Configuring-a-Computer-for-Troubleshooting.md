---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: Configuration d’un ordinateur pour la résolution des problèmes
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1acb5f7d309d58ed4a5a3aca6bb89f01c0cbf933
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854120"
---
# <a name="configuring-a-computer-for-troubleshooting"></a>Configuration d’un ordinateur pour la résolution des problèmes

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Avant d’utiliser des techniques de dépannage avancé pour identifier et résoudre les problèmes d’Active Directory, configurer vos ordinateurs pour le dépannage. Vous devez également avoir une connaissance élémentaire de la résolution des problèmes de concepts, procédures et outils.

Pour plus d’informations sur les outils d’analyse pour Windows Server, consultez le Guide pas à pas pour [analyse des performances et fiabilité dans Windows Server](https://go.microsoft.com/fwlink/?LinkId=123737)

## <a name="configuration-tasks-for-troubleshooting"></a>Tâches de configuration pour la résolution des problèmes

Pour configurer votre ordinateur pour le dépannage des Services de domaine Active Directory (AD DS), procédez comme suit :

### <a name="install-remote-server-administration-tools-for-ad-ds"></a>Installer les outils d’Administration de serveur distant pour AD DS

Lorsque vous installez les services AD DS pour créer un contrôleur de domaine, les outils d’administration que vous utilisez pour gérer les services AD DS sont installés automatiquement. Si vous souhaitez gérer à distance des contrôleurs de domaine à partir d’un ordinateur qui n’est pas un contrôleur de domaine, vous pouvez installer les outils d’Administration de serveur distant (RSAT) sur un serveur membre ou une station de travail qui exécute une version prise en charge de Windows. RSAT remplace les outils de Support Windows à partir de Windows Server 2003.

Pour plus d’informations sur l’installation de serveur distant, consultez l’article [outils d’Administration de serveur distant](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).

### <a name="configure-reliability-and-performance-monitor"></a>Configurer le moniteur de fiabilité et performances

Windows Server inclut la fiabilité de Windows et l’Analyseur de performances, qui est un composant logiciel enfichable Microsoft Management Console (MMC) qui combine les fonctionnalités d’outils autonomes précédents, y compris les journaux de performances et alertes, Server Performance Advisor, et le Moniteur système. Ce composant logiciel enfichable fournit une interface utilisateur graphique (GUI) pour personnaliser les ensembles de collecteurs de données et des Sessions de suivi d’événements.

Moniteur de fiabilité et performances inclut également Reliability Monitor, un composant logiciel enfichable MMC qui suit les modifications apportées au système et les compare aux modifications dans la stabilité du système, en fournissant une vue graphique de leur relation.

### <a name="set-logging-levels"></a>Définir des niveaux de journalisation

Si les informations que vous recevez dans le journal du Service d’annuaire dans l’Observateur d’événements ne sont pas suffisantes pour la résolution des problèmes, augmenter les niveaux de journalisation à l’aide de l’entrée de Registre appropriées dans **HKEY_LOCAL_ MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics**.

Par défaut, les niveaux de journalisation pour toutes les entrées sont définies **0**, qui fournit la quantité minimale d’informations. Le niveau de journalisation la plus élevé est **5**. Augmenter le niveau de rechercher une entrée provoque des événements supplémentaires dans le journal des événements Service d’annuaire.

Utilisez la procédure suivante pour modifier le niveau de journalisation pour une entrée de diagnostic. L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

> [!WARNING]
> Nous vous recommandons de ne pas modifier directement le Registre, sauf s’il n’y a pas d’autre solution. Modifications du Registre ne sont pas validées par l’Éditeur du Registre ou par Windows avant qu’ils sont appliqués, et par conséquent, les valeurs incorrectes peuvent être stockées. Cela peut entraîner des erreurs irrécupérables dans le système. Si possible, utilisez la stratégie de groupe ou d’autres outils Windows, tels que les composants logiciel enfichables MMC, pour accomplir des tâches, plutôt que de modifier le Registre directement. Si vous devez modifier le Registre, soyez très vigilant.
>

Pour modifier le niveau de journalisation pour une entrée de diagnostic

1. Cliquez sur **Démarrer** > **exécuter** > type **regedit** > cliquez sur **OK**.
2. Accédez à l’entrée pour laquelle vous souhaitez définir la connexion.
   * EXEMPLE : HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics
3. Double-cliquez sur l’entrée, puis, dans **Base**, cliquez sur **décimal**.
4. Dans **valeur**, tapez un entier compris entre **0** via **5**, puis cliquez sur **OK**.
