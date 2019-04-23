---
title: Initialiser le cluster SGH à l’aide du mode AD dans une forêt bastion
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 98003745823cf780a38487dff997798ebef12fc9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870090"
---
# <a name="initialize-the-hgs-cluster-using-ad-mode-in-an-existing-bastion-forest"></a>Initialiser le cluster SGH à l’aide du mode AD dans une forêt bastion existante

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016


>[!IMPORTANT]
>Attestation approuvée par l’administrateur (mode AD) est déconseillée à compter de Windows Server 2019. Pour les environnements où l’attestation TPM n’est pas possible, configurez [héberger l’attestation de clé](guarded-fabric-initialize-hgs-key-mode-bastion.md). L’attestation de clé hôte fournit la garantie similaire au mode d’AD et est plus simple à configurer. 

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

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -ClusterName 'HgsCluster' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustActiveDirectory
```

Si vous utilisez des certificats installés sur l’ordinateur local (par exemple, reposant sur les HSM les certificats et les certificats non exportables), utilisez le `-SigningCertificateThumbprint` et `-EncryptionCertificateThumbprint` paramètres à la place.

## <a name="next-step"></a>Étape suivante

>[!div class="nextstepaction"]
[Configuration de l’infrastructure DNS](guarded-fabric-configuring-fabric-dns-ad.md)

