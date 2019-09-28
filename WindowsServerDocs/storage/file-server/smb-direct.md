---
title: Améliorer les performances d’un serveur de fichiers avec SMB direct
description: Décrit la fonctionnalité SMB direct dans Windows Server 2012 R2, Windows Server 2012 et Windows Server 2016.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 41126aa0d054607449d57928c1777679e5087e73
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394458"
---
# <a name="smb-direct"></a>SMB Direct

>S’applique à : Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Windows Server 2012 R2, Windows Server 2012 et Windows Server 2016 incluent une fonctionnalité appelée SMB direct, qui prend en charge l’utilisation de cartes réseau dotées de la fonctionnalité d’accès direct à la mémoire à distance (RDMA). Les cartes réseau dotées de la fonction RDMA sont capables de fonctionner à pleine vitesse avec une très faible latence et une consommation minime du processeur. Pour des charges de travail de type Hyper-V ou Microsoft SQL Server, cette fonctionnalité permet à un serveur de fichiers distant d’agir comme un système de stockage local. SMB Direct présente les avantages suivants :

- Débit accru : il tire parti du débit complet des réseaux à haut débit dans lesquels les cartes réseau coordonnent le transfert de grandes quantités de données à la vitesse de ligne.
- Latence faible : il fournit des réponses extrêmement rapides aux demandes du réseau et il semble par conséquent que le stockage de fichiers étendu soit un stockage de bloc directement attaché.
- Utilisation faible du processeur : il utilise moins de cycles processeur lors du transfert de données sur le réseau, ce qui laisse plus d’alimentation disponible pour les applications serveur.

SMB direct est automatiquement configuré par Windows Server 2012 R2 et Windows Server 2012.

## <a name="smb-multichannel-and-smb-direct"></a>SMB Multichannel et SMB Direct

SMB Multichannel est la fonctionnalité chargée de la détection des fonctions RDMA des cartes réseau pour activer SMB Direct. Sans SMB Multichannel, SMB utilise la connexion TCP/IP standard avec les cartes réseau prenant en charge RDMA (toutes les cartes réseau fournissent une pile TCP/IP avec la nouvelle pile RDMA).

Avec SMB Multichannel, SMB détecte si une carte réseau est dotée de la fonction RDMA, puis crée plusieurs connexions RDMA pour cette seule session (deux par interface). Cela permet à SMB d’utiliser le débit élevé, la faible latence et l’utilisation faible du processeur proposés par les cartes réseau prenant en charge RDMA. Il offre également une tolérance de panne si vous utilisez plusieurs interfaces RDMA.

>[!NOTE]
>Vous ne devez pas associer des cartes réseau prenant en charge RDMA si vous envisagez d’utiliser la fonction RDMA des cartes réseau. Lorsqu’elles sont associées, les cartes réseau ne prennent pas en charge RDMA.
>Une fois qu’au moins une connexion réseau RDMA est créée, la connexion TCP/IP utilisée pour la négociation du protocole d’origine n’est plus utilisée. Toutefois, la connexion TCP/IP est conservée en cas d’échec des connexions réseau RDMA.

## <a name="requirements"></a>Configuration requise

SMB Direct nécessite la configuration suivante :

- Au moins deux ordinateurs exécutant Windows Server 2012 R2 ou Windows Server 2012
- Une ou plusieurs cartes réseau dotées de la fonction RDMA.

### <a name="considerations-when-using-smb-direct"></a>Éléments à prendre en compte lors de l’utilisation de SMB Direct

- Vous pouvez utiliser SMB Direct dans un cluster de basculement ; toutefois, vous devez vous assurer que les réseaux de clusters utilisés pour l’accès client conviennent à SMB Direct. Le clustering avec basculement prend en charge l’utilisation de plusieurs réseaux pour l’accès client ainsi que les cartes réseau prenant en charge le partage du trafic entrant (RSS) et RDMA.
- Vous pouvez utiliser SMB Direct sur le système d’exploitation de gestion Hyper-V pour prendre en charge l’utilisation d’Hyper-V sur SMB et pour fournir un stockage à un ordinateur virtuel qui utilise la pile de stockage Hyper-V. Toutefois, les cartes réseau prenant en charge RDMA ne sont pas directement exposées à un client Hyper-V. Si vous connectez une carte réseau compatible RDMA à un commutateur virtuel, les cartes réseau virtuelles du commutateur ne prennent pas en charge RDMA.
- Si vous désactivez SMB Multichannel, SMB Direct est également désactivé. Dans la mesure où SMB Multichannel détecte les fonctions des cartes réseau et détermine si une carte réseau prend en charge RDMA, SMB Direct ne peut pas être utilisé par le client si SMB Multichannel est désactivé.
- SMB direct n’est pas pris en charge sur Windows RT. SMB direct nécessite la prise en charge des cartes réseau compatibles RDMA, qui est disponible uniquement sur Windows Server 2012 R2 et Windows Server 2012.
- SMB Direct n’est pas pris en charge sur les versions de bas niveau de Windows Server. Il est pris en charge uniquement sur Windows Server 2012 R2 et Windows Server 2012.

