---
title: Capturer les informations de mode de module de plateforme sécurisée requises par SGH
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 915b1338-5085-481b-8904-75d29e609e93
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 12/12/2018
ms.openlocfilehash: 82171eee10a06cad6bb3ac30e8f771086975c242
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841660"
---
# <a name="authorize-guarded-hosts-using-tpm-based-attestation"></a>Autoriser les hôtes service Guardian à l’aide de l’attestation basée sur le module de plateforme sécurisée

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Mode de module de plateforme sécurisée utilise un identificateur de module de plateforme sécurisée (également appelé une clé d’identificateur ou endorsement plateforme \[EKpub\]) pour commencer à déterminer si un hôte particulier est autorisé comme « protégé ». Ce mode d’attestation utilise le démarrage sécurisé et le code les mesures d’intégrité pour vous assurer qu’un hôte Hyper-V donné est dans un état sain et seul le code fiable est en cours d’exécution. Dans l’ordre pour l’attestation de comprendre ce qui est et n’est pas sain, vous devez capturer les artefacts suivants :

1.  Identificateur de module de plateforme sécurisée (EKpub)

    -  Ces informations sont propres à chaque hôte Hyper-V

2.  Ligne de base du module de plateforme sécurisée (des mesures de démarrage)

    -  Cela s’applique à tous les hôtes Hyper-V qui s’exécutent sur la même classe de matériel

3.  Stratégie d’intégrité du code (une liste verte des fichiers binaires autorisées)

    -  Cela s’applique à tous les hôtes Hyper-V qui partagent le même matériel et logiciel

Nous vous recommandons de capturer de base et de la stratégie d’intégrité d’un hôte « référence » qui est responsable de chaque classe unique de la configuration matérielle de Hyper-V dans votre centre de données. À compter de Windows Server version 1709, des exemples de stratégies CI sont fournies à C:\Windows\schemas\CodeIntegrity\ExamplePolicies. 

## <a name="versioned-attestation-policies"></a>Stratégies d’attestation avec contrôle de version

Windows Server 2019 introduit une nouvelle méthode pour l’attestation, appelée *v2 attestation*, où un certificat de module de plateforme sécurisée doit être présent afin d’ajouter le EKPub à SGH. La méthode d’attestation de v1 utilisée dans Windows Server 2016 autorisés vous permettent de remplacer cette vérification de sécurité en spécifiant l’option - indicateur de Force lorsque vous exécutez Add-HgsAttestationTpmHost ou autres applets de commande de l’attestation TPM pour capturer les artefacts. À compter de Windows Server 2019, l’attestation v2 est utilisée par défaut et vous devez spécifier l’indicateur de v1 - PolicyVersion lorsque vous exécutez Add-HgsAttestationTpmHost si vous avez besoin inscrire un module de plateforme sécurisée sans un certificat. -Force indicateur ne fonctionne pas avec l’attestation de v2. 

Un hôte peut attester uniquement si tous les artefacts (EKPub + TPM baseline + stratégie de l’élément de configuration) utilisent la même version d’attestation. L’attestation v2 est essayée en premier, et si cette tentative échoue, l’attestation v1 est utilisée. Cela signifie que si vous avez besoin inscrire un identificateur de module de plateforme sécurisée à l’aide de l’attestation de v1, vous devez également spécifier l’indicateur de v1 - PolicyVersion pour utiliser l’attestation v1 lors de la capture de la ligne de base du module de plateforme sécurisée et de créer la stratégie de l’élément de configuration. Si la base de module de plateforme sécurisée et de la stratégie d’intégrité ont été créés à l’aide de l’attestation v2 et ultérieurement vous devez ensuite ajouter un hôte service Guardian sans un certificat de module de plateforme sécurisée, vous devez recréer chaque artefact avec l’indicateur de v1 - PolicyVersion. 

## <a name="capture-the-tpm-identifier-platform-identifier-or-ekpub-for-each-host"></a>Capturer l’identificateur de module de plateforme sécurisée (identificateur de plateforme ou EKpub) pour chaque hôte

