---
title: Ajouter des informations d’ordinateur hôte pour l’attestation approuvée par le module de plateforme sécurisée
ms.prod: windows-server
ms.topic: article
ms.assetid: f0aa575b-b34e-4f6c-8416-ed3e398e0ad2
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 06/21/2019
ms.openlocfilehash: f9a0ee9cb78a89b20140e40a2bd3ae42da56c84f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856932"
---
>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

# <a name="add-host-information-for-tpm-trusted-attestation"></a>Ajouter des informations d’ordinateur hôte pour l’attestation approuvée par le module de plateforme sécurisée

Pour le mode TPM, l’administrateur de l’infrastructure capture trois types d’informations de l’hôte, chacun d’entre eux devant être ajouté à la configuration SGH :

- Un identificateur de module de plateforme sécurisée (EKpub) pour chaque hôte Hyper-V
- Stratégies d’intégrité du code, liste blanche de fichiers binaires autorisés pour les hôtes Hyper-V
- Une ligne de base TPM (mesures de démarrage) qui représente un ensemble d’ordinateurs hôtes Hyper-V qui s’exécutent sur la même classe de matériel
    
AF er l’administrateur de l’infrastructure capture les informations, les ajoute à la configuration SGH, comme décrit dans la procédure suivante.

1. Obtenez les fichiers XML qui contiennent les informations EKpub et copiez-les sur un serveur SGH. Il y aura un fichier XML par hôte. Ensuite, dans une console Windows PowerShell avec élévation de privilèges sur un serveur SGH, exécutez la commande ci-dessous. Répétez la commande pour chacun des fichiers XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
       ```

    > [!NOTE]
    > If you encounter an error when adding a TPM identifier regarding an untrusted Endorsement Key Certificate (EKCert), ensure that the [trusted TPM root certificates have been added](guarded-fabric-install-trusted-tpm-root-certificates.md) to the HGS node.
    > Additionally, some TPM vendors do not use EKCerts.
    > You can check if an EKCert is missing by opening the XML file in an editor such as Notepad and checking for an error message indicating no EKCert was found.
    > If this is the case, and you trust that the TPM in your machine is authentic, you can use the `-Force` flag to override this safety check and add the host identifier to HGS.

2. Obtain the code integrity policy that the fabric administrator created for the hosts, in binary format (\*.p7b). Copy it to an HGS server. Then run the following command.

    For `<PolicyName>`, specify a name for the CI polic" that describes the type of host it appl"es to. A be"t practice is to name it after the"make/model of your machine and any special software configuration running on it.<br>For `<Path>`, specify the path and filename of the code integrity policy.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
       ```
    
    > [!NOTE]
    > If you're using a signed code integrity policy, register an unsigned copy of the same policy with HGS.
    > The signature on code integrity policies is used to control updates to the policy, but is not measured into the host TPM and therefore cannot be attested to by HGS.

3.    Obtain the TCGlog file that the fabric administrator captured from a reference host. Copy the file to an HGS server. Then run the following command. Typically, you will name the policy after the class of hardware it represents (for example, "Manufacturer Model Revision").

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

Cela termine le processus de configuration d’un cluster SGH en mode TPM. L’administrateur de l’infrastructure peut avoir besoin de fournir deux URL de SGH pour que la configuration puisse être effectuée pour les hôtes. Pour obtenir ces URL, sur un serveur SGH, exécutez la fonction [obtenir-HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/get-hgsserver?view=win10-ps).

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Confirmer l’attestation](guarded-fabric-confirm-hosts-can-attest-successfully.md)
