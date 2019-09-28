---
title: Ajouter des informations d’ordinateur hôte pour l’attestation approuvée par l’administrateur
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 87089ebc-b953-4aa3-96b5-966cf91acb02
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 946f91d05063475ae45fb334c67f8d5081d3984d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403714"
---
>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

# <a name="authorize-hyper-v-hosts-using-admin-trusted-attestation"></a>Autoriser les hôtes Hyper-V à l’aide d’une attestation approuvée par l’administrateur

>[!IMPORTANT]
>L’attestation approuvée par l’administrateur (mode AD) est déconseillée à compter de Windows Server 2019. Pour les environnements où l’attestation de module de plateforme sécurisée n’est pas possible, configurez l' [attestation de clé hôte](guarded-fabric-initialize-hgs-key-mode.md). L’attestation de clé hôte offre une garantie similaire au mode AD et est plus simple à configurer. 


Pour autoriser un hôte service Guardian en mode Active Directory : 

1. Dans le domaine de l’infrastructure, ajoutez les ordinateurs hôtes Hyper-V à un groupe de sécurité.
2. Dans le domaine SGH, enregistrez le SID du groupe de sécurité avec SGH. 

## <a name="add-the-hyper-v-host-to-a-security-group-and-reboot-the-host"></a>Ajouter l’hôte Hyper-V à un groupe de sécurité et redémarrer l’ordinateur hôte

1. Créez un groupe de sécurité **Global** dans le domaine de l’infrastructure et ajoutez des ordinateurs hôtes Hyper-V qui exécuteront des machines virtuelles dotées d’une protection maximale. 
   Redémarrez les ordinateurs hôtes pour mettre à jour leur appartenance à un groupe.

2. Utilisez l’opération obtenir un ID de sécurité pour obtenir l’identificateur de sécurité (SID) du groupe de sécurité et le fournir à l’administrateur SGH. 

   ```powershell
   Get-ADGroup "Guarded Hosts"
   ```

   ![Commande d’extraction avec sortie](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>Inscrire le SID du groupe de sécurité auprès de SGH  

1. Obtenez le SID du groupe de sécurité pour les hôtes service Guardian auprès de l’administrateur de l’infrastructure et exécutez la commande suivante pour inscrire le groupe de sécurité auprès de SGH. 
   Exécutez à nouveau la commande si nécessaire pour des groupes supplémentaires. 
   Fournissez un nom convivial pour le groupe. 
   Elle n’a pas besoin de correspondre au nom du groupe de sécurité Active Directory. 

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. Pour vérifier que le groupe a été ajouté, exécutez la [HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx). 


