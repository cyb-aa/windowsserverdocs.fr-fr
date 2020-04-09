---
title: Créer un groupe de sécurité pour les hôtes service Guardian et inscrire le groupe auprès de SGH
ms.prod: windows-server
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: b29f8bb4bfb8b2b685a6c4ec1a1d2965d8fbde58
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856942"
---
# <a name="create-a-security-group-for-guarded-hosts-and-register-the-group-with-hgs"></a>Créer un groupe de sécurité pour les hôtes service Guardian et inscrire le groupe auprès de SGH

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

>[!IMPORTANT]
>Le mode AD est déconseillé à partir de Windows Server 2019. Pour les environnements où l’attestation de module de plateforme sécurisée n’est pas possible, configurez l' [attestation de clé hôte](guarded-fabric-initialize-hgs-key-mode.md). L’attestation de clé hôte offre une garantie similaire au mode AD et est plus simple à configurer. 


Cette rubrique décrit les étapes intermédiaires permettant de préparer les ordinateurs hôtes Hyper-V pour qu’ils deviennent des hôtes protégés à l’aide d’une attestation approuvée par l’administrateur (mode AD). Avant de suivre ces étapes, effectuez les étapes de la section [configuration du DNS de l’infrastructure pour les hôtes qui deviendront des hôtes service Guardian](guarded-fabric-configuring-fabric-dns-ad.md).


## <a name="create-a-security-group-and-add-hosts"></a>Créer un groupe de sécurité et ajouter des ordinateurs hôtes

1. Créez un groupe de sécurité **Global** dans le domaine de l’infrastructure et ajoutez des ordinateurs hôtes Hyper-V qui exécuteront des machines virtuelles dotées d’une protection maximale. Redémarrez les ordinateurs hôtes pour mettre à jour leur appartenance à un groupe.

2. Utilisez l’opération obtenir un ID de sécurité pour obtenir l’identificateur de sécurité (SID) du groupe de sécurité et le fournir à l’administrateur SGH. 

    ```powershell
    Get-ADGroup "Guarded Hosts"
    ```

    ![Commande d’extraction avec sortie](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>Inscrire le SID du groupe de sécurité auprès de SGH  

1. Sur un serveur SGH, exécutez la commande suivante pour enregistrer le groupe de sécurité avec SGH. 
   Exécutez à nouveau la commande si nécessaire pour des groupes supplémentaires. 
   Fournissez un nom convivial pour le groupe. 
   Elle n’a pas besoin de correspondre au nom du groupe de sécurité Active Directory. 

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. Pour vérifier que le groupe a été ajouté, exécutez la [HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx). 

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Confirmer l’attestation](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>Voir aussi

- [Déploiement du service Guardian hôte pour les hôtes service Guardian et les machines virtuelles protégées](guarded-fabric-deploying-hgs-overview.md)
