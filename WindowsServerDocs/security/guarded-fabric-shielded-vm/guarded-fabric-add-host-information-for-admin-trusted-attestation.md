---
title: Ajouter des informations sur l’hôte pour l’attestation approuvée par l’administrateur
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 87089ebc-b953-4aa3-96b5-966cf91acb02
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7949711dbb0f89f5404b491d60938985bfa98c22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849460"
---
>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

# <a name="authorize-hyper-v-hosts-using-admin-trusted-attestation"></a>Autoriser les hôtes Hyper-V à l’aide de l’attestation approuvée par l’administrateur

>[!IMPORTANT]
>Attestation approuvée par l’administrateur (mode AD) est déconseillée à compter de Windows Server 2019. Pour les environnements où l’attestation TPM n’est pas possible, configurez [héberger l’attestation de clé](guarded-fabric-initialize-hgs-key-mode.md). L’attestation de clé hôte fournit la garantie similaire au mode d’AD et est plus simple à configurer. 


Pour autoriser un hôte service Guardian dans mode AD : 

1. Dans le domaine de l’infrastructure, ajoutez les hôtes Hyper-V à un groupe de sécurité.
2. Dans le domaine SGH, inscrivez le SID du groupe de sécurité avec SGH. 

## <a name="add-the-hyper-v-host-to-a-security-group-and-reboot-the-host"></a>Ajouter l’ordinateur hôte Hyper-V à un groupe de sécurité et de redémarrer l’ordinateur hôte

1. Créer un **GLOBAL** groupe du domaine de l’infrastructure de sécurité et ajouter des hôtes Hyper-V qui exécuteront les machines virtuelles protégées. 
   Redémarrez les ordinateurs hôtes pour mettre à jour leur appartenance aux groupes.

2. Utilisez Get-ADGroup pour obtenir l’identificateur de sécurité (SID) du groupe de sécurité et à le fournir à l’administrateur SGH. 

   ```powershell
   Get-ADGroup "Guarded Hosts"
   ```

   ![Commande Get-AdGroup avec sortie](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>Inscrire le SID du groupe de sécurité avec SGH  

1. Obtenir le SID du groupe de sécurité pour les hôtes service Guardian à partir de l’administrateur de structure et exécutez la commande suivante pour inscrire le groupe de sécurité avec SGH. 
   Réexécutez la commande si nécessaire pour des groupes supplémentaires. 
   Fournissez un nom convivial pour le groupe. 
   Il n’a pas besoin de correspondre au nom de groupe de sécurité Active Directory. 

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. Pour vérifier que le groupe a été ajouté, exécutez [Get-HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx). 


