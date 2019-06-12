---
title: Ajouter des informations sur l’hôte pour l’attestation approuvée par le module de plateforme sécurisée
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: f0aa575b-b34e-4f6c-8416-ed3e398e0ad2
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 3647c9708ad68dec0ac13c85fced2b12150ccf60
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447188"
---
>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

### <a name="add-host-information-for-tpm-trusted-attestation"></a>Ajouter des informations sur l’hôte pour l’attestation approuvée par le module de plateforme sécurisée

Pour le mode de module de plateforme sécurisée, l’administrateur de structure capture les trois types d’informations sur l’hôte, chacun des qui doit être ajouté à la configuration de SGH :

- Un identificateur de module de plateforme sécurisée (EKpub) pour chaque hôte Hyper-V
- Stratégies d’intégrité du code, une liste verte de binaires autorisées pour les hôtes Hyper-V
- Une ligne de base du module de plateforme sécurisée (démarrage des mesures) qui représente un ensemble d’Hyper-V héberge qui s’exécutent sur la même classe de matériel

Une fois l’infrastructure administrateur capture les informations, ajoutez-le à la configuration de SGH, comme décrit dans la procédure suivante.

1.  Obtenir les fichiers XML qui contiennent les informations EKpub puis les copier sur un serveur SGH. Il y a un fichier XML par hôte. Ensuite, dans une console Windows PowerShell avec élévation de privilèges sur un serveur SGH, exécutez la commande ci-dessous. Répétez la commande pour chacun des fichiers XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > Si vous rencontrez une erreur lors de l’ajout d’un identificateur de module de plateforme sécurisée concernant un certificat de clé Endorsement non approuvé (EKCert), vérifiez que le [approuvé du certificat racine de module de plateforme sécurisée a été ajouté](guarded-fabric-install-trusted-tpm-root-certificates.md) au nœud SGH.
    > En outre, certains fournisseurs de module de plateforme sécurisée n’utilisent pas EKCerts.
    > Vous pouvez vérifier si un EKCert est manquant, en ouvrant le fichier XML dans un éditeur tel que le bloc-notes et la vérification d’un message d’erreur n’indiquant aucune EKCert a été trouvée.
    > Si c’est le cas, et que vous faites confiance que le TPM sur votre ordinateur est authentique, vous pouvez utiliser le `-Force` indicateur pour remplacer cette vérification de sécurité et d’ajouter l’identificateur de l’hôte à SGH.

2. Obtenir la stratégie d’intégrité de code créé par l’administrateur d’infrastructure pour les ordinateurs hôtes, au format binaire (*.p7b). Copiez-le sur un serveur SGH. Puis exécutez la commande suivante.

    Pour `<PolicyName>`, spécifiez un nom pour la stratégie d’élément de configuration qui décrit le type d’hôte, il s’applique à. Une bonne pratique consiste à nommer après la marque/modèle de votre machine et toute configuration du logiciel spécial n’est en cours d’exécution sur ce dernier.<br>Pour `<Path>`, spécifiez le chemin d’accès et le nom de la stratégie d’intégrité du code.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

3. Obtenir le fichier de TCGlog que l’administrateur de structure capturée à partir d’un hôte de référence. Copiez le fichier à un serveur SGH. Puis exécutez la commande suivante. En règle générale, nommer la stratégie après la classe de matériel qu’il représente (par exemple, « Fabricant modèle révision »).

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

Cette étape termine le processus de configuration d’un cluster de SGH pour le mode de module de plateforme sécurisée. L’administrateur de structure peut-être vous permettent de fournir les deux URL à partir de SGH avant que la configuration puisse être terminée pour les ordinateurs hôtes. Pour obtenir ces URL, sur un serveur SGH, exécutez [Get-HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/get-hgsserver?view=win10-ps).

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Confirmer l’attestation](guarded-fabric-confirm-hosts-can-attest-successfully.md)