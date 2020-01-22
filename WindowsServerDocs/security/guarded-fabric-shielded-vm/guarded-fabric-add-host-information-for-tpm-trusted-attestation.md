---
title: Ajouter des informations d’ordinateur hôte pour l’attestation approuvée par le module de plateforme sécurisée
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: f0aa575b-b34e-4f6c-8416-ed3e398e0ad2
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 06/21/2019
ms.openlocfilehash: 923bc2c46f37cf7e631a744c9eae85c3dd7506dd
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949814"
---
>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

# <a name="add-host-information-for-tpm-trusted-attestation"></a>Ajouter des informations d’ordinateur hôte pour l’attestation approuvée par le module de plateforme sécurisée

Pour le mode TPM, l’administrateur de l’infrastructure capture trois types d’informations de l’hôte, chacun d’entre eux devant être ajouté à la configuration SGH :

- Un identificateur de module de plateforme sécurisée (EKpub) pour chaque hôte Hyper-V
- Stratégies d’intégrité du code, liste verte de fichiers binaires autorisés pour les hôtes Hyper-V
- Une ligne de base TPM (mesures de démarrage) qui représente un ensemble d’ordinateurs hôtes Hyper-V qui s’exécutent sur la même classe de matériel

Une fois que l’administrateur de l’infrastructure a capturé les informations, ajoutez-les à la configuration SGH comme décrit dans la procédure suivante.

1. Obtenez les fichiers XML qui contiennent les informations EKpub et copiez-les sur un serveur SGH. Il y aura un fichier XML par hôte. Ensuite, dans une console Windows PowerShell avec élévation de privilèges sur un serveur SGH, exécutez la commande ci-dessous. Répétez la commande pour chacun des fichiers XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > Si vous rencontrez une erreur lors de l’ajout d’un identificateur TPM concernant un certificat de clé de type EK non approuvé (EKCert), assurez-vous que les [certificats racines TPM approuvés ont été ajoutés](guarded-fabric-install-trusted-tpm-root-certificates.md) au nœud SGH.
    > En outre, certains fournisseurs de module de plateforme sécurisée n’utilisent pas EKCerts.
    > Vous pouvez vérifier si un EKCert est manquant en ouvrant le fichier XML dans un éditeur tel que le bloc-notes et en recherchant un message d’erreur indiquant qu’aucun EKCert n’a été trouvé.
    > Si c’est le cas et que vous faites confiance à l’authenticité du module TPM de votre ordinateur, vous pouvez utiliser l’indicateur `-Force` pour remplacer ce contrôle de sécurité et ajouter l’identificateur d’hôte à SGH.

2. Obtenez la stratégie d’intégrité du code que l’administrateur de l’infrastructure a créée pour les ordinateurs hôtes, au format binaire (\*. p7b). Copiez-le sur un serveur SGH. Exécutez ensuite la commande suivante.

    Pour `<PolicyName>`, spécifiez un nom pour la stratégie CI qui décrit le type d’hôte auquel elle s’applique. Une bonne pratique consiste à la nommer après la marque/le modèle de votre machine et toute configuration logicielle spéciale exécutée sur celle-ci.<br>Pour `<Path>`, spécifiez le chemin d’accès et le nom de fichier de la stratégie d’intégrité du code.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```
    
    > [!NOTE]
    > Si vous utilisez une stratégie d’intégrité du code signée, inscrivez une copie non signée de la même stratégie avec SGH.
    > La signature sur les stratégies d’intégrité du code est utilisée pour contrôler les mises à jour de la stratégie, mais n’est pas mesurée dans le module de plateforme sécurisée de l’hôte et ne peut donc pas être sanctionnée par SGH.

3. Obtenez le fichier TCGlog que l’administrateur de l’infrastructure a capturé à partir d’un hôte de référence. Copiez le fichier sur un serveur SGH. Exécutez ensuite la commande suivante. En règle générale, vous nommez la stratégie après la classe de matériel qu’elle représente (par exemple, « fabricant modèle révision »).

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

Cela termine le processus de configuration d’un cluster SGH en mode TPM. L’administrateur de l’infrastructure peut avoir besoin de fournir deux URL de SGH pour que la configuration puisse être effectuée pour les hôtes. Pour obtenir ces URL, sur un serveur SGH, exécutez la fonction [obtenir-HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/get-hgsserver?view=win10-ps).

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Confirmer l’attestation](guarded-fabric-confirm-hosts-can-attest-successfully.md)
