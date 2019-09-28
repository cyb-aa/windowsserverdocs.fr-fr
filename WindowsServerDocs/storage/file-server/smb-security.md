---
title: Améliorations en matière de sécurité SMB
description: Explication de la fonctionnalité de chiffrement SMB dans Windows Server 2012 R2, Windows Server 2012 et Windows Server 2016.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7221d3ea94ff9f2d7fca8e95cee66597e2dc6270
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402064"
---
# <a name="smb-security-enhancements"></a>Améliorations en matière de sécurité SMB

>S’applique à : Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Cette rubrique explique les améliorations de la sécurité SMB dans Windows Server 2012 R2, Windows Server 2012 et Windows Server 2016.

## <a name="smb-encryption"></a>Chiffrement SMB

Le chiffrement SMB fournit un chiffrement de bout en bout des données SMB et protège les données contre les écoutes clandestines sur des réseaux non approuvés. Vous pouvez déployer le chiffrement SMB avec un minimum d’effort, mais cela peut nécessiter des coûts supplémentaires pour le matériel ou les logiciels spécialisés. Il n’est pas nécessaire pour la sécurité du protocole Internet (IPsec) ou les accélérateurs WAN. Le chiffrement SMB peut être configuré sur une base par partage ou pour l’ensemble du serveur de fichiers, et il peut être activé pour divers scénarios dans lesquels les données traversent des réseaux non approuvés.

>[!NOTE]
>Le chiffrement SMB ne couvre pas la sécurité au repos, qui est généralement gérée par Chiffrement de lecteur BitLocker.

Le chiffrement SMB doit être pris en compte pour tout scénario dans lequel les données sensibles doivent être protégées contre les attaques de l’intercepteur. Les scénarios possibles sont :

- Les données sensibles d’un travailleur de l’information sont déplacées à l’aide du protocole SMB. Le chiffrement SMB offre une assurance de la confidentialité et de l’intégrité de bout en bout entre le serveur de fichiers et le client, quels que soient les réseaux parcourus, tels que les connexions de réseau étendu (WAN) qui sont gérées par des fournisseurs non-Microsoft.
- SMB 3,0 permet aux serveurs de fichiers de fournir un stockage disponible en continu pour les applications serveur, telles que SQL Server ou Hyper-V. L’activation du chiffrement SMB offre la possibilité de protéger ces informations contre les attaques d’espionnage. Le chiffrement SMB est plus simple à utiliser que les solutions matérielles dédiées requises pour la plupart des réseaux de zone de stockage (San).

>[!IMPORTANT]
>Notez qu’il existe un coût d’exploitation de performances notable avec une protection de chiffrement de bout en bout par rapport à un chiffrement non chiffré.

## <a name="enable-smb-encryption"></a>Activer le chiffrement SMB

Vous pouvez activer le chiffrement SMB pour l’ensemble du serveur de fichiers ou uniquement pour des partages de fichiers spécifiques. Utilisez l’une des procédures suivantes pour activer le chiffrement SMB :

### <a name="enable-smb-encryption-with-windows-powershell"></a>Activer le chiffrement SMB avec Windows PowerShell

1. Pour activer le chiffrement SMB pour un partage de fichiers individuel, tapez le script suivant sur le serveur :
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. Pour activer le chiffrement SMB pour l’ensemble du serveur de fichiers, tapez le script suivant sur le serveur :
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. Pour créer un nouveau partage de fichiers SMB avec le chiffrement SMB activé, tapez le script suivant :
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>Activer le chiffrement SMB avec Gestionnaire de serveur

1. Dans Gestionnaire de serveur, ouvrez **services de fichiers et de stockage**.
2. Sélectionnez **partages** pour ouvrir la page de gestion des partages.
3. Cliquez avec le bouton droit sur le partage sur lequel vous souhaitez activer le chiffrement SMB, puis sélectionnez **Propriétés**.
4. Sur la page **paramètres** du partage, sélectionnez **chiffrer l’accès aux données**. L’accès au fichier distant à ce partage est chiffré.

