---
title: Déployer des machines virtuelles Azure à l’aide du centre d’administration Windows
description: Déploiement de machines virtuelles Azure à l’aide du centre d’administration Windows. Configuration de machines virtuelles Azure dans le cadre du centre d’administration Windows-scénarios gérés.
ms.technology: manage
ms.topic: article
author: nedpyle
ms.author: nedpyle
manager: jgerend
ms.date: 01/28/2020
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 551f97d257b7ea3d05cdded73421d2e5c088171e
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830615"
---
# <a name="deploy-azure-virtual-machines-from-within-windows-admin-center"></a>Déployer des machines virtuelles Azure à partir du centre d’administration Windows

>S’applique à : Centre d’administration Windows, version préliminaire du centre d’administration Windows

Le centre d’administration Windows version 1910 vous permet de déployer des machines virtuelles Azure. Cela intègre le déploiement de machine virtuelle dans les charges de travail gérées par le centre d’administration Windows, telles que le [service de migration de stockage](../../../storage/storage-migration-service/overview.md) et le réplica de [stockage](../../../storage/storage-replica/storage-replica-overview.md). Au lieu de créer des serveurs et des machines virtuelles dans le portail Azure manuellement avant de déployer votre charge de travail, et éventuellement des étapes et des configurations requises, le centre d’administration Windows peut déployer la machine virtuelle Azure, configurer son stockage, la joindre à votre domaine, installer des rôles et Configurez ensuite votre système distribué. Vous pouvez également déployer de nouvelles machines virtuelles Azure sans une charge de travail à partir de la page des connexions du centre d’administration Windows.

Le centre d’administration Windows gère également un large éventail de services Azure. [En savoir plus sur les options d’intégration Azure disponibles dans le centre d’administration Windows](../plan/azure-integration-options.md).

## <a name="scenarios"></a>Scénarios

Le centre d’administration Windows version 1910 le déploiement de machines virtuelles Azure prend en charge les scénarios suivants :

- [Service de migration de stockage](../../../storage/storage-migration-service/overview.md)
- [Réplica de stockage](../../../storage/storage-replica/storage-replica-overview.md)
- [Nouveau serveur autonome (sans rôles)](index.md#extend-on-premises-capacity-with-azure)

## <a name="requirements"></a>Conditions préalables

Pour créer une nouvelle machine virtuelle Azure à partir du centre d’administration Windows, vous devez disposer des éléments suivants :

- Un [abonnement Azure](https://azure.microsoft.com).
- Une [passerelle du centre d’administration Windows inscrite auprès d’Azure](azure-integration.md)
- Un [groupe de ressources Azure](https://docs.microsoft.com/azure/azure-resource-manager/management/overview) existant dans lequel vous disposez d’autorisations de création.
- Un [réseau virtuel](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) et un sous-réseau Azure existants.
- Une solution [Azure Express route](https://azure.microsoft.com/services/expressroute/) ou [VPN Azure](https://azure.microsoft.com/services/vpn-gateway/) liée au réseau virtuel et au sous-réseau qui permet la connectivité à partir de machines virtuelles Azure vers vos clients locaux, les contrôleurs de domaine, l’ordinateur du centre d’administration Windows et tous les serveurs nécessitant une communication avec cette machine virtuelle dans le cadre d’un déploiement de charges de travail. Par exemple, pour utiliser Storage migration service pour migrer le stockage vers une machine virtuelle Azure, l’ordinateur Orchestrator et l’ordinateur source doivent tous deux être en mesure de contacter la machine virtuelle Azure de destination vers laquelle vous effectuez la migration.

## <a name="usage"></a>Utilisation

Les étapes et les assistants de déploiement d’une machine virtuelle Azure varient selon le scénario. Consultez la documentation de la charge de travail pour obtenir des informations détaillées sur le scénario global.

### <a name="deploying-azure-vms-as-part-of-storage-migration-service"></a>Déploiement de machines virtuelles Azure dans le cadre du service de migration de stockage

À partir de l’outil *Storage migration service* dans le centre d’administration Windows, effectuez un inventaire d’un ou plusieurs serveurs sources. Une fois que vous êtes dans la phase de *transfert des données* , sélectionnez **créer une nouvelle machine virtuelle Azure** dans la page *spécifier une destination* , puis cliquez sur **créer une machine virtuelle**. Cela démarre un outil de création étape par étape qui sélectionne une machine virtuelle Windows Server 2012 R2, Windows Server 2016 ou Windows Server 2019 Azure comme destination de la migration. Le service de migration de stockage fournit des tailles de machine virtuelle recommandées pour correspondre à votre source, mais vous pouvez les remplacer en cliquant sur **afficher toutes les tailles**. Les données du serveur source sont également utilisées pour configurer automatiquement vos disques gérés et leurs systèmes de fichiers, ainsi que pour joindre votre nouvelle machine virtuelle Azure à votre domaine Active Directory. Si la machine virtuelle est Windows Server 2019 (que nous vous recommandons), le centre d’administration Windows installe la fonctionnalité de proxy du service de migration de stockage. Une fois qu’il a créé la machine virtuelle Azure, le centre d’administration Windows revient au flux de travail de transfert du service de migration de stockage normal.  


### <a name="deploying-azure-vms-as-part-of-storage-replica"></a>Déploiement de machines virtuelles Azure dans le cadre du réplica de stockage

À partir de l’outil *réplica de stockage* dans le centre d’administration Windows, sous l’onglet *partenariats* , cliquez sur **nouveau** , puis sous *répliquer avec un autre serveur* , cliquez sur **utiliser une nouvelle machine virtuelle Azure** , puis cliquez sur **suivant**. Spécifiez les informations du serveur source et le nom du groupe de réplication, puis cliquez sur **suivant**. Cela démarre un processus qui sélectionne automatiquement une machine virtuelle Windows Server 2016 ou Windows Server 2019 Azure comme destination de la source de migration. Storage Migration Service recommande que les tailles de machine virtuelle correspondent à votre source, mais vous pouvez la remplacer en sélectionnant **afficher toutes les tailles**. Les données d’inventaire sont utilisées pour configurer automatiquement vos disques gérés et leurs systèmes de fichiers, ainsi que pour joindre votre nouvelle machine virtuelle Azure à votre domaine Active Directory. Une fois que le centre d’administration Windows a créé la machine virtuelle Azure, fournissez un nom de groupe de réplication, puis cliquez sur **créer**. Le centre d’administration Windows lance ensuite le processus normal de synchronisation du réplica de stockage pour commencer à protéger vos données.


### <a name="deploying-a-new-standalone-azure-vm"></a>Déploiement d’une nouvelle machine virtuelle Azure autonome

À partir de la page *Toutes les connexions* dans Windows Admin Center, accédez à **Ajouter**, puis sélectionnez **Créer** sous **Machine virtuelle Azure**. Cela démarre un outil de création étape par étape qui vous permet de sélectionner une machine virtuelle Windows Server 2012 R2, Windows Server 2016 ou Windows Server 2019 Azure, de choisir une taille, d’ajouter des disques gérés et éventuellement de joindre votre domaine Active Directory.