## <a name="enabling-and-disabling-smb-direct"></a>Activation et désactivation de SMB Direct

SMB direct est activé par défaut lorsque Windows Server 2012 R2 ou Windows Server 2012 est installé. Le client SMB détecte et utilise automatiquement plusieurs connexions réseau si une configuration appropriée est identifiée.

### <a name="disable-smb-direct"></a>Désactiver SMB direct

En règle générale, vous n’avez pas besoin de désactiver SMB Direct ; toutefois, vous pouvez le faire en exécutant l’un des scripts Windows PowerShell suivants.

Pour désactiver RDMA pour une interface spécifique, tapez :

```PowerShell
Disable-NetAdapterRdma <name>
```

Pour désactiver RDMA pour toutes les interfaces, tapez :

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Disabled
```

Lorsque vous désactivez RDMA sur le client ou le serveur, les systèmes ne peuvent pas l’utiliser. *Network direct* est le nom interne de la prise en charge de la mise en réseau de base de windows server 2012 R2 et windows server 2012 pour les interfaces RDMA.

### <a name="re-enable-smb-direct"></a>Réactiver SMB direct

Après avoir désactivé RDMA, vous pouvez le réactiver en exécutant l’un des scripts Windows PowerShell suivants.

Pour réactiver RDMA pour une interface spécifique, tapez :

```PowerShell
Enable-NetAdapterRDMA <name>
```

Pour réactiver RDMA pour toutes les interfaces, tapez :

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Enabled
```

Vous devez activer RDMA à la fois sur le client et le serveur pour recommencer à l’utiliser.

## <a name="test-performance-of-smb-direct"></a>Tester les performances de SMB Direct

Vous pouvez tester les performances en utilisant l’une des procédures suivantes.

### <a name="compare-a-file-copy-with-and-without-using-smb-direct"></a>Comparez une copie de fichiers avec et sans SMB Direct

Voici comment mesurer le débit accru de SMB direct :

1. Configurez SMB Direct
2. Mesurez le temps d’exécution d’une importante copie de fichiers avec SMB Direct.
3. Désactivez RDMA sur la carte réseau, voir [Activation et désactivation de SMB Direct](#enabling-and-disabling-smb-direct).
4. Mesurez le temps d’exécution d’une importante copie de fichiers sans SMB Direct.
5. Réactivez RDMA sur la carte réseau, puis comparez les deux résultats.
6. Pour éviter l’impact de la mise en cache, vous devez procéder comme suit :
    1. Copiez une grande quantité de données (plus de données que ce que la mémoire est capable de gérer).
    2. Copiez les données deux fois, la première fois comme exercice, puis en chronométrant la seconde copie.
    3. Redémarrez le serveur et le client avant chaque test pour être certain qu’ils fonctionnent dans des conditions semblables.

### <a name="fail-one-of-multiple-network-adapters-during-a-file-copy-with-smb-direct"></a>Provoquez l’échec de l’une des nombreuses cartes réseau pendant une copie de fichiers avec SMB Direct

Voici comment vérifier la fonctionnalité de basculement de SMB direct :

1. Vérifiez que SMB Direct fonctionne dans une configuration avec plusieurs cartes réseau.
2. Exécutez une importante copie de fichiers. Pendant que la copie est en cours d’exécution, simulez l’échec de l’un des chemins réseau en débranchant l’un des câbles (ou en désactivant l’une des cartes réseau).
3. Confirmez que la copie de fichiers se poursuit en utilisant l’une des cartes réseau restantes et qu’aucune erreur de copie de fichier ne se produit.

>[!NOTE]
>Pour éviter les échecs d’une charge de travail qui n’utilise pas SMB Direct, vérifiez qu’aucune autre charge de travail n’utilise le chemin réseau déconnecté.

## <a name="more-information"></a>Plus d’informations

- [Vue d’ensemble du bloc de message serveur](file-server-smb-overview.md)
- @no__t 0Increasing-disponibilité du serveur, du stockage et du réseau : Vue d’ensemble du scénario](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [Déployer Hyper-V sur SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
