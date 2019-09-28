---
title: Créer une clé d’hôte et l’ajouter à SGH
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2aea6c8416a0f3af04ad6056c5d09a4d07708eaa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386651"
---
# <a name="create-a-host-key-and-add-it-to-hgs"></a>Créer une clé d’hôte et l’ajouter à SGH

>S’applique à : Windows Server 2019


Cette rubrique explique comment préparer des hôtes Hyper-V pour qu’ils deviennent des hôtes protégés à l’aide de l’attestation de clé hôte (mode clé). Vous allez créer une paire de clés d’hôte (ou utiliser un certificat existant) et ajouter la moitié publique de la clé à SGH.

## <a name="create-a-host-key"></a>Créer une clé hôte

1.  Installez Windows Server 2019 sur votre ordinateur hôte Hyper-V.
2.  Installez les fonctionnalités de prise en charge d’Hyper-V et de Guardian hôte pour Hyper-V :

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ``` 

3.  Générez automatiquement une clé d’hôte ou sélectionnez un certificat existant. Si vous utilisez un certificat personnalisé, il doit avoir au moins une clé RSA 2048 bits, une utilisation améliorée de la clé d’authentification client et une clé de signature numérique.

    ```powershell
    Set-HgsClientHostKey
    ```

    Vous pouvez également spécifier une empreinte numérique si vous souhaitez utiliser votre propre certificat. 
    Cela peut être utile si vous souhaitez partager un certificat sur plusieurs ordinateurs ou utiliser un certificat lié à un module de plateforme sécurisée (TPM) ou à un HSM. Voici un exemple de création d’un certificat lié au module de plateforme sécurisée (TPM) (ce qui empêche le vol et l’utilisation de la clé privée sur un autre ordinateur et requiert uniquement un TPM 1,2) :

    ```powershell
    $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
    Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
    ```

4.  Obtenir la partie publique de la clé à fournir au serveur SGH. Vous pouvez utiliser l’applet de commande suivante ou, si vous avez le certificat stocké ailleurs, fournissez un. cer contenant la moitié publique de la clé. Notez que nous enregistrons et validons uniquement la clé publique sur SGH ; Nous ne conservons pas les informations de certificat et nous validons la chaîne de certificats ou la date d’expiration.

    ```powershell
    Get-HgsClientHostKey -Path "C:\temp\$env:hostname-HostKey.cer"
    ```

5.  Copiez le fichier. cer sur votre serveur SGH.

## <a name="add-the-host-key-to-the-attestation-service"></a>Ajouter la clé d’hôte au service d’attestation

Cette étape est effectuée sur le serveur SGH et permet à l’hôte d’exécuter des machines virtuelles protégées. Il est recommandé de définir le nom sur le nom de domaine complet ou l’identificateur de ressource de l’ordinateur hôte. vous pouvez donc facilement vous référer à l’hôte sur lequel la clé est installée.

```powershell
Add-HgsAttestationHostKey -Name MyHost01 -Path "C:\temp\MyHost01-HostKey.cer"
``` 

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Confirmer que les hôtes peuvent attester correctement](guarded-fabric-confirm-hosts-can-attest-successfully.md)

## <a name="see-also"></a>Voir aussi

- [Déploiement du service Guardian hôte pour les hôtes service Guardian et les machines virtuelles protégées](guarded-fabric-deploying-hgs-overview.md)
