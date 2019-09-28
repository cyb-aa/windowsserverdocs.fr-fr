---
title: Forum aux questions sur vRSS
description: Dans cette rubrique, vous trouverez des questions fréquentes et des réponses sur l’utilisation de vRSS.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 61ae242e-82a8-430d-b07d-52b86c01e686
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5c7feb696c6ee9014032229543a4f43fb5884527
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395846"
---
# <a name="vrss-frequently-asked-questions"></a>Forum aux questions sur vRSS

Dans cette rubrique, vous trouverez des questions fréquentes et des réponses sur l’utilisation de vRSS.

## <a name="what-are-the-requirements-for-the-physical-network-adapters-that-i-use-with-vrss"></a>Quelles sont les conditions requises pour les cartes réseau physiques que j’utilise avec vRSS ?

Les cartes réseau doivent être compatibles avec file d’attente d’ordinateurs virtuels \(\) d’ordinateurs virtuels et doivent avoir une vitesse de liaison de 10 Gbits/s ou plus.

Pour plus d’informations, consultez [planifier l’utilisation de vRSS](vrss-plan.md).

## <a name="does-vrss-work-with-hyper-threaded-processor-cores"></a>L’vRSS utilise-t\--il les cœurs de processeurs hyper-threads ?

Non. VRSS et des ordinateurs virtuels ignorent les cœurs de processeurs hyper\--thread.

## <a name="does-vrss-work-for-host-virtual-nics-vnics"></a>L’ordinateur vRSS fonctionne-t- \(il\)pour les cartes réseau virtuelles hôtes cartes réseau virtuelles ?

Oui. Utilisez le paramètre **-managementos** au lieu du\) nom de \(machine virtuelle de l’ordinateur virtuel sur la commande Windows PowerShell **Set-VMNetworkAdapter** , puis **activez-NetAdapterRss** sur l’hôte carte réseau virtuelle.

Pour plus d’informations, consultez [commandes Windows PowerShell pour RSS et vRSS](vrss-wps.md).

## <a name="how-many-logical-processors-does-a-vm-need-to-use-vrss"></a>Combien de processeurs logiques un ordinateur virtuel doit-il utiliser vRSS ?

Les machines virtuelles ont besoin \(\) de deux processeurs logiques ou plus pour pouvoir utiliser vRSS.

Pour plus d’informations, consultez [planifier l’utilisation de vRSS](vrss-plan.md).

## <a name="is-vrss-compatible-with-nic-teaming"></a>VRSS est-il compatible avec l’Association de cartes réseau ?

Oui. Si vous utilisez l’Association de cartes réseau, il est important de configurer correctement les ordinateurs virtuels de manière à ce qu’ils fonctionnent avec les paramètres d’association de cartes réseau. Pour plus d’informations sur le déploiement et la gestion de l’Association de cartes réseau, consultez [Association de cartes réseau](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

## <a name="vrss-is-enabled-but-how-do-i-know-if-it-is-working"></a>l’option vRSS est activée, mais comment savoir si elle fonctionne ? 

Vous pouvez indiquer à vRSS qu’il fonctionne en ouvrant le gestionnaire des tâches dans votre machine virtuelle et en affichant l’utilisation du processeur virtuel. Si plusieurs connexions sont établies avec la machine virtuelle, vous pouvez voir plusieurs cœurs au-dessus de 0% d’utilisation.

Étant donné qu’une seule session TCP ne peut pas faire l’équilibrage de charge entre plusieurs cœurs de processeurs logiques, votre machine virtuelle doit recevoir plusieurs sessions TCP pour que vous puissiez observer si vRSS fonctionne.

Si la machine virtuelle reçoit plusieurs sessions TCP, mais que vous ne voyez pas plus d’un cœur LP au-dessus de 0% d’utilisation, assurez-vous que vous avez effectué toutes les étapes de préparation de la rubrique [planifier l’utilisation de vRSS](vrss-plan.md).

## <a name="im-looking-at-the-host-and-not-all-of-the-processors-are-being-used-it-looks-like-every-other-one-is-being-skipped"></a>Je regarde l’hôte et les processeurs ne sont pas tous utilisés. Il semble qu’un sur deux soit ignoré.
  
Vérifiez si l’hyperthreading est activé. Les ordinateurs virtuels et vRSS sont conçus pour ignorer\-les cœurs hyper-thread.

## <a name="are-there-different-windows-powershell-commands-for-rss-and-vrss"></a>Existe-t-il différentes commandes Windows PowerShell pour RSS et vRSS ?

Oui et non. Si vous utilisez les mêmes commandes pour RSS dans des hôtes natifs et RSS dans des machines virtuelles, vRSS requiert également l’activation de l’ordinateur virtuel sur la carte réseau physique, et pour l’activation de la machine virtuelle et de vRSS sur le port de commutateur.

Pour plus d’informations, consultez [commandes Windows PowerShell pour RSS et vRSS](vrss-wps.md).

---
