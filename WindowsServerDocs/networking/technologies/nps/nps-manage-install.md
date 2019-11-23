---
title: Installer un serveur NPS (Network Policy Server)
description: Vous pouvez utiliser cette rubrique pour installer le serveur NPS (Network Policy Server) à l’aide de Windows PowerShell ou de l’Assistant Ajout de rôles et de fonctionnalités dans Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 4842a4ab-70bb-4744-bea7-70f2ac892ad1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 25b8586d370865dd3ae4393c2536c348a5d0810f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396191"
---
# <a name="install-network-policy-server"></a>Installer un serveur NPS (Network Policy Server)

Vous pouvez utiliser cette rubrique pour installer le serveur NPS (Network Policy Server) à l’aide de Windows PowerShell ou de l’Assistant Ajout de rôles et de fonctionnalités. NPS est un service de rôle du rôle serveur Services de stratégie et d’accès réseau.

> [!NOTE]
> Par défaut, le serveur NPS écoute le trafic RADIUS sur les ports 1812, 1813, 1645 et 1646 sur toutes les cartes réseau installées. Si le pare-feu Windows avec fonctions avancées de sécurité est activé lors de l’installation du serveur NPS, des exceptions de pare-feu pour ces ports sont automatiquement créées au cours du processus d’installation pour le\) protocole Internet version 6 \(IPv6 et le trafic IPv4. Si vos serveurs d’accès réseau sont configurés pour envoyer le trafic RADIUS sur des ports autres que ces paramètres par défaut, supprimez les exceptions créées dans le pare-feu Windows avec fonctions avancées de sécurité lors de l’installation du serveur NPS, et créez des exceptions pour les ports que vous utilisez pour Trafic RADIUS.

**Informations d’identification d’administration**

Pour effectuer cette procédure, vous devez être membre du groupe **Administrateurs du domaine**.

## <a name="to-install-nps-by-using-windows-powershell"></a>Pour installer NPS à l’aide de Windows PowerShell

Pour effectuer cette procédure à l’aide de Windows PowerShell, exécutez Windows PowerShell en tant qu’administrateur, tapez la commande suivante, puis appuyez sur entrée.

`Install-WindowsFeature NPAS -IncludeManagementTools`

## <a name="to-install-nps-by-using-server-manager"></a>Pour installer NPS à l’aide de Gestionnaire de serveur

1.  Sur le serveur NPS1, dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités s’ouvre.

2.  Dans **Avant de commencer**, cliquez sur **Suivant**.

    > [!NOTE]
    > La page **Avant de commencer** de l’Assistant Ajout de rôles et de fonctionnalités ne s’affiche pas si vous avez préalablement sélectionné **Ignorer cette page par défaut** dans l’Assistant.

3.  Dans **Sélectionner le type d’installation**, vérifiez que **Installation basée sur un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **Suivant**.

4.  Dans **Sélectionner le serveur de destination**, vérifiez que **Sélectionner un serveur du pool de serveurs** est sélectionné. Dans **Pool de serveurs**, vérifiez que l’ordinateur local est sélectionné. Cliquez sur **Suivant**.

5.  Dans **Sélectionner des rôles de serveurs**, dans **rôles**, sélectionnez **services de stratégie et d’accès réseau**. Une boîte de dialogue s’ouvre et vous demande s’il doit ajouter des fonctionnalités requises pour les services de stratégie et d’accès réseau. Cliquez sur **Ajouter des fonctionnalités**, puis sur **suivant** .

6.  Dans **Sélectionner des fonctionnalités**, cliquez sur **Suivant**, puis dans **Services de stratégie et d’accès réseau**, vérifiez les informations fournies et cliquez sur **Suivant**.

7.  Dans **Sélectionner des services de rôle**, cliquez sur **Serveur NPS (Network Policy Server)** .  Dans **Ajouter les fonctionnalités requises pour le serveur NPS (Network Policy Server)** , cliquez sur **Ajouter des fonctionnalités**. Cliquez sur **Suivant**.

8.  Dans **Confirmer les sélections d’installation**, cliquez sur **Redémarrer automatiquement le serveur de destination, si nécessaire**. Lorsque vous êtes invité à confirmer ce choix, cliquez sur **Oui**, puis sur **Installer**. La page de progression de l’installation indique l’état du processus d’installation. Une fois le processus terminé, le message « installation réussie sur *nomordinateur*» s’affiche, où *nomordinateur* est le nom de l’ordinateur sur lequel vous avez installé le serveur de stratégie réseau. Cliquez sur **Fermer**.

Pour plus d’informations, consultez [gérer NPSs](nps-manage-servers.md).