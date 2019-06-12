---
title: Initialiser le cluster SGH à l’aide du mode de module de plateforme sécurisée dans une forêt bastion
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 458642eedbfdc94ef0f3d6f6fe08ed4ead475ab0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447429"
---
# <a name="initialize-the-hgs-cluster-using-tpm-mode-in-an-existing-bastion-forest"></a>Initialiser le cluster SGH à l’aide du mode de module de plateforme sécurisée dans une forêt bastion existante

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Les Services de domaine Active Directory sera installés sur l’ordinateur, mais doivent rester non configurés.

[!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

Avant de continuer, vérifiez que vous avez préparé vos objets de cluster pour le Service Guardian hôte et accordées connecté dans utilisateur **contrôle total** sur les objets VCO et CNO dans Active Directory.
Le nom d’objet ordinateur virtuel doit être transmis à la `-HgsServiceName` et le nom de cluster à le `-ClusterName` paramètre.

> [!TIP]
> Vérifiez vos contrôleurs de domaine Active Directory pour vous assurer de vos objets de cluster ont répliquées sur tous les contrôleurs de domaine avant de continuer.

Si vous utilisez des certificats PFX, exécutez les commandes suivantes sur le serveur SGH :

```powershell
$signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
$encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

Install-ADServiceAccount -Identity 'HGSgMSA'

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustTpm
```

Si vous utilisez des certificats installés sur l’ordinateur local (par exemple, reposant sur les HSM les certificats et les certificats non exportables), utilisez le `-SigningCertificateThumbprint` et `-EncryptionCertificateThumbprint` paramètres à la place.

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Installer des certificats racine TPM](guarded-fabric-install-trusted-tpm-root-certificates.md)