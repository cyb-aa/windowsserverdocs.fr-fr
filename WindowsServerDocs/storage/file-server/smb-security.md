---
title: Améliorations en matière de sécurité SMB
description: Explication de la fonctionnalité Chiffrement SMB dans Windows Server 2012 R2, Windows Server 2012 et Windows Server 2016.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: d7b96574dcfc2a4417aa36780d7bd87c2556f61f
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "75950260"
---
# <a name="smb-security-enhancements"></a>Améliorations en matière de sécurité SMB

>S'applique à : Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Cette rubrique explique les améliorations apportées à la sécurité SMB dans Windows Server 2012 R2, Windows Server 2012 et Windows Server 2016.

## <a name="smb-encryption"></a>Chiffrement SMB

La fonctionnalité Chiffrement SMB permet un chiffrement de bout en bout des données SMB et protège les données contre les tentatives d’écoute clandestine sur des réseaux non approuvés. Vous pouvez déployer la fonctionnalité Chiffrement SMB facilement, mais elle peut impliquer des coûts supplémentaires liés au matériel ou aux logiciels spécialisés. Elle n’exige pas de protocole IPsec (Internet Protocol Security) ni d’accélérateurs WAN. La fonctionnalité Chiffrement SMB peut être configurée par partage ou pour le serveur de fichiers tout entier et vous pouvez l’activer pour un large éventail de scénarios où les données parcourent des réseaux non approuvés.

>[!NOTE]
>Elle ne couvre pas la sécurité au repos, qui est généralement gérée par la fonctionnalité Chiffrement de lecteur BitLocker.

Considérez la fonctionnalité Chiffrement SMB pour tout scénario dans lequel les données sensibles ont besoin d’être protégées contre les attaques de l’intercepteur. Les scénarios possibles sont :

- Les données sensibles d’un travailleur de l’information sont déplacées à l’aide du protocole SMB. La fonctionnalité Chiffrement SMB assure la confidentialité et l’intégrité de bout en bout entre le serveur de fichiers et le client, quels que soient les réseaux parcourus, comme des connexions de réseau étendu (WAN) gérées par des fournisseurs non-Microsoft.
- SMB 3.0 permet aux serveurs de fichiers de fournir un stockage disponible en continu pour les applications serveur, comme SQL Server ou Hyper-V. L’activation de la fonctionnalité Chiffrement SMB offre la possibilité de protéger ces informations contre les attaques par espionnage. La fonctionnalité est plus simple à utiliser que les solutions matérielles dédiées nécessaires à la plupart des réseaux de zone de stockage (SAN).

>[!IMPORTANT]
>Notez qu’il existe un coût d’exploitation des performances notable avec une protection à chiffrement de bout en bout par rapport à une protection non chiffrée.

## <a name="enable-smb-encryption"></a>Activer Chiffrement SMB

Vous pouvez activer la fonctionnalité Chiffrement SMB pour l’ensemble du serveur de fichiers ou uniquement pour des partages de fichiers spécifiques. Utilisez l’une des procédures suivantes pour activer la fonctionnalité Chiffrement SMB :

### <a name="enable-smb-encryption-with-windows-powershell"></a>Activer la fonctionnalité Chiffrement SMB avec Windows PowerShell

1. Pour activer la fonctionnalité Chiffrement SMB pour un partage de fichiers individuel, tapez le script suivant sur le serveur :
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. Pour activer la fonctionnalité Chiffrement SMB pour l’ensemble du serveur de fichiers, tapez le script suivant sur le serveur :
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. Pour créer un partage de fichiers SMB avec la fonctionnalité Chiffrement SMB activée, tapez le script suivant :
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>Activer la fonctionnalité Chiffrement SMB avec le Gestionnaire de serveur

1. Dans le Gestionnaire de serveur, ouvrez **Services de fichiers et de stockage**.
2. Sélectionnez **Partages** pour ouvrir la page de gestion des partages.
3. Cliquez avec le bouton droit sur le partage pour lequel vous voulez activer la fonctionnalité Chiffrement SMB, puis sélectionnez **Propriétés**.
4. Dans la page **Paramètres** du partage, sélectionnez **Chiffrer l’accès aux données**. L’accès aux fichiers distants de ce partage est chiffré.

