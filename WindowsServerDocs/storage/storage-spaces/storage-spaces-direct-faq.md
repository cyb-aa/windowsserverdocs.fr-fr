---
title: Espaces de stockage directs - Forum aux questions
description: Découvrez qu’en est-il des espaces de stockage Direct
keywords: Espaces de stockage
ms.prod: windows-server-threshold
ms.author: kaushik
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: b17aa7ddc783e95fbcc19fe3913192d245133c7f
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066883"
---
# Espaces de stockage directs - Forum aux questions (FAQ)

Cet article répertorie quelques questions fréquentes et fréquemment posées relatives aux [Espaces de stockage Direct](storage-spaces-direct-overview.md).

## Lorsque vous utilisez les espaces de stockage Direct avec 3 nœuds, vous trouverez à la fois des performances et des niveaux de capacité?

Oui, vous pouvez obtenir à la fois des performances et capacité dans une configuration d’espaces de stockage Direct 2 ou 3-nœuds. Toutefois, il se peut que vous devez vous assurer que vous disposez de 2 lecteurs de capacité. Cela signifie que vous devez utiliser les trois types d’appareils: NVME, SSD et HDD.
 
## Système de fichiers Refs fournit tiaring en temps réel avec des espaces de stockage Direct. REFS fournit les mêmes fonctionnalités avec des espaces de stockage partagé dans 2016?

Non, vous n’obtiendrez pas en temps réel hiérarchisation avec des espaces de stockage partagé avec 2016. Il s’agit uniquement pour les espaces de stockage Direct. 
 
## Puis-je utiliser un système de fichiers NTFS avec des espaces de stockage Direct?
  
Oui, vous pouvez utiliser le système de fichiers NTFS avec des espaces de stockage Direct. Toutefois, il est recommandé de REFS. NTFS ne fournit pas la hiérarchisation en temps réel. 
 
## J’ai configuré les espaces de stockage Direct clusters de 2 nœuds, où le disque virtuel est configuré en tant que la résilience en miroir à 2 facteurs. Si j’ajouter un nouveau domaine d’erreur, la résilience du disque virtuel existant change?

Une fois que vous avez ajouté le nouveau domaine de tolérance, les nouveaux disques virtuels que vous créez saute en miroir 3 voies. Toutefois, le disque virtuel reste un disque en miroir à 2 facteurs. Vous pouvez copier les données sur les nouveaux disques virtuels sur les volumes existants pour bénéficier des avantages de la résilience nouvelle.
 
## Les espaces de stockage Direct a été créé à l’aide du commutateur autoconfig:0 et le pool créé manuellement. Quand j’essaie d’interroger le pool d’espaces de stockage Direct pour créer un nouveau volume, je reçois le message d’erreur, «Enable-ClusterS2D à nouveau.» Que dois-je faire?

Par défaut, lorsque vous configurez des espaces de stockage Direct à l’aide de l’applet de commande enable-S2D, l’applet de commande effectue tous les éléments pour vous. Il crée le pool et les niveaux. Lorsque vous utilisez autoconfig:0, tous les éléments doivent être effectuées manuellement. Si vous avez créé uniquement le pool, le niveau n’est pas nécessairement créé. Vous recevrez un message d’erreur «Enable-ClusterS2D à nouveau» si vous avez ne pas créé niveaux du tout ou ne pas créé les niveaux d’une manière correspondant aux périphériques connectés. Nous vous recommandons de ne pas utiliser le commutateur de configuration automatique dans un environnement de production. 
 
## Est-il possible d’ajouter un disque de rotation (HDD) à la liste des espaces de stockage Direct après avoir créé des espaces de stockage Direct avec des appareils SSD?

Non. Par défaut, si vous utilisez le type d’appareil unique pour créer le pool, il est inutile de configurer des disques de cache et permettraient tous les disques de capacité. Vous pouvez ajouter des disques NVME à la configuration et des disques NVME doit être configurés pour le cache.
 
