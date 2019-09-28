---
title: Initialiser le cluster SGH à l’aide du mode clé dans une forêt bastion
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: e72de1c85e0a9c3decf1fd3b5085363b57ca387c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403640"
---
# <a name="initialize-the-hgs-cluster-using-key-mode-in-an-existing-bastion-forest"></a>Initialiser le cluster SGH à l’aide du mode clé dans une forêt bastion existante

> S’applique à : Windows Server 2019
> 
> [!div class="step-by-step"]
> [« Installer SGH dans une nouvelle forêt](guarded-fabric-install-hgs-in-a-bastion-forest.md)
> [créer une clé d’hôte »](guarded-fabric-create-host-key.md)

Active Directory Domain Services sera installé sur l’ordinateur, mais il doit rester non configuré.

[!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)] 

Avant de continuer, assurez-vous que vous avez prédéfini vos objets de cluster pour le service Guardian hôte et accordé à l’utilisateur connecté le **contrôle total** sur les objets VCO et CNO dans Active Directory.
Le nom d’objet de l’ordinateur virtuel doit être passé au paramètre `-HgsServiceName`, et le nom du cluster au paramètre `-ClusterName`.

> [!TIP]
> Vérifiez vos contrôleurs de domaine Active Directory pour vous assurer que vos objets de cluster ont été répliqués sur tous les contrôleurs de domaine avant de continuer.

Si vous utilisez des certificats PFX, exécutez les commandes suivantes sur le serveur SGH :

```powershell
$signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
$encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

Install-ADServiceAccount -Identity 'HGSgMSA'

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -ClusterName 'HgsCluster' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustHostKey
```

Si vous utilisez des certificats installés sur l’ordinateur local (tels que des certificats sauvegardés par HSM et des certificats non exportables), utilisez les paramètres `-SigningCertificateThumbprint` et `-EncryptionCertificateThumbprint` à la place.

