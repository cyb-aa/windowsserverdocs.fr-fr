---
title: Améliorations de sécurité SMB
description: Explication de la fonctionnalité de chiffrement SMB dans Windows Server 2012 R2, Windows Server 2012 et Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 831ca8266c3ec18ffb83227dcb2d39b3f953ad1a
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233525"
---
# <a name="smb-security-enhancements"></a>Améliorations de sécurité SMB

>S’applique à: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Cette rubrique décrit les améliorations de sécurité SMB dans Windows Server 2012 R2, Windows Server 2012 et Windows Server 2016.

## <a name="smb-encryption"></a>Chiffrement SMB

Chiffrement SMB fournit le chiffrement de bout en bout des données SMB et protège les données des occurrences préviennent sur des réseaux non approuvés. Vous pouvez déployer le chiffrement SMB avec un minimum d’effort, mais vous devrez peut-être petites coûts supplémentaires pour le matériel ou logiciel. Elle n’a aucune configuration requise pour la sécurité du protocole Internet (IPsec) ou accélérateurs de réseau étendu. Chiffrement SMB peuvent être configuré sur une base de partage par ou pour le serveur de la totalité du fichier, et il peut être activé pour divers scénarios où les données parcourt les réseaux non approuvés.

>[!NOTE]
>Chiffrement SMB n’aborde pas la sécurité au repos, qui est généralement gérés par le chiffrement de lecteur BitLocker.

Chiffrement SMB doivent être considéré pour les scénarios dans lesquels les données sensibles doivent être protégé contre les attaques man-in-the-middle. Les scénarios possibles sont les suivantes:

- Les données sensibles du travailleur de l’information sont déplacées à l’aide du protocole SMB. Chiffrement SMB offre une garantie de confidentialité et d’intégrité bout en bout entre le serveur de fichiers et le client, quel que soit les réseaux parcourus, par exemple étendu (WAN) connexions réseau sont gérées par des fournisseurs tiers.
- SMB 3.0 permet aux serveurs de fichiers fournir un stockage en permanence disponible pour les applications serveur, telles que SQL Server ou Hyper-V. Activation de chiffrement SMB permet de protéger contre les attaques snooping. Chiffrement SMB est plus simple à utiliser que les solutions de matériel dédié qui sont requises pour la plupart des réseaux de stockage (SAN).

>[!IMPORTANT]
>Notez qu’il existe une amélioration notable d’exploitation coût avec une protection de chiffrement de bout en bout par rapport aux non chiffrés.

## <a name="enable-smb-encryption"></a>Activer le chiffrement SMB

Vous pouvez activer le chiffrement SMB pour le serveur de fichiers entier ou uniquement pour les partages de fichiers spécifiques. Utilisez une des procédures suivantes pour activer le chiffrement SMB:

### <a name="enable-smb-encryption-with-windows-powershell"></a>Activer le chiffrement SMB avec Windows PowerShell

1. Pour activer le chiffrement SMB pour un partage de fichier individuel, tapez le script suivant sur le serveur:
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. Pour activer le chiffrement SMB pour le serveur de la totalité du fichier, tapez le script suivant sur le serveur:
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. Pour créer un nouveau partage de fichiers SMB avec chiffrement SMB activée, tapez le script suivant:
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>Activer le chiffrement SMB avec le Gestionnaire de serveur

1. Dans le Gestionnaire de serveur, ouvrez le **fichier et Services de stockage**.
2. Sélectionnez **actions** pour ouvrir la page Gestion des partages.
3. Le partage sur lequel vous souhaitez activer le chiffrement SMB d’avec le bouton droit, puis sélectionnez **Propriétés**.
4. Dans la page **paramètres** du partage, sélectionnez **chiffrer l’accès aux données**. Accès au fichier distant à ce partage est chiffré.

### <a name="considerations-for-deploying-smb-encryption"></a>Considérations relatives au déploiement de chiffrement SMB

Par défaut, lorsque le chiffrement SMB est activé pour un partage de fichiers ou un serveur, seuls les clients SMB 3.0 sont autorisés à accéder aux partages de fichier spécifié. Il met en œuvre l’objectif de l’administrateur de sauvegarder les données pour tous les clients qui accèdent aux partages. Toutefois, dans certains cas, un administrateur peut souhaiter autoriser l’accès non chiffré pour les clients qui ne prennent pas en charge SMB 3.0 (par exemple, pendant une période de transition lorsque les versions du système d’exploitation client mixte sont utilisées). Pour autoriser l’accès non chiffré pour les clients qui ne prennent pas en charge SMB 3.0, tapez le script suivant dans Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

