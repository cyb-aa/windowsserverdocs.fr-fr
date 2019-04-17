---
title: Améliorer les performances d’un serveur de fichiers avec SMB Direct
description: Décrit la fonctionnalité SMB Direct dans Windows Server 2012 R2, Windows Server 2012 et Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: ed8fd5b4114fc9fd9c7dc278a98cea8cc67a8749
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239226"
---
# SMB Direct

>S’applique à: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Windows Server 2012 R2, Windows Server 2012 et Windows Server 2016 incluent une fonctionnalité appelée SMB Direct, qui prend en charge l’utilisation de cartes réseau qui ont la fonctionnalité d’accès de mémoire Direct à distance (RDMA). Les cartes réseau qui ont RDMA peuvent fonctionner à la vitesse complète avec une très faible latence, lors de l’utilisation du processeur très peu. Pour les charges de travail telles que Microsoft SQL Server ou Hyper-V, cela permet à un serveur de fichiers à distance ressembler à un stockage local. SMB Direct comprend:

- Augmenter la productivité: s’appuie sur le débit complète des réseaux de haut débit dans lesquels les cartes réseau coordonner le transfert de grandes quantités de données à la vitesse de ligne.
- Faible latence: fournit des réponses aux demandes réseau extrêmement rapides et, par conséquent, met le stockage de fichiers à distance paraître comme s’il s’agit de stockage par blocs directement connectés.
- L’utilisation du processeur faible: utilise les cycles de processeur moins quand transférer les données sur le réseau, qui laisse plus mis sous tension disponibles pour les applications de serveur.

SMB Direct est configuré automatiquement par Windows Server 2012 R2 et Windows Server 2012.

## SMB Multichannel et SMB Direct

SMB Multichannel est la fonctionnalité chargée de détecter les fonctionnalités RDMA de cartes réseau pour activer SMB Direct. Sans SMB Multichannel, SMB utilise TCP/IP régulières avec les cartes réseau RDMA compatible (toutes les cartes réseau fournissent une pile TCP/IP, ainsi que la nouvelle pile RDMA).

Avec SMB Multichannel, SMB détecte si une carte réseau a la capacité RDMA et crée ensuite plusieurs connexions RDMA pour cette session unique (deux par interface). Cela permet de SMB à utiliser le débit élevé, faible latence et l’UC faible offerte par les cartes réseau RDMA compatible. Elle propose également une tolérance de panne si vous utilisez plusieurs interfaces RDMA.

>[!NOTE]
>Vous ne devez pas équipe de cartes réseau RDMA compatible si vous envisagez d’utiliser la fonctionnalité RDMA des cartes réseau. Lors de l’équipe, les cartes réseau ne prendra pas en charge RDMA.
>Une fois au moins une connexion de réseau RDMA est créée, la connexion TCP/IP utilisée pour la négociation du protocole d’origine n’est plus utilisée. Toutefois, la connexion TCP/IP est conservée en cas d’échouent de connexions réseau RDMA.

## Conditions requises

SMB Direct nécessite les éléments suivants:

- Au moins deux ordinateurs exécutant Windows Server 2012 R2 ou Windows Server 2012
- Une ou plusieurs cartes réseau avec la fonctionnalité RDMA.

### Considérations lors de l’utilisation SMB Direct

- Vous pouvez utiliser SMB Direct dans un cluster de basculement; Toutefois, vous devez vous assurer que les réseaux de clusters utilisés pour l’accès client sont adaptées à SMB Direct. Prend en charge à l’aide de plusieurs réseaux pour l’accès client le clustering de basculement, ainsi que des cartes réseau qui sont RSS (l’évolutivité côté réception)-capable et compatibles RDMA.
- Vous pouvez utiliser SMB Direct sur le système d’exploitation de gestion Hyper-V pour prendre en charge à l’aide d’Hyper-V sur SMB et pour fournir un stockage à un ordinateur virtuel qui utilise la pile de stockage Hyper-V. Toutefois, les adaptateurs réseau compatibles RDMA ne sont pas exposés directement à un client Hyper-V. Si vous vous connectez une carte réseau RDMA compatible à un commutateur virtuel, il se peut que les cartes réseau virtuelles à partir du commutateur ne sera pas compatible avec RDMA.
- Si vous désactivez SMB Multichannel, SMB Direct est également désactivé. Dans la mesure où SMB Multichannel détecte les fonctionnalités de carte réseau et détermine si une carte réseau est compatible avec RDMA, SMB Direct ne peut pas être utilisé par le client si SMB Multichannel est désactivé.
- SMB Direct n’est pas pris en charge sur Windows le modèle RT. SMB Direct nécessite prise en charge pour les cartes réseau RDMA compatible, qui est disponible uniquement sur Windows Server 2012 R2 et Windows Server 2012.
- SMB Direct n’est pas pris en charge sur les versions de bas niveau de Windows Server. Il est pris en charge uniquement sur Windows Server 2012 R2 et Windows Server 2012.

