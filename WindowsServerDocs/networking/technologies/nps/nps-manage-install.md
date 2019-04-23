---
title: Installer un serveur NPS (Network Policy Server)
description: Vous pouvez utiliser cette rubrique pour installer le serveur NPS (Network Policy Server) à l’aide de Windows PowerShell ou l’ajout de rôles et l’Assistant de fonctionnalités dans Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4842a4ab-70bb-4744-bea7-70f2ac892ad1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9d11f77ff8b9db8bc10970b325afae60ba937cfa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831190"
---
# <a name="install-network-policy-server"></a>Installer un serveur NPS (Network Policy Server)

Vous pouvez utiliser cette rubrique pour installer le serveur NPS (Network Policy Server) à l’aide de Windows PowerShell ou l’ajout de rôles et fonctionnalités Assistant. NPS est un service de rôle du rôle serveur Services de stratégie et d’accès réseau.

> [!NOTE]
> Par défaut, le serveur NPS écoute le trafic RADIUS sur les ports 1812, 1813, 1645 et 1646 sur toutes les cartes réseau installées. Si le pare-feu Windows avec fonctions avancées de sécurité est activé lorsque vous installez NPS, les exceptions de pare-feu pour ces ports sont automatiquement créées pendant le processus d’installation pour Internet Protocol version 6 \(IPv6\) et le trafic IPv4. Si vos serveurs d’accès réseau sont configurés pour envoyer le trafic RADIUS sur des ports autres que ces valeurs par défaut, supprimez les exceptions créées dans le pare-feu Windows avec fonctions avancées de sécurité pendant l’installation du serveur NPS et créer des exceptions pour les ports que vous n’utilisez pas pour Trafic RADIUS.

**Informations d’identification administratives**

Pour effectuer cette procédure, vous devez être membre du groupe **Administrateurs du domaine**.

## <a name="to-install-nps-by-using-windows-powershell"></a>Pour installer le serveur NPS à l’aide de Windows PowerShell

Pour effectuer cette procédure à l’aide de Windows PowerShell, exécutez Windows PowerShell en tant qu’administrateur, tapez la commande suivante, puis appuyez sur ENTRÉE.

`Install-WindowsFeature NPAS -IncludeManagementTools`

## <a name="to-install-nps-by-using-server-manager"></a>Pour installer le serveur NPS à l’aide du Gestionnaire de serveur

1.  Sur le serveur NPS1, dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités s’ouvre.

2.  Dans **Avant de commencer**, cliquez sur **Suivant**.

    > [!NOTE]
    > La page **Avant de commencer** de l’Assistant Ajout de rôles et de fonctionnalités ne s’affiche pas si vous avez préalablement sélectionné **Ignorer cette page par défaut** dans l’Assistant.

3.  Dans **Sélectionner le type d’installation**, vérifiez que **Installation basée sur un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **Suivant**.

4.  Dans **Sélectionner le serveur de destination**, vérifiez que **Sélectionner un serveur du pool de serveurs** est sélectionné. Dans **Pool de serveurs**, vérifiez que l’ordinateur local est sélectionné. Cliquez sur **Suivant**.

5.  Dans **sélectionner des rôles de serveur**, dans **rôles**, sélectionnez **stratégie réseau et Services d’accès**. Une boîte de dialogue s’ouvre, vous demandant si elle doit ajouter des fonctionnalités qui sont nécessaires pour la stratégie de réseau et les Services d’accès. Cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant**

6.  Dans **Sélectionner des fonctionnalités**, cliquez sur **Suivant**, puis dans **Services de stratégie et d’accès réseau**, vérifiez les informations fournies et cliquez sur **Suivant**.

7.  Dans **Sélectionner des services de rôle**, cliquez sur **Serveur NPS (Network Policy Server)**.  Dans **Ajouter les fonctionnalités requises pour le serveur NPS (Network Policy Server)**, cliquez sur **Ajouter des fonctionnalités**. Cliquez sur **Suivant**.

8.  Dans **Confirmer les sélections d’installation**, cliquez sur **Redémarrer automatiquement le serveur de destination, si nécessaire**. Lorsque vous êtes invité à confirmer ce choix, cliquez sur **Oui**, puis sur **Installer**. La page de progression de l’installation indique l’état du processus d’installation. La fin du processus, le message « Installation réussie sur *ComputerName*» s’affiche, où *ComputerName* est le nom de l’ordinateur sur lequel vous avez installé le serveur de stratégie réseau. Cliquez sur **Fermer**.

Pour plus d’informations, consultez [NPSs gérer](nps-manage-servers.md).