### <a name="considerations-for-deploying-smb-encryption"></a>Considérations relatives au déploiement de la fonctionnalité Chiffrement SMB

Par défaut, lorsque la fonctionnalité Chiffrement SMB est activée pour un partage ou serveur de fichiers, seuls les clients SMB 3.0 sont autorisés à accéder aux partages de fichiers spécifiés. L’administrateur impose ainsi son intention de protéger les données de tous les clients qui accèdent aux partages. Toutefois, dans certains cas, un administrateur peut avoir besoin d’autoriser un accès non chiffré pour les clients qui ne prennent pas en charge SMB 3.0 (par exemple, pendant une période de transition où des versions de système d’exploitation client diverses sont utilisées). Pour autoriser l’accès non chiffré pour les clients qui ne prennent pas en charge SMB 3.0, tapez le script suivant dans Windows PowerShell :

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

La fonction de négociation de dialecte sécurisée décrite dans la section suivante empêche une éventuelle attaque de l’intercepteur de rétrograder une connexion de SMB 3.0 à SMB 2.0 (qui utiliserait un accès non chiffré). Toutefois, elle n’empêche pas une rétrogradation à SMB 1.0, qui déboucherait aussi sur un accès non chiffré. Pour garantir que les clients SMB 3.0 utilisent toujours la fonctionnalité Chiffrement SMB pour accéder aux partages chiffrés, vous devez désactiver le serveur SMB 1.0. (Pour obtenir des instructions, consultez la section [Désactivation de SMB 1.0](#disabling-smb-10).) Si le paramètre **– RejectUnencryptedAccess** conserve sa valeur par défaut **$true**, seuls les clients SMB 3 0 compatibles avec le chiffrement sont autorisés à accéder aux partages de fichiers (les clients SMB 1.0 sont également refusés).

>[!NOTE]
>* La fonctionnalité Chiffrement SMB utilise l’algorithme Advanced Encryption Standard (AES)-CCM pour chiffrer et déchiffrer les données. L’algorithme AES-CCM assure également la validation de l’intégrité des données (signature) pour les partages de fichiers chiffrés, quels que soient les paramètres de signature SMB. Si vous voulez activer la signature SMB sans chiffrement, vous pouvez continuer à le faire. Pour plus d’informations, consultez l’article sur les [bases de la signature SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).
>* Vous risquez de rencontrer des problèmes quand vous tentez d’accéder au partage de fichiers ou serveur si votre organisation utilise des appliances d’accélération de réseau étendu (WAN).
>* Avec une configuration par défaut (où aucun accès non chiffré aux partages de fichiers chiffrés n’est autorisé), si les clients qui ne prennent pas en charge SMB 3.0 tentent d’accéder à un partage de fichiers chiffré, l’ID d’événement 1003 est enregistré dans le journal des événements Microsoft-Windows-SmbServer/Operational et le client reçoit un message d’erreur **Accès refusé**.
>* La fonctionnalité Chiffrement SMB et le système de fichiers EFS inclus dans le système de fichiers NTFS ne sont pas liés. La fonctionnalité Chiffrement SMB n’a pas besoin ou ne dépend pas de l’utilisation d’EFS.
>* Les fonctionnalités Chiffrement SMB et Chiffrement de lecteur BitLocker ne sont pas liées, et la fonctionnalité Chiffrement SMB n’a pas besoin ou ne dépend pas de l’utilisation de Chiffrement de lecteur BitLocker.

## <a name="secure-dialect-negotiation"></a>Négociation de dialecte sécurisée

SMB 3.0 est capable de détecter les attaques de l’intercepteur qui tentent de rétrograder le protocole SMB 2.0 ou SMB 3.0 ou les fonctionnalités que le client et le serveur négocient. Quand une telle attaque est détectée par le client ou le serveur, la connexion est rompue et l’ID d’événement 1005 est enregistré dans le journal des événements Microsoft-Windows-SmbServer/Operational. La négociation de dialecte sécurisée ne peut pas détecter ni empêcher les rétrogradations de SMB 2.0 ou 3.0 à SMB 1.0. C’est pour cette raison, et pour exploiter toutes les capacités de la fonctionnalité Chiffrement SMB, que nous vous recommandons vivement de désactiver le serveur SMB 1.0. Pour plus d’informations, consultez [Désactivation de SMB 1.0](#disabling-smb-10).

La fonction de négociation de dialecte sécurisée décrite dans la section suivante permet d’empêcher une attaque de l’intercepteur de rétrograder une connexion de SMB 3 à SMB 2 (qui pourrait utiliser un accès non chiffré). En revanche, elle n’empêche pas les rétrogradations à SMB 1, qui pourraient aussi déboucher sur un accès non chiffré. Pour plus d’informations sur les problèmes potentiels liés aux implémentations non-Windows antérieures de SMB, consultez la [Base de connaissances Microsoft](https://support.microsoft.com/kb/2686098).

## <a name="new-signing-algorithm"></a>Nouvel algorithme de signature

SMB 3.0 utilise un algorithme de chiffrement plus récent pour la signature : AES-CMAC (Advanced Encryption Standard-Cipher-based Message Authentication Code). SMB 2.0 utilisait l’ancien algorithme de chiffrement HMAC-SHA256. AES-CMAC et AES-CCM peuvent accélérer considérablement le chiffrement des données sur la plupart des processeurs modernes prenant en charge les instructions AES. Pour plus d’informations, consultez l’article sur les [bases de la signature SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).

## <a name="disabling-smb-10"></a>Désactivation de SMB 1.0

Les fonctionnalités du service Explorateur d’ordinateurs hérité et du protocole RAP sont désormais séparées dans SMB 1.0 et peuvent être supprimées. Ces fonctionnalités sont toujours activées par défaut, mais si vous n’utilisez pas d’anciens clients SMB, comme des ordinateurs qui exécutent Windows Server 2003 ou Windows XP, vous pouvez supprimer les fonctionnalités SMB 1.0 pour renforcer la sécurité et limiter les mises à jour correctives.

>[!NOTE]
>SMB 2.0 a été introduit dans Windows Server 2008 et Windows Vista. Les clients plus anciens, comme les ordinateurs exécutant Windows Server 2003 ou Windows XP, ne prennent pas en charge SMB 2.0. Par conséquent, ils ne peuvent pas accéder aux partages de fichiers ou aux partages d’impression si le serveur SMB 1.0 est désactivé. De plus, certains clients SMB non-Microsoft peuvent ne pas être en mesure d’accéder aux partages de fichiers ou partages d’impression SMB 2.0 (par exemple, les imprimantes dotées d’une fonctionnalité de « numérisation à des fins de partage »).

Avant de commencer à désactiver SMB 1.0, vous devez déterminer si vos clients SMB sont actuellement connectés au serveur qui exécute SMB 1.0. Pour cela, entrez l’applet de commande suivante dans Windows PowerShell :

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>Vous devez exécuter ce script plusieurs fois sur une semaine (plusieurs fois par jour) pour générer une piste d’audit. Vous pouvez également l’exécuter en tant que tâche planifiée.

Pour désactiver SMB 1.0, entrez le script suivant dans Windows PowerShell :

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>Si une connexion de client SMB est refusée parce que le serveur exécutant SMB 1.0 a été désactivé, l’ID d’événement 1001 est enregistré dans le journal des événements Microsoft-Windows-SmbServer/Operational.

## <a name="more-information"></a>Autres informations

Voici quelques ressources supplémentaires sur SMB et les technologies associées dans Windows Server 2012.

- [Protocole SMB](file-server-smb-overview.md)
- [Storage](../storage.md) (Stockage)
- [Serveur de fichiers avec montée en puissance parallèle pour les données d’application](../../failover-clustering/sofs-overview.md)