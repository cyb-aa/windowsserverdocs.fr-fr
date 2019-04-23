---
title: Machines virtuelles prises en charge Linux et FreeBSD pour Hyper-V sur Windows
description: Répertorie les fonctionnalités incluses dans chaque version et les services d’intégration Linux
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 990ff94a-30fb-434b-b4a2-3804a5245ba6
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 9df495bdc67b06a675fec050fb4c2960337ce8ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832900"
---
# <a name="supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows"></a>Machines virtuelles prises en charge Linux et FreeBSD pour Hyper-V sur Windows

>S'applique à : Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Hyper-V prend en charge les périphériques émulés et spécifique à Hyper-V pour les machines virtuelles Linux et FreeBSD. Lors de l’exécution des périphériques émulés, aucun logiciel supplémentaire n’est nécessaire d’installer. Toutefois, les périphériques émulés ne fournissent pas de hautes performances et ne peut pas tirer parti de l’infrastructure de gestion riche machine virtuelle qui offre la technologie Hyper-V. Afin de tirer pleinement parti de tous les avantages de Hyper-V, il est préférable d’utiliser des appareils spécifiques à Hyper-V pour Linux et FreeBSD. La collection des pilotes qui sont nécessaires pour exécuter des appareils spécifiques à Hyper-V sont appelées des Services d’intégration Linux (LIS) ou les Services d’intégration de FreeBSD (BRI).

LIS a été ajouté au noyau Linux et est mis à jour pour les nouvelles versions. Mais les distributions Linux basées sur les anciens noyaux peut-être pas les dernières améliorations ou des correctifs. Microsoft fournit un téléchargement contenant les pilotes LIS installables pour certaines installations de Linux en fonction de ces noyaux plus anciens. Étant donné que les fournisseurs de distributions comprennent les versions de Linux Integration Services, il est préférable d’installer la dernière version téléchargeable de LIS, le cas échéant, pour votre installation.

Pour les autres distributions Linux LIS modifications sont régulièrement intégrées dans le noyau du système d’exploitation et les applications par conséquent, aucune installation ou téléchargement distinct n’est requise.

Pour les versions plus anciennes de FreeBSD (avant 10.0), Microsoft fournit des ports qui contiennent les pilotes installables de BIS et les démons correspondantes pour les machines virtuelles FreeBSD. Pour les versions plus récentes de FreeBSD, BIS est intégrée au système d’exploitation FreeBSD, et aucune installation ou téléchargement distinct n’est nécessaire à l’exception d’un téléchargement de ports de paire clé/valeur qui est nécessaire pour FreeBSD 10.0.

> [!TIP]
> - Télécharger [Windows Server 2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016) à partir du centre d’évaluation.
> - Télécharger [Microsoft Hyper-V Server 2016](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2016) à partir du centre d’évaluation.

L’objectif de ce contenu est de fournir les informations que vous aide à faciliter votre déploiement Linux ou de FreeBSD sur Hyper-V. Des détails spécifiques sont les suivantes :

* Distributions de Linux ou les versions FreeBSD qui nécessitent le téléchargement et l’installation des pilotes LIS ou BIS.

* Distributions de Linux ou les versions FreeBSD qui contiennent les pilotes LIS ou BIS intégrés.

* Mappages de distribution de fonctionnalité qui indiquent les fonctionnalités dans les distributions Linux majeures ou des versions de FreeBSD.

* Problèmes connus et solutions de contournement pour chaque distribution ou de la mise en production.

* Description de la fonctionnalité pour chaque fonctionnalité LIS ou BIS.

**Vous souhaitez faire une suggestion sur les fonctionnalités ?** Y a-t-il quelque chose que nous pourrions faire mieux ? Vous pouvez utiliser la [Windows Server User Voice](https://windowsserver.uservoice.com/forums/295062-linux-support) site suggérer de nouvelles fonctionnalités et fonctions pour Linux et les Machines virtuelles de FreeBSD sur Hyper-V et pour voir ce que disent les autres personnes.

## <a name="in-this-section"></a>Dans cette section

* [Prise en charge de CentOS et les machines virtuelles de Red Hat Enterprise Linux sur Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Prise en charge des machines virtuelles Debian sur Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Oracle Linux prises en charge sur Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles SUSE prises en charge sur Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Ubuntu prises en charge sur Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles FreeBSD prises en charge sur Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descriptions des fonctionnalités pour les machines virtuelles Linux et FreeBSD sur Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Meilleures pratiques pour l’exécution de Linux sur Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Meilleures pratiques pour l’exécution de FreeBSD sur Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