### <a name="considerations-for-deploying-smb-encryption"></a>Considérations relatives au déploiement du chiffrement SMB

Par défaut, lorsque le chiffrement SMB est activé pour un partage de fichiers ou un serveur, seuls les clients SMB 3,0 sont autorisés à accéder aux partages de fichiers spécifiés. Cela garantit l’intention de l’administrateur de protéger les données pour tous les clients qui accèdent aux partages. Toutefois, dans certains cas, un administrateur peut souhaiter autoriser un accès non chiffré pour les clients qui ne prennent pas en charge SMB 3,0 (par exemple, pendant une période de transition lorsque des versions de système d’exploitation client mixte sont utilisées). Pour autoriser l’accès non chiffré pour les clients qui ne prennent pas en charge SMB 3,0, tapez le script suivant dans Windows PowerShell :

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

La fonctionnalité de négociation de dialecte sécurisé décrite dans la section suivante empêche une attaque de l’intercepteur de rétrograder une connexion de SMB 3,0 vers SMB 2,0 (qui utiliserait un accès non chiffré). Toutefois, cela n’empêche pas la rétrogradation vers SMB 1,0, ce qui entraînerait également un accès non chiffré. Pour garantir que les clients SMB 3,0 utilisent toujours le chiffrement SMB pour accéder aux partages chiffrés, vous devez désactiver le serveur SMB 1,0. (Pour obtenir des instructions, consultez la section [désactivation de SMB 1,0](#disabling-smb-10).) Si le paramètre **– RejectUnencryptedAccess** a la valeur par défaut de **$true**, seuls les clients SMB 3,0 compatibles avec le chiffrement sont autorisés à accéder aux partages de fichiers (les clients SMB 1,0 seront également rejetés).

>[!NOTE]
>* Le chiffrement SMB utilise l’algorithme Advanced Encryption Standard (AES)-CCM pour chiffrer et déchiffrer les données. AES-CCM assure également la validation de l’intégrité des données (signature) pour les partages de fichiers chiffrés, quels que soient les paramètres de signature SMB. Si vous souhaitez activer la signature SMB sans chiffrement, vous pouvez continuer à le faire. Pour plus d’informations, consultez [les principes de base de la signature SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).
>* Vous pouvez rencontrer des problèmes lorsque vous tentez d’accéder au partage de fichiers ou au serveur si votre organisation utilise des appliances d’accélération de réseau étendu (WAN).
>* Avec une configuration par défaut (où aucun accès non chiffré n’est autorisé pour les partages de fichiers chiffrés), si les clients qui ne prennent pas en charge SMB 3,0 essaient d’accéder à un partage de fichiers chiffré, l’ID d’événement 1003 est enregistré dans le journal des événements opérationnels de Microsoft-Windows-SmbServer/. et le client recevra un message d’erreur d' **accès refusé** .
>* Le chiffrement SMB et le système de fichiers EFS (EFS) dans le système de fichiers NTFS ne sont pas liés, et le chiffrement SMB ne nécessite pas ou ne dépend pas de l’utilisation de EFS.
>* Le chiffrement SMB et le Chiffrement de lecteur BitLocker ne sont pas liés, et le chiffrement SMB ne nécessite pas ou ne dépend pas de l’utilisation de Chiffrement de lecteur BitLocker.

## <a name="secure-dialect-negotiation"></a>Négociation de dialecte sécurisée

SMB 3,0 est capable de détecter les attaques de l’intercepteur qui tentent de rétrograder le protocole SMB 2,0 ou SMB 3,0 ou les fonctionnalités négociées par le client et le serveur. Lorsqu’une telle attaque est détectée par le client ou le serveur, la connexion est déconnectée et l’ID d’événement 1005 est consigné dans le journal des événements Microsoft-Windows-SmbServer/Operational. La négociation de dialecte sécurisé ne peut pas détecter ou empêcher les rétrogradations de SMB 2,0 ou 3,0 vers SMB 1,0. Pour cette raison, et pour tirer parti des fonctionnalités complètes du chiffrement SMB, nous vous recommandons vivement de désactiver le serveur SMB 1,0. Pour plus d’informations, consultez [désactivation de SMB 1,0](#disabling-smb-10).

La fonctionnalité de négociation de dialecte sécurisé décrite dans la section suivante empêche une attaque de l’intercepteur de la rétrogradation d’une connexion de SMB 3 vers SMB 2 (qui utiliserait un accès non chiffré). Toutefois, elle n’empêche pas les rétrogradations vers SMB 1, ce qui entraînerait également un accès non chiffré. Pour plus d’informations sur les problèmes potentiels liés aux implémentations antérieures à Windows de SMB, consultez la [base de connaissances Microsoft](http://support.microsoft.com/kb/2686098).

## <a name="new-signing-algorithm"></a>Nouvel algorithme de signature

SMB 3,0 utilise un algorithme de chiffrement plus récent pour la signature : Advanced Encryption Standard (AES)-code d’authentification de message basé sur le chiffrement (CMAC). SMB 2,0 a utilisé l’ancien algorithme de chiffrement HMAC-SHA256. AES-CMAC et AES-CCM peuvent accélérer considérablement le chiffrement des données sur la plupart des processeurs modernes disposant d’une prise en charge des instructions AES. Pour plus d’informations, consultez [les principes de base de la signature SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).

## <a name="disabling-smb-10"></a>Désactivation de SMB 1,0

Les fonctionnalités héritées du service Explorateur d’ordinateurs et du protocole d’administration à distance de SMB 1,0 sont désormais séparées et peuvent être éliminées. Ces fonctionnalités sont toujours activées par défaut, mais si vous n’avez pas de clients SMB plus anciens, tels que des ordinateurs exécutant Windows Server 2003 ou Windows XP, vous pouvez supprimer les fonctionnalités SMB 1,0 pour renforcer la sécurité et éventuellement réduire les correctifs.

>[!NOTE]
>SMB 2,0 a été introduit dans Windows Server 2008 et Windows Vista. Les clients plus anciens, tels que les ordinateurs exécutant Windows Server 2003 ou Windows XP, ne prennent pas en charge SMB 2,0 ; et par conséquent, ils ne peuvent pas accéder aux partages de fichiers ou aux partages d’impression si le serveur SMB 1,0 est désactivé. En outre, certains clients SMB non-Microsoft peuvent ne pas être en mesure d’accéder aux partages de fichiers SMB 2,0 ou aux partages d’impression (par exemple, les imprimantes avec la fonctionnalité « analyser sur le partage »).

Avant de commencer la désactivation de SMB 1,0, vous devez déterminer si vos clients SMB sont actuellement connectés au serveur exécutant SMB 1,0. Pour ce faire, entrez l’applet de commande suivante dans Windows PowerShell :

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>Vous devez exécuter ce script de manière répétée au cours d’une semaine (plusieurs fois par jour) pour créer une piste d’audit. Vous pouvez également l’exécuter en tant que tâche planifiée.

Pour désactiver SMB 1,0, entrez le script suivant dans Windows PowerShell :

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>Si une connexion client SMB est refusée parce que le serveur exécutant SMB 1,0 a été désactivé, l’ID d’événement 1001 sera consigné dans le journal des événements Microsoft-Windows-SmbServer/Operational.

## <a name="more-information"></a>Plus d’informations

Voici quelques ressources supplémentaires sur SMB et les technologies associées dans Windows Server 2012.

- [SMB (Server Message Block)](file-server-smb-overview.md)
- [Storage](../storage.md) (Stockage)
- [Serveur de fichiers avec montée en puissance parallèle pour les données d’application](../../failover-clustering/sofs-overview.md)