1.  Dans le domaine de fabric, assurez-vous que le module de plateforme sécurisée sur chaque hôte est prêt à être utilisé : autrement dit, le module de plateforme sécurisée est initialisé et obtenu de la propriété. Vous pouvez vérifier l’état du module TPM en ouvrant la Console de gestion TPM (tpm.msc) ou en exécutant **Get-module de plateforme sécurisée** dans une fenêtre Windows PowerShell avec élévation de privilèges. Si votre module de plateforme sécurisée n’est pas dans le **prêt** d’état, vous devez l’initialiser et définir la propriété. Cela est possible dans la Console de gestion du module de plateforme sécurisée ou en exécutant **Initialize-Tpm**.

2.  Sur chaque hôte service Guardian, exécutez la commande suivante dans une console Windows PowerShell avec élévation de privilèges pour obtenir son EKpub. Pour `<HostName>`, remplacez le nom d’hôte unique avec quelque chose d’approprié identifier cet hôte : il peut s’agir son nom d’hôte ou le nom utilisé par un service d’inventaire de fabric (si disponible). Pour plus de commodité, nommez le fichier de sortie à l’aide du nom de l’hôte.

    ```powershell
    (Get-PlatformIdentifier -Name '<HostName>').InnerXml | Out-file <Path><HostName>.xml -Encoding UTF8
    ```
3.  Répétez les étapes précédentes pour chaque hôte qui deviendra un hôte service Guardian, en veillant à donnez un nom unique à chaque fichier XML.

4.  Fournir les fichiers XML résultants à l’administrateur SGH.

5.  Dans le domaine SGH, ouvrez une console Windows PowerShell avec élévation de privilèges sur un serveur SGH et exécutez la commande suivante. Répétez la commande pour chacun des fichiers XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > Si vous rencontrez une erreur lors de l’ajout d’un identificateur de module de plateforme sécurisée concernant un certificat de clé Endorsement non approuvé (EKCert), vérifiez que le [approuvé du certificat racine de module de plateforme sécurisée a été ajouté](guarded-fabric-install-trusted-tpm-root-certificates.md) au nœud SGH.
    > En outre, certains fournisseurs de module de plateforme sécurisée n’utilisent pas EKCerts.
    > Vous pouvez vérifier si un EKCert est manquant, en ouvrant le fichier XML dans un éditeur tel que le bloc-notes et la vérification d’un message d’erreur n’indiquant aucune EKCert a été trouvée.
    > Si c’est le cas, et que vous faites confiance que le TPM sur votre ordinateur est authentique, vous pouvez utiliser le `-Force` paramètre à ajouter l’identificateur de l’hôte à SGH. Dans Windows Server 2019, vous devez également utiliser le `-PolicyVersion v1` paramètre lors de l’utilisation `-Force`. Cela crée une stratégie cohérente avec le comportement de Windows Server 2016 et vous oblige à utiliser `-PolicyVersion v1` lors de l’inscription de la stratégie de l’élément de configuration et de la ligne de base TPM.

## <a name="create-and-apply-a-code-integrity-policy"></a>Créer et appliquer une stratégie d’intégrité du code

Une stratégie d’intégrité du code permet de garantir que seuls les exécutables que vous faites confiance pour s’exécuter sur un ordinateur hôte sont autorisés à exécuter. Les logiciels malveillants et autres exécutables à l’extérieur les exécutables approuvés ne peuvent pas en cours d’exécution.

Chaque hôte service Guardian doit avoir une stratégie d’intégrité de code appliquée afin d’exécuter des machines virtuelles protégées en mode de module de plateforme sécurisée. Vous spécifiez les stratégies d’intégrité du code exact que vous faites confiance en les ajoutant à SGH. Stratégies d’intégrité du code peuvent être configurés pour appliquer la stratégie bloque tout logiciel qui ne pas se conformer à la stratégie, ou simplement d’audit (journal d’un événement lors de l’exécution de logiciels non défini dans la stratégie). 

