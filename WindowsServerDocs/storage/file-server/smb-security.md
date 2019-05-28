---
title: Améliorations en matière de sécurité SMB
description: Une explication de la fonctionnalité de chiffrement SMB dans Windows Server 2012 R2, Windows Server 2012 et Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: b1586c8c63e46452075b4106c944670395734142
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034402"
---
# <a name="smb-security-enhancements"></a>Améliorations en matière de sécurité SMB

>S’applique à : Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Cette rubrique explique les améliorations de sécurité SMB dans Windows Server 2012 R2, Windows Server 2012 et Windows Server 2016.

## <a name="smb-encryption"></a>Chiffrement SMB

Le chiffrement SMB fournit le chiffrement de bout en bout des données SMB et protège les données à partir de clandestine sur des réseaux non approuvés. Vous pouvez déployer le chiffrement SMB avec un minimum d’effort, mais elle peut nécessiter des petites coûts supplémentaires pour le matériel ou logiciel. Il n’a aucune condition requise pour Internet Protocol security (IPsec) ou accélérateur WAN. Le chiffrement SMB peut être configuré par partage ou pour le serveur de fichiers entier, et il peut être activé pour un large éventail de scénarios où les données parcourent les réseaux non approuvés.

>[!NOTE]
>Le chiffrement SMB ne couvre pas la sécurité au repos, qui est généralement contrôlée par le chiffrement de lecteur BitLocker.

Le chiffrement SMB doit être envisagé pour n’importe quel scénario dans lequel les données sensibles doivent être protégées contre les attaques man-in-the-middle. Les scénarios possibles sont :

- Les données sensibles d’un travailleur de l’information sont déplacées à l’aide du protocole SMB. Le chiffrement SMB offre une garantie de confidentialité et l’intégrité de bout en bout entre le serveur de fichiers et le client, quel que soit les réseaux parcourus, telles (qu’étendus WAN) connexions réseau qui sont gérées par des fournisseurs non Microsoft.
- SMB 3.0 permet aux serveurs de fichiers fournir le stockage disponible en continu pour les applications serveur, telles que SQL Server ou Hyper-V. L’activation du chiffrement de SMB offre la possibilité de protéger ces informations les tentatives d’espionnage. Le chiffrement SMB est plus simple à utiliser que les solutions matérielles dédiées qui sont requises pour la plupart des réseaux de stockage (SAN).

>[!IMPORTANT]
>Notez qu’il existe un coût avec toute protection de chiffrement de bout en bout par rapport à non chiffrées d’exploitation de performances notables.

## <a name="enable-smb-encryption"></a>Activer le chiffrement SMB

Vous pouvez activer le chiffrement SMB pour le serveur de fichiers entier ou uniquement pour les partages de fichiers spécifiques. Utilisez une des procédures suivantes pour activer le chiffrement SMB :

### <a name="enable-smb-encryption-with-windows-powershell"></a>Activer le chiffrement SMB avec Windows PowerShell

1. Pour activer le chiffrement SMB pour un partage de fichiers individuels, tapez le script suivant sur le serveur :
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. Pour activer le chiffrement SMB pour le serveur de la totalité du fichier, tapez le script suivant sur le serveur :
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. Pour créer un nouveau partage de fichiers SMB avec chiffrement SMB est activé, tapez le script suivant :
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>Activer le chiffrement SMB avec le Gestionnaire de serveur

1. Dans le Gestionnaire de serveur, ouvrez **File and Storage Services**.
2. Sélectionnez **partages** pour ouvrir la page de gestion de partages.
3. Cliquez sur le partage sur lequel vous souhaitez activer le chiffrement SMB, puis sélectionnez **propriétés**.
4. Sur le **paramètres** page du partage, sélectionnez **chiffrer l’accès aux données**. Accès au fichier à distance à ce partage est chiffré.

### <a name="considerations-for-deploying-smb-encryption"></a>Considérations relatives au déploiement du chiffrement SMB

Par défaut, lorsque le chiffrement SMB est activé pour un partage de fichiers ou un serveur, seuls les clients SMB 3.0 sont autorisés à accéder aux partages de fichier spécifié. Il met en œuvre d’intention de l’administrateur de sauvegarder les données pour tous les clients qui accèdent aux partages. Toutefois, dans certaines circonstances, un administrateur souhaiterez peut-être autoriser l’accès non chiffré pour les clients qui ne prennent pas en charge SMB 3.0 (par exemple, pendant une période de transition lorsque les versions de système d’exploitation clients mixtes sont utilisées). Pour autoriser l’accès non chiffré pour les clients qui ne prennent pas en charge SMB 3.0, tapez le script suivant dans Windows PowerShell :

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

