---
title: Mise à niveau d’une structure protégée vers Windows Server 2019
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/21/2018
ms.openlocfilehash: 39974806c02e55b37d3d16748c4ca0e3f361ee45
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284107"
---
# <a name="upgrade-a-guarded-fabric-to-windows-server-2019"></a>Mise à niveau d’une structure protégée vers Windows Server 2019

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Cet article décrit les étapes nécessaires pour mettre à niveau une infrastructure protégée existante à partir de Windows Server 2016, Windows Server version 1709 ou Windows Server version 1803 pour Windows Server 2019.

## <a name="whats-new-in-windows-server-2019"></a>Nouveautés de Windows Server 2019

Lorsque vous exécutez une infrastructure protégée sur Windows Server 2019, vous pouvez tirer parti de plusieurs nouvelles fonctionnalités :

**Héberger l’Attestation de clé** est notre mode d’attestation plus récent, conçu pour le rendre plus facile d’exécuter des machines virtuelles protégées lorsque vos hôtes Hyper-V n’ont pas de TPM 2.0 appareils disponibles pour l’attestation TPM. Attestation de clé hôte utilise des paires de clés pour authentifier les hôtes avec SGH, supprimant la nécessité pour les ordinateurs hôtes d’être joint à un domaine Active Directory, éliminant l’approbation AD entre SGH et de la forêt d’entreprise et en réduisant le nombre de ports de pare-feu ouvert. Attestation de clé hôte remplace l’attestation Active Directory, ce qui est déconseillée dans Windows Server 2019.

**Version de l’Attestation v2** - pour prendre en charge les nouvelles fonctionnalités et Attestation de clé d’hôte à l’avenir, nous avons introduit le contrôle de version à SGH. Le serveur à l’aide de l’attestation v2, ce qui signifie qu’il peut prendre en charge l’attestation de clé d’hôte pour les ordinateurs hôtes Windows Server 2019 toujours prendre en charge des hôtes de v1 sur Windows Server 2016 entraîne une nouvelle installation du service HGS sur Windows Server 2019. Mises à niveau sur place à 2019 restera à la version v1 jusqu'à ce que vous activiez manuellement v2. La plupart des applets de commande ont maintenant un paramètre - HgsVersion qui vous permet de spécifier si vous souhaitez travailler avec des stratégies d’attestation hérités ou modernes.

**Prise en charge de Linux de machines virtuelles protégées** -ordinateurs hôtes Hyper-V exécutant Windows Server 2019 peut exécuter Linux de machines virtuelles protégées. Bien que Linux protégée machines virtuelles existent à peu près depuis Windows Server version 1709, Windows Server 2019 est la première version de canal de maintenance Long terme pour les prendre en charge.

**Créer une branche améliorations d’office** -nous avons apporté plus facile d’exécuter des machines virtuelles protégées dans les succursales avec prise en charge pour les machines virtuelles protégées en mode hors connexion et les configurations de secours sur les ordinateurs hôtes Hyper-V.

**Hôte de module de plateforme sécurisée de liaison** -pour les plus sûr de charges de travail, où vous souhaitez une machine virtuelle protégée pour s’exécuter uniquement le premier hôte où il a été créé, mais aucune autre, vous pouvez maintenant lier la machine virtuelle à l’hôte à l’aide du module de plateforme sécurisée de l’hôte. Cela est surtout pour les stations de travail à accès privilégié et succursales, plutôt que les charges de travail général de centre de données qui doivent migrer entre des ordinateurs hôtes.

## <a name="compatibility-matrix"></a>Matrice de compatibilité

Avant de mettre à niveau de votre infrastructure protégée pour Windows Server 2019, passez en revue la matrice de compatibilité suivant pour voir si votre configuration est prise en charge.

|  | WS2016 HGS | WS2019 HGS|
|---|---|---|
|**WS2016 Hôte Hyper-V** | Prise en charge | Supported<sup>1</sup>|
|**WS2019 Hôte Hyper-V** | Non pris en charge<sup>2</sup> | Prise en charge|

<sup>1</sup> hôtes Windows Server 2016 peuvent attester uniquement sur des serveurs Windows Server 2019 SGH à l’aide du protocole d’attestation v1. Nouvelles fonctionnalités qui sont exclusivement disponibles dans le protocole d’attestation v2, notamment l’Attestation de clé hôte, ne sont pas pris en charge pour les hôtes Windows Server 2016.

<sup>2</sup> Microsoft a connaissance d’un problème empêchant les hôtes de Windows Server 2019 à l’aide de l’attestation TPM à partir de l’attestation avec succès sur un serveur Windows Server 2016 SGH. Cette limitation sera résolue dans une prochaine mise à jour pour Windows Server 2016.

## <a name="upgrade-hgs-to-windows-server-2019"></a>SGH mise à niveau vers Windows Server 2019

Nous vous recommandons de la mise à niveau votre cluster SGH pour Windows Server 2019 avant que vous mettez à niveau vos hôtes Hyper-V pour vous assurer que tous les hôtes, qu’ils s’exécutent Windows Server 2016 ou 2019, continuez à attester avec succès.

