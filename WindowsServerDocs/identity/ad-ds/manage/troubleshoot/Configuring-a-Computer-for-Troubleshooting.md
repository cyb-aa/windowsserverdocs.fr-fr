---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: Configuration d’un ordinateur pour la résolution des problèmes
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 8e11883de9f89d0b95ed0fc35b4f5f3941ef82a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71368896"
---
# <a name="configuring-a-computer-for-troubleshooting"></a>Configuration d’un ordinateur pour la résolution des problèmes

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Avant d’utiliser des techniques de dépannage avancées pour identifier et résoudre les problèmes de Active Directory, configurez vos ordinateurs pour le dépannage. Vous devez également avoir une compréhension de base des concepts, des procédures et des outils de dépannage.

Pour plus d’informations sur les outils de surveillance de Windows Server, consultez le guide pas à pas pour la [surveillance des performances et de la fiabilité dans Windows Server](https://go.microsoft.com/fwlink/?LinkId=123737) .

## <a name="configuration-tasks-for-troubleshooting"></a>Tâches de configuration pour le dépannage

Pour configurer votre ordinateur pour la résolution des problèmes Active Directory Domain Services (AD DS), effectuez les tâches suivantes :

### <a name="install-remote-server-administration-tools-for-ad-ds"></a>Installer Outils d’administration de serveur distant pour AD DS

Lorsque vous installez AD DS pour créer un contrôleur de domaine, les outils d’administration que vous utilisez pour gérer les AD DS sont installés automatiquement. Si vous souhaitez gérer des contrôleurs de domaine à distance à partir d’un ordinateur qui n’est pas un contrôleur de domaine, vous pouvez installer les Outils d’administration de serveur distant (RSAT) sur un serveur membre ou une station de travail qui exécute une version prise en charge de Windows. Les outils d’aide à la décision remplacent les outils de support de Windows Server 2003.

Pour plus d’informations sur l’installation de RSAT, consultez l’article [Outils d’administration de serveur distant](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).

### <a name="configure-reliability-and-performance-monitor"></a>Configurer l’analyseur de fiabilité et de performances

Windows Server inclut le moniteur de fiabilité et de performances Windows, qui est un composant logiciel enfichable MMC (Microsoft Management Console) qui combine les fonctionnalités des outils autonomes précédents, y compris Journaux et alertes de performance, Server Performance Advisor, et le moniteur système. Ce composant logiciel enfichable fournit une interface graphique utilisateur pour la personnalisation des ensembles de collecteurs de données et des sessions de suivi d’événements.

Le moniteur de fiabilité et de performances comprend également le moniteur de fiabilité, un composant logiciel enfichable MMC qui effectue le suivi des modifications apportées au système et les compare aux modifications de la stabilité du système, en fournissant une vue graphique de leur relation.

### <a name="set-logging-levels"></a>Définir les niveaux de journalisation

Si les informations que vous recevez dans le journal du service d’annuaire observateur d’événements ne sont pas suffisantes pour la résolution des problèmes, augmentez les niveaux de journalisation à l’aide de l’entrée de registre appropriée dans **HKEY_LOCAL_ MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics**.

Par défaut, les niveaux de journalisation pour toutes les entrées ont la valeur **0**, ce qui fournit la quantité minimale d’informations. Le niveau de journalisation le plus élevé est **5**. Le fait d’accroître le niveau d’une entrée entraîne la journalisation des événements supplémentaires dans le journal des événements du service d’annuaire.

Utilisez la procédure suivante pour modifier le niveau de journalisation d’une entrée de diagnostic. L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

> [!WARNING]
> Nous vous recommandons de ne pas modifier directement le Registre, sauf s’il n’y a pas d’autre solution. Les modifications apportées au registre ne sont pas validées par l’éditeur du registre ou par Windows avant d’être appliquées. par conséquent, des valeurs incorrectes peuvent être stockées. Cela peut entraîner des erreurs irrécupérables dans le système. Dans la mesure du possible, utilisez stratégie de groupe ou d’autres outils Windows, tels que les composants logiciels enfichables MMC, pour accomplir des tâches, plutôt que de modifier directement le registre. Si vous devez modifier le Registre, soyez très vigilant.
>

Pour modifier le niveau de journalisation d’une entrée de diagnostic

1. Cliquez sur **démarrer** > **exécuter** > tapez **regedit** > cliquez sur **OK**.
2. Accédez à l’entrée pour laquelle vous souhaitez définir la connexion.
   * EXEMPLE : HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics
3. Double-cliquez sur l’entrée, puis, dans **base**, cliquez sur **décimal**.
4. Dans **valeur**, tapez un entier compris entre **0** et **5**, puis cliquez sur **OK**.
