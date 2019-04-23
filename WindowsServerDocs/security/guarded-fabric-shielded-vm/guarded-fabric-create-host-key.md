---
title: Créer une clé d’hôte et ajoutez-le à SGH
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 82a561d51e9a396dcdc86ee3e37a8d7ffb8efd6e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870780"
---
# <a name="create-a-host-key-and-add-it-to-hgs"></a>Créer une clé d’hôte et ajoutez-le à SGH

>S’applique à : Windows Server 2019


Cette rubrique explique comment préparer des ordinateurs hôtes Hyper-V pour devenir des hôtes service Guardian à l’aide de l’attestation de clé hôte (mode de clé). Vous allez créer une paire de clés d’hôte (ou utiliser un certificat existant) et ajouter la moitié de la clé publique à SGH.

## <a name="create-a-host-key"></a>Créer une clé d’hôte

1.  Installez Windows Server 2019 sur votre ordinateur hôte de Hyper-V.
2.  Installez les composants Hyper-V et de Support de Hyper-V Guardian hôte :

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ``` 

3.  Générer automatiquement une clé d’hôte, ou sélectionner un certificat existant. Si vous utilisez un certificat personnalisé, il doit avoir au moins une clé RSA 2048 bits, EKU de l’authentification de Client et l’utilisation de la clé de Signature numérique.

    ```powershell
    Set-HgsClientHostKey
    ```

    Vous pouvez également spécifier une empreinte numérique si vous souhaitez utiliser votre propre certificat. 
    Cela peut être utile si vous souhaitez partager un certificat sur plusieurs ordinateurs, ou utiliser un certificat lié à un module de plateforme sécurisée ou d’un module HSM. Voici un exemple de création d’un certificat lié à du module de plateforme sécurisée (ce qui empêche de générer la clé privée volé et utilisé sur un autre ordinateur et nécessite un TPM 1.2) :

    ```powershell
    $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
    Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
    ```

4.  Obtenir la moitié publique de la clé pour fournir au serveur SGH. Vous pouvez utiliser l’applet de commande suivant ou, si vous disposez du certificat stocké ailleurs, fournir un fichier .cer contenant le public la moitié de la clé. Notez que nous utilisons uniquement stocker et de validation de la clé publique sur SGH ; Nous ne conservons aucune des informations de certificat ni nous valident la date de chaîne ou d’expiration du certificat.

    ```powershell
    Get-HgsClientHostKey -Path "C:\temp\$env:hostname-HostKey.cer"
    ```

5.  Copiez le fichier .cer à votre serveur SGH.

## <a name="add-the-host-key-to-the-attestation-service"></a>Ajouter la clé d’hôte pour le service d’attestation

Cette étape est effectuée sur le serveur SGH et permet à l’hôte exécuter des machines virtuelles protégées. Il est recommandé que vous définissez le nom sur le nom de domaine complet ou identificateur de ressource de l’ordinateur hôte, vous pouvez facilement faire référence hôte sur lequel la clé est installé sur.

```powershell
Add-HgsAttestationHostKey -Name MyHost01 -Path "C:\temp\MyHost01-HostKey.cer"
``` 

## <a name="next-step"></a>Étape suivante

>[!div class="nextstepaction"]
[Confirmer les ordinateurs hôtes peuvent correctement attester](guarded-fabric-confirm-hosts-can-attest-successfully.md)

## <a name="see-also"></a>Voir aussi

- [Déploiement du Service Guardian hôte pour les hôtes service Guardian et des machines virtuelles protégées](guarded-fabric-deploying-hgs-overview.md)
