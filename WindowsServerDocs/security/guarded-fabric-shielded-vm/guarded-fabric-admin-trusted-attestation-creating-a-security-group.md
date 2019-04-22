---
title: Créer un groupe de sécurité pour les hôtes service Guardian et inscrire le groupe avec SGH
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: ba547dff862a283b68ff105a14b14ed367891745
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816340"
---
# <a name="create-a-security-group-for-guarded-hosts-and-register-the-group-with-hgs"></a>Créer un groupe de sécurité pour les hôtes service Guardian et inscrire le groupe avec SGH

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

>[!IMPORTANT]
>Mode AD est déconseillé à compter de Windows Server 2019. Pour les environnements où l’attestation TPM n’est pas possible, configurez [héberger l’attestation de clé](guarded-fabric-initialize-hgs-key-mode.md). L’attestation de clé hôte fournit la garantie similaire au mode d’AD et est plus simple à configurer. 


Cette rubrique décrit les étapes intermédiaires pour préparer les hôtes Hyper-V pour devenir des hôtes service Guardian à l’aide de l’attestation approuvée par l’administrateur (mode AD). Avant d’effectuer ces étapes, suivez les étapes de [configuration de la structure DNS pour les hôtes qui deviendront des hôtes service Guardian](guarded-fabric-configuring-fabric-dns-ad.md).


## <a name="create-a-security-group-and-add-hosts"></a>Créer un groupe de sécurité et ajouter des ordinateurs hôtes

1. Créer un nouveau **GLOBAL** groupe du domaine de l’infrastructure de sécurité et ajouter des hôtes Hyper-V qui exécuteront les machines virtuelles protégées. Redémarrez les ordinateurs hôtes pour mettre à jour leur appartenance aux groupes.

2. Utilisez Get-ADGroup pour obtenir l’identificateur de sécurité (SID) du groupe de sécurité et à le fournir à l’administrateur SGH. 

    ```powershell
    Get-ADGroup "Guarded Hosts"
    ```

    ![Commande Get-AdGroup avec sortie](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>Inscrire le SID du groupe de sécurité avec SGH  

1. Sur un serveur SGH, exécutez la commande suivante pour inscrire le groupe de sécurité avec SGH. 
   Réexécutez la commande si nécessaire pour des groupes supplémentaires. 
   Fournissez un nom convivial pour le groupe. 
   Il n’a pas besoin de correspondre au nom de groupe de sécurité Active Directory. 

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. Pour vérifier que le groupe a été ajouté, exécutez [Get-HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx). 

## <a name="next-step"></a>Étape suivante

>[!div class="nextstepaction"]
[Confirmer l’attestation](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>Voir aussi

- [Déploiement du Service Guardian hôte pour les hôtes service Guardian et des machines virtuelles protégées](guarded-fabric-deploying-hgs-overview.md)
