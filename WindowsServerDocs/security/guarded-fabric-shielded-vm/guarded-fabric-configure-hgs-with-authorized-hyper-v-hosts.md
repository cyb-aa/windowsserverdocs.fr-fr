---
title: Déployer des hôtes protégés
ms.prod: windows-server
ms.topic: article
ms.assetid: 2379ca26-b32d-4055-8b4b-99d1f2df37e1
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 0f678172d397ff61fd336b7c844d43f77bea7fad
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856832"
---
# <a name="deploy-guarded-hosts"></a>Déployer des hôtes protégés

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Les rubriques de cette section décrivent les étapes nécessaires à un administrateur de structure pour configurer des ordinateurs hôtes Hyper-V pour qu’ils fonctionnent avec le service Guardian hôte (SGH). Avant de pouvoir démarrer ces étapes, au moins un nœud du [cluster SGH doit être configuré](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).

**Pour l’attestation approuvée par le module de plateforme sécurisée**:
1. [Configuration de l’infrastructure DNS](guarded-fabric-configuring-fabric-dns.md): indique comment configurer un redirecteur DNS du domaine de l’infrastructure vers le domaine SGH.
2. [Informations de capture requises par SGH](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md): indique comment capturer des identificateurs de module de plateforme sécurisée (également appelés identificateurs de plateforme), créer une stratégie d’intégrité du code et créer une ligne de base du module de plateforme sécurisée (TPM). Ensuite, vous fournissez ces informations à l’administrateur SGH pour configurer l’attestation.
3. [Confirmer que les hôtes protégés peuvent attester](guarded-fabric-confirm-hosts-can-attest-successfully.md)

**Pour l’attestation de clé hôte**:
1. [Créer une clé d’hôte](guarded-fabric-create-host-key.md#create-a-host-key): indique comment configurer un redirecteur DNS du domaine de l’infrastructure vers le domaine SGH.
2. [Ajouter la clé d’hôte au service d’attestation](guarded-fabric-create-host-key.md#add-the-host-key-to-the-attestation-service): indique comment configurer un groupe de sécurité Active Directory dans le domaine de l’infrastructure, ajouter des hôtes service Guardian en tant que membres de ce groupe et fournir cet identificateur de groupe à l’administrateur SGH. 
3. [Confirmer que les hôtes protégés peuvent attester](guarded-fabric-confirm-hosts-can-attest-successfully.md)


**Pour une attestation approuvée par l’administrateur**:
1. [Configuration de l’infrastructure DNS](guarded-fabric-configuring-fabric-dns.md): indique comment configurer un redirecteur DNS du domaine de l’infrastructure vers le domaine SGH.
2. [Créer un groupe de sécurité](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md): indique comment configurer un groupe de sécurité Active Directory dans le domaine de l’infrastructure, ajouter des hôtes service Guardian en tant que membres de ce groupe et fournir cet identificateur de groupe à l’administrateur SGH. 
3. [Confirmer que les hôtes protégés peuvent attester](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>Voir aussi

- [Tâches de déploiement pour les infrastructures protégées et les machines virtuelles protégées](guarded-fabric-deploying-hgs-overview.md#deployment-tasks-for-guarded-fabrics-and-shielded-vms)
