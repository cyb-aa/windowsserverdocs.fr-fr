---
title: Capturer les informations du mode TPM requises par SGH
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 915b1338-5085-481b-8904-75d29e609e93
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 04/01/2019
ms.openlocfilehash: ba67cb80a405cd1c6d368a52798e3dec4d9861cb
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371364"
---
# <a name="authorize-guarded-hosts-using-tpm-based-attestation"></a>Autoriser des hôtes service Guardian à l’aide d’une attestation TPM

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Le mode TPM utilise un identificateur de module de plateforme sécurisée (également appelé identificateur de plateforme ou clé de type EK \[EKpub\]) pour commencer à déterminer si un hôte particulier est autorisé comme « protégé ». Ce mode d’attestation utilise les mesures de démarrage sécurisé et d’intégrité du code pour s’assurer qu’un ordinateur hôte Hyper-V donné se trouve dans un état sain et exécute uniquement du code de confiance. Pour que l’attestation comprenne ce qui est et n’est pas intègre, vous devez capturer les artefacts suivants :

1.  Identificateur du module de plateforme sécurisée (EKpub)

    -  Ces informations sont uniques pour chaque hôte Hyper-V

2.  Ligne de base du module de plateforme sécurisée (mesures de démarrage)

    -  Cela s’applique à tous les ordinateurs hôtes Hyper-V qui s’exécutent sur la même classe de matériel

3.  Stratégie d’intégrité du code (une liste verte de fichiers binaires autorisés)

    -  Cela s’applique à tous les ordinateurs hôtes Hyper-V qui partagent du matériel et des logiciels communs.

Nous vous recommandons de capturer la stratégie de référence et l’élément de configuration à partir d’un « hôte de référence » représentatif de chaque classe unique de configuration matérielle Hyper-V au sein de votre centre de ressources. À compter de Windows Server version 1709, les exemples de stratégies CI sont inclus dans C:\Windows\schemas\CodeIntegrity\ExamplePolicies. 

## <a name="versioned-attestation-policies"></a>Stratégies d’attestation avec version

Windows Server 2019 introduit une nouvelle méthode d’attestation, appelée *attestation v2*, où un certificat TPM doit être présent pour ajouter EKPUB à SGH. La méthode d’attestation v1 utilisée dans Windows Server 2016 vous permettait de remplacer cette vérification de sécurité en spécifiant l’indicateur-force lorsque vous exécutez Add-HgsAttestationTpmHost ou d’autres applets de commande d’attestation TPM pour capturer les artefacts. À compter de Windows Server 2019, l’attestation v2 est utilisée par défaut et vous devez spécifier l’indicateur-PolicyVersion v1 quand vous exécutez Add-HgsAttestationTpmHost si vous devez inscrire un module de plateforme sécurisée sans certificat. L’indicateur-force ne fonctionne pas avec l’attestation v2. 

Un hôte peut uniquement attester si tous les artefacts (EKPub + TPM de référence + CI) utilisent la même version de l’attestation. L’attestation v2 est tentée en premier et, en cas d’échec, l’attestation v1 est utilisée. Cela signifie que si vous devez inscrire un identificateur de module de plateforme sécurisée à l’aide de l’attestation v1, vous devez également spécifier l’indicateur-PolicyVersion v1 pour utiliser l’attestation v1 lorsque vous capturez la ligne de base du module de plateforme sécurisée et créez la stratégie CI. Si la ligne de base du module de plateforme sécurisée et la stratégie CI ont été créées à l’aide de l’attestation v2, puis que vous devez ajouter un hôte service Guardian sans certificat TPM, vous devez recréer chaque artefact avec l’indicateur-PolicyVersion v1. 

## <a name="capture-the-tpm-identifier-platform-identifier-or-ekpub-for-each-host"></a>Capturer l’identificateur du module de plateforme sécurisée (identificateur de plateforme ou EKpub) pour chaque hôte

