---
title: Déployer des hôtes protégés
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 2379ca26-b32d-4055-8b4b-99d1f2df37e1
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 3b20a7eb2b5097d8ddb7381fd0304581ca4e6722
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845350"
---
# <a name="deploy-guarded-hosts"></a>Déployer des hôtes protégés

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Les rubriques de cette section décrivent les étapes un administrateur de l’infrastructure nécessaire pour configurer des hôtes Hyper-V fonctionne avec le Service Guardian hôte (SGH). Avant de commencer ces étapes, au moins un nœud dans le [cluster du service HGS doit être configuré](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).

**Pour l’attestation approuvée par le module de plateforme sécurisée**:
1. [Configuration de l’infrastructure DNS](guarded-fabric-configuring-fabric-dns.md): Indique comment configurer un redirecteur DNS du domaine de l’infrastructure pour le domaine SGH.
2. [Capturer les informations requises par SGH](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md): Indique comment capturer des identificateurs de module de plateforme sécurisée (également appelés identificateurs de plateforme), créer une stratégie d’intégrité du Code et créer une base de référence de module de plateforme sécurisée. Ensuite, vous fournirez ces informations à l’administrateur SGH pour configurer l’attestation.
3. [Confirmer le diront hôtes service Guardian](guarded-fabric-confirm-hosts-can-attest-successfully.md)

**Pour l’attestation de clé hôte**:
1. [Créer une clé d’hôte](guarded-fabric-create-host-key.md#create-a-host-key): Indique comment configurer un redirecteur DNS du domaine de l’infrastructure pour le domaine SGH.
2. [Ajouter la clé d’hôte pour le service d’attestation](guarded-fabric-create-host-key.md#add-the-host-key-to-the-attestation-service): Indique comment configurer un groupe de sécurité Active Directory dans le domaine de l’infrastructure, d’ajouter des hôtes service Guardian en tant que membres de ce groupe et fournissez cet identificateur de groupe à l’administrateur SGH. 
3. [Confirmer le diront hôtes service Guardian](guarded-fabric-confirm-hosts-can-attest-successfully.md)


**Pour l’attestation approuvée par l’administrateur**:
1. [Configuration de l’infrastructure DNS](guarded-fabric-configuring-fabric-dns.md): Indique comment configurer un redirecteur DNS du domaine de l’infrastructure pour le domaine SGH.
2. [Créer un groupe de sécurité](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md): Indique comment configurer un groupe de sécurité Active Directory dans le domaine de l’infrastructure, d’ajouter des hôtes service Guardian en tant que membres de ce groupe et fournissez cet identificateur de groupe à l’administrateur SGH. 
3. [Confirmer le diront hôtes service Guardian](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>Voir aussi

- [Tâches de déploiement pour service Guardian infrastructures et des machines virtuelles protégées](guarded-fabric-deploying-hgs-overview.md#deployment-tasks-for-guarded-fabrics-and-shielded-vms)
