---
title: Résolution des problèmes du Service Guardian hôte
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 80ea38f4-4de6-4f85-8188-33a63bb1cf81
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7fe01039b47c36d940973fba97d25c401f5af8a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851370"
---
# <a name="troubleshooting-guarded-hosts"></a>Résolution des problèmes d’hôtes service Guardian

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit les solutions aux problèmes courants rencontrés lors du déploiement ou d’exploitation d’un hôte Hyper-V service Guardian dans votre infrastructure protégée.
Si vous n’êtes pas sûr de la nature de votre problème, essayez d’exécuter première le [service Guardian diagnostics de fabric](guarded-fabric-troubleshoot-diagnostics.md) sur vos ordinateurs hôtes Hyper-V afin de réduire le risque provoque.

## <a name="guarded-host-feature"></a>Service Guardian hôte fonctionnalité

Si vous rencontrez des problèmes avec votre hôte Hyper-V, vérifiez d’abord que le **prise en charge de Hyper-V hôte Guardian** fonctionnalité est installée.
Sans cette fonctionnalité, certains paramètres de configuration critiques est absentes de l’hôte Hyper-V et les logiciels qui lui permettent de passer d’attestation et approvisionner des machines virtuelles protégées.

Pour vérifier si la fonctionnalité est installée, utilisez le Gestionnaire de serveur, ou exécutez la commande suivante dans une fenêtre PowerShell avec élévation de privilèges :

```powershell
Get-WindowsFeature HostGuardian
```

Si la fonctionnalité n’est pas installée, installez-le avec la commande PowerShell suivante :

```powershell
Install-WindowsFeature HostGuardian -Restart
```

## <a name="attestation-failures"></a>Échecs de l’attestation