1.  Dans le domaine de l’infrastructure, assurez-vous que le module de plateforme sécurisée sur chaque ordinateur hôte est prêt à être utilisé, c’est-à-dire que le module de plateforme sécurisée est initialisé et que la propriété a été obtenue. Vous pouvez vérifier l’état du module de plateforme sécurisée en ouvrant la console de gestion du module de plateforme sécurisée (TPM. msc) ou **en exécutant le module de** plateforme sécurisée (TPM) dans une fenêtre Windows PowerShell avec élévation de privilèges. Si votre module de plateforme sécurisée n’est pas à l’état **prêt** , vous devez l’initialiser et définir sa propriété. Vous pouvez effectuer cette opération dans la console de gestion du module de plateforme sécurisée ou en exécutant **Initialize-TPM**.

2.  Sur chaque hôte service Guardian, exécutez la commande suivante dans une console Windows PowerShell avec élévation de privilèges pour obtenir son EKpub. Par `<HostName>`, remplacez le nom d’hôte unique par un nom approprié pour identifier cet ordinateur hôte. il peut s’agir de son nom d’hôte ou du nom utilisé par un service d’inventaire de la structure (s’il est disponible). Pour plus de commodité, nommez le fichier de sortie à l’aide du nom de l’hôte.

    ```powershell
    (Get-PlatformIdentifier -Name '<HostName>').InnerXml | Out-file <Path><HostName>.xml -Encoding UTF8
    ```
3.  Répétez les étapes précédentes pour chaque ordinateur hôte qui deviendra un hôte service Guardian, en veillant à attribuer à chaque fichier XML un nom unique.

4.  Fournissez les fichiers XML obtenus à l’administrateur SGH.

5.  Dans le domaine SGH, ouvrez une console Windows PowerShell avec élévation de privilèges sur un serveur SGH et exécutez la commande suivante. Répétez la commande pour chacun des fichiers XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > Si vous rencontrez une erreur lors de l’ajout d’un identificateur TPM concernant un certificat de clé de type EK non approuvé (EKCert), assurez-vous que les [certificats racines TPM approuvés ont été ajoutés](guarded-fabric-install-trusted-tpm-root-certificates.md) au nœud SGH.
    > En outre, certains fournisseurs de module de plateforme sécurisée n’utilisent pas EKCerts.
    > Vous pouvez vérifier si un EKCert est manquant en ouvrant le fichier XML dans un éditeur tel que le bloc-notes et en recherchant un message d’erreur indiquant qu’aucun EKCert n’a été trouvé.
    > Si c’est le cas et que vous faites confiance à l’authenticité du module de plateforme sécurisée de votre ordinateur, vous pouvez utiliser le paramètre `-Force` pour ajouter l’identificateur d’hôte à SGH. Dans Windows Server 2019, vous devez également utiliser le paramètre `-PolicyVersion v1` lors de l’utilisation de `-Force`. Cela crée une stratégie cohérente avec le comportement de Windows Server 2016 et vous oblige à utiliser `-PolicyVersion v1` lors de l’enregistrement de la stratégie CI et de la ligne de base du module de plateforme sécurisée.

## <a name="create-and-apply-a-code-integrity-policy"></a>Créer et appliquer une stratégie d’intégrité du code

Une stratégie d’intégrité du code permet de s’assurer que seuls les exécutables que vous approuvez pour s’exécuter sur un hôte sont autorisés à s’exécuter. Les programmes malveillants et autres exécutables en dehors des exécutables approuvés ne peuvent pas s’exécuter.

Une stratégie d’intégrité du code doit être appliquée à chaque hôte service Guardian afin d’exécuter des machines virtuelles protégées en mode TPM. Vous spécifiez les stratégies d’intégrité de code exactes que vous approuvez en les ajoutant à SGH. Les stratégies d’intégrité du code peuvent être configurées pour appliquer la stratégie, bloquant tout logiciel non conforme à la stratégie ou simplement effectuer un audit (consigner un événement lorsque le logiciel non défini dans la stratégie est exécuté). 

