---
title: Initialiser le cluster SGH à l’aide du mode clé dans une nouvelle forêt dédiée (par défaut)
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: feae5230eec6b3f3c0471f75199970a47016a583
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856662"
---
# <a name="initialize-the-hgs-cluster-using-key-mode-in-a-new-dedicated-forest-default"></a>Initialiser le cluster SGH à l’aide du mode clé dans une nouvelle forêt dédiée (par défaut)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016


1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)] 
2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  Exécutez [Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx) dans une fenêtre PowerShell avec élévation de privilèges sur le premier nœud SGH. La syntaxe de cette applet de commande prend en charge de nombreuses entrées différentes, mais les deux appels les plus courants sont les suivants :

    -   Si vous utilisez des fichiers PFX pour vos certificats de signature et de chiffrement, exécutez les commandes suivantes :

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustHostkey
        ```

    -   Si vous utilisez des certificats non exportables qui sont installés dans le magasin de certificats local, exécutez la commande suivante. Si vous ne connaissez pas les empreintes numériques de vos certificats, vous pouvez répertorier les certificats disponibles en exécutant `Get-ChildItem Cert:\LocalMachine\My`.

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' --TrustHostKey
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]  

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]


## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Créer une clé d’hôte](guarded-fabric-create-host-key.md)