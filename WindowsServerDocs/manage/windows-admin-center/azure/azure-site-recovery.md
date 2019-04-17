---
title: Protéger vos Machines virtuelles de Hyper-V avec Azure Site Recovery et Windows Admin Center
description: Utilisez Windows Admin Center (projet Honolulu) pour protéger vos machines virtuelles Hyper-V avec Azure Site Recovery.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 8363343d3378f382a3b7f467e5df2a1948c07381
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296937"
---
# Protéger vos Machines virtuelles de Hyper-V avec Azure Site Recovery et Windows Admin Center

>S’applique à: Windows Admin Center Preview, Windows Admin Center

[En savoir plus sur l’intégration d’Azure avec Windows Admin Center.](../plan/azure-integration-options.md)

Windows Admin Center simplifie le processus de réplication de vos machines virtuelles sur vos serveurs ou clusters Hyper-V. Vous pouvez ainsi exploiter plus facilement la puissance d'Azure à partir de votre propre centre de données. Pour automatiser l’installation, vous pouvez connecter la passerelle Windows Admin Center à Azure.

Utilisez les informations suivantes pour configurer les paramètres de réplication et créer un plan de récupération à partir du Portail Azure. Windows Admin Center pourra ainsi démarrer la réplication de vos machines virtuelles et les protéger.

## Qu’est-ce qu'Azure Site Recovery et comment fonctionne-t-il avec Windows Admin Center? 

**Azure Site Recovery** est un service Azure qui réplique les charges de travail exécutées sur les machines virtuelles afin que votre infrastructure critique soit protégée en cas d'incident.  [En savoir plus sur Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)

Azure Site Recovery comprend deux composants: la **réplication** et le **basculement**. La partie réplication protège vos machines virtuelles contre d'éventuels incidents en répliquant leur disque dur virtuel cible sur un compte de stockage Azure. En cas d'urgence, vous pouvez alors les basculer et les exécuter dans Azure. Vous pouvez également effectuer un test de basculement sans impact sur vos machines virtuelles principales pour tester le processus de récupération dans Azure.

L’installation du composant de réplication uniquement suffit à protéger votre ordinateur virtuel des incidents. Toutefois, vous ne pouvez démarrer la machine virtuelle dans Azure que si vous avez configuré la partie basculement. Vous pouvez configurer la partie basculement au moment où vous souhaitez basculer vers une machine virtuelle Azure. Ce n’est pas nécessaire dans le cadre de l’installation initiale. Si le serveur hôte tombe en panne et que vous n’avez pas encore configuré le composant de basculement, vous pouvez le faire à ce moment-là et accéder aux charges de travail de la machine virtuelle protégée. Toutefois, il est recommandé de configurer les paramètres relatifs au basculement avant qu'un incident se produise.
 

## Conditions préalables et planification

