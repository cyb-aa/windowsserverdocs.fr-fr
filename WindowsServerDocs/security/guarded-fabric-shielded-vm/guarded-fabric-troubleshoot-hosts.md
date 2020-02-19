---
title: Résolution des problèmes du service Guardian hôte
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 80ea38f4-4de6-4f85-8188-33a63bb1cf81
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 09/25/2019
ms.openlocfilehash: ec885670ca6808e89c63848781c4ff3dc27799b8
ms.sourcegitcommit: 2a15de216edde8b8e240a4aa679dc6d470e4159e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465603"
---
# <a name="troubleshooting-guarded-hosts"></a>Dépannage des hôtes service Guardian

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit les solutions aux problèmes courants rencontrés lors du déploiement ou de l’utilisation d’un hôte Hyper-V protégé dans votre infrastructure protégée.
Si vous n’êtes pas sûr de la nature de votre problème, essayez d’abord d’exécuter les [Diagnostics de l’infrastructure protégée](guarded-fabric-troubleshoot-diagnostics.md) sur vos ordinateurs hôtes Hyper-V pour réduire les causes potentielles.

## <a name="guarded-host-feature"></a>Fonctionnalité hôte service Guardian

Si vous rencontrez des problèmes avec votre hôte Hyper-V, assurez-vous d’abord que la fonctionnalité de **prise en charge d’Hyper-v Guardian hôte** est installée.
Sans cette fonctionnalité, certains paramètres de configuration critiques et les logiciels de l’hôte Hyper-V seront manquants, ce qui lui permettra de passer l’attestation et de configurer des machines virtuelles protégées.

Pour vérifier si la fonctionnalité est installée, utilisez Gestionnaire de serveur ou exécutez la commande suivante dans une fenêtre PowerShell avec élévation de privilèges :

```powershell
Get-WindowsFeature HostGuardian
```

Si le composant n’est pas installé, installez-le avec la commande PowerShell suivante :

```powershell
Install-WindowsFeature HostGuardian -Restart
```

## <a name="attestation-failures"></a>Échecs d’attestation

