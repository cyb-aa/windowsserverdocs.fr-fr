---
title: Déployer des hôtes protégés
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 2379ca26-b32d-4055-8b4b-99d1f2df37e1
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: bc79d13b4dda96cd3e760958a6310276d2c45bae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386754"
---
# <a name="deploy-guarded-hosts"></a>Déployer des hôtes protégés

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Les rubriques de cette section décrivent les étapes nécessaires à un administrateur de structure pour configurer des ordinateurs hôtes Hyper-V pour qu’ils fonctionnent avec le service Guardian hôte (SGH). Avant de pouvoir démarrer ces étapes, au moins un nœud du [cluster SGH doit être configuré](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).

**Pour l’attestation approuvée par le module de plateforme sécurisée**:
1. [Configurez le DNS de l’infrastructure](guarded-fabric-configuring-fabric-dns.md): Indique comment configurer un redirecteur DNS à partir du domaine de l’infrastructure dans le domaine SGH.
2. [Capturer les informations requises par le SGH](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md): Indique comment capturer des identificateurs TPM (également appelés identificateurs de plateforme), créer une stratégie d’intégrité du code et créer une ligne de base du module de plateforme sécurisée (TPM). Ensuite, vous fournissez ces informations à l’administrateur SGH pour configurer l’attestation.
3. [Confirmer que les hôtes protégés peuvent attester](guarded-fabric-confirm-hosts-can-attest-successfully.md)

**Pour l’attestation de clé hôte**:
1. [Créez une clé d’hôte](guarded-fabric-create-host-key.md#create-a-host-key): Indique comment configurer un redirecteur DNS à partir du domaine de l’infrastructure dans le domaine SGH.
2. [Ajoutez la clé hôte au service d’attestation](guarded-fabric-create-host-key.md#add-the-host-key-to-the-attestation-service): Indique comment configurer un groupe de sécurité Active Directory dans le domaine de l’infrastructure, ajouter des hôtes service Guardian en tant que membres de ce groupe et fournir cet identificateur de groupe à l’administrateur SGH. 
3. [Confirmer que les hôtes protégés peuvent attester](guarded-fabric-confirm-hosts-can-attest-successfully.md)


**Pour une attestation approuvée par l’administrateur**:
1. [Configurez le DNS de l’infrastructure](guarded-fabric-configuring-fabric-dns.md): Indique comment configurer un redirecteur DNS à partir du domaine de l’infrastructure dans le domaine SGH.
2. [Créez un groupe de sécurité](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md): Indique comment configurer un groupe de sécurité Active Directory dans le domaine de l’infrastructure, ajouter des hôtes service Guardian en tant que membres de ce groupe et fournir cet identificateur de groupe à l’administrateur SGH. 
3. [Confirmer que les hôtes protégés peuvent attester](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>Voir aussi

- [Tâches de déploiement pour les infrastructures protégées et les machines virtuelles protégées](guarded-fabric-deploying-hgs-overview.md#deployment-tasks-for-guarded-fabrics-and-shielded-vms)
