---
title: Configurer la redirection DNS et approbation de domaine
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: ebc9c2a3cac85ab998075d784111808b3d590d46
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854140"
---
# <a name="configure-dns-forwarding-in-the-hgs-domain-and-a-one-way-trust-with-the-fabric-domain"></a>Configurer la redirection DNS dans le domaine SGH et une approbation à sens unique avec le domaine de fabric

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

>[!IMPORTANT]
>Mode AD est déconseillé à compter de Windows Server 2019. Pour les environnements où l’attestation TPM n’est pas possible, configurez [héberger l’attestation de clé](guarded-fabric-initialize-hgs-key-mode.md). L’attestation de clé hôte fournit la garantie similaire au mode d’AD et est plus simple à configurer. 

Utilisez les étapes suivantes pour configurer la redirection DNS et établir une approbation à sens unique avec le domaine de fabric. Ces étapes permettent le SGH pour localiser l’infrastructure de contrôleurs de domaine et valider l’appartenance au groupe des ordinateurs hôtes Hyper-V.

1.  Exécutez la commande suivante dans une session PowerShell avec élévation de privilèges pour configurer la redirection DNS. Remplacez fabrikam.com par le nom du domaine fabric et tapez les adresses IP des serveurs DNS dans le domaine de fabric. Pour accroître la disponibilité, pointez sur plusieurs serveurs DNS.

    ```powershell
    Add-DnsServerConditionalForwarderZone -Name "fabrikam.com" -ReplicationScope "Forest" -MasterServers <DNSserverAddress1>, <DNSserverAddress2>
    ```

2.  Pour créer une approbation de forêt à sens unique, exécutez la commande suivante dans une invite de commandes avec élévation de privilèges :

    Remplacez `bastion.local` avec le nom du domaine SGH et `fabrikam.com` avec le nom du domaine de fabric. Fournir le mot de passe pour un administrateur du domaine de fabric.

        netdom trust bastion.local /domain:fabrikam.com /userD:fabrikam.com\Administrator /passwordD:<password> /add

## <a name="next-step"></a>Étape suivante 

>[!div class="nextstepaction"]
[Configurer le protocole HTTPS](guarded-fabric-configure-hgs-https.md)
