---
title: Mise à niveau d’une structure protégée vers Windows Server 2019
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/21/2018
ms.openlocfilehash: dfc9d9cb4282823bb774b8dca136bbd4e5ccb496
ms.sourcegitcommit: ccec91c1d32a978159f9b8bb5e39ead5805c26c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71143773"
---
# <a name="upgrade-a-guarded-fabric-to-windows-server-2019"></a>Mise à niveau d’une structure protégée vers Windows Server 2019

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Cet article décrit les étapes nécessaires à la mise à niveau d’une infrastructure protégée existante à partir de Windows Server 2016, Windows Server version 1709 ou Windows Server version 1803 vers Windows Server 2019.

## <a name="whats-new-in-windows-server-2019"></a>Nouveautés de Windows Server 2019

Lorsque vous exécutez une infrastructure protégée sur Windows Server 2019, vous pouvez tirer parti de plusieurs nouvelles fonctionnalités :

L' **attestation de clé hôte** est notre mode d’attestation le plus récent, conçu pour faciliter l’exécution de machines virtuelles protégées quand vos hôtes Hyper-V n’ont pas d’appareils TPM 2,0 disponibles pour l’attestation TPM. L’attestation de clé d’hôte utilise des paires de clés pour authentifier les ordinateurs hôtes avec SGH, supprimant ainsi la nécessité d’associer les hôtes à un domaine Active Directory, en éliminant l’approbation AD entre SGH et la forêt d’entreprise, et en réduisant le nombre de ports de pare-feu ouverts. L’attestation de clé hôte remplace Active Directory attestation, qui est dépréciée dans Windows Server 2019.

**Version d’attestation v2** : pour prendre en charge l’attestation de clé hôte et de nouvelles fonctionnalités à l’avenir, nous avons introduit le contrôle de version dans SGH. Une nouvelle installation de SGH sur Windows Server 2019 entraînera le serveur à l’aide de l’attestation v2, ce qui signifie qu’il peut prendre en charge l’attestation de clé hôte pour les ordinateurs hôtes Windows Server 2019 tout en prenant en charge les hôtes v1 sur Windows Server 2016. Les mises à niveau sur place vers 2019 restent à la version v1 jusqu’à ce que vous activiez manuellement v2. La plupart des cmdlets disposent désormais d’un paramètre-HgsVersion qui vous permet de spécifier si vous souhaitez utiliser des stratégies d’attestation anciennes ou modernes.

**Prise en charge des machines virtuelles Linux protégées** : les hôtes Hyper-V exécutant Windows Server 2019 peuvent exécuter des machines virtuelles Linux protégées. Alors que les machines virtuelles Linux protégées existent depuis la version 1709 de Windows Server, Windows Server 2019 est la première version du canal de maintenance à long terme pour les prendre en charge.

**Améliorations des succursales** : nous avons facilité l’exécution de machines virtuelles protégées dans les succursales grâce à la prise en charge des machines virtuelles protégées hors connexion et des configurations de secours sur les hôtes Hyper-V.

**Liaison d’hôte TPM** : pour les charges de travail les plus sécurisées, où vous souhaitez qu’une machine virtuelle protégée s’exécute uniquement sur le premier hôte où elle a été créée, mais pas sur l’autre, vous pouvez maintenant lier la machine virtuelle à cet hôte à l’aide du module de plateforme sécurisée de l’hôte. Cette méthode est idéale pour les stations de travail d’accès privilégié et les succursales, plutôt que pour les charges de travail de centre de résultats générales qui doivent migrer entre les hôtes.

## <a name="compatibility-matrix"></a>Matrice de compatibilité

Avant de mettre à niveau votre infrastructure protégée vers Windows Server 2019, passez en revue la matrice de compatibilité suivante pour déterminer si votre configuration est prise en charge.

|  | WS2016 SGH | WS2019 SGH|
|---|---|---|
|**Hôte Hyper-V WS2016** | Prise en charge | Pris en charge<sup>1</sup>|
|**Hôte Hyper-V WS2019** | Non pris en charge<sup>2</sup> | Prise en charge|

<sup>1</sup> les hôtes windows server 2016 peuvent uniquement attester par rapport aux serveurs windows server 2019 SGH à l’aide du protocole d’attestation v1. Les nouvelles fonctionnalités exclusivement disponibles dans le protocole d’attestation v2, y compris l’attestation de clé d’hôte, ne sont pas prises en charge pour les ordinateurs hôtes Windows Server 2016.

<sup>2</sup> Microsoft a connaissance d’un problème qui empêche les hôtes windows server 2019 utilisant l’attestation TPM de s’attester correctement par rapport à un serveur SGH windows server 2016. Cette limitation sera traitée dans une prochaine mise à jour pour Windows Server 2016.

## <a name="upgrade-hgs-to-windows-server-2019"></a>Mettre à niveau SGH vers Windows Server 2019

Nous vous recommandons de mettre à niveau votre cluster SGH vers Windows Server 2019 avant de mettre à niveau vos ordinateurs hôtes Hyper-V pour vous assurer que tous les ordinateurs hôtes, qu’ils exécutent Windows Server 2016 ou 2019, peuvent continuer à attester correctement.