## Activation et désactivation des SMB Direct

SMB Direct est activé par défaut lorsque Windows Server 2012 R2 ou Windows Server 2012 est installé. Le client SMB détecte automatiquement et utilise plusieurs connexions réseau si une configuration appropriée est identifiée.

### Désactiver SMB Direct

En règle générale, vous n’aurez pas à désactiver SMB Direct, toutefois, vous pouvez la désactiver en exécutant l’une des scripts Windows PowerShell suivantes.

Pour désactiver RDMA pour une interface spécifique, tapez:

```PowerShell
Disable-NetAdapterRdma <name>
```

Pour désactiver RDMA pour toutes les interfaces, tapez:

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Disabled
```

Lorsque vous désactivez RDMA sur le client ou le serveur, les systèmes ne peuvent pas l’utiliser. *Réseau Direct* est le nom interne pour Windows Server 2012 R2 et Windows Server 2012 base prise en charge réseau pour les interfaces RDMA.

### Réactivez SMB Direct

Après avoir désactivé RDMA, vous pouvez réactiver en exécutant l’une des scripts Windows PowerShell suivantes.

Pour réactiver RDMA pour une interface spécifique, tapez:

```PowerShell
Enable-NetAdapterRDMA <name>
```

Pour réactiver RDMA pour toutes les interfaces, tapez:

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Enabled
```

Vous devez activer RDMA sur le client et le serveur pour commencer à l’utiliser à nouveau.

## Tester les performances de SMB Direct

Vous pouvez tester le fonctionne de l’exécution en utilisant l’une des procédures suivantes.

### Comparer une copie de fichier avec et sans utiliser SMB Direct

Voici comment procéder pour mesurer le débit accru de SMB Direct:

1. Configurer les SMB Direct
2. Mesurer la quantité de temps d’exécuter une copie des fichiers volumineux à l’aide de SMB Direct.
3. Désactiver RDMA sur la carte réseau, consultez [activation et désactivation de SMB Direct](#enabling-and-disabling-smb-direct).
4. Mesurer la quantité de temps d’exécuter une copie des fichiers volumineux sans utiliser SMB Direct.
5. Réactivez RDMA sur la carte réseau et comparez les résultats de deux.
6. Pour éviter l’impact de la mise en cache, vous devez effectuer les opérations suivantes:
    1. Copier une grande quantité de données (plus de données que la mémoire est capable de gestion).
    2. Copiez les données de deux fois, à la première copie en tant que pratique et le minutage ensuite la deuxième copie.
    3. Redémarrer le serveur et le client avant de chaque test pour vous assurer que ces outils fonctionnent dans des conditions similaires.

### L’une de plusieurs cartes réseau échouer lors d’une copie de fichier avec SMB Direct

Voici comment procéder vérifier que la fonctionnalité de basculement de SMB Direct:

1. Assurez-vous que SMB Direct fonctionne dans une configuration de carte réseau multiples.
2. Exécutez une copie des fichiers volumineux. Tandis que la copie est exécutée, simulez une panne de l’un des chemins d’accès réseau en déconnectant un des câbles (ou en désactivant l’une des cartes réseau).
3. Confirmer que la copie des fichiers continue à l’aide de l’une des cartes réseau restants, et qu’il n’y a aucune erreur de copie de fichier.

>[!NOTE]
>Pour éviter les échecs d’une charge de travail qui n’utilise pas de SMB Direct, assurez-vous qu’il n’existe aucune autres charges de travail en utilisant le chemin réseau déconnecté.

## Informations supplémentaires

- [Vue d’ensemble de Server Message Block](file-server-smb-overview.md)
- [Augmentation de serveur, de stockage et de disponibilité du réseau: vue d’ensemble du scénario](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [Déployer Hyper\-V sur SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
