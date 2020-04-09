---
title: Configuration du transfert DNS et de l’approbation de domaine
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 6d6ad10dacf9c667069ecd43f38473a3f20bc781
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856852"
---
# <a name="configure-dns-forwarding-in-the-hgs-domain-and-a-one-way-trust-with-the-fabric-domain"></a>Configurer le transfert DNS dans le domaine SGH et une approbation unidirectionnelle avec le domaine de l’infrastructure

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

>[!IMPORTANT]
>Le mode AD est déconseillé à partir de Windows Server 2019. Pour les environnements où l’attestation de module de plateforme sécurisée n’est pas possible, configurez l' [attestation de clé hôte](guarded-fabric-initialize-hgs-key-mode.md). L’attestation de clé hôte offre une garantie similaire au mode AD et est plus simple à configurer. 

Procédez comme suit pour configurer le transfert DNS et établir une relation d’approbation unidirectionnelle avec le domaine de l’infrastructure. Ces étapes permettent à SGH de localiser les contrôleurs de domaine de l’infrastructure et de valider l’appartenance au groupe des ordinateurs hôtes Hyper-V.

1.  Exécutez la commande suivante dans une session PowerShell avec élévation de privilèges pour configurer le transfert DNS. Remplacez fabrikam.com par le nom du domaine de l’infrastructure et tapez les adresses IP des serveurs DNS dans le domaine de la structure. Pour une haute disponibilité, pointez sur plusieurs serveurs DNS.

    ```powershell
    Add-DnsServerConditionalForwarderZone -Name "fabrikam.com" -ReplicationScope "Forest" -MasterServers <DNSserverAddress1>, <DNSserverAddress2>
    ```

2.  Pour créer une approbation de forêt unidirectionnelle, exécutez la commande suivante dans une invite de commandes avec élévation de privilèges :

    Remplacez `bastion.local` par le nom du domaine SGH et `fabrikam.com` par le nom du domaine de l’infrastructure. Indiquez le mot de passe d’un administrateur du domaine de l’infrastructure.

        netdom trust bastion.local /domain:fabrikam.com /userD:fabrikam.com\Administrator /passwordD:<password> /add

## <a name="next-step"></a>Étape suivante 

> [!div class="nextstepaction"]
> [Configurer HTTPS](guarded-fabric-configure-hgs-https.md)