Pour mettre à niveau votre cluster SGH, vous devez supprimer temporairement un nœud du cluster à la fois lors de sa mise à niveau. Cela réduira la capacité de votre cluster à répondre aux demandes de vos hôtes Hyper-V et pourrait entraîner des temps de réponse ou des interruptions de service lents pour vos locataires. Vérifiez que vous disposez d’une capacité suffisante pour gérer votre attestation et vos demandes de version de clé avant de mettre à niveau un serveur SGH.

Pour mettre à niveau votre cluster SGH, procédez comme suit sur chaque nœud de votre cluster, un nœud à la fois :

1.  Supprimez le serveur SGH de votre cluster en `Clear-HgsServer` exécutant dans une invite PowerShell avec élévation de privilèges. Cette applet de commande permet de supprimer le magasin de basculement SGH, les sites Web SGH et le nœud du cluster de basculement.
2.  Si votre serveur SGH est un contrôleur de domaine (configuration par défaut), vous devez exécuter `adprep /forestprep` et `adprep /domainprep` sur le premier nœud en cours de mise à niveau pour préparer le domaine pour une mise à niveau du système d’exploitation. Pour plus d’informations, consultez la [documentation relative à la mise à niveau de Active Directory Domain Services](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/upgrade-domain-controllers#supported-in-place-upgrade-paths) .
3.  Effectuez une [mise à niveau sur place](../../get-started-19/install-upgrade-migrate-19.md) vers Windows Server 2019.
4.  Exécutez [Initialize-HgsServer](guarded-fabric-configure-additional-hgs-nodes.md) pour replacer le nœud dans le cluster.

Une fois que tous les nœuds ont été mis à niveau vers Windows Server 2019, vous pouvez éventuellement mettre à niveau la version SGH vers v2 pour prendre en charge de nouvelles fonctionnalités telles que l’attestation de clé hôte.

```powershell
Set-HgsServerVersion  v2
```

## <a name="upgrade-hyper-v-hosts-to-windows-server-2019"></a>Mettre à niveau les ordinateurs hôtes Hyper-V vers Windows Server 2019

Avant de mettre à niveau vos ordinateurs hôtes Hyper-V vers Windows Server 2019, vérifiez que votre cluster SGH est déjà mis à niveau vers Windows Server 2019 et que vous avez déplacé toutes les machines virtuelles du serveur Hyper-V.

1.  Si vous utilisez des stratégies d’intégrité du code de contrôle des applications Windows Defender sur votre serveur (toujours en cas d’utilisation de l’attestation TPM), assurez-vous que la stratégie est en mode audit ou désactivée avant de tenter de mettre à niveau le serveur. [En savoir plus sur la désactivation d’une stratégie WDAC](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/disable-windows-defender-application-control-policies)
2.  Suivez les instructions dans le [contenu de la mise à niveau de Windows Server](../../upgrade/upgrade-overview.md) pour mettre à niveau votre ordinateur hôte vers Windows Server 2019. Si votre hôte Hyper-V fait partie d’un cluster de basculement, envisagez d’utiliser une [mise à niveau propagée du système d’exploitation du cluster](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md).
3.  [Testez et réactivez](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/audit-windows-defender-application-control-policies) votre stratégie Windows Defender application Control, si celle-ci a été activée avant la mise à niveau.
4.  Exécutez `Get-HgsClientConfiguration` pour vérifier si **IsHostGuarded = true**, ce qui signifie que l’hôte réussit l’attestation avec votre serveur SGH.
5.  Si vous utilisez l’attestation TPM, vous devrez peut-être [recapturer la ligne de base du module de plateforme sécurisée ou la stratégie d’intégrité du code](guarded-fabric-add-host-information-for-tpm-trusted-attestation.md) après la mise à niveau pour passer l’attestation.
6.  Recommencez l’exécution des machines virtuelles protégées sur l’ordinateur hôte.

## <a name="switch-to-host-key-attestation"></a>Basculer vers l’attestation de clé hôte

Suivez les étapes ci-dessous si vous exécutez actuellement une attestation basée sur Active Directory et que vous souhaitez effectuer une mise à niveau vers l’attestation de clé hôte. Notez que la Active Directory attestation est déconseillée dans Windows Server 2019 et peut être supprimée dans une version ultérieure.

1.  Assurez-vous que votre serveur SGH fonctionne en mode d’attestation V2 en exécutant la commande suivante. Les hôtes v1 existants continueront d’attester même lorsque le serveur SGH sera mis à niveau vers v2.

    ```powershell
    Set-HgsServerVersion v2
    ```

2.  [Générez des clés d’hôte](guarded-fabric-create-host-key.md) à partir de chacun de vos ordinateurs hôtes Hyper-V et inscrivez-les auprès de SGH. Étant donné que SGH fonctionne toujours en mode Active Directory, vous recevrez un avertissement indiquant que les nouvelles clés d’hôte ne sont pas immédiatement effectives. Cela est intentionnel, car vous ne souhaitez pas passer en mode de clé hôte tant que tous vos hôtes ne peuvent pas attester avec succès les clés d’hôte.

3.  Une fois les clés d’hôte inscrites pour chaque hôte, vous pouvez configurer SGH pour utiliser le mode d’attestation de clé hôte :

    ```powershell
    Set-HgsServer -TrustHostKey
    ```

    Si vous rencontrez des problèmes avec le mode de clé hôte et que vous devez revenir à l’attestation de Active Directory, exécutez la commande suivante sur SGH :

    ```powershell
    Set-HgsServer -TrustActiveDirectory
    ```