La fonctionnalité de négociation de dialecte sécurisé décrite dans la section suivante empêche une attaque man-in-the-middle de rétrogradation d’une connexion entre SMB 3.0 et SMB 2.0 (ce qui utiliserait un accès non chiffré). Toutefois, il n’empêche pas une mise à niveau à SMB 1.0, ce qui entraînerait également un accès non chiffré. Pour garantir que les clients SMB 3.0 utilisent toujours le chiffrement SMB pour accéder aux partages chiffrés, vous devez désactiver le serveur SMB 1.0. (Pour obtenir des instructions, consultez la section [la désactivation de SMB 1.0](#disabling-smb-10).) Si le **– RejectUnencryptedAccess** paramètre conserve sa valeur par défaut **$true**, uniquement chiffrement prenant en charge SMB 3.0 les clients sont autorisés à accéder aux partages de fichiers (les clients SMB 1.0 peut également être rejetées).

>[!NOTE]
>* Le chiffrement SMB utilise la norme AES (Advanced Encryption)-algorithme CCM pour chiffrer et déchiffrer les données. AES-CCM fournit également la validation d’intégrité des données (signature) pour les partages de fichiers chiffrés, indépendamment des paramètres de signature SMB. Si vous souhaitez activer la signature sans chiffrement SMB, vous pouvez continuer à le faire. Pour plus d’informations, consultez [les principes fondamentaux de la signature SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).
>* Vous pouvez rencontrer des problèmes lorsque vous essayez d’accéder au partage de fichiers ou d’un serveur si votre organisation utilise des appliances de l’accélération de réseau (étendu WAN) étendu.
>* Avec une configuration par défaut (où il n’existe aucun accès non chiffrés autorisées aux partages de fichiers chiffrés), si les clients qui ne prennent pas en charge SMB 3.0 tente d’accéder à un partage de fichiers chiffrés, ID d’événement 1003 est consignée dans le journal des événements Microsoft-Windows-SmbServer/Operational , et le client recevra un **accès refusé** message d’erreur.
>* Le chiffrement SMB et le système de fichier de cryptage (EFS) dans le système de fichiers NTFS ne sont pas liées, et le chiffrement SMB ne requièrent ni ne dépendent de l’utilisation d’EFS.
>* Le chiffrement SMB et le chiffrement de lecteur BitLocker ne sont pas liées, et le chiffrement SMB ne requièrent ni ne dépendent à l’aide du chiffrement de lecteur BitLocker.

## <a name="secure-dialect-negotiation"></a>Négociation de dialecte sécurisé

SMB 3.0 est capable de détecter les attaques man-in-the-middle visant à rétrograder le protocole SMB 2.0 ou SMB 3.0 ou les fonctionnalités que le client et le serveur négocient. Lorsqu’une telle attaque est détectée par le client ou le serveur, la connexion est déconnectée et ID d’événement 1005 est consigné dans le journal des événements Microsoft-Windows-SmbServer/Operational. Sécuriser le dialecte négociation ne peut pas détecter ou prévenir les versions antérieures de SMB 2.0 ou 3.0 à SMB 1.0. Pour cette raison et pour tirer parti de toutes les fonctionnalités de chiffrement SMB, nous vous recommandons vivement de désactiver le serveur SMB 1.0. Pour plus d’informations, consultez [la désactivation de SMB 1.0](#disabling-smb-10).

La fonctionnalité de négociation de dialecte sécurisé qui est décrite dans la section suivante empêche une attaque man-in-the-middle de rétrogradation d’une connexion entre SMB 3 et SMB 2 (ce qui utiliserait un accès non chiffré) ; Toutefois, il n’empêche pas les baisses de SMB 1, ce qui entraînerait également un accès non chiffré. Pour plus d’informations sur les problèmes potentiels de précédemment non Windows implémentations de SMB, consultez le [Base de connaissances Microsoft](http://support.microsoft.com/kb/2686098).

## <a name="new-signing-algorithm"></a>Nouvel algorithme de signature

SMB 3.0 utilise un algorithme de chiffrement plus récent pour la signature : Advanced Encryption Standard (AES) – cipher - en fonction de code d’authentification de message (CMAC). SMB 2.0 utilisé l’ancien algorithme de chiffrement HMAC-SHA256. AES-CMAC et AES-CCM peuvent accélérer considérablement le chiffrement des données sur la plupart des processeurs prenant en charge les instructions AES. Pour plus d’informations, consultez [les principes fondamentaux de la signature SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).

## <a name="disabling-smb-10"></a>Désactivation de SMB 1.0

Le service Explorateur d’ordinateurs hérité et les fonctionnalités de protocole d’Administration à distance dans SMB 1.0 sont désormais distinctes, et ils peuvent être éliminées. Ces fonctionnalités sont toujours activées par défaut, mais si vous n’avez pas les anciens clients SMB, tels que des ordinateurs exécutant Windows Server 2003 ou Windows XP, vous pouvez supprimer les fonctionnalités SMB 1.0 pour augmenter la sécurité et réduire éventuellement la mise à jour corrective.

>[!NOTE]
>SMB 2.0 a été introduite dans Windows Server 2008 et Windows Vista. Les clients plus anciens, tels que des ordinateurs exécutant Windows Server 2003 ou Windows XP, ne prennent pas en charge SMB 2.0 ; et, par conséquent, ils ne seront pas en mesure d’accéder aux partages de fichiers ou partages d’impression si le serveur SMB 1.0 est désactivé. En outre, certains clients SMB non-Microsoft ne peuvent pas être en mesure d’accéder aux partages de fichiers SMB 2.0 ou imprimer les partages (par exemple, les imprimantes avec la fonctionnalité de « analyse de partage »).

Avant de commencer la désactivation de SMB 1.0, vous devrez savoir si vos clients SMB sont actuellement connectés au serveur SMB 1.0 en cours d’exécution. Pour ce faire, entrez la cmdlet suivante dans Windows PowerShell :

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>Vous devez exécuter ce script à plusieurs reprises au cours d’une semaine (plusieurs fois par jour) pour créer une piste d’audit. Vous pouvez également exécuter ceci comme une tâche planifiée.

Pour désactiver SMB 1.0, entrez le script suivant dans Windows PowerShell :

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>Si une connexion de client SMB est refusée car le serveur exécutant SMB 1.0 a été désactivé, l’événement ID 1001 est consigné dans le journal des événements Microsoft-Windows-SmbServer/Operational.

## <a name="more-information"></a>Informations supplémentaires

Voici quelques ressources supplémentaires sur SMB et les technologies connexes dans Windows Server 2012.

- [Server Message Block](file-server-smb-overview.md)
- [Stockage dans Windows Server](../storage.md)
- [Serveur de fichiers de montée en puissance pour les données d’Application](../../failover-clustering/sofs-overview.md)