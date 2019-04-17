---
title: Installer le serveur de stratégie réseau
description: Vous pouvez utiliser cette rubrique pour installer le serveur NPS (Network Policy) à l’aide de Windows PowerShell ou l’ajout de rôles et fonctionnalités Assistant dans Windows Server2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4842a4ab-70bb-4744-bea7-70f2ac892ad1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0b76654fa01b68b5a8c9ea1efc80dfbc47e6a62f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="install-network-policy-server"></a>Installer le serveur de stratégie réseau

Vous pouvez utiliser cette rubrique pour installer le serveur NPS (Network Policy) à l’aide de Windows PowerShell ou l’ajout de rôles et fonctionnalités Assistant. NPS est un service de rôle du rôle de serveur de stratégie réseau et Services d’accès.

> [!NOTE]
> Par défaut, le serveur NPS écoute le trafic RADIUS sur les ports 1812, 1813, 1645 et 1646 sur toutes les cartes réseau installées. Si le pare-feu Windows avec fonctions avancées de sécurité est activé lorsque vous installez le serveur NPS, les exceptions de pare-feu pour ces ports sont créées automatiquement pendant le processus d’installation pour Internet Protocol version6 \(IPv6\) et le trafic IPv4. Si vos serveurs d’accès réseau sont configurés pour envoyer le trafic RADIUS sur des ports autres que ces paramètres par défaut, supprimez les exceptions créées dans le pare-feu Windows avec fonctions avancées de sécurité lors de l’installation du serveur NPS et créer des exceptions pour les ports que vous n’utilisez pas pour le trafic RADIUS.

**Informations d’identification d’administration**

Pour effectuer cette procédure, vous devez être un membre de la **Admins du domaine** groupe.

## <a name="to-install-nps-by-using-windows-powershell"></a>Pour installer le serveur NPS à l’aide de Windows PowerShell

Pour effectuer cette procédure à l’aide de Windows PowerShell, exécutez Windows PowerShell en tant qu’administrateur, tapez la commande suivante et appuyez sur ENTRÉE.

`Install-WindowsFeature NPAS -IncludeManagementTools`

## <a name="to-install-nps-by-using-server-manager"></a>Pour installer le serveur NPS à l’aide du Gestionnaire de serveur

1.  Sur le serveur NPS1, dans le Gestionnaire de serveur, cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités**. L’Assistant Ajouter des rôles et fonctionnalités s’ouvre.

2.  Dans **avant de commencer**, cliquez sur **suivant**.

    > [!NOTE]
    > Le **avant de commencer** page de l’ajout de rôles et fonctionnalités Assistant ne s’affiche pas si vous avez sélectionné précédemment **ignorer cette page par défaut** lorsque l’ajout de rôles et fonctionnalités Assistant a été exécuté.

3.  Dans **sélectionner le Type d’Installation**, assurez-vous que **installation basée sur un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **suivant**.

4.  Dans **serveur de destination sélectionnez**, assurez-vous que **sélectionner un serveur du pool de serveurs** est sélectionné. Dans **Pool de serveurs**, vérifiez que l’ordinateur local est sélectionné. Cliquez sur **suivant**.

5.  Dans **sélectionner des rôles de serveur**, dans **rôles**, sélectionnez **stratégie réseau et Services d’accès**. Une boîte de dialogue s’ouvre et vous demande si elle doit ajouter des fonctionnalités qui sont nécessaires pour la stratégie réseau et Services d’accès. Cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant**

6.  Dans **sélectionner des fonctionnalités**, cliquez sur **suivant**et dans **stratégie réseau et Services d’accès**, passez en revue les informations qui sont fournies, puis cliquez sur **suivant**.

7.  Dans **sélectionner les services de rôle**, cliquez sur **serveur NPS**.  Dans **ajouter des fonctionnalités qui sont nécessaires pour le serveur NPS**, cliquez sur **ajouter des fonctionnalités**. Cliquez sur **suivant**.

8.  Dans **confirmer les sélections d’installation**, cliquez sur **redémarrer automatiquement le serveur de destination si nécessaire**. Lorsque vous êtes invité à confirmer ce choix, cliquez sur **Oui**, puis cliquez sur **installer**. La page de progression Installation affiche l’état pendant le processus d’installation. Lorsque le processus terminé, le message «Installation réussie sur *Nom_Ordinateur*» s’affiche, où *Nom_Ordinateur* est le nom de l’ordinateur sur lequel vous avez installé le serveur de stratégie réseau. Cliquez sur **fermer **.

Pour plus d’informations, voir [gérer les serveurs NPS](nps-manage-servers.md).