- Les serveurs cible qui hébergent les machines virtuelles à protéger doivent disposer d'un accès à Internet pour effectuer la réplication sur Azure.
- [Connectez votre passerelle Windows Admin Center à Azure](azure-integration.md).
- [Examinez l'outil de planification de la capacité pour évaluer les conditions requises pour une réplication et un basculement corrects](https://docs.microsoft.com/azure/site-recovery/hyper-v-site-walkthrough-capacity).

## Étape1: Configurer la protection de l'ordinateur virtuel sur votre ordinateur hôte cible

> [!NOTE] 
> Vous devez effectuer cette étape une fois par serveur ou cluster hôte contenant des machines virtuelles ciblées pour la protection.

1. Accédez au serveur ou au cluster hébergeant des machines virtuelles à protéger (à l'aide du Gestionnaire de serveur ou du Gestionnaire du cluster hyperconvergé).
2. Accédez à **Machines Virtuelles** > **Inventaire**.
3. Sélectionnez n’importe quel ordinateur virtuel (pas forcément celui que vous souhaitez protéger).
4. Sélectionnez **Plus** > **Configurer la protection de l'ordinateur virtuel**.
5. Connectez-vous à votre compte Azure.
6. Entrez les informations requises:

 - **Abonnement:** l’abonnement Azure que vous souhaitez utiliser pour la réplication des machines virtuelles sur cet ordinateur hôte.
 - **Emplacement:** région Azure où créer les ressources de récupération automatique du système.
 - **Compte de stockage:** compte de stockage dans lequel les charges de travail des machines virtuelles répliquées sur cet ordinateur hôte seront enregistrées.
 - **Coffre:** choisissez un nom de coffre Azure Site Recovery pour les machines virtuelles protégées sur cet ordinateur hôte.

7.  Sélectionnez **Setup ASR**.
8.  Patientez jusqu'à ce que la notification **Site Recovery Setting Completed** s'affiche.
 
Cela peut prendre jusqu'à 10minutes. Vous pouvez surveiller la progression en accédant à **Notifications** (icône en forme de cloche en haut à droite).

>[!NOTE]
> Cette étape installe automatiquement l’agent de récupération automatique du système sur le serveur ou les nœuds (en cas de configuration sur un cluster) cible, crée un **Groupe de ressources** avec le **Compte de stockage** et le **Coffre** spécifiés dans l'**Emplacement** indiqué. Cela inscrit également l’ordinateur hôte cible avec le service de récupération automatique du système et configure une stratégie de réplication par défaut.

## Étape2: Sélectionner les machines virtuelles à protéger

1. Revenez au serveur ou au cluster que vous avez configuré à l’étape2 ci-dessus, puis accédez à **Machines Virtuelles > Inventaire**.
2. Sélectionnez la machine virtuelle à protéger.
3. Sélectionnez **Plus** > **Protéger une machine virtuelle**.
4. Passez en revue les [besoins en capacité pour la protection de la machine virtuelle](https://docs.microsoft.com/azure/site-recovery/site-recovery-capacity-planner).

    [Créez un compte de stockage premium dans le Portail Azure](https://docs.microsoft.com/azure/storage/common/storage-premium-storage) si vous souhaitez en utiliser un. L'option **Créer nouveau** fournie dans le volet Windows Admin Center crée un compte de stockage standard.

5. Entrez le nom du **Compte de stockage** à utiliser pour la réplication de cette machine virtuelle, puis sélectionnez **Protéger une machine virtuelle**. Cette étape permet d’activer la réplication de la machine virtuelle sélectionnée. 

6. La récupération automatique du système démarrera la réplication. La réplication est terminée et la machine virtuelle est protégée lorsque la valeur dans la colonne **Protégée** du tableau **Virtual Machine Inventory** est remplacée par **Oui**. L'opération peut prendre quelques minutes.  

## Étape3: Configurer et exécuter un test de basculement dans le Portail Azure

 Cette étape n'est pas nécessaire lorsque vous lancez la réplication d’une machine virtuelle (celle-ci est déjà protégée avec la réplication uniquement), mais nous vous recommandons de configurer les paramètres de basculement lorsque vous configurez Azure Site Recovery. Si vous souhaitez préparer le basculement vers une machine virtuelle Azure, procédez comme suit:

1. [Configurez un réseau Azure](https://docs.microsoft.com/azure/site-recovery/hyper-v-site-walkthrough-prepare-azure). La machine virtuelle basculée sera attachée à ce réseau virtuel. Notez que les autres étapes répertoriées dans la page liée sont automatiquement effectuées par Windows Admin Center; vous devez uniquement configurer le réseau Azure.

2. [Effectuer un test de basculement](https://docs.microsoft.com/azure/site-recovery/hyper-v-site-walkthrough-test-failover)

## Étape4: Créer des plans de récupération

Le **Plan de récupération** est une fonctionnalité d'Azure Site Recovery qui permet de basculer et de récupérer une application entière comprenant un regroupement de machines virtuelles. Il est possible de récupérer chaque machine virtuelle protégée une par une, mais en ajoutant les machines virtuelles comprenant une application à un plan de récupération, vous pouvez basculer l’ensemble de l’application via le plan de récupération. Vous pouvez également utiliser la fonctionnalité de test de basculement du plan de récupération pour tester la récupération de l’application. Le plan de récupération vous permet de regrouper des machines virtuelles, de définir l’ordre dans lequel celles-ci doivent être appelées lors d'un basculement et d'automatiser des étapes supplémentaires à effectuer dans le cadre du processus de récupération. Une fois que vous avez protégé vos machines virtuelles, vous pouvez accéder au coffre Azure Site Recovery dans le Portail Azure et créer des plans de récupération pour ces machines virtuelles. [En savoir plus sur les plans de récupération](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans).

## Analyse des machines virtuelles répliquées dans Azure ##

Pour vérifier qu'il n'y a pas d'erreurs dans l’inscription du serveur, accédez à **Portail Azure** > **Toutes les ressources** > **Coffre Recovery Services** (celui spécifié à l’étape2) > **Travaux** > **Travaux Site Recovery**.

Vous pouvez surveiller la réplication d'une machine virtuelle en accédant à **Coffre Recovery Services** > **Éléments répliqués**.

Pour voir tous les serveurs inscrits dans le coffre, accédez à **Coffre Recovery Services** > **Infrastructure Site Recovery** > **Ordinateurs hôtes Hyper-V** (sous la section Sites Hyper-V).

## Problème connu ##

Lors de l'inscription de la récupération automatique du système avec un cluster, si un nœud ne parvient pas à installer la récupération automatique du système ou à s’inscrire à ce service, vos machines virtuelles ne peuvent pas être protégées. Vérifiez que tous les nœuds du cluster sont enregistrés dans le Portail Azure en accédant à **Coffre Recovery Services** > **Travaux** > **Travaux Site Recovery**.