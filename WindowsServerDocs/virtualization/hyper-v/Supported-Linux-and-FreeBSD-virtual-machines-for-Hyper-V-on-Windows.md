---
title: Machines virtuelles Linux et FreeBSD prises en charge pour Hyper-V sur Windows
description: Répertorie les fonctionnalités et les services d’intégration Linux inclus dans chaque version
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 990ff94a-30fb-434b-b4a2-3804a5245ba6
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: dff7a0f829f11a92f2c87305da806b9be43f42fe
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855942"
---
# <a name="supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows"></a>Machines virtuelles Linux et FreeBSD prises en charge pour Hyper-V sur Windows

>S’applique à : Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

Hyper-V prend en charge les appareils émulés et Hyper-V spécifiques pour les machines virtuelles Linux et FreeBSD. Lorsqu’il est exécuté avec des appareils émulés, aucun logiciel supplémentaire n’est requis pour l’installation. Toutefois, les appareils émulés n’offrent pas de performances élevées et ne peuvent pas tirer parti de la richesse de l’infrastructure de gestion des machines virtuelles offerte par la technologie Hyper-V. Pour tirer pleinement parti de tous les avantages offerts par Hyper-V, il est préférable d’utiliser des appareils spécifiques à Hyper-V pour Linux et FreeBSD. La collection de pilotes requis pour exécuter des appareils spécifiques à Hyper-V est appelée Linux Integration Services (LIS) ou FreeBSD Integration Services (BIS).

LIS a été ajouté au noyau Linux et est mis à jour pour les nouvelles versions. Toutefois, les distributions Linux basées sur des noyaux plus anciens peuvent ne pas avoir les améliorations ou les correctifs les plus récents. Microsoft fournit un téléchargement contenant des pilotes LIS installables pour certaines installations Linux basées sur ces noyaux plus anciens. Étant donné que les fournisseurs de distribution incluent des versions de Integration Services Linux, il est préférable d’installer la dernière version téléchargeable de LIS, le cas échéant, pour votre installation.

Pour les autres distributions Linux, les modifications de la LIS sont intégrées régulièrement au noyau du système d’exploitation et aux applications, de sorte qu’aucun téléchargement ou installation distinct n’est nécessaire.

Pour les anciennes versions de FreeBSD (avant 10,0), Microsoft fournit des ports contenant les pilotes de BIS installables et les démons correspondants pour les machines virtuelles FreeBSD. Pour les versions plus récentes de FreeBSD, BIS est intégré au système d’exploitation FreeBSD, et aucun téléchargement ou installation distinct n’est requis, à l’exception d’un téléchargement de ports KVP requis pour FreeBSD 10,0.

> [!TIP]
> - Téléchargez [Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) à partir du centre d’évaluation.

L’objectif de ce contenu est de fournir des informations qui facilitent le déploiement de Linux ou FreeBSD sur Hyper-V. Les détails spécifiques sont les suivants :

* Distributions Linux ou versions FreeBSD qui requièrent le téléchargement et l’installation des pilotes LIS ou BIS.

* Distributions Linux ou versions FreeBSD qui contiennent des pilotes LIS ou BIS intégrés.

* Les cartes de distribution des fonctionnalités qui indiquent les fonctionnalités des principales distributions Linux ou des versions FreeBSD.

* Problèmes connus et solutions de contournement pour chaque distribution ou publication.

* Description de la fonctionnalité pour chaque fonctionnalité LIS ou BIS.

**Vous souhaitez faire une suggestion sur les fonctionnalités et les fonctionnalités ?** Y a-t-il une meilleure amélioration ? Vous pouvez utiliser le site [Windows Server User Voice](https://windowsserver.uservoice.com/forums/295062-linux-support) pour suggérer de nouvelles fonctionnalités et fonctionnalités pour les machines virtuelles Linux et FreeBSD sur Hyper-V et pour voir ce que d’autres personnes disent.

## <a name="in-this-section"></a>Dans cette section

* [Ordinateurs virtuels CentOS et Red Hat Enterprise Linux pris en charge sur Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Debian prises en charge sur Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Ordinateurs virtuels Oracle Linux pris en charge sur Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Ordinateurs virtuels SUSE pris en charge sur Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Ubuntu prises en charge sur Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Ordinateurs virtuels FreeBSD pris en charge sur Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descriptions des fonctionnalités pour les machines virtuelles Linux et FreeBSD sur Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Meilleures pratiques pour l’exécution de Linux sur Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Meilleures pratiques pour exécuter FreeBSD sur Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