À compter de Windows Server version 1709, les exemples de stratégies d’intégrité du code sont inclus avec Windows sur C:\Windows\schemas\CodeIntegrity\ExamplePolicies. Deux stratégies sont recommandées pour Windows Server :

- **AllowMicrosoft**: autorise tous les fichiers signés par Microsoft. Cette stratégie est recommandée pour les applications serveur telles que SQL ou Exchange, ou si le serveur est analysé par les agents publiés par Microsoft.
- **DefaultWindows_Enforced**: autorise uniquement les fichiers livrés dans Windows et n’autorise pas les autres applications publiées par Microsoft, comme Office. Cette stratégie est recommandée pour les serveurs qui exécutent uniquement des rôles serveur intégrés et des fonctionnalités telles que Hyper-V. 

Il est recommandé de créer d’abord la stratégie CI en mode audit (journalisation) pour voir s’il manque quelque chose, puis d’appliquer la stratégie pour les charges de travail de production hôtes. 

Si vous utilisez l’applet de commande [New-CIPolicy](https://docs.microsoft.com/powershell/module/configci/new-cipolicy?view=win10-ps) pour générer votre propre stratégie d’intégrité du code, vous devrez déterminer les niveaux de règle à utiliser. Nous vous recommandons d’utiliser un niveau principal de serveur de **publication** avec le paramètre de **hachage**de secours, ce qui permet la mise à jour de la plupart des logiciels signés numériquement sans modifier la stratégie ci.
Les nouveaux logiciels écrits par le même serveur de publication peuvent également être installés sur le serveur sans modifier la stratégie CI.
Les fichiers exécutables qui ne sont pas signés numériquement seront hachés--les mises à jour de ces fichiers vous obligeront à créer une nouvelle stratégie CI.
Pour plus d’informations sur les niveaux de règle de stratégie CI disponibles, consultez [déployer des stratégies d’intégrité du code : règles de stratégie et règles de fichier](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/select-types-of-rules-to-create#windows-defender-application-control-policy-rules) et aide sur les applets de commande.

1.  Sur l’hôte de référence, générez une nouvelle stratégie d’intégrité du code. Les commandes suivantes créent une stratégie au niveau du serveur de **publication** avec le paramètre de **hachage**de secours. Il convertit ensuite le fichier XML au format de fichier binaire Windows et SGH doit appliquer et mesurer la stratégie CI, respectivement.

    ```powershell
    New-CIPolicy -Level Publisher -Fallback Hash -FilePath 'C:\temp\HW1CodeIntegrity.xml' -UserPEs

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity.p7b'
    ```

    >[!NOTE]
    >La commande ci-dessus crée une stratégie CI en mode audit uniquement. Elle ne bloque pas l’exécution de fichiers binaires non autorisés sur l’ordinateur hôte. Vous ne devez utiliser que des stratégies appliquées en production.

2.  Conservez votre fichier de stratégie d’intégrité du code (fichier XML) où vous pouvez facilement le trouver. Vous devrez modifier ce fichier ultérieurement pour appliquer la stratégie d’intégration continue ou fusionner les modifications apportées aux futures mises à jour du système.

3.  Appliquez la stratégie CI à votre hôte de référence :

    1.  Exécutez la commande suivante pour configurer l’ordinateur afin qu’il utilise votre stratégie CI. Vous pouvez également déployer la stratégie CI avec [stratégie de groupe](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/deploy-windows-defender-application-control-policies-using-group-policy) ou [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/guarded-deploy-host?view=sc-vmm-2019#manage-and-deploy-code-integrity-policies-with-vmm).

        ```powershell
        Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }
        ```

    2.  Redémarrez l’hôte pour appliquer la stratégie.

3.  Testez la stratégie d’intégrité du code en exécutant une charge de travail classique. Cela peut inclure des machines virtuelles en cours d’exécution, des agents de gestion de structure, des agents de sauvegarde ou des outils de dépannage sur l’ordinateur. Vérifiez s’il existe des violations d’intégrité du code et mettez à jour votre stratégie CI si nécessaire.

4.  Modifiez votre stratégie d’élément de configuration en mode appliqué en exécutant les commandes suivantes sur votre fichier XML de stratégie CI mis à jour.

    ```powershell
    Set-RuleOption -FilePath 'C:\temp\HW1CodeIntegrity.xml' -Option 3 -Delete

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity_enforced.p7b'
    ```

5.  Appliquez la stratégie CI à tous vos hôtes (avec une configuration matérielle et logicielle identique) à l’aide des commandes suivantes :

    ```powershell
    Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }
    
    Restart-Computer
    ```

    >[!NOTE]
    >Soyez prudent lorsque vous appliquez des stratégies CI aux hôtes et lorsque vous mettez à jour des logiciels sur ces ordinateurs. Les pilotes en mode noyau qui ne sont pas conformes à la stratégie d’intégration continue peuvent empêcher le démarrage de l’ordinateur. 

6.  Fournissez le fichier binaire (dans cet exemple, HW1CodeIntegrity\_imposés. p7b) à l’administrateur SGH.

7.  Dans le domaine SGH, copiez la stratégie d’intégrité du code sur un serveur SGH et exécutez la commande suivante.

    Pour `<PolicyName>`, spécifiez un nom pour la stratégie CI qui décrit le type d’hôte auquel elle s’applique. Une bonne pratique consiste à la nommer après la marque/le modèle de votre machine et toute configuration logicielle spéciale exécutée sur celle-ci.<br>Pour `<Path>`, spécifiez le chemin d’accès et le nom de fichier de la stratégie d’intégrité du code.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

## <a name="capture-the-tpm-baseline-for-each-unique-class-of-hardware"></a>Capturer la ligne de base du module de plateforme sécurisée pour chaque classe de matériel unique

Une ligne de base du module de plateforme sécurisée est requise pour chaque classe de matériel unique dans votre infrastructure de centre de ressources. Utilisez à nouveau un « hôte de référence ». 

1. Sur l’hôte de référence, assurez-vous que le rôle Hyper-V et la fonctionnalité de prise en charge d’Hyper-V Guardian hôte sont installés.

    >[!WARNING]
    >La fonctionnalité de prise en charge d’Hyper-V Guardian hôte permet une protection basée sur la virtualisation de l’intégrité du code qui peut être incompatible avec certains appareils. Nous vous recommandons vivement de tester cette configuration dans votre laboratoire avant d’activer cette fonctionnalité. Dans le cas contraire, cela risque d’entraîner des échecs inattendus pouvant aller jusqu'à la perte de données ou un écran bleu d’erreur (également appelé erreur d’arrêt).

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```
        
2. Pour capturer la stratégie de base, exécutez la commande suivante dans une console Windows PowerShell avec élévation de privilèges.
        
    ```powershell
    Get-HgsAttestationBaselinePolicy -Path 'HWConfig1.tcglog'
    ```

    >[!NOTE]
    >Vous devez utiliser l’indicateur **-SkipValidation** si le démarrage sécurisé n’est pas activé pour l’hôte de référence, si un environnement d’exploitation virtuel est présent, si la sécurité basée sur la virtualisation est activée et en cours d’exécution, ou si une stratégie d’intégrité du code est appliquée. Ces validations sont conçues pour vous tenir informé de la configuration minimale requise pour l’exécution d’une machine virtuelle protégée sur l’ordinateur hôte. L’utilisation de l’indicateur-SkipValidation ne modifie pas la sortie de l’applet de commande. il interrompt simplement les erreurs.

3.  Fournissez la ligne de base du module de plateforme sécurisée (fichier TCGlog) à l’administrateur SGH.

4.  Dans le domaine SGH, copiez le fichier TCGlog sur un serveur SGH et exécutez la commande suivante. En règle générale, vous nommez la stratégie après la classe de matériel qu’elle représente (par exemple, « fabricant modèle révision »).

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Confirmer l’attestation](guarded-fabric-confirm-hosts-can-attest-successfully.md)
