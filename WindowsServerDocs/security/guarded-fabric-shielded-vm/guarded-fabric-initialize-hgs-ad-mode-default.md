---
title: Initialiser le cluster SGH à l’aide du mode AD dans une nouvelle forêt dédiée (par défaut)
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 4dd10efecf391f7087962e514db7a59135bd93e8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403644"
---
# <a name="initialize-the-hgs-cluster-using-ad-mode-in-a-new-dedicated-forest-default"></a>Initialiser le cluster SGH à l’aide du mode AD dans une nouvelle forêt dédiée (par défaut)

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

>[!IMPORTANT]
>L’attestation approuvée par l’administrateur (mode AD) est déconseillée à compter de Windows Server 2019. Pour les environnements où l’attestation de module de plateforme sécurisée n’est pas possible, configurez l' [attestation de clé hôte](guarded-fabric-initialize-hgs-key-mode-default.md). L’attestation de clé hôte offre une garantie similaire au mode AD et est plus simple à configurer. 

1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)] 
2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  Exécutez [Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx) dans une fenêtre PowerShell avec élévation de privilèges sur le premier nœud SGH. La syntaxe de cette applet de commande prend en charge de nombreuses entrées différentes, mais les deux appels les plus courants sont les suivants :

    -   Si vous utilisez des fichiers PFX pour vos certificats de signature et de chiffrement, exécutez les commandes suivantes :

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

> [!div class="nextstepaction"]
> [Configurer une structure DNS](guarded-fabric-configuring-fabric-dns-ad.md)