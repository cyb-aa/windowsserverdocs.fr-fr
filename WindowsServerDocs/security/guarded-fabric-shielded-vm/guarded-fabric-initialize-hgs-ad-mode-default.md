---
title: Initialiser le cluster SGH à l’aide du mode AD dans une nouvelle forêt dédiée (par défaut)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 4ef70ab9ce8150a9a3ed774d6df407da097880e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865670"
---
# <a name="initialize-the-hgs-cluster-using-ad-mode-in-a-new-dedicated-forest-default"></a>Initialiser le cluster SGH à l’aide du mode AD dans une nouvelle forêt dédiée (par défaut)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

>[!IMPORTANT]
>Attestation approuvée par l’administrateur (mode AD) est déconseillée à compter de Windows Server 2019. Pour les environnements où l’attestation TPM n’est pas possible, configurez [héberger l’attestation de clé](guarded-fabric-initialize-hgs-key-mode-default.md). L’attestation de clé hôte fournit la garantie similaire au mode d’AD et est plus simple à configurer. 

1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)] 
2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  Exécutez [Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx) dans une fenêtre PowerShell avec élévation de privilèges sur le premier nœud SGH. La syntaxe de cette applet de commande prend en charge le nombre d’entrées différents, mais les appels courants 2 sont ci-dessous :

    -   Si vous utilisez des fichiers PFX pour vos certificats de signature et le chiffrement, exécutez les commandes suivantes :

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustActiveDirectory
        ```

    -   Si vous utilisez des certificats non exportables qui sont installés dans le magasin de certificats local, exécutez la commande suivante. Si vous ne connaissez pas les empreintes numériques de vos certificats, vous pouvez répertorier les certificats disponibles en exécutant `Get-ChildItem Cert:\LocalMachine\My`.

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' --TrustActiveDirectory
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]  

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]

## <a name="next-step"></a>Étape suivante

>[!div class="nextstepaction"]
[Configuration de l’infrastructure DNS](guarded-fabric-configuring-fabric-dns-ad.md)