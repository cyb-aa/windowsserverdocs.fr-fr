---
ms.assetid: 134840f3-c416-4a10-ad73-ef7855b206f7
title: Vue d’ensemble du démarrage de cible iSCSI
ms.prod: windows-server
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 9c359c7929ff90968d16e92b1128f4fc8ebde37c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402987"
---
# <a name="iscsi-target-boot-overview"></a>Vue d’ensemble du démarrage de cible iSCSI

> S’applique à : Windows Server 2016

Dans Windows Server, un serveur cible iSCSI peut démarrer des centaines d’ordinateurs à partir d’une seule image de système d’exploitation stockée à un emplacement centralisé. Cela contribue à améliorer l’efficacité, la simplicité de gestion, la disponibilité et la sécurité.  
  
## <a name="BKMK_OVER"></a>Description de la fonctionnalité  
En utilisant des disques durs virtuels de différenciation \(\)VHD, vous pouvez utiliser une seule image de système d’exploitation \(le\) « image maître » pour démarrer jusqu’à 256 ordinateurs. Par exemple, supposons que vous avez déployé Windows Server avec une image du système d’exploitation d’environ 20 Go, et que vous avez utilisé deux lecteurs de disque en miroir comme volume de démarrage. L’image de système d’exploitation aurait nécessité environ 10 To de stockage à elle seule pour démarrer 256 ordinateurs. Toutefois, avec le serveur cible iSCSI, vous utilisez 40 Go pour l’image de base du système d’exploitation et 2 Go pour les disques durs virtuels de différenciation par instance de serveur, soit un total de 552 Go pour les images de systèmes d’exploitation. Cela permet de gagner plus de 90 % d’espace de stockage juste pour les images de systèmes d’exploitation.  
  
## <a name="BKMK_APP"></a>Applications pratiques  
L’utilisation d’une image de système d’exploitation contrôlé offre les avantages suivants :  
  
**Plus sécurisée et plus facile à gérer.** Certaines entreprises exigent que les données soient protégées par un verrouillage physique du stockage dans un emplacement centralisé. Dans ce scénario, les serveurs accèdent aux données à distance, y compris pour l’image du système d’exploitation. Avec le serveur cible iSCSI, les administrateurs peuvent gérer de manière centralisée les images de démarrage des systèmes d’exploitation et contrôler les applications utilisant l’image principale.  
  
**Déploiement rapide.** . Dans la mesure où l’image principale est créée à l’aide de l’outil Sysprep, lorsque les ordinateurs démarrent à partir de cette image, ils ignorent la phase d’installation et de copie des fichiers qui a lieu pendant l’installation de Windows pour passer directement à la phase de personnalisation. Dans les tests internes de Microsoft, 256 ordinateurs ont été déployés en 34 minutes.  
  
**Récupération rapide.** Dans la mesure où les images de systèmes d’exploitation sont hébergées sur l’ordinateur exécutant le serveur cible iSCSI, si le client sans disque doit être remplacé, le nouvel ordinateur peut pointer vers l’image du système d’exploitation et démarrer immédiatement.  
  
> [!NOTE]  
> Plusieurs fournisseurs proposent une solution de démarrage de réseau de zone de stockage \(SAN\), qui peut être utilisée par le serveur cible iSCSI dans Windows Server sur du matériel standard.  
  
## <a name="BKMK_HARD"></a>Configuration matérielle requise  
Le serveur cible iSCSI ne nécessite pas de matériel spécifique. Dans les centres de données avec des déploiements à grande échelle\-, la conception doit être validée par rapport à un matériel spécifique. Pour référence, les tests internes de Microsoft ont indiqué qu’un déploiement d’ordinateur 256 nécessitait sur\-disques RPM dans une configuration RAID 10 pour le stockage. Une bande passante réseau de 10 Go est optimale. L’estimation générale est de 60 serveurs de démarrage iSCSI par gigaoctet de carte réseau.  
  
Aucune carte réseau n’est nécessaire pour ce scénario et un chargeur de démarrage logiciel peut être utilisé \(par exemple, le microprogramme de démarrage open source iPXE\).  
  
## <a name="BKMK_SOFT"></a>Configuration logicielle requise  
Le serveur cible iSCSI peut être installé en tant que service de rôle Services de fichiers et iSCSI dans le Gestionnaire de serveur.

> [!NOTE]
> Le démarrage d’un serveur Nano à partir d’iSCSI (à partir du serveur cible iSCSI Windows ou d’une implémentation cible tierce) n’est pas pris en charge.

## <a name="see-also"></a>Voir également
* [Serveur cible iSCSI](https://technet.microsoft.com/library/hh848272(v=ws.11).aspx)
* [applets de commande de l’initiateur iSCSI](https://technet.microsoft.com/library/hh826099(v=wps.640).aspx)
* [applets de commande du serveur cible iSCSI](https://technet.microsoft.com/library/jj612803(v=wps.630).aspx)