Si un ordinateur hôte ne passe pas l’attestation auprès du service Guardian hôte, il ne pourra pas exécuter les machines virtuelles protégées.
La sortie de l’opération [obtenir-HgsClientConfiguration](https://technet.microsoft.com/library/dn914500.aspx) sur cet hôte affiche des informations sur la raison de l’échec de l’attestation de l’hôte.

Le tableau ci-dessous décrit les valeurs qui peuvent apparaître dans le champ **AttestationStatus** et les étapes suivantes, le cas échéant.

AttestationStatus         | Explication
--------------------------|------------
Expiré                   | L’ordinateur hôte a passé l’attestation précédemment, mais le certificat d’intégrité qu’il a été émis a expiré. Assurez-vous que l’hôte et l’heure du SGH sont synchronisés.
InsecureHostConfiguration | L’ordinateur hôte n’a pas réussi l’attestation, car il n’est pas conforme aux stratégies d’attestation configurées sur le service SGH. Pour plus d’informations, consultez la table AttestationSubStatus.
NotConfigured             | L’hôte n’est pas configuré pour utiliser un SGH pour l’attestation et la protection de clé. Elle est configurée pour le mode local à la place. Si cet ordinateur hôte se trouve dans une infrastructure protégée, utilisez [Set-HgsClientConfiguration](https://technet.microsoft.com/library/dn914494.aspx) pour lui fournir les URL de votre serveur SGH.
Réussite                    | L’ordinateur hôte a passé l’attestation.
TransientError            | La dernière tentative d’attestation a échoué en raison d’une mise en réseau, d’un service ou d’une autre erreur temporaire. Réessayez votre dernière opération.
TpmError                  | L’hôte n’a pas pu terminer sa dernière tentative d’attestation en raison d’une erreur liée à votre TPM. Pour plus d’informations, consultez les journaux du module de plateforme sécurisée.
UnauthorizedHost          | L’ordinateur hôte n’a pas réussi l’attestation, car il n’a pas été autorisé à exécuter des machines virtuelles protégées. Assurez-vous que l’hôte appartient à un groupe de sécurité approuvé par SGH pour exécuter des machines virtuelles protégées.
Inconnu.                   | L’hôte n’a pas encore effectué d’attestation avec SGH.

Quand **AttestationStatus** est signalé comme **InsecureHostConfiguration**, une ou plusieurs raisons sont renseignées dans le champ **AttestationSubStatus** .
Le tableau ci-dessous décrit les valeurs possibles pour AttestationSubStatus et des conseils sur la façon de résoudre le problème.

AttestationSubStatus       | Ce que cela signifie et que faire
---------------------------|-------------------------------
BitLocker                  | Le volume du système d’exploitation de l’ordinateur hôte n’est pas chiffré par BitLocker. Pour résoudre ce cas, [activez BitLocker](https://technet.microsoft.com/itpro/windows/keep-secure/bitlocker-basic-deployment) sur le volume du système d’exploitation ou [désactivez la stratégie BitLocker sur SGH](guarded-fabric-manage-hgs.md#review-attestation-policies).
CodeIntegrityPolicy        | L’ordinateur hôte n’est pas configuré pour utiliser une stratégie d’intégrité du code ou n’utilise pas une stratégie approuvée par le serveur SGH. Assurez-vous qu’une stratégie d’intégrité du code a été configurée, que l’hôte a été redémarré et que la stratégie est inscrite auprès du serveur SGH. Pour plus d’informations, consultez [créer et appliquer une stratégie d’intégrité du code](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#create-and-apply-a-code-integrity-policy).
DumpsEnabled               | L’hôte est configuré pour autoriser les vidages sur incident ou les vidages de la mémoire dynamique, ce qui n’est pas autorisé par vos stratégies SGH. Pour résoudre ce cas, désactivez les dumps sur l’hôte.
DumpEncryption             | L’hôte est configuré pour autoriser les vidages sur incident ou les vidages de la mémoire dynamique, mais ne chiffre pas ces vidages. Désactivez les dumps sur l’hôte ou [configurez le chiffrement de vidage](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).
DumpEncryptionKey          | L’ordinateur hôte est configuré pour autoriser et chiffrer les vidages, mais n’utilise pas un certificat connu pour être utilisé pour les chiffrer. Pour résoudre ce cas, [Mettez à jour la clé de chiffrement de vidage](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption) sur l’hôte ou [Enregistrez la clé avec SGH](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts).
FullBoot                   | L’hôte a repris à un état de veille ou à la mise en veille prolongée. Redémarrez l’hôte pour permettre un démarrage propre et complet.
HibernationEnabled         | L’ordinateur hôte est configuré pour autoriser la mise en veille prolongée sans chiffrer le fichier de mise en veille prolongée, ce qui n’est pas autorisé par vos stratégies SGH. Désactivez la mise en veille prolongée et redémarrez l’hôte ou [configurez le chiffrement de vidage](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).
HypervisorEnforcedCodeIntegrityPolicy | L’hôte n’est pas configuré pour utiliser une stratégie d’intégrité du code appliquée par l’hyperviseur. Vérifiez que l’intégrité du code est activée, configurée et appliquée par l’hyperviseur. Pour plus d’informations, consultez le [Guide de déploiement de Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies) .
IOMMU                      | Les fonctionnalités de sécurité basées sur la virtualisation de l’hôte ne sont pas configurées pour exiger un appareil IOMMU pour la protection contre les attaques d’accès direct à la mémoire, comme requis par vos stratégies SGH. Vérifiez que l’ordinateur hôte dispose d’une unité IOMMU, qu’il est activé et que Device Guard est [configuré pour exiger des protections DMA](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#enable-virtualization-based-security-vbs-and-device-guard) lors du lancement de vbs.
PagefileEncryption         | Le chiffrement du fichier d’échange n’est pas activé sur l’ordinateur hôte. Pour résoudre ce message, exécutez `fsutil behavior set encryptpagingfile 1` pour activer le chiffrement du fichier d’échange. Pour plus d’informations, consultez la page [comportement de fsutil](https://technet.microsoft.com/library/cc785435.aspx).
SecureBoot                 | Le démarrage sécurisé n’est pas activé sur cet ordinateur hôte ou n’utilise pas le modèle de démarrage sécurisé Microsoft. [Activez le démarrage sécurisé](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/disabling-secure-boot#enable_secure_boot) avec le modèle de démarrage sécurisé Microsoft pour résoudre ce problème.
SecureBootSettings         | La ligne de base du module de plateforme sécurisée sur cet ordinateur hôte ne correspond à aucune des personnes approuvées par SGH. Cela peut se produire lorsque votre autorité de lancement UEFI, votre variable DBX, cet indicateur de débogage ou vos stratégies de démarrage sécurisé personnalisées sont modifiées en installant le nouveau matériel ou logiciel. Si vous faites confiance au matériel, au microprogramme et à la configuration logicielle actuels de cet ordinateur, vous pouvez [capturer une nouvelle ligne de base du module de plateforme sécurisée](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#capture-the-tpm-baseline-for-each-unique-class-of-hardware) et l' [inscrire auprès du service SGH](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts).
TcgLogVerification         | Impossible d’obtenir ou de vérifier le journal TCG (base de référence du module de plateforme sécurisée). Cela peut indiquer un problème avec le microprogramme de l’hôte, le module de plateforme sécurisée ou d’autres composants matériels. Si votre ordinateur hôte est configuré pour effectuer un démarrage PXE avant de démarrer Windows, un programme de démarrage réseau (NBP) obsolète peut également provoquer cette erreur. Assurez-vous que tous les programmes sont à jour lorsque le démarrage PXE est activé.
VirtualSecureMode          | Les fonctionnalités de sécurité basées sur la virtualisation ne sont pas en cours d’exécution sur l’ordinateur hôte. Vérifiez que VBS est activé et que votre système est conforme aux [fonctionnalités de sécurité](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#validate-enabled-device-guard-hardware-based-security-features)de la plateforme configurées. Pour plus d’informations sur la configuration requise pour VBS, consultez la [documentation de Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide) .

## <a name="modern-tls"></a>TLS moderne

Si vous avez déployé une stratégie de groupe ou configuré votre hôte Hyper-V pour empêcher l’utilisation de TLS 1,0, vous pouvez rencontrer des erreurs « le client du service Guardian hôte n’a pas réussi à désencapsuler un protecteur de clé pour le compte d’un processus appelant » lors de la tentative de démarrage d’une machine virtuelle protégée.
Cela est dû à un comportement par défaut dans .NET 4,6 où la version TLS par défaut du système n’est pas prise en compte lors de la négociation des versions TLS prises en charge avec le serveur SGH.

Pour contourner ce problème, exécutez les deux commandes suivantes pour configurer .NET afin d’utiliser les versions TLS par défaut du système pour toutes les applications .NET.

```cmd
reg add HKLM\SOFTWARE\Microsoft\.NETFramework\v4.0.30319 /v SystemDefaultTlsVersions /t REG_DWORD /d 1 /f /reg:64
reg add HKLM\SOFTWARE\Microsoft\.NETFramework\v4.0.30319 /v SystemDefaultTlsVersions /t REG_DWORD /d 1 /f /reg:32
```

> [!WARNING]
> Le paramètre versions TLS par défaut du système affecte toutes les applications .NET sur votre ordinateur. Veillez à tester les clés de Registre dans un environnement isolé avant de les déployer sur vos ordinateurs de production.

Pour plus d’informations sur .NET 4,6 et TLS 1,0, consultez [résolution du problème tls 1,0, 2e édition](https://docs.microsoft.com/security/solving-tls1-problem).
