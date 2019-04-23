---
title: Mise à l'échelle côté réception virtuelle (vRSS, Virtual Receive Side Scaling)
description: Découvrez Virtual l’échelle côté réception (vRSS) dans Windows Server et comment configurer une carte réseau virtuelle pour la charge de répartir le trafic réseau entrant sur plusieurs cœurs de processeurs logiques dans une machine virtuelle. Vous pouvez également configurer des cœurs physiques multiples pour un ordinateur hôte carte d’Interface réseau virtuelle (vNIC).
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9be477b3-f81d-4e84-a6b0-ac4c1ea97715
ms.date: 09/05/2018
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0c1cb11cb8ce69463a31cfa5061290f79d8dda91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875230"
---
# <a name="virtual-receive-side-scaling-vrss"></a>Virtuel trafic entrant \(vRSS\)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous découvrez virtuel l’échelle côté réception (vRSS) et comment configurer une carte réseau virtuelle pour la charge de répartir le trafic réseau entrant sur plusieurs cœurs de processeurs logiques dans une machine virtuelle. Vous pouvez également utiliser vRSS pour configurer plusieurs cœurs physiques d’un hôte virtuel carte d’Interface réseau \(carte réseau virtuelle\).

Cette configuration permet la charge d’une carte réseau virtuelle doit être réparti entre plusieurs processeurs virtuels dans une machine virtuelle \(machine virtuelle\), ce qui permet de traiter plus de trafic réseau plus rapidement que possible avec une seule de la machine virtuelle processeur logique.

>[!TIP]
>Vous pouvez utiliser vRSS des machines virtuelles sur Hyper\-hôtes V avec plusieurs processeurs, un seul cœur plusieurs, ou plusieurs plusieurs processeurs core installé et configuré pour une utilisation de la machine virtuelle.

vRSS est compatible avec tous les autres Hyper\-V technologies réseau. vRSS est dépendante de la file d’attente de la Machine virtuelle \(VMQ\) dans Hyper\-hôte V et RSS dans la machine virtuelle ou sur la carte réseau virtuelle hôte.

Par défaut, Windows Server permet de vRSS, mais vous pouvez le désactiver dans une machine virtuelle à l’aide de Windows PowerShell. Pour plus d’informations, consultez [gérer vRSS](vrss-manage.md) et [commandes PowerShell de Windows pour les flux RSS et vRSS](vrss-wps.md).



## <a name="operating-system-compatibility"></a>Compatibilité de système d’exploitation

Vous pouvez utiliser RSS sur n’importe quel ordinateur multiprocesseur ou multinoyau ou le vRSS sur toute machine virtuelle multiprocesseur ou multinoyau - qui exécute Windows Server 2016.

Multiprocesseur ou multicœur, les machines virtuelles exécutant les systèmes d’exploitation Microsoft suivants prennent également en charge vRSS.

- Windows Server 2016
- Windows 10 Pro ou entreprise
- Windows Server 2012 R2
- Windows 8.1 Pro ou entreprise
- Windows Server 2012 avec les composants d’intégration de Windows Server 2012 R2 installés.
- 8 Windows avec les composants d’intégration de Windows Server 2012 R2 installés.

Pour plus d’informations sur la prise en charge vRSS des ordinateurs virtuels en cours d’exécution FreeBSD ou Linux comme système d’exploitation invité sur Hyper-V, consultez [pris en charge de Linux et FreeBSD machines virtuelles pour Hyper-V sur Windows](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows).
  
## <a name="hardware-requirements"></a>Configuration matérielle requise

Voici la configuration matérielle requise pour vRSS.
 
- Cartes réseau physiques doivent prendre en charge la file d’attente de la Machine virtuelle \(VMQ\). Si VMQ est désactivé ou n’est pas pris en charge, vRSS est désactivé pour le Hyper\-hôte V et les machines virtuelles configurées sur l’ordinateur hôte.
- Cartes réseau doivent avoir une vitesse de liaison de 10 Gbits/s ou plus.
- Hyper\-hôtes V doivent être configurés avec au moins une multiples de plusieurs processeurs\-cœur de processeur pour utiliser vRSS.
- Machines virtuelles \(machines virtuelles\) doit être configuré pour utiliser deux ou plusieurs processeurs logiques.


## <a name="use-case-scenarios"></a>Scénarios d’utilisation

Les scénarios d’utilisation de deux suivantes décrivent une utilisation courante de vRSS pour l’équilibrage de charge de processeur et d’équilibrage de charge logicielle.

### <a name="processor-load-balancing"></a>Équilibrage de la charge processeur
  
Anthony, un administrateur réseau, consiste à configurer un nouvel hôte Hyper-V avec carte réseau deux qui prend en charge la virtualisation de sortie d’une racine unique \(SR\-IOV\). Il déploie Windows Server 2016 afin d’héberger un serveur de fichiers de machine virtuelle.

Après avoir installé les matériels et logiciels, Anthony configure un ordinateur virtuel utilise huit processeurs virtuels et 4 096 Mo de mémoire. Malheureusement, Anthony n’a pas la possibilité d’activer SR\-IOV, car ses machines virtuelles s’appuient sur l’application des stratégies via le commutateur virtuel, il a créé avec Hyper\-Gestionnaire de commutateur virtuel V. Pour cette raison, Anthony décide d’utiliser des vRSS au lieu de SR\-IOV.

Initialement, Anthony quatre processeurs virtuels à l’aide de Windows PowerShell soit disponible pour une utilisation avec vRSS. L’utilisation du serveur de fichiers après une semaine semblait être très populaire, Anthony vérifie les performances de la machine virtuelle.  Il découvre la pleine utilisation des quatre processeurs virtuels.

Pour cette raison, Anthony décide d’ajouter des processeurs à la machine virtuelle pour une utilisation par vRSS.  Il assigne deux processeurs virtuels supplémentaires à la machine virtuelle, qui sont automatiquement disponibles pour vRSS pour aider à gérer la charge réseau de grande taille. Ses efforts entraînent de meilleures performances pour le serveur de fichiers de machine virtuelle, avec les six processeurs gérant efficacement la charge du trafic réseau.


### <a name="software-load-balancing"></a>Équilibrage de la charge logicielle

Sandra, un administrateur réseau, consiste à configurer une seule machine virtuelle de hautes performances sur l’un de ses systèmes d’agir comme un équilibreur de charge logiciel. Elle a attribué à tous les processeurs logiques disponibles à cette machine virtuelle unique.

Après l’installation de Windows Server, elle utilise vRSS parallèles de trafic réseau de traitement sur plusieurs processeurs logiques dans la machine virtuelle. Windows Server permet de vRSS, Sandra n’a pas d’apporter des modifications de configuration.


## <a name="related-topics"></a>Rubriques connexes

- [Planifiez l’utilisation de vRSS](vrss-plan.md)
- [Activez vRSS sur une carte réseau virtuelle](vrss-enable.md)
- [Gérer vRSS](vrss-manage.md)
- [vRSS Forum aux Questions](vrss-faq.md)
- [Commandes PowerShell de Windows pour les flux RSS et vRSS](vrss-wps.md)

---
