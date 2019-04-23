---
title: Initialiser le cluster SGH à l’aide du mode de module de plateforme sécurisée dans une nouvelle forêt dédiée (par défaut)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 6d4232b0b248aba8b64f31ac28db1480c14db53d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863870"
---
# <a name="initialize-the-hgs-cluster-using-tpm-mode-in-a-new-dedicated-forest-default"></a>Initialiser le cluster SGH à l’aide du mode de module de plateforme sécurisée dans une nouvelle forêt dédiée (par défaut)

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  Exécutez [Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx) dans une fenêtre PowerShell avec élévation de privilèges sur le premier nœud SGH. La syntaxe de cette applet de commande prend en charge le nombre d’entrées différents, mais les appels courants 2 sont ci-dessous :

    -   Si vous utilisez des fichiers PFX pour vos certificats de signature et le chiffrement, exécutez les commandes suivantes :

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustTpm
        ```

    -   Si vous utilisez des certificats non exportables qui sont installés dans le magasin de certificats local, exécutez la commande suivante. Si vous ne connaissez pas les empreintes numériques de vos certificats, vous pouvez répertorier les certificats disponibles en exécutant `Get-ChildItem Cert:\LocalMachine\My`.

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' -TrustTpm
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]

## <a name="next-step"></a>Étape suivante

>[!div class="nextstepaction"]
[Installer les certificats racine de module de plateforme sécurisée](guarded-fabric-install-trusted-tpm-root-certificates.md)
  