La fonctionnalité de négociation sécurisée dialecte décrite dans la section suivante empêche une attaque man-in-the-middle d’est une connexion de 3.0 SMB à SMB 2.0 (utilisez accès non chiffré). Toutefois, elle n’empêche pas une rétrogradation SMB 1.0, entraînant également accès non chiffré. Pour garantir que les clients SMB 3.0 toujours utilisent chiffrement SMB d’accéder aux partages chiffrées, vous devez désactiver le serveur SMB 1.0. (Pour plus d’informations, voir la section [désactivation SMB 1.0](#disabling-smb-1.0)). Si le paramètre **– RejectUnencryptedAccess** reste à sa valeur par défaut de **$true**, uniquement capable de chiffrement SMB 3.0 clients sont autorisés à accéder aux partages de fichiers (SMB 1.0 clients peut également être rejetées).

>[!NOTE]
>* Chiffrement SMB utilise le chiffrement AES (Advanced Standard)-algorithme CCM pour chiffrer et déchiffrer les données. AES-CCM fournit également la validation de l’intégrité des données (signature) pour les partages de fichiers, quel que soit les paramètres de signature SMB. Si vous souhaitez activer la signature sans chiffrement SMB, vous pouvez continuer à cela. Pour plus d’informations, voir [Les concepts de base de la signature SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).
>* Vous pouvez rencontrer des problèmes lorsque vous essayez d’accéder le partage de fichiers ou le serveur, si votre organisation utilise des équipements d’accélération WAN réseau (étendu WAN).
>* Avec une configuration par défaut (où il n’existe aucun accès non chiffré autorisé aux partages de fichiers chiffrés), si les clients qui ne prennent pas en charge la tentative SMB 3.0 pour accéder à un partage de fichiers, ID d’événement 1003 sont consignées dans le journal des événements Microsoft-Windows-SmbServer/opérationnel , et le client reçoit un message d’erreur **accès refusé** .
>* Chiffrement SMB et le système de fichier de cryptage (EFS) dans le système de fichiers NTFS ne sont pas liés et chiffrement SMB ne nécessitent pas ou ne dépendent de l’utilisation d’EFS.
>* Chiffrement SMB et le chiffrement de lecteur BitLocker ne sont pas liés et chiffrement SMB ne nécessitent pas ou ne dépend de chiffrement de lecteur BitLocker.

## <a name="secure-dialect-negotiation"></a>Sécuriser la négociation de dialecte

SMB 3.0 est capable de détecter les attaques man-in-the-middle visant à rétrograder le protocole SMB 2.0 ou 3.0 SMB ou les fonctionnalités de négocier le client et le serveur. Lorsque ce type d’attaque est détectée par le client ou le serveur, la connexion est déconnectée et ID d’événement 1005 est consigné dans le journal des événements Microsoft-Windows-SmbServer/opérationnel. Sécuriser le dialecte négociation ne peut pas détecter ou empêcher les baisses de SMB 2.0 ou 3.0 SMB 1.0. Pour cette raison et pour tirer parti des fonctionnalités complètes de chiffrement SMB, nous vous recommandons vivement de désactiver le serveur SMB 1.0. Pour plus d’informations, voir [désactivation SMB 1.0](#disabling-smb-1.0).

La capacité de la négociation de dialecte sécurisé qui est décrite dans la section suivante empêche une attaque man-in-the-middle de mise à niveau d’une connexion de 3 SMB SMB 2 (utilisez accès non chiffré); Toutefois, elle n’empêche pas les anciens SMB 1, ce qui donne également accès non chiffré. Pour plus d’informations sur les problèmes potentiels avec précédemment non-Windows implémentations de SMB, reportez-vous à la [Base de connaissances Microsoft](http://support.microsoft.com/kb/2686098).

## <a name="new-signing-algorithm"></a>Nouvel algorithme de signature

SMB 3.0 utilise un algorithme de cryptage plus récent pour la signature: le chiffrement AES (Advanced Standard) - chiffrement - en fonction de code d’authentification de message (CMAC). SMB 2.0 utilisé l’algorithme de chiffrement HMAC-SHA256 antérieur. AES-CMAC et AES-CCM peuvent accélérer considérablement le chiffrement des données sur les plus modernes UC dont l’instruction AES prennent en charge. Pour plus d’informations, voir [Les concepts de base de la signature SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).

## <a name="disabling-smb-10"></a>Désactivation de SMB 1.0

Les fonctionnalités de protocole de l’Administration à distance dans SMB 1.0 et le service Explorateur d’ordinateurs hérités sont désormais distinctes, et qu’ils peuvent être éliminés. Ces fonctionnalités sont toujours activées par défaut, mais si vous n’avez pas d’anciens clients SMB, tels que des ordinateurs qui exécutent Windows Server 2003 ou Windows XP, vous pouvez supprimer les fonctionnalités SMB 1.0 pour renforcer la sécurité et réduire potentiellement la mise à jour corrective.

>[!NOTE]
>SMB 2.0 a été introduite dans Windows Server 2008 et Windows Vista. Des clients antérieurs, tels que des ordinateurs qui exécutent Windows Server 2003 ou Windows XP, ne prennent pas en charge SMB 2.0; et, par conséquent, ils ne seront pas en mesure d’accéder aux partages de fichiers ou l’impression des partages si le serveur SMB 1.0 est désactivé. En outre, certains clients SMB non-Microsoft ne peuvent pas être en mesure d’accéder aux partages de fichiers SMB 2.0 ou l’impression des actions (par exemple, les imprimantes avec la fonctionnalité «analyse à partager»).

Avant de commencer la désactivation SMB 1.0, vous devez savoir si vos clients SMB sont actuellement connectés au serveur exécutant SMB 1.0. Pour ce faire, entrez l’applet de commande suivante dans Windows PowerShell:

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>Vous devez exécuter ce script à plusieurs reprises au cours d’une semaine (plusieurs fois par jour) pour créer un journal d’audit. Vous pouvez également exécuter ce comme une tâche planifiée.

Pour désactiver SMB 1.0, entrez le script suivant dans Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>Si une connexion de client SMB est refusée car le serveur qui exécute SMB 1.0 a été désactivé, l’événement 1001 ID sera connecté dans le journal des événements Microsoft-Windows-SmbServer/opérationnel.

## <a name="more-information"></a>Informations supplémentaires

Voici quelques ressources supplémentaires sur les PME/PMI et les technologies associées dans Windows Server 2012.

- [SMB](file-server-smb-overview.md)
- [Stockage dans WindowsServer](../storage.md)
- [Serveur de fichiers de mise à niveau pour les données d’Application](../../failover-clustering/sofs-overview.md)