## J’ai configuré un domaine d’erreur 2-rack: RACK 1 a 2 domaines d’erreur, RACK 2 a 1 domaine d’erreur. Chaque serveur a 4 unités de 100 Go de capacité. Puis-je utiliser totalité 1 200 Go d’espace à partir du pool?

Non, vous pouvez utiliser seulement 800 Go. Dans un domaine d’erreur rack, il se peut que vous devez vous assurer que vous disposez d’une configuration de mise en miroir à 2 facteurs afin que chaque chuck et ses terres en double dans un rack différents.
 
## Quelle doit être la taille du cache lorsque je suis configuration des espaces de stockage Direct?

Le cache doit être dimensionné pour la plage de travail (qui sont en cours activement les données lues ou écrites à tout moment) de vos applications et charges de travail.

## Comment puis-je déterminer la taille du cache qui est utilisé par les espaces de stockage Direct?

Utiliser l’utilitaire intégré Analyseur de performances pour inspecter les échecs du cache. Passez en revue le cache miss lectures/s depuis le compteur de Cluster Storage Hybrid Disk. N’oubliez pas que si trop de lectures manque le cache, le cache est probablement trop petit et vous devrez peut-être développer. 
 
## Existe-t-il une calculatrice qui montre la taille exacte des disques qui sont en cours réservée pour le cache, la capacité et la résilience qui serait me permettre de mieux planifier?

Vous pouvez utiliser la calculatrice d’espaces de stockage pour faciliter votre planification. Il est disponible à http://aka.ms/s2dcalc.
 
## Qu’est la meilleure configuration à recommander lors de la configuration des 6 serveurs et 3 racks?

Utilisez 2 serveurs sur chacun des racks pour obtenir la résilience de disque virtuel d’un miroir 3 voies. N’oubliez pas que la configuration de rack fonctionne correctement uniquement si vous fournissez la configuration au système d’exploitation de la manière qu’il est placé sur le rack. 
 
## Puis-je activer le mode de maintenance pour un disque spécifique sur un serveur spécifique dans un cluster d’espaces de stockage Direct?

Oui, vous pouvez activer le mode de maintenance stockage sur un disque spécifique et d’un domaine d’erreur spécifique. La commande Enable-StorageMaintenanceMode est appelée automatiquement lorsque vous suspendez un nœud. Vous pouvez l’activer pour un disque spécifique en exécutant la commande suivante:

```powershell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Enable-StorageMaintenanceMode
```

## Espaces de stockage Direct en charge mon matériel?

Nous vous conseillons de contacter votre fournisseur de matériel afin de vérifier la prise en charge. Fournisseurs de matériel testent la solution sur leur matériel et un commentaire sur si elle est prise en charge ou non. Par exemple, au moment de ce jour, serveurs tels que les R730 / R730xd / R630 ayant plus de 8 connecteurs de lecteur peut prendre en charge SES et sont compatibles avec les espaces de stockage Direct. Dell prend en charge uniquement les HBA330 avec des espaces de stockage Direct. R620 ne prend pas en charge SES et n’est pas compatible avec les espaces de stockage Direct.

Pour plus de matériel plus d’informations, accédez au site Web suivant: catalogue Windows Server
 
## Comment les espaces de stockage Direct effectue-t-il utiliser d’es?

Espaces de stockage Direct utilise mappage SCSI Enclosure Services (SES) pour vous assurer que les plaques de données et les métadonnées sont réparties sur les domaines d’erreur de manière résiliente. Si le matériel ne prend pas en charge SES, il n’existe aucun mappage des boîtiers et le positionnement des données n’est pas résilient.
 
## Vous pouvez utiliser pour vérifier l’extension physique d’un disque virtuel commande?
  
Celui-là:

```powershell
get-virtualdisk -friendlyname “xyz” | get-physicalextent
```
