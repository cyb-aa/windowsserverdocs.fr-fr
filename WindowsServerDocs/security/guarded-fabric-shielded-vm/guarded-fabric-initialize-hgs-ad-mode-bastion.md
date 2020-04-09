---
title: Initialiser le cluster SGH à l’aide du mode AD dans une forêt bastion
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 75520c7afe1d3b274e643ab63bbf53159c0e2531
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856692"
---
# <a name="initialize-the-hgs-cluster-using-ad-mode-in-an-existing-bastion-forest"></a>Initialiser le cluster SGH à l’aide du mode AD dans une forêt bastion existante

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016


>[!IMPORTANT]
>L’attestation approuvée par l’administrateur (mode AD) est déconseillée à compter de Windows Server 2019. Pour les environnements où l’attestation de module de plateforme sécurisée n’est pas possible, configurez l' [attestation de clé hôte](guarded-fabric-initialize-hgs-key-mode-bastion.md). L’attestation de clé hôte offre une garantie similaire au mode AD et est plus simple à configurer. 

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

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -ClusterName 'HgsCluster' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustActiveDirectory
```

Si vous utilisez des certificats installés sur l’ordinateur local (tels que des certificats sauvegardés par HSM et des certificats non exportables), utilisez plutôt les paramètres `-SigningCertificateThumbprint` et `-EncryptionCertificateThumbprint`.

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Configurer une structure DNS](guarded-fabric-configuring-fabric-dns-ad.md)