La mise à niveau votre cluster SGH vous obligera à temporairement supprimer un nœud du cluster à la fois lorsqu’elle est mise à niveau. Cela réduit la capacité de votre cluster pour répondre aux demandes de vos hôtes Hyper-V et peut entraîner des temps de réponse lent ou des interruptions de service pour vos clients. Assurez-vous de qu'avoir une capacité suffisante pour gérer votre d’attestation et les demandes de clé mise en production avant la mise à niveau un serveur SGH.

Pour mettre à niveau votre cluster SGH, procédez comme suit sur chaque nœud de votre cluster, un seul nœud à la fois :

1.  Supprimer le serveur SGH de votre cluster en exécutant `Clear-HgsServer` dans une invite de PowerShell avec élévation de privilèges. Cette applet de commande supprime le magasin de SGH répliquées, SGH sites Web et nœud du cluster de basculement.
2.  Si votre serveur SGH est un contrôleur de domaine (configuration par défaut), vous devez exécuter `adprep /forestprep` et `adprep /domainprep` sur le premier nœud mis à niveau pour préparer le domaine pour une mise à niveau du système d’exploitation. Consultez le [mise à niveau de Services de domaine Active Directory documentation](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/upgrade-domain-controllers#supported-in-place-upgrade-paths) pour plus d’informations.
3.  Effectuer un [mise à niveau in situ](../../get-started-19/install-upgrade-migrate-19.md) à Windows Server 2019.
4.  Exécutez [Initialize-HgsServer](guarded-fabric-configure-additional-hgs-nodes.md) pour joindre le nœud au cluster.

Une fois que tous les nœuds ont été mis à niveau vers Windows Server 2019, vous pouvez éventuellement mettre à niveau la version SGH pour v2 pour prendre en charge de nouvelles fonctionnalités telles que l’Attestation de clé hôte.

```powershell
Set-HgsServerVersion  v2
```

## <a name="upgrade-hyper-v-hosts-to-windows-server-2019"></a>Mettre à niveau des hôtes Hyper-V pour Windows Server 2019

Avant de vous mettre à niveau vos hôtes Hyper-V pour Windows Server 2019, assurez-vous que votre cluster SGH est déjà mis à niveau vers Windows Server 2019 et que vous avez déplacé toutes les machines virtuelles hors tension le serveur Hyper-V.

1.  Si vous utilisez des stratégies d’intégrité du code Windows Defender Application Control sur votre serveur (toujours le cas avec l’attestation TPM), assurez-vous que la stratégie est en mode audit ou désactivé avant d’essayer de mettre à niveau le serveur. [Découvrez comment désactiver une stratégie WDAC](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/disable-windows-defender-application-control-policies)
2.  Suivez les instructions de la [centre de mise à niveau de Windows Server](http://aka.ms/upgradecenter) pour mettre à niveau de votre hôte Windows Server 2019. Si votre hôte Hyper-V fait partie d’un Cluster de basculement, envisagez d’utiliser un [niveau propagée de Cluster système d’exploitation](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md).
3.  [Tester et de réactiver](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/audit-windows-defender-application-control-policies) votre stratégie Windows Defender Application Control, si vous aviez un activé avant la mise à niveau.
4.  Exécutez `Get-HgsClientConfiguration` pour vérifier si **IsHostGuarded = True**, ce qui signifie que l’ordinateur hôte est correctement le passage d’attestation avec votre serveur SGH.
5.  Si vous utilisez l’attestation TPM, vous devrez peut-être [capturez de nouveau la stratégie d’intégrité de ligne de base ou code TPM](guarded-fabric-add-host-information-for-tpm-trusted-attestation.md) après la mise à niveau pour passer une attestation.
6.  Démarrage en cours d’exécution des machines virtuelles protégées sur l’ordinateur hôte à nouveau !

## <a name="switch-to-host-key-attestation"></a>Commutateur pour héberger l’Attestation de clé

Suivez les étapes ci-dessous si vous sont en cours d’exécution l’attestation basée sur Active Directory et que vous souhaitez mettre à niveau à l’Attestation de clé hôte. Notez que l’attestation basée sur Active Directory est déconseillée dans Windows Server 2019 et peut être supprimée dans une version ultérieure.

1.  Assurez-vous que votre serveur SGH fonctionne en mode d’attestation de v2 en exécutant la commande suivante. Les hôtes de v1 existants continueront à attester même lorsque le serveur SGH est mis à niveau vers la version 2.

    ```powershell
    Set-HgsServerVersion v2
    ```

2.  [Générer les clés d’hôte](guarded-fabric-create-host-key.md) à partir de chacun de vos Hyper-V héberge et les enregistrer avec SGH. Étant donné que SGH est continue à fonctionner en mode Active Directory, vous recevrez un avertissement que les nouvelles clés d’hôte ne sont pas pris en compte immédiatement. Ceci est intentionnel, comme vous ne souhaitez pas modifier en mode de clé hôte jusqu'à ce que tous les ordinateurs hôtes peuvent attester correctement avec les clés d’hôte.

3.  Une fois que les clés d’hôte ont été inscrit pour chaque hôte, vous pouvez configurer SGH pour utiliser le mode de l’attestation de clé d’hôte :

    ```powershell
    Set-HgsServer -TrustHostKey
    ```

    Si vous rencontrez des problèmes avec le mode de clé hôte et que vous devez revenir à l’attestation basée sur Active Directory, exécutez la commande suivante sur SGH :

    ```powershell
    Set-HgsServer -TrustActiveDirectory
    ```