Si un ordinateur hôte ne passe pas d’attestation avec le Service Guardian hôte, il sera impossible d’exécuter des machines virtuelles protégées.
La sortie de [Get-HgsClientConfiguration](https://technet.microsoft.com/library/dn914500.aspx) sur cet ordinateur hôte affiche des informations sur la raison pour laquelle qui hébergent l’attestation a échoué.

Le tableau ci-dessous explique les valeurs qui peuvent apparaître dans le **AttestationStatus** champ et les prochaines étapes possibles, le cas échéant.

AttestationStatus         | Explication
--------------------------|------------
Expiré                   | L’hôte passé une attestation précédemment, mais le certificat d’intégrité, qu'il a été émis a expiré. Vérifiez l’hôte et l’heure de SGH sont synchronisés.
InsecureHostConfiguration | L’hôte n’a pas réussi l’attestation, car il n’était pas conforme aux stratégies d’attestation configurés sur SGH. Pour plus d’informations, consultez le tableau AttestationSubStatus.
NotConfigured             | L’hôte n’est pas configuré pour utiliser un service SGH pour l’attestation et de protection de clé. Il est configuré pour le mode local, à la place. Si cet ordinateur hôte est dans une structure protégée, utilisez [Set-HgsClientConfiguration](https://technet.microsoft.com/library/dn914494.aspx) lui fournir l’URL de votre serveur SGH.
Passé                    | L’hôte passé une attestation.
TransientError            | La dernière tentative de l’attestation a échoué en raison d’une mise en réseau, service ou autre erreur temporaire. Recommencez votre dernière opération.
TpmError                  | L’hôte n’a pas pu terminer sa dernière tentative d’attestation en raison d’une erreur avec le module TPM. Pour plus d’informations, consultez vos journaux de module de plateforme sécurisée.
UnauthorizedHost          | L’hôte n’a pas réussi l’attestation, car il n’était pas autorisé à exécuter des machines virtuelles protégées. Vérifiez que l’ordinateur hôte appartient à un groupe de sécurité approuvé par SGH pour exécuter des machines virtuelles protégées.
Inconnu                   | L’hôte n’a pas tenté d’attester encore avec SGH.


Lorsque **AttestationStatus** est signalé comme étant **InsecureHostConfiguration**, un ou plusieurs motifs seront remplis dans la **AttestationSubStatus** champ.
Le tableau ci-dessous explique les valeurs possibles pour AttestationSubStatus et des conseils sur la façon de résoudre le problème.

AttestationSubStatus       | Ce que cela signifie et comment procéder
---------------------------|-------------------------------
BitLocker                  | Volume de système d’exploitation de l’hôte n’est pas chiffré par BitLocker. Pour résoudre ce problème, [activer BitLocker](https://technet.microsoft.com/itpro/windows/keep-secure/bitlocker-basic-deployment) sur le volume du système d’exploitation ou [désactiver la stratégie BitLocker sur SGH](guarded-fabric-manage-hgs.md#review-attestation-policies).
CodeIntegrityPolicy        | L’hôte n’est pas configuré pour utiliser une stratégie d’intégrité du code ou n’utilise pas une stratégie approuvée par le serveur SGH. Vérifiez une stratégie d’intégrité du code a été configurée, que l’hôte a été redémarré et que la stratégie est inscrit auprès du serveur SGH. Pour plus d’informations, consultez [créer et appliquer une stratégie d’intégrité du code](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#create-and-apply-a-code-integrity-policy).
DumpsEnabled               | L’hôte est configuré pour autoriser les vidages sur incident ou live vidages de mémoire, ce qui n’est pas autorisé par vos stratégies SGH. Pour résoudre ce problème, désactivez les vidages sur l’hôte.
DumpEncryption             | L’hôte est configuré pour autoriser les vidages sur incident ou de mémoire en temps réel fait un dump, mais ne chiffre pas ces vidages. Désactivez les vidages sur l’ordinateur hôte ou [configurer le chiffrement de vidage](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).
DumpEncryptionKey          | L’hôte est configuré pour autoriser et chiffrer les dumps, mais n’utilise pas un certificat connu SGH pour chiffrer les. Pour résoudre ce problème, [mettre à jour la clé de chiffrement de vidage](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption) sur l’ordinateur hôte ou [enregistrer la clé avec SGH](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts).
FullBoot                   | Reprise de l’hôte à partir d’un état de veille ou de la mise en veille prolongée. Redémarrez l’hôte pour permettre un démarrage propre et complète.
HibernationEnabled         | L’hôte est configuré pour autoriser la mise en veille prolongée sans chiffrer le fichier de mise en veille prolongée, ce qui n’est pas autorisé par vos stratégies SGH. Désactiver la mise en veille prolongée et redémarrez l’hôte, ou [configurer le chiffrement de vidage](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).
HypervisorEnforcedCodeIntegrityPolicy | L’hôte n’est pas configuré pour utiliser une stratégie d’intégrité du code appliquée par l’hyperviseur. Vérifiez que l’intégrité du code est activée, configurée et appliquée par l’hyperviseur. Consultez le [guide de déploiement de Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies) pour plus d’informations.
Iommu                      | Fonctionnalités de sécurité en fonction de la virtualisation de l’hôte ne sont pas configurées pour exiger un appareil IOMMU pour la protection contre les attaques de Direct Memory Access, comme requis par vos stratégies SGH. Vérifiez que l’hôte a un IOMMU, il est activé, ce Device Guard est [configuré pour exiger les protections DMA](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#enable-virtualization-based-security-vbs-and-device-guard) lors du lancement VBS.
PagefileEncryption         | Chiffrement de fichier de page n’est pas activé sur l’ordinateur hôte. Pour résoudre ce problème, exécutez `fsutil behavior set encryptpagingfile 1` pour activer le chiffrement de fichier de page. Pour plus d’informations, consultez [comportement de fsutil](https://technet.microsoft.com/library/cc785435.aspx).
SecureBoot                 | Démarrage sécurisé est soit ne pas activé sur cet ordinateur hôte ou non à l’aide du modèle de démarrage sécurisé de Microsoft. [Activer le démarrage sécurisé](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/disabling-secure-boot#enable_secure_boot) avec le modèle de démarrage sécurisé de Microsoft pour résoudre ce problème.
SecureBootSettings         | La ligne de base du module de plateforme sécurisée sur cet ordinateur hôte ne correspond pas à un de ces par SGH approuvé. Cela peut se produire lorsque votre UEFI autorités de lancement, DBX variable, indicateur de débogage ou le démarrage sécurisé stratégies personnalisées sont modifiés en installant le nouveau matériel ou logiciel. Si vous faites confiance à l’actuel, du microprogramme, configuration matérielle et logicielle de cet ordinateur, vous pouvez [capturer une nouvelle ligne de base du module de plateforme sécurisée](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#capture-the-tpm-baseline-for-each-unique-class-of-hardware) et [Inscrivez-le avec SGH](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts).
TcgLogVerification         | Le journal TCG (référence de module de plateforme sécurisée) ne peut pas être obtenu ou vérifié. Cela peut indiquer un problème avec le microprogramme de l’hôte, le module de plateforme sécurisée ou autres composants matériels. Si votre hôte est configuré pour tenter de démarrage PXE avant le démarrage de Windows, un programme obsolète de démarrage Net (NBP) peut également provoquer cette erreur. Vérifiez que tous les programmes de démarrage réseau sont à jour lorsque le démarrage PXE est activé.
VirtualSecureMode          | Fonctionnalités de sécurité en fonction de la virtualisation ne s’exécutent pas sur l’ordinateur hôte. Vérifiez VBS est activé et que votre système répond à configuré [les fonctionnalités de sécurité de plateforme](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#validate-enabled-device-guard-hardware-based-security-features). Consultez le [documentation de Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide) pour plus d’informations sur la configuration requise VBS.
