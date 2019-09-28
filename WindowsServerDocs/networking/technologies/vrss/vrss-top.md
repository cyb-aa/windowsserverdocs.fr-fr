---
title: Mise à l'échelle côté réception virtuelle (vRSS, Virtual Receive Side Scaling)
description: En savoir plus sur la mise à l’échelle côté réception virtuelle (vRSS) dans Windows Server et sur la configuration d’une carte réseau virtuelle pour équilibrer la charge du trafic réseau entrant sur plusieurs cœurs de processeur logiques dans une machine virtuelle. Vous pouvez également configurer de multicœurs physiques pour une carte d’interface réseau virtuelle hôte (carte réseau virtuelle).
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 9be477b3-f81d-4e84-a6b0-ac4c1ea97715
ms.date: 09/05/2018
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b24cabe3597af35e7c7f3c6f81d360bb11675e23
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395790"
---
# <a name="virtual-receive-side-scaling-vrss"></a>\(VRSS de mise à l’échelle côté réception virtuelle\)

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous découvrez la mise à l’échelle côté réception virtuelle (vRSS) et la configuration d’une carte réseau virtuelle pour équilibrer la charge du trafic réseau entrant sur plusieurs cœurs de processeurs logiques dans une machine virtuelle. Vous pouvez également utiliser vRSS pour configurer plusieurs cœurs physiques pour une carte \(d’interface réseau virtuelle d’ordinateur hôte carte réseau virtuelle.\)

Cette configuration permet de répartir la charge d’une carte réseau virtuelle entre plusieurs processeurs virtuels dans une machine \(virtuelle\)d’ordinateur virtuel, ce qui permet à la machine virtuelle de traiter davantage de trafic réseau plus rapidement qu’avec une seule processeur logique.

>[!TIP]
>Vous pouvez utiliser vRSS dans des machines virtuelles sur des ordinateurs hôtes Hyper\--V qui ont plusieurs processeurs, un processeur à plusieurs cœurs ou plusieurs processeurs à plusieurs cœurs installés et configurés pour l’utilisation de machines virtuelles.

vRSS est compatible avec toutes les autres\-technologies de mise en réseau Hyper-V. vRSS dépend de file d’attente d’ordinateurs virtuels \(\) ordinateur virtuel dans l’hôte\-Hyper-V et RSS dans la machine virtuelle ou sur l’ordinateur hôte carte réseau virtuelle.

Par défaut, Windows Server active vRSS, mais vous pouvez le désactiver dans une machine virtuelle à l’aide de Windows PowerShell. Pour plus d’informations, consultez [gérer](vrss-manage.md) les [commandes VRSS et Windows PowerShell pour RSS et vRSS](vrss-wps.md).



## <a name="operating-system-compatibility"></a>Compatibilité du système d’exploitation

Vous pouvez utiliser RSS sur n’importe quel ordinateur multiprocesseur ou multicœur (vRSS) sur n’importe quelle machine virtuelle multiprocesseur ou multicœur, qui exécute Windows Server 2016.

Les machines virtuelles multiprocesseur ou multicœur qui exécutent les systèmes d’exploitation Microsoft suivants prennent également en charge vRSS.

- Windows Server 2016
- Windows 10 professionnel ou entreprise
- Windows Server 2012 R2
- Windows 8.1 Pro ou Enterprise
- Windows Server 2012 avec les composants d’intégration Windows Server 2012 R2 installés.
- Windows 8 avec les composants d’intégration Windows Server 2012 R2 installés.

Pour plus d’informations sur la prise en charge de vRSS pour les machines virtuelles exécutant FreeBSD ou Linux en tant que système d’exploitation invité sur Hyper-V, consultez [machines virtuelles Linux et FreeBSD prises en charge pour Hyper-v sur Windows](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows).
  
## <a name="hardware-requirements"></a>Configuration matérielle requise

Voici la configuration matérielle requise pour vRSS.
 
- Les cartes réseau physiques doivent prendre en \(charge\)file d’attente d’ordinateurs virtuels d’ordinateurs virtuels. Si la machine virtuelle est désactivée ou n’est pas prise en charge\-, vRSS est désactivé pour l’hôte Hyper-V et toutes les machines virtuelles configurées sur l’ordinateur hôte.
- Les cartes réseau doivent avoir une vitesse de liaison de 10 Gbits/s ou plus.
- Les\-ordinateurs hôtes Hyper-V doivent être configurés avec plusieurs processeurs\-ou au moins un processeur multicœur pour utiliser le protocole vRSS.
- Les machines \(virtuelles de machines\) virtuelles doivent être configurées pour utiliser au moins deux processeurs logiques.


## <a name="use-case-scenarios"></a>Scénarios de cas d’usage

Les deux scénarios de cas d’usage suivants illustrent l’utilisation courante de vRSS pour l’équilibrage de la charge du processeur et l’équilibrage de la charge logicielle.

### <a name="processor-load-balancing"></a>Équilibrage de la charge processeur
  
Anthony, administrateur réseau, configure un nouvel hôte Hyper-V avec deux cartes réseau qui prennent en charge la virtualisation \(d’entrée-sortie à racine unique SR\-IOV\). Il déploie Windows Server 2016 pour héberger un serveur de fichiers de machine virtuelle.

Une fois le matériel et les logiciels installés, Anthony configure une machine virtuelle pour qu’elle utilise huit processeurs virtuels et 4096 Mo de mémoire. Malheureusement, Anthony n’a pas la possibilité d’activer SR\-IOV, car ses machines virtuelles s’appuient sur l’application de la stratégie via le commutateur virtuel qu’il a créé avec le gestionnaire de commutateur virtuel Hyper\-V. Pour cette raison, Anthony décide d’utiliser vRSS au lieu de\-SR IOV.

Au départ, Anthony affecte quatre processeurs virtuels à l’aide de Windows PowerShell pour une utilisation avec vRSS. L’utilisation du serveur de fichiers après une semaine semblait très populaire, de sorte que Anthony vérifie les performances de la machine virtuelle.  Il découvre l’utilisation complète des quatre processeurs virtuels.

Pour cette raison, Anthony décide d’ajouter des processeurs à la machine virtuelle pour une utilisation par vRSS.  Il affecte deux autres processeurs virtuels à la machine virtuelle, qui sont automatiquement disponibles pour vRSS afin de gérer la charge réseau importante. Ses efforts améliorent les performances du serveur de fichiers de machine virtuelle, avec les six processeurs qui gèrent efficacement la charge du trafic réseau.


### <a name="software-load-balancing"></a>Équilibrage de la charge logicielle

Sandra, administrateur réseau, configure une machine virtuelle haute performance unique sur l’un de ses systèmes pour qu’elle agisse comme un équilibreur de charge logiciel. Elle a affecté tous les processeurs logiques disponibles à cette machine virtuelle unique.

Après l’installation de Windows Server, elle utilise vRSS pour recevoir le trafic réseau parallèle sur plusieurs processeurs logiques de la machine virtuelle. Étant donné que Windows Server active vRSS, Sandra n’a pas besoin d’apporter de modifications à la configuration.


## <a name="related-topics"></a>Rubriques connexes

- [Planifier l’utilisation de vRSS](vrss-plan.md)
- [Activer vRSS sur une carte réseau virtuelle](vrss-enable.md)
- [Gérer vRSS](vrss-manage.md)
- [Forum aux questions sur vRSS](vrss-faq.md)
- [Commandes Windows PowerShell pour RSS et vRSS](vrss-wps.md)

---