À compter de Windows Server version 1709, les stratégies d’intégrité du code exemple sont inclus dans Windows à C:\Windows\schemas\CodeIntegrity\ExamplePolicies. Deux stratégies sont recommandés pour Windows Server :

- **AllowMicrosoft**: Permet à tous les fichiers signés par Microsoft. Cette stratégie est recommandée pour les applications serveur telles que SQL ou Exchange, ou si le serveur est analysé par les agents publiés par Microsoft.
- **DefaultWindows_Enforced**: Autorise uniquement les fichiers fournis dans Windows et autres applications publiées par Microsoft, tels qu’Office n’autorisent pas. Cette stratégie est recommandée pour les serveurs qui exécutent uniquement des rôles serveur intégrés et des fonctionnalités telles que Hyper-V. 

Il est recommandé que vous commencez par créez la stratégie de l’élément de configuration en mode d’audit (journalisation) pour voir si elle manque quoi que ce soit, puis appliquer la stratégie pour les charges de production. 

Si vous utilisez le [New-CIPolicy](https://docs.microsoft.com/powershell/module/configci/new-cipolicy?view=win10-ps) applet de commande pour générer votre propre stratégie d’intégrité du code, vous devez déterminer les niveaux de règle à utiliser. Nous recommandons un niveau principal de **Publisher** avec secours **hachage**, ce qui permet plus numériquement signé logiciel à mettre à jour sans modifier la stratégie de l’élément de configuration.
Nouveau logiciel écrit par le même serveur de publication peut également être installé sur le serveur sans modifier la stratégie de l’élément de configuration.
Fichiers exécutables qui ne sont pas signés numériquement seront hachées : mises à jour de ces fichiers vous obligera à créer une nouvelle stratégie d’élément de configuration.
Pour plus d’informations sur les niveaux de règle de stratégie CI disponibles, consultez [déployer des stratégies d’intégrité du code : règles de stratégie et les règles de fichier](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/select-types-of-rules-to-create#windows-defender-application-control-policy-rules) et de l’aide de l’applet de commande.

1.  Sur l’hôte de référence, générer une nouvelle stratégie d’intégrité de code. Les commandes suivantes créent une stratégie à le **Publisher** au même niveau de secours à **hachage**. Il convertit ensuite le fichier XML au format de fichier binaire Windows et SGH doivent appliquer et de mesurer la stratégie CI, respectivement.

    ```powershell
    New-CIPolicy -Level Publisher -Fallback Hash -FilePath 'C:\temp\HW1CodeIntegrity.xml' -UserPEs

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity.p7b'
    ```

    >[!NOTE]
    >La commande ci-dessus crée une stratégie d’intégrité en mode audit uniquement. Elle ne bloque pas les fichiers binaires non autorisés de s’exécuter sur l’ordinateur hôte. Vous devez uniquement utiliser des stratégies mises en application en production.

2.  Gardez votre fichier de stratégie de l’intégrité du Code (fichier XML) où vous pouvez facilement retrouver. Vous devez modifier ce fichier ultérieurement pour appliquer la stratégie d’intégrité ou de fusion des modifications à partir de futures mises à jour apportées au système.

3.  Appliquer la stratégie de l’élément de configuration à votre hôte de référence :

    1.  Copiez le fichier de stratégie CI binaire (HW1CodeIntegrity.p7b) à l’emplacement suivant sur votre hôte de référence (le nom de fichier doit correspondre exactement à) :<br>
        **C:\\Windows\\System32\\CodeIntegrity\\SIPolicy.p7b**

    2.  Redémarrez l’hôte pour appliquer la stratégie.

3.  Tester la stratégie d’intégrité du code en exécutant une charge de travail standard. Cela peut inclure des machines virtuelles, les agents de gestion d’infrastructure, les agents de sauvegarde, en cours d’exécution ou le dépannage des outils sur la machine. Vérifiez s’il existe des violations de l’intégrité du code et mettre à jour votre stratégie d’élément de configuration si nécessaire.

4.  Modifier votre stratégie d’élément de configuration appliquées en mode en exécutant les commandes suivantes par rapport à votre fichier XML de stratégie CI mis à jour.

    ```powershell
    Set-RuleOption -FilePath 'C:\temp\HW1CodeIntegrity.xml' -Option 3 -Delete

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity_enforced.p7b'
    ```

5.  Appliquer la stratégie CI pour tous les ordinateurs hôtes (avec une configuration matérielle et logicielle identiques) en utilisant les commandes suivantes :

    ```powershell
    Copy-Item -Path '<Path to HW1CodeIntegrity\_enforced.p7b>' -Destination 'C:\Windows\System32\CodeIntegrity\SIPolicy.p7b'

    Restart-Computer
    ```

    >[!NOTE]
    >Soyez prudent lors de l’application des stratégies de l’élément de configuration aux ordinateurs hôtes et lors de la mise à jour tous les logiciels sur ces ordinateurs. Les pilotes en mode noyau qui ne sont pas conformes à la stratégie de l’élément de configuration peuvent empêcher le démarrage de l’ordinateur. 

6.  Fournissez le fichier binaire (dans cet exemple, HW1CodeIntegrity\_enforced.p7b) à l’administrateur SGH.

7.  Dans le domaine SGH, copiez la stratégie d’intégrité du code à un serveur SGH et exécutez la commande suivante.

    Pour `<PolicyName>`, spécifiez un nom pour la stratégie d’élément de configuration qui décrit le type d’hôte, il s’applique à. Une bonne pratique consiste à nommer après la marque/modèle de votre machine et toute configuration du logiciel spécial n’est en cours d’exécution sur ce dernier.<br>Pour `<Path>`, spécifiez le chemin d’accès et le nom de la stratégie d’intégrité du code.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

## <a name="capture-the-tpm-baseline-for-each-unique-class-of-hardware"></a>Capturer la ligne de base du module de plateforme sécurisée pour chaque classe de matériel unique

Une ligne de base du module de plateforme sécurisée est requis pour chaque classe de matériel dans votre infrastructure de centre de données unique. Réutiliser un « hôte de référence ». 

1. Sur l’hôte de référence, assurez-vous que le rôle Hyper-V et la fonctionnalité de Support de Hyper-V Guardian hôte sont installés.

    >[!WARNING]
    >La fonctionnalité de Support de Hyper-V Guardian hôte active basés sur la virtualisation de protection de l’intégrité du code qui peut-être être incompatible avec certains appareils. Nous vous recommandons vivement de tester cette configuration dans votre laboratoire avant d’activer cette fonctionnalité. Dans le cas contraire, cela risque d’entraîner des échecs inattendus pouvant aller jusqu'à la perte de données ou un écran bleu d’erreur (également appelé erreur d’arrêt).

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```
        
2. Pour capturer la stratégie de référence, exécutez la commande suivante dans une console Windows PowerShell avec élévation de privilèges.
        
    ```powershell
    Get-HgsAttestationBaselinePolicy -Path 'HWConfig1.tcglog'
    ```

    >[!NOTE]
    >Vous devez utiliser le **- SkipValidation** indicateur si l’hôte de référence n’a pas le démarrage sécurisé est activé, un IOMMU présent, la sécurité d’en fonction de virtualisation activé et en cours d’exécution ou une stratégie d’intégrité de code appliquées. Ces validations sont conçues pour vous faire part de la configuration minimale requise de l’exécution d’une machine virtuelle protégée sur l’ordinateur hôte. À l’aide de l’indicateur - SkipValidation ne modifie pas la sortie de l’applet de commande ; simplement, il réduit au silence les erreurs.

3.  Fournir la ligne de base du module de plateforme sécurisée (fichier TCGlog) à l’administrateur SGH.

4.  Dans le domaine SGH, copiez le fichier de TCGlog sur un serveur SGH et exécutez la commande suivante. En règle générale, nommer la stratégie après la classe de matériel qu’il représente (par exemple, « Fabricant modèle révision »).

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

## <a name="next-step"></a>Étape suivante

>[!div class="nextstepaction"]
[Confirmer l’attestation](guarded-fabric-confirm-hosts-can-attest